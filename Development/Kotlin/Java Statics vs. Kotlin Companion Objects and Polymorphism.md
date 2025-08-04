- [ ] Java Statics vs. Kotlin Companion Objects and Polymorphism ➕ 2025-08-03 

### I need an understanding of fields and methods in a Java class.

#### What Are Fields in Java?

Fields are **variables** inside a class that store data (like attributes of an object).

- **Types**: Primitive (e.g., `int num = 5;`) or reference (e.g., `String name = "Alice";`).
    
- **Scope**: Can be instance (per object) or static (shared across all objects).
    
- **Default**: Uninitialized fields get default values (e.g., `int` is `0`, `boolean` is `false`).
    

_Example_


```Java
class Person {
    int age;  // Field (instance variable)
    static String species = "Human";  // Static field
}
```

#### What Are Methods in Java?

Methods are **blocks of code** that perform actions or calculations (like behaviors of an object).

- **Structure**: Return type, name, parameters, and a body.
    
- **Types**: Instance (requires an object) or static (class-level, no object needed).
    
- **Calling**: Use the method name with parentheses `()` (e.g., `obj.myMethod();`).
    

_Example_



```Java
class Person {
    void greet(String name) {  // Method with a parameter
        System.out.println("Hello, " + name);
    }

    static void info() {  // Static method
        System.out.println("This is static.");
    }
}
```

#### Key Differences

|Aspect|Fields|Methods|
|---|---|---|
|**Purpose**|Store data|Perform actions|
|**Example**|`int x = 10;`|`void doSomething() {}`|
|**Access**|Directly (e.g., `obj.x`)|Call with `()` (e.g., `obj.doSomething()`)|

---

### A static field is loaded when the class is loaded, so we can access it without an instance. If we try to access a non-static field in a static method, it will cause a runtime error. Is this correct?

This is a **compile-time error**, not a runtime error.

- **Static fields/methods**: Are loaded with the class and do not require an instance to be created. They are accessed via the class name (e.g., `MyClass.staticField`).
    
- **Non-static (instance) fields/methods**: Require an object instance to exist (e.g., `MyClass obj = new MyClass(); obj.instanceField;`).
    
- **The Error**: Static methods lack a `this` reference to a specific object instance. Therefore, they cannot directly access instance members (fields or methods). The Java compiler catches this logical impossibility before the program runs to ensure safety.
    

To fix this, you must create an instance of the class within the static method or make the field static as well.

_Example_



```Java
class Example {
    int instanceField = 10;  // Non-static

    static void staticMethod() {
        // System.out.println(instanceField);  // This line causes a COMPILE ERROR!

        Example obj = new Example();  // Fix: Instantiate the class
        System.out.println(obj.instanceField);  // OK: Access the field via the instance
    }
}
```

---

### Regarding Kotlin's companion object, it's similar to Java's static members but is a true object that can inherit from other classes or interfaces, providing polymorphism. It's loaded when the class is loaded. In contrast, Java's static fields are also loaded with the class but are just variables and cannot inherit. A Java static nested class is similar but must be instantiated before use. Am I right?

You are mostly correct. Here are the precise details.

#### Quick Corrections

- **Companion Objects**: You are right. They are like Java statics in that they are class-level and loaded with the class. However, they are full singleton **objects**. This means they can implement interfaces or extend classes, enabling polymorphism. You can access their members via the class name (e.g., `MyClass.Companion.method()` or simply `MyClass.method()`).
    
- **Java Static Fields/Methods**: Correct. They are loaded with the class but are simply variables and methods, not objects. They cannot participate in inheritance or polymorphism.
    
- **Java Static Nested Classes**: Correct. They are class-level (do not require an instance of the outer class), but you must **instantiate** them to use their members (e.g., `new Outer.StaticNested()`). They are not singletons by default.
    

#### Key Differences Table

|Feature|Kotlin Companion Object|Java Static Field/Method|Java Static Nested Class|
|---|---|---|---|
|**Type**|Singleton object (can inherit)|Just a variable/method (no inheritance)|A class (needs `new` instance)|
|**Polymorphism**|**Yes** (implements/extends)|**No**|Possible, but requires manual setup|
|**Access**|`ClassName.method()`|`ClassName.field`|`new Outer.StaticNested()`|
|**Loaded**|With the class|With the class|With the class|

