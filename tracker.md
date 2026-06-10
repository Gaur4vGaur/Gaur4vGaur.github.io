---
layout: randompage
permalink: /g7k9/
sitemap: false
robots: noindex
---

<script src="https://cdn.tailwindcss.com"></script>
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

<div class="bg-gray-100 min-h-screen p-4 md:p-8">

    <div class="max-w-4xl mx-auto grid grid-cols-1 md:grid-cols-2 gap-8">
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
        // Prompt credentials securely per device
        let GH_PAT = localStorage.getItem('gh_pat');
        let GH_REPO = localStorage.getItem('gh_repo'); // format: username/repo-name

        if (!GH_PAT) {
            GH_PAT = prompt("Enter your GitHub Personal Access Token (PAT):");
            if(GH_PAT) localStorage.setItem('gh_pat', GH_PAT);
        }
        if (!GH_REPO) {
            GH_REPO = prompt("Enter your private database path (e.g., yourusername/my-task-database):");
            if(GH_REPO) localStorage.setItem('gh_repo', GH_REPO);
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

        // Fetch data from Private Github File
        async function loadData() {
            if(!GH_PAT || !GH_REPO) return updateStatus("Missing configuration", "red");
            updateStatus("Syncing...", "yellow");
            
            try {
                const response = await fetch(API_URL, {
                    headers: { 'Authorization': `token ${GH_PAT}` }
                });
                if (!response.ok) throw new Error();
                const fileData = await response.json();
                
                fileSha = fileData.sha; // Capture SHA version required for saving updates
                // Convert base64 data file to text JSON
                // Strip out any newlines or spaces added by GitHub API formatting
                let cleanBase64 = fileData.content.replace(/\s/g, '');

                // Safely convert base64 data file to text JSON
                tasks = JSON.parse(decodeURIComponent(escape(atob(cleanBase64))));
                
                updateStatus("Synced with Cloud", "green");
                renderTasks();
            } catch (err) {
                updateStatus("Authentication/Sync Error", "red");
            }
        }

        // Push updated array back to GitHub
        async function pushData() {
            updateStatus("Saving...", "yellow");
            try {
                const response = await fetch(API_URL, {
                    method: 'PUT',
                    headers: {
                        'Authorization': `token ${GH_PAT}`,
                        'Content-Type': 'application/json'
                    },
                    body: JSON.stringify({
                        message: "Sync task changes via web panel",
                        content: btoa(unescape(encodeURIComponent(JSON.stringify(tasks)))), // Encode payload to base64
                        sha: fileSha
                    })
                });
                const resData = await response.json();
                fileSha = resData.content.sha; // Keep track of the newly generated file version SHA
                updateStatus("Synced with Cloud", "green");
            } catch (err) {
                updateStatus("Save error to GitHub", "red");
            }
        }

        function renderTasks() {
            taskList.innerHTML = '';
            const sortedTasks = [...tasks].sort((a, b) => a.completed - b.completed);

            sortedTasks.forEach((task) => {
                const li = document.createElement('li');
                li.className = `flex items-center justify-between p-3.5 rounded-xl border ${
                    task.completed ? 'bg-gray-50 border-gray-200 opacity-60' : 'bg-white border-gray-200 shadow-sm'
                }`;

                li.innerHTML = `
                    <div class="flex items-center gap-3 flex-1 min-w-0">
                        <input type="checkbox" ${task.completed ? 'checked' : ''} class="w-5 h-5 rounded cursor-pointer checkbox-toggle">
                        <span class="truncate text-gray-700 font-medium ${task.completed ? 'line-through text-gray-400' : ''}">${task.name}</span>
                    </div>
                    <button class="text-gray-400 hover:text-red-500 px-2 delete-btn">Delete</button>
                `;

                li.querySelector('.checkbox-toggle').addEventListener('change', () => toggleTask(task.id));
                li.querySelector('.delete-btn').addEventListener('click', () => deleteTask(task.id));
                taskList.appendChild(li);
            });

            updateChart();
        }

        taskForm.addEventListener('submit', async (e) => {
            e.preventDefault();
            const name = taskInput.value.trim();
            if (!name) return;

            tasks.push({ id: Date.now().toString(), name: name, completed: false, history: {} });
            taskInput.value = '';
            renderTasks();
            await pushData();
        });

        async function toggleTask(id) {
            const task = tasks.find(t => t.id === id);
            if (!task) return;

            task.completed = !task.completed;
            const currentMonth = getCurrentMonthKey();

            if (task.completed) {
                task.history[currentMonth] = (task.history[currentMonth] || 0) + 1;
            } else if (task.history[currentMonth] > 0) {
                task.history[currentMonth]--;
            }

            renderTasks();
            await pushData();
        }

        async function deleteTask(id) {
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

            const datasets = tasks.map((task, idx) => {
                const dataPoints = monthsToShow.map(month => task.history?.[month] || 0);
                const colors = ['#3b82f6', '#10b981', '#f59e0b', '#ef4444', '#8b5cf6'];
                return {
                    label: task.name,
                    data: dataPoints,
                    backgroundColor: colors[idx % colors.length],
                    borderWidth: 0,
                    borderRadius: 6
                };
            }).filter(ds => ds.data.reduce((a, b) => a + b, 0) > 0);

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
