---
title: 'Understanding Immutable Objects in Java'
description: 
tags: ["advancedjava"] 
category: ["advanced java", "programming", "tutorial"]
date: 2024-12-22
permalink: '/advancedJava_understandingImmutability'
image:
  path: https://raw.githubusercontent.com/Gaur4vGaur/traveller/refs/heads/master/images/java/2024-12-22-advancedJava_understandingImmutability.jpg
  width: 800
  height: 500
---

## What is Immutability?
Before I discuss records and why they are needed, I need to articulate the concept of immutability. Immutability is a key aspect of clean and safe programming.

Let us define immutability - an immutable object is one whose state cannot be changed once instantiated, where the state is the data contained in the object instance. When an object's state is set, it stays the same throughout its lifetime. In Java, for example, immutable objects do not have any setter methods to guarantee their state never changes.

### Examples of Immutable Objects

Java’s standard library is rich with immutable classes, including:

* `String`
* Wrapper classes for primitives (e.g. `Integer`, `Double`)
* `BigInteger` and `BigDecimal`
* Date and time classes from the `java.time` package

These classes demonstrate the effectiveness of immutability in various contexts, from text processing to complex arithmetic and date-time manipulation.


## Why Choose Immutability?
Immutability is considered as best practice in many scenarios:

* **Simplified Reasoning**: 
Immutable objects make programs easier to understand and debug. When an object’s state cannot change unexpectedly, you can reason about its behaviour with confidence.

* **Thread Safety**: 
Immutable objects shine in multi-threaded environments. They are inherently thread-safe, eliminating the need for complicated synchronization mechanisms.

* **Reliability in Collections**: 
Immutable objects are perfect for use as keys in `HashMap` or elements in `HashSet` because their hash codes never change, ensuring data integrity.

* **Performance and Memory Efficiency**: 
Immutable objects can often be shared and reused, reducing memory overhead. For example, the JVM’s string pool allows reusing `String` objects to save memory and enhance performance. Java also reuse any existing objects while autoboxing and wrapping primitive values.

## Challenges of Immutability

While immutability has many advantages, it’s not without trade-offs:

* **Efficiency Concerns**: 
Modifying immutable objects is done by creating new instances. This, under certain conditions, leads to serious performance issues; for example, the concatenation of strings within a loop does not work well since every time a new `String` instance will be created. A better approach is to use `StringBuilder`, which is designed to mutate for such scenarios.

* **Circular References**: 
Creating circular references among immutable objects can present challenges. For example, when objects X and Y are required to reference one another. If objects are mutable, that is easy, initialise the fields at the time of creation. But, accomplishing this while preserving immutability is a challenge. It becomes a chicken-and-egg problem, where Object A must initialise with reference to B which does not exist yet and vice versa.
I can argue that it is not really a disadvantage as circular references are a code smell and show coupling.

These challenges notwithstanding, the benefits of immutability often outweigh its costs. Immutability promotes better design by avoiding problems such as tightly coupled classes, and it encourages separation of concerns.

## Creating an Immutable Class

### Sample `Product` POJO
Let me first create a usual POJO `Product` class that contains information about the product. I will add below three attributes to the class
* `id` of type `long` to uniquely identify the product
* `name` of type `string` to hold the name of the product
* `description` of type `string` to describe the product

```java
package demo;

public class Product {

    private long id;
    private String name;
    private String description;

    public Product(long id, String name, String description) {
        this.id = id;
        this.name = name;
        this.description = description;
    }

    public long getId() {
        return id;
    }

    public void setId(long id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getDescription() {
        return description;
    }

    public void setDescription(String description) {
        this.description = description;
    }

}

```

### Immutability Strategies
There is a list of rules that we need to apply to make the above class immutable. Let me first list them down:
* I first remove all the `setter` methods to restrict the modification of fields.
* I will make all fields `final` and `private`.
* I will make the class `final` to restrict mutable subclasses and use override methods.
* I will make sure that instance fields do not have any reference to mutable objects (I will cover this in detail in later articles).
* Finally, I will also provide `equals`, `hashCode` and `toString` methods for this class.

Now that I know, what needs to be done, let me action it next.

### Immutable `Product`
Here is the immutable `Product`.

As you can observe, there are quite a few considerations and the code is verbose. Although a lot of code I have generated is through my IDE, it is still verbose.

```java
public final class Product {

    private final long id;
    private final String name;
    private final String description;

    public Product(long id, String name, String description) {
        this.id = id;
        this.name = name;
        this.description = description;
    }

    public long getId() {
        return id;
    }

    public String getName() {
        return name;
    }

    public String getDescription() {
        return description;
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Product product = (Product) o;
        return id == product.id && Objects.equals(name, product.name) && Objects.equals(description, product.description);
    }

    @Override
    public int hashCode() {
        return Objects.hash(id, name, description);
    }

    @Override
    public String toString() {
        return "Product{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", description='" + description + '\'' +
                '}';
    }
}

```
 


## Wrap Up
By leveraging immutable objects, you write code that is more predictable, maintainable, and robust. But there are easier ways to achieve all of the above. This is where `Record` classes come in handy and allow the creation of immutable objects much easier. In my next post, I will introduce and discuss `Record` classes.

__Photo Credits__<br>
<sup>Header page image by <a href="https://unsplash.com/@markusspiske?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash">Markus Spiske</a> on Unsplash</sup><br>