---

### What is Polymorphism, and can you show some examples in Kotlin?

Polymorphism, meaning "many forms," allows objects of different classes to be treated as objects of a common superclass or interface. This is fundamental to writing flexible and reusable code.

#### Compile-Time Polymorphism (Overloading)

This occurs when you have multiple methods with the same name but different parameters (either in number or type). The correct method to call is determined at compile time.

_Example_



```Kotlin
fun add(a: Int, b: Int): Int = a + b      // Function for two integers
fun add(a: Double, b: Double): Double = a + b // Function for two doubles

fun main() {
    println(add(2, 3))      // Outputs: 5
    println(add(2.5, 3.5))  // Outputs: 6.0
}
```

#### Runtime Polymorphism (Overriding)

This happens when a subclass provides a specific implementation for a method that is already defined in its superclass. The correct method is determined at runtime based on the actual object type.

_Example_



```Kotlin
// The 'open' keyword allows this class to be inherited from
open class Shape {
    // The 'open' keyword allows this method to be overridden
    open fun area(): Double = 0.0
}

class Circle(val radius: Double) : Shape() {
    override fun area(): Double = Math.PI * radius * radius
}

class Square(val side: Double) : Shape() {
    override fun area(): Double = side * side
}

fun main() {
    val shapes: Array<Shape> = arrayOf(Circle(2.0), Square(3.0))

    for (shape in shapes) {
        // The correct 'area()' method is called depending on the actual object
        println(shape.area())
    }
    // Outputs: 12.566... (for the circle), 9.0 (for the square)
}
```

---

### So, my understanding is this: since a companion object supports inheritance, it can inherit from an interface or another class and override methods from that superclass. Am I right?

Yes, you are correct. A companion object is a full-fledged singleton object and can extend a class or implement one or more interfaces. This allows it to override methods and participate in polymorphism.

_Example_



```Kotlin
interface Logger {
    fun log(message: String)
}

class MyClass {
    // The companion object implements the Logger interface
    companion object : Logger {
        override fun log(message: String) {
            println("Logged: $message")
        }
    }
}

fun main() {
    // Call the log method directly on the class
    MyClass.log("Hello")  // Outputs: Logged: Hello
}
```

---

### Can a companion object extend another class? I have no knowledge about this.

Yes, a companion object can extend **one class**. However, it cannot extend multiple classes, as Kotlin enforces a **single inheritance** rule for all classes, including companion objects. It can, however, implement multiple interfaces.

This feature allows the "static" part of your class to inherit behavior and state from a base class.

_Example_



```Kotlin
open class Base {
    open fun greet() = "Hello from Base"
}

class MyClass {
    // The companion object extends the Base class
    companion object : Base() {
        // It overrides the greet method from Base
        override fun greet() = "Hello from Companion"
    }
}

fun main() {
    println(MyClass.greet())  // Outputs: Hello from Companion
}
```

---

### Then, in Kotlin, can any class inherit from only one class?

Yes, that is correct. Kotlin enforces **single inheritance**, which means a class can extend (or inherit from) only **one** superclass. This design choice helps avoid the complexity and ambiguity that can arise from multiple class inheritance (like the "Diamond Problem").

However, a class can implement **multiple interfaces**, allowing it to conform to many different contracts or behaviors.

_Example_



```Kotlin
open class Animal { /* ... */ }
interface Flyable { /* ... */ }
interface Swimmable { /* ... */ }

// Bird extends one class (Animal) but implements two interfaces (Flyable, Swimmable)
class Bird : Animal(), Flyable, Swimmable {
    /* ... */
}
```

---

### Questions for Review

1. What is the main difference between a field and a method in a Java class?
    
2. Why does accessing a non-static member from a static context result in a compile-time error instead of a runtime error?
    
3. What are the three key differences between a Kotlin `companion object` and a Java `static nested class`?
    
4. Explain the difference between compile-time (overloading) and runtime (overriding) polymorphism.
    
5. How does a Kotlin companion object's ability to inherit from an interface make it more powerful than Java's static methods?
    
6. What is the "single inheritance" rule in Kotlin?
    
7. If a Kotlin class can only extend one superclass, how can it take on multiple "behaviors" or "types"?