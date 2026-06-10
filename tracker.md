---
layout: tracker
permalink: /g7k9/
sitemap: false
robots: noindex
---

<script src="https://cdn.tailwindcss.com"></script>
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

<div class="bg-gray-100 min-h-screen p-4 md:p-6">

    <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
        <div class="bg-white p-6 rounded-2xl shadow-sm border border-gray-200">
            <div class="flex justify-between items-center mb-6">
                <h1 class="text-2xl font-bold text-gray-800">My Tasks</h1>
                <span id="sync-status" class="text-xs font-semibold px-2.5 py-1 rounded bg-yellow-100 text-yellow-800">Loading...</span>
            </div>

            <form id="task-form" class="flex gap-2 mb-6">
                <input type="text" id="task-input" required placeholder="Add a task..." class="flex-1 px-4 py-2 border rounded-xl text-gray-700">
                <button type="submit" class="bg-blue-600 text-white px-5 py-2 rounded-xl font-medium">Add</button>
            </form>

            <ul id="task-list" class="space-y-3"></ul>
        </div>

        <div class="bg-white p-6 rounded-2xl shadow-sm border border-gray-200 flex flex-col justify-between">
            <div>
                <h2 class="text-xl font-bold text-gray-800 mb-2">Analytics</h2>
                <p class="text-sm text-gray-500 mb-6">Total completions per month</p>
                <div class="w-full h-64 relative">
                    <canvas id="analyticsChart"></canvas>
                </div>
            </div>
            <button onclick="clearCredentials()" class="text-xs text-red-400 hover:text-red-600 self-end mt-4">Reset App Credentials</button>
        </div>
    </div>

    <script>
        let GH_PAT = localStorage.getItem('gh_pat');
        let GH_REPO = localStorage.getItem('gh_repo');

        if (!GH_PAT) {
            GH_PAT = prompt("Enter your GitHub Personal Access Token (PAT):");
            if (GH_PAT) localStorage.setItem('gh_pat', GH_PAT);
        }
        if (!GH_REPO) {
            GH_REPO = prompt("Enter your private database path (e.g., yourusername/my-task-database):");
            if (GH_REPO) localStorage.setItem('gh_repo', GH_REPO);
        }

        const API_URL = `https://api.github.com/repos/${GH_REPO}/contents/tasks/tasks.json`;
        let tasks = [];
        let fileSha = null;
        let chartInstance = null;

        const statusEl = document.getElementById('sync-status');
        const taskForm = document.getElementById('task-form');
        const taskInput = document.getElementById('task-input');
        const taskList = document.getElementById('task-list');

        function getCurrentMonthKey() {
            const now = new Date();
            return `${now.getFullYear()}-${String(now.getMonth() + 1).padStart(2, '0')}`;
        }

        function getRelativeTime(isoString) {
            if (!isoString) return 'never';
            const diff = Date.now() - new Date(isoString).getTime();
            const minutes = Math.floor(diff / 60000);
            if (minutes < 2) return 'just now';
            if (minutes < 60) return `${minutes} minutes ago`;
            const hours = Math.floor(minutes / 60);
            if (hours < 24) return `${hours} hour${hours > 1 ? 's' : ''} ago`;
            const days = Math.floor(hours / 24);
            if (days < 30) return `${days} day${days > 1 ? 's' : ''} ago`;
            const months = Math.floor(days / 30);
            return `${months} month${months > 1 ? 's' : ''} ago`;
        }

        function sortedTasks() {
            return [...tasks].sort((a, b) => {
                if (!a.lastCompleted && !b.lastCompleted) return 0;
                if (!a.lastCompleted) return -1;
                if (!b.lastCompleted) return 1;
                return new Date(a.lastCompleted) - new Date(b.lastCompleted);
            });
        }

        async function loadData() {
            if (!GH_PAT || !GH_REPO) return updateStatus("Missing configuration", "red");
            updateStatus("Syncing...", "yellow");
            try {
                const response = await fetch(API_URL, {
                    headers: { 'Authorization': `token ${GH_PAT}` }
                });
                if (!response.ok) throw new Error();
                const fileData = await response.json();
                fileSha = fileData.sha;
                let cleanBase64 = fileData.content.replace(/\s/g, '');
                tasks = JSON.parse(decodeURIComponent(escape(atob(cleanBase64))));

                // Migrate old completed tasks to new model
                let needsMigration = false;
                tasks.forEach(t => {
                    if (!('lastCompleted' in t)) {
                        t.lastCompleted = t.completed ? new Date().toISOString() : null;
                        needsMigration = true;
                    }
                    t.completed = false;
                    if (!t.history) t.history = {};
                });
                if (needsMigration) await pushData(false);

                updateStatus("Synced with Cloud", "green");
                renderTasks();
            } catch (err) {
                updateStatus("Authentication/Sync Error", "red");
            }
        }

        async function pushData(updateStatus_ = true) {
            if (updateStatus_) updateStatus("Saving...", "yellow");
            try {
                const response = await fetch(API_URL, {
                    method: 'PUT',
                    headers: {
                        'Authorization': `token ${GH_PAT}`,
                        'Content-Type': 'application/json'
                    },
                    body: JSON.stringify({
                        message: "Sync task changes via web panel",
                        content: btoa(unescape(encodeURIComponent(JSON.stringify(tasks)))),
                        sha: fileSha
                    })
                });
                const resData = await response.json();
                fileSha = resData.content.sha;
                if (updateStatus_) updateStatus("Synced with Cloud", "green");
            } catch (err) {
                if (updateStatus_) updateStatus("Save error to GitHub", "red");
            }
        }

        function renderTasks() {
            taskList.innerHTML = '';
            sortedTasks().forEach((task) => {
                const li = document.createElement('li');
                li.className = 'flex items-center justify-between p-3.5 rounded-xl border bg-white border-gray-200 shadow-sm';

                const lastDoneText = getRelativeTime(task.lastCompleted);
                const monthCount = task.history[getCurrentMonthKey()] || 0;

                li.innerHTML = `
                    <div class="flex items-center gap-3 flex-1 min-w-0">
                        <input type="checkbox" class="w-5 h-5 rounded cursor-pointer checkbox-toggle">
                        <div class="min-w-0">
                            <span class="truncate text-gray-700 font-medium block">${task.name}</span>
                            <span class="text-xs text-gray-400">Last done: ${lastDoneText} &middot; This month: ${monthCount}</span>
                        </div>
                    </div>
                    <button class="text-gray-400 hover:text-red-500 px-2 delete-btn flex-shrink-0">Delete</button>
                `;

                li.querySelector('.checkbox-toggle').addEventListener('change', (e) => {
                    e.target.checked = false;
                    tickTask(task.id);
                });
                li.querySelector('.delete-btn').addEventListener('click', () => deleteTask(task.id));
                taskList.appendChild(li);
            });

            updateChart();
        }

        taskForm.addEventListener('submit', async (e) => {
            e.preventDefault();
            const name = taskInput.value.trim();
            if (!name) return;
            tasks.push({ id: Date.now().toString(), name: name, lastCompleted: null, history: {} });
            taskInput.value = '';
            renderTasks();
            await pushData();
        });

        async function tickTask(id) {
            const task = tasks.find(t => t.id === id);
            if (!task) return;
            const currentMonth = getCurrentMonthKey();
            task.history[currentMonth] = (task.history[currentMonth] || 0) + 1;
            task.lastCompleted = new Date().toISOString();
            renderTasks();
            await pushData();
        }

        async function deleteTask(id) {
            const task = tasks.find(t => t.id === id);
            if (!task) return;
            const hasHistory = Object.values(task.history || {}).some(v => v > 0);
            if (hasHistory && !confirm(`Delete "${task.name}"? This will permanently remove its completion history.`)) return;
            tasks = tasks.filter(t => t.id !== id);
            renderTasks();
            await pushData();
        }

        function updateChart() {
            const ctx = document.getElementById('analyticsChart').getContext('2d');
            const monthsToShow = [];
            const d = new Date();
            for (let i = 3; i >= 0; i--) {
                const targetDate = new Date(d.getFullYear(), d.getMonth() - i, 1);
                monthsToShow.push(`${targetDate.getFullYear()}-${String(targetDate.getMonth() + 1).padStart(2, '0')}`);
            }

            const colors = ['#3b82f6', '#10b981', '#f59e0b', '#ef4444', '#8b5cf6'];
            const datasets = tasks.map((task, idx) => ({
                label: task.name,
                data: monthsToShow.map(month => task.history?.[month] || 0),
                backgroundColor: colors[idx % colors.length],
                borderWidth: 0,
                borderRadius: 6
            })).filter(ds => ds.data.reduce((a, b) => a + b, 0) > 0);

            if (chartInstance) chartInstance.destroy();

            chartInstance = new Chart(ctx, {
                type: 'bar',
                data: {
                    labels: monthsToShow.map(m => {
                        const [year, month] = m.split('-');
                        return new Date(year, month - 1).toLocaleString('default', { month: 'short', year: '2-digit' });
                    }),
                    datasets: datasets
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    scales: { y: { beginAtZero: true, ticks: { stepSize: 1 } } }
                }
            });
        }

        function updateStatus(text, color) {
            statusEl.textContent = text;
            const colors = {
                yellow: "bg-yellow-100 text-yellow-800",
                green: "bg-green-100 text-green-800",
                red: "bg-red-100 text-red-800"
            };
            statusEl.className = `text-xs font-semibold px-2.5 py-1 rounded ${colors[color]}`;
        }

        function clearCredentials() {
            localStorage.clear();
            location.reload();
        }

        loadData();
    </script>
</div>
