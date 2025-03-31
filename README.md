# Java-most-asked-questions

### **Static Blocks and Static Initializers in Java**  

Static blocks (also called **static initializers**) are special blocks of code in Java that are executed **when the class is loaded into memory**, before any objects are created or static methods are called.  

---

## **1. What is a Static Block?**  
- Defined using the `static` keyword followed by `{}`.  
- Runs **only once** when the class is first loaded by the JVM.  
- Used for **initializing static variables** or performing **one-time setup tasks**.  

### **Syntax**  
```java
public class MyClass {
    static {
        // Static block code
        System.out.println("Static block executed!");
    }
}
```

---

## **2. Why Use Static Blocks?**  
- Initialize **static variables** that require complex logic.  
- Load **native libraries** (e.g., `System.loadLibrary()`).  
- Perform **one-time initialization** (e.g., database connections).  

---

## **3. Example of Static Block**  

### **Example 1: Basic Static Block**  
```java
public class Test {
    static int x;

    static {
        x = 10; // Initialize static variable
        System.out.println("Static block: x = " + x);
    }

    public static void main(String[] args) {
        System.out.println("Main method: x = " + x);
    }
}
```
**Output:**  
```
Static block: x = 10  
Main method: x = 10  
```

### **Example 2: Multiple Static Blocks**  
Static blocks execute **in the order they appear** in the class.  
```java
public class Test {
    static int a;

    static {
        a = 5;
        System.out.println("First static block: a = " + a);
    }

    static {
        a = 10;
        System.out.println("Second static block: a = " + a);
    }

    public static void main(String[] args) {
        System.out.println("Main method: a = " + a);
    }
}
```
**Output:**  
```
First static block: a = 5  
Second static block: a = 10  
Main method: a = 10  
```

---

## **4. Static Block vs Instance Initializer Block**  
| Feature               | Static Block (`static {}`)       | Instance Initializer Block (`{}`) |  
|-----------------------|----------------------------------|----------------------------------|  
| **Execution Time**    | When class is loaded (once)      | When object is created (per object) |  
| **Usage**             | Initialize `static` variables    | Initialize instance variables    |  
| **Example**           | `static { x = 10; }`            | `{ System.out.println("Init block"); }` |  

---

## **5. Real-World Use Case**  
### **Loading Database Driver (JDBC)**  
```java
public class Database {
    static {
        try {
            Class.forName("com.mysql.cj.jdbc.Driver"); // Load driver
            System.out.println("Driver loaded!");
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
    }

    public static void main(String[] args) {
        System.out.println("Main method executed.");
    }
}
```
**Output:**  
```
Driver loaded!  
Main method executed.  
```

---

## **6. Key Points to Remember**  
✔ **Executes only once** when the class is loaded.  
✔ Runs **before** `main()` and **before** any static method call.  
✔ Can be used to **initialize static variables** with complex logic.  
✔ Multiple static blocks run **in the order they are defined**.  

---

## **7. Common Pitfalls**  
❌ **Cannot access non-static members** inside a static block.  
❌ **Cannot throw checked exceptions** unless handled.  

```java
static {
    int y = 20; // OK (local variable)
    // System.out.println(this.x); ❌ Error (cannot use 'this')
}
```

---

## **8. When to Use Static Blocks?**  
✅ Loading **JDBC drivers**.  
✅ Initializing **static maps or configurations**.  
✅ Setting up **logging frameworks**.  

---

### **Final Summary**  
- **Static blocks** (`static {}`) run when the class is loaded.  
- Used for **static variable initialization** and **one-time setup**.  
- Multiple blocks execute **in order of appearance**.  
- Cannot access **non-static members** or throw **unhandled checked exceptions**.  

### **How to Call One Constructor from Another in Java**  

In Java, you can call one constructor from another within the same class using the **`this()`** keyword. This is known as **constructor chaining**.  

---

## **1. Rules for Constructor Chaining**  
✔ Must be the **first statement** in the constructor.  
✔ Can only call **one constructor per `this()`**.  
✔ Cannot use `this()` and `super()` together (both can’t be first).  

---

## **2. Example: Basic Constructor Chaining**  
```java
public class Person {
    String name;
    int age;

    // Default constructor
    public Person() {
        this("Anonymous", 18); // Calls parameterized constructor
        System.out.println("Default constructor called.");
    }

    // Parameterized constructor
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
        System.out.println("Parameterized constructor called.");
    }

    public static void main(String[] args) {
        Person p1 = new Person(); // Triggers both constructors
    }
}
```
**Output:**  
```
Parameterized constructor called.  
Default constructor called.  
```

---

## **3. Real-World Use Case: Reducing Code Duplication**  
Instead of repeating initialization logic:  
```java
public class Car {
    String model;
    int year;
    double price;

    // Constructor 1: Sets default price
    public Car(String model, int year) {
        this(model, year, 25000.0); // Calls Constructor 2
    }

    // Constructor 2: Full initialization
    public Car(String model, int year, double price) {
        this.model = model;
        this.year = year;
        this.price = price;
    }
}
```
**Usage:**  
```java
Car car1 = new Car("Tesla", 2023);      // Uses default price  
Car car2 = new Car("BMW", 2022, 50000); // Custom price  
```

---

## **4. Constructor Chaining with Inheritance (`super()`)**  
If the class extends another class, use `super()` to call the parent constructor first.  
```java
class Animal {
    String type;
    Animal(String type) {
        this.type = type;
    }
}

class Dog extends Animal {
    String breed;
    
    Dog(String type, String breed) {
        super(type); // Calls Animal's constructor
        this.breed = breed;
    }
}
```

---

## **5. Common Mistakes**  
❌ **Calling `this()` after other statements:**  
```java
public Person() {
    System.out.println("Hello"); // ❌ Compile error
    this("Anonymous", 18);       // Must be the first statement
}
```

❌ **Circular Chaining (StackOverflowError):**  
```java
public class Circle {
    public Circle() {
        this(10); // Calls parameterized constructor
    }
    public Circle(int radius) {
        this();   // ❌ Infinite loop
    }
}
```

---

## **6. When to Use Constructor Chaining?**  
✅ **Avoid duplicate code** (e.g., setting default values).  
✅ **Simplify complex object creation** (e.g., builders).  
✅ **Initialize inherited fields** (using `super()`).  

---

### **Key Takeaways**  
- Use **`this(args)`** to call another constructor in the same class.  
- Must be the **first statement** in the constructor.  
- Works with **`super()`** in inheritance hierarchies.  
- Avoid **circular chaining** (leads to infinite loops).  

