Exploring Java Records
Records are a way to make creating immutable data classes much easier. These are not a standard feature in Java. Continuing from my previous post on immutable objects <TODO link>, in this post, I will try to explore Record classes with example.

What Are Record Classes?
Record classes are a special kind of class in Java designed to simplify the creation of data-carrying classes. They are ideal for classes that are primarily used to store data and have minimal behaviour. A record class automatically generates boilerplate code such as constructors, getters, equals(), hashCode(), and toString() methods.

Key Features of Record Classes
1.	Concise Syntax: Record classes reduce the verbosity of Java code. Instead of writing lengthy classes with multiple fields and methods, you can define a record class in a single line.
2.	Immutable Data: By default, record classes are immutable. Once an instance is created, its state cannot be changed, which makes them thread-safe and easier to reason about.
3.	Automatic Method Generation: Java automatically generates essential methods like equals(), hashCode(), and toString(), reducing boilerplate code and potential errors.
Creating Record Class
I will convert the previous immutable `Product` class into a record class <TODO link> with the same fields. Let me rewrite it to show the difference.

```java
public record Product(long id, String name, String description) {}
```
And that's it! With just a single line of code, I have created my first record class. The compiler translates this into a final class with private final fields and automatically generates the following for me:
•	A constructor: public Product (long id, String name, String description), where it assigns its parameters to the fields that are generated for the components
•	Getter methods without `get` in front: 
o	public long id() { return this.id; } 
o	public String name() { return this.name; } 
o	public String description() { return this.description; }
•	equals(), hashCode(), and toString() methods
To create a record class, I used the keyword record. Following the name of the record, I specified the list of attributes, which resembles the parameter list of constructors. Finally, the body of the class is enclosed within curly braces, similar to a regular class.
Using Record Classes
Creating an instance of a record works exactly like creating an instance of a regular class, using the new keyword. I will create a new instance of the Product record by providing a list of arguments. Additionally, I will demonstrate the usage of accessor methods by fetching the values of ID, name, and description from the Product record and printing them to the console.

```java
public static void main(String[] args) {
    var product = new Product(101L, "Milk", "Whole milk from Fresh farms");

    var productId = product.id();
    var productName = product.name();
    var productDescription = product.description();

    System.out.printf("The product %d is %s (%s)", productId, productName, productDescription);
}
```

Customizing Record Classes
While record classes are designed to be concise, they can still be customized as needed. You can add custom methods to the body of a record class. For example, I will add a custom method to check if a product has a valid description. This method will return `true` if the description is neither null nor blank.
```java
public record Product(long id, String name, String description) {

    public boolean hasValidDescription() {
        return description != null && !description.isBlank();
    }
}
```
And this method can called from the main() like any other method
```java
product.hasValidDescription();
```
There are, however, some limitations on what you can do within the body of a record class. For instance, you cannot add instance fields, although static fields are allowed. Even, non-final and mutable static fields are permitted. This is because mutable static fields exist at the class level and do not compromise the immutability of record instances.
```java
public record Product(long id, String name, String description) {

    private String newField; // <- not allowed
    private static String newStaticField;
}
```
Wrap Up
I believe Record classes are best suited when we need to use them as Data transfer object (DTOs), Value object or simple data carriers, with minimal boiler plate code. However, they might not be best choice to have behaviour or state.
