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

### **Why Java is Platform Independent?**  

Java is **platform-independent** because of its **"Write Once, Run Anywhere" (WORA)** capability. This is achieved through a combination of **bytecode** and the **Java Virtual Machine (JVM)**.  

---

## **1. Key Reasons for Platform Independence**  

### **1. Compilation to Bytecode (Not Machine Code)**  
- Java code is compiled into **bytecode** (`.class` files) instead of platform-specific machine code.  
- Bytecode is a **universal intermediate format** that can run on any system with a JVM.  

### **2. JVM Executes Bytecode**  
- The **JVM** (Java Virtual Machine) reads and executes bytecode.  
- Each OS (Windows, Linux, macOS) has its own **JVM implementation**, but all understand the **same bytecode**.  

### **3. No Direct Interaction with OS**  
- Java programs run inside the JVM, **not directly on the hardware**.  
- The JVM handles OS-specific operations (e.g., memory management, threading).  

---

## **2. How It Works?**  
```
Java Code (.java) → (Compiled by javac) → Bytecode (.class) → (Executed by JVM) → Runs on any OS  
```

### **Example:**  
```java
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello, World!");
    }
}
```
- **Step 1:** `javac HelloWorld.java` → Produces `HelloWorld.class` (bytecode).  
- **Step 2:** `java HelloWorld` → JVM executes the bytecode.  
- The **same `.class` file** runs on **Windows, Linux, or macOS** without recompilation.  

---

## **3. Comparison with Platform-Dependent Languages (C/C++)**  
| Feature          | Java (Platform Independent) | C/C++ (Platform Dependent) |  
|------------------|----------------------------|----------------------------|  
| **Compilation**  | → Bytecode (`.class`)       | → Machine code (`.exe`, `.o`) |  
| **Execution**    | Needs JVM                  | Runs directly on OS         |  
| **Portability**  | WORA (Write Once, Run Anywhere) | Needs recompilation for each OS |  

---

## **4. Role of JVM and JIT Compiler**  
- **JVM** interprets bytecode line-by-line (slower).  
- **JIT Compiler (Just-In-Time)** optimizes bytecode into **machine code at runtime** (faster execution).  

---

## **5. Why is Java Not 100% Platform Independent?**  
- **GUI apps** may behave differently across OS (e.g., `Swing`, `JavaFX`).  
- **Native methods** (`JNI`) break portability.  
- **System-dependent libraries** (e.g., file paths differ in Windows vs. Linux).  

---

## **6. Key Benefits of Platform Independence**  
✅ **No need to rewrite code** for different OS.  
✅ **Easier software distribution** (single `.jar` or `.class` file works everywhere).  
✅ **Reduced development cost** (no OS-specific versions).  

---

### **Final Summary**  
- Java is **platform-independent** because it uses **bytecode + JVM**.  
- The **same `.class` file** runs on any OS with a compatible JVM.  
- **WORA (Write Once, Run Anywhere)** is possible because the JVM abstracts OS differences.  

Here’s a concise yet comprehensive breakdown of all your Java questions:

---

### **16) What is Encapsulation?**  
**Definition:**  
- Encapsulation is an OOP concept that **bundles data (fields) and methods (behavior)** into a single unit (class) and **restricts direct access** to internal state.  

**How to Achieve It:**  
- Use **`private`** fields + **public getters/setters**.  
```java
public class BankAccount {
    private double balance; // Hidden data

    public void deposit(double amount) { // Controlled access
        if (amount > 0) balance += amount;
    }
    public double getBalance() { return balance; }
}
```

**Why Use It?**  
✔ Prevents invalid state changes.  
✔ Improves maintainability.  

---

### **17) Why is `main()` Method `public`, `static`, and `void` in Java?**  
- **`public`** → JVM must access it from anywhere.  
- **`static`** → No need to create an object to call `main()`.  
- **`void`** → Returns nothing to the JVM.  
```java
public static void main(String[] args) { ... }
```

---

### **19) What is a Constructor in Java?**  
**Definition:**  
- A special method that **initializes objects** when created.  

**Key Points:**  
- Same name as the class.  
- No return type.  
- Default constructor is added if none is defined.  
```java
class Car {
    Car() { // Constructor
        System.out.println("Object created!");
    }
}
```

---

### **10) What is JIT Compiler?**  
**Just-In-Time Compiler:**  
- Part of the JVM that **optimizes bytecode into machine code at runtime** for faster execution.  
- **Why?** → Improves performance by avoiding interpretation of bytecode repeatedly.  

---

### **11) What is Bytecode in Java?**  
- **Platform-independent intermediate code** (`.class` files) generated by `javac`.  
- Executed by the JVM (not directly by the CPU).  
```bash
javac Hello.java → Hello.class (bytecode) → JVM runs it
```

---

### **Difference Between `this()` and `super()`**  
| **`this()`**                          | **`super()`**                          |  
|---------------------------------------|---------------------------------------|  
| Calls **another constructor** in the **same class**. | Calls a **parent class constructor**. |  
| Must be the **first line** in a constructor. | Must be the **first line** in a constructor. |  
| Used for **constructor chaining**.     | Used to **initialize inherited fields**. |  

**Example:**  
```java
class Animal {
    Animal() { System.out.println("Animal constructor"); }
}

class Dog extends Animal {
    Dog() {
        super(); // Calls Animal() (optional if no-arg)
        System.out.println("Dog constructor");
    }
}
```

---

### **Summary Table**  
| Concept               | Key Point                                                                 |  
|-----------------------|--------------------------------------------------------------------------|  
| **Encapsulation**     | Hide data with `private` + expose via methods.                           |  
| **`main()` method**   | `public static void` for JVM access without object creation.             |  
| **Constructor**       | Initializes objects; no return type.                                     |  
| **JIT Compiler**      | Speeds up execution by converting bytecode → machine code at runtime.    |  
| **Bytecode**          | `.class` files run by JVM (not OS-specific).                             |  
| **`this()` vs `super()`** | `this()` chains constructors; `super()` calls parent constructors.   |  

### **How Bit Shifting Works in Java (`>>`, `>>>`, `<<`)**  

Bit shifting is a **low-level operation** that moves the bits of a number left or right. Java has **three shift operators**:  

| Operator | Name                     | Behavior                                                                 |
|----------|--------------------------|--------------------------------------------------------------------------|
| `<<`     | **Left Shift**           | Shifts bits left, fills empty spots with `0`. Equivalent to **multiplying by 2ⁿ**. |
| `>>`     | **Signed Right Shift**   | Shifts bits right, fills empty spots with the **sign bit** (keeps negative numbers negative). |
| `>>>`    | **Unsigned Right Shift** | Shifts bits right, fills empty spots with `0` (ignores sign bit). |

---

## **1. Left Shift (`<<`)**  
- **Moves bits to the left** and inserts `0`s on the right.  
- **Equivalent to multiplying by `2ⁿ`** (where `n` = shift amount).  

### **Example: `5 << 2`**  
1. Binary of `5`:  
   ```
   00000000 00000000 00000000 00000101
   ```
2. Shift left by `2`:  
   ```
   00000000 00000000 00000000 00010100 (Result: 20)
   ```
   - **Calculation:** `5 * 2² = 20`  

**Code:**  
```java
System.out.println(5 << 2); // Output: 20
```

---

## **2. Signed Right Shift (`>>`)**  
- **Moves bits to the right** and fills the leftmost bits with the **sign bit** (`1` for negative, `0` for positive).  
- **Equivalent to dividing by `2ⁿ`** (for positive numbers).  

### **Example 1: `-8 >> 1`**  
1. Binary of `-8` (32-bit):  
   ```
   11111111 11111111 11111111 11111000
   ```
2. Shift right by `1` (fills left with `1` because the sign is negative):  
   ```
   11111111 11111111 11111111 11111100 (Result: -4)
   ```
   - **Calculation:** `-8 / 2¹ = -4`  

**Code:**  
```java
System.out.println(-8 >> 1); // Output: -4
```

### **Example 2: `16 >> 2`**  
1. Binary of `16`:  
   ```
   00000000 00000000 00000000 00010000
   ```
2. Shift right by `2` (fills left with `0` because the sign is positive):  
   ```
   00000000 00000000 00000000 00000100 (Result: 4)
   ```
   - **Calculation:** `16 / 2² = 4`  

**Code:**  
```java
System.out.println(16 >> 2); // Output: 4
```

---

## **3. Unsigned Right Shift (`>>>`)**  
- **Moves bits to the right** and **always fills left with `0`** (ignores the sign bit).  
- Used when treating numbers as **unsigned** (e.g., for bit manipulation).  

### **Example: `-8 >>> 1`**  
1. Binary of `-8` (32-bit):  
   ```
   11111111 11111111 11111111 11111000
   ```
2. Shift right by `1` (fills left with `0`):  
   ```
   01111111 11111111 11111111 11111100 (Result: 2147483644)
   ```
   - **No direct arithmetic meaning** (treats `-8` as an unsigned number).  

**Code:**  
```java
System.out.println(-8 >>> 1); // Output: 2147483644
```

---

## **Key Differences Between `>>` and `>>>`**  
| Scenario          | `>>` (Signed)                  | `>>>` (Unsigned)               |
|-------------------|--------------------------------|--------------------------------|
| **Positive Number** | Fills left with `0`.           | Fills left with `0`.           |
| **Negative Number** | Fills left with `1`.           | Fills left with `0`.           |
| **Use Case**      | Arithmetic operations (e.g., division). | Bit manipulation (e.g., hashing). |

---

## **When to Use Bit Shifting?**  
✔ **Fast multiplication/division** by powers of 2 (`x << n` = `x * 2ⁿ`, `x >> n` = `x / 2ⁿ`).  
✔ **Bitmasking** (e.g., extracting RGB values from a pixel).  
✔ **Low-level optimizations** (e.g., cryptography, hashing).  

**Example: Extracting Red from RGB**  
```java
int pixel = 0xFFAABB; // Hex for RGB (255, 170, 187)
int red = (pixel >> 16) & 0xFF; // Right-shift 16 bits, then mask
System.out.println(red); // Output: 255
```

---

### **Summary**  
- `<<` → **Left shift** (multiply by `2ⁿ`).  
- `>>` → **Signed right shift** (preserves sign, divides by `2ⁿ`).  
- `>>>` → **Unsigned right shift** (fills with `0`, ignores sign).  

### **20) Difference Between `length` and `length()` in Java**  

| **`length`**                          | **`length()`**                          |
|--------------------------------------|----------------------------------------|
| Used with **arrays** (e.g., `int[]`, `String[]`). | Used with **String objects**. |
| A **property** (not a method). | A **method** of the `String` class. |
| Example: `arr.length` | Example: `str.length()` |

**Example:**  
```java
int[] numbers = {1, 2, 3};
System.out.println(numbers.length); // Output: 3 (array property)

String name = "Java";
System.out.println(name.length()); // Output: 4 (String method)
```

---

### **Difference Between Character Constant and String Constant in Java**  

| **Character Constant (`' '`)**        | **String Constant (`" "`)**            |
|--------------------------------------|----------------------------------------|
| Single quotes (`'a'`). | Double quotes (`"hello"`). |
| Holds **a single Unicode character**. | Holds **a sequence of characters**. |
| **Primitive type** (`char`). | **Object type** (`String`). |
| Example: `char c = 'A';` | Example: `String s = "ABC";` |

**Example:**  
```java
char letter = 'J'; // Valid (character constant)
String word = "Java"; // Valid (string constant)
// char invalid = 'AB'; ❌ Error (multiple chars)
```

---

### **25) Difference Between `>>` and `>>>` Operators in Java**  

| **`>>` (Signed Right Shift)**         | **`>>>` (Unsigned Right Shift)**       |
|--------------------------------------|----------------------------------------|
| Preserves the **sign bit** (fills leftmost bits with the original sign). | Ignores the sign bit (fills leftmost bits with **0**). |
| Used for **arithmetic shifts** (negative numbers stay negative). | Used for **logical shifts** (treats numbers as unsigned). |
| Example: `-8 >> 1` → `-4` (keeps sign). | Example: `-8 >>> 1` → `2147483644` (fills with 0). |

**Example:**  
```java
int a = -8; // Binary: 11111111111111111111111111111000
System.out.println(a >> 1);  // Output: -4 (Signed shift)
System.out.println(a >>> 1); // Output: 2147483644 (Unsigned shift)
```

---

### **Key Takeaways**  
1. **`length` vs `length()`**  
   - `length` → Arrays (property).  
   - `length()` → Strings (method).  

2. **Character (`' '`) vs String (`" "`) Constants**  
   - `char` → Single character, primitive.  
   - `String` → Sequence of chars, object.  

3. **`>>` vs `>>>`**  
   - `>>` → Preserves sign (arithmetic shift).  
   - `>>>` → Ignores sign (logical shift).  

### **38) Allowed Access Modifiers for Top-Level Classes in Java**  
In Java, a **top-level class** (not nested) can have only **two access modifiers**:  

1. **`public`** → The class is accessible from any other class.  
   ```java
   public class MyClass { ... }  // Accessible everywhere
   ```
2. **Default (no modifier)** → The class is accessible only within the **same package**.  
   ```java
   class MyPackagePrivateClass { ... }  // Accessible only in its package
   ```

❌ **Not Allowed for Top-Level Classes:**  
- `private` (only for nested classes).  
- `protected` (only for members, not classes).  

---

### **Java Coding Standards for Interfaces**  
1. **Naming Convention**:  
   - Use **PascalCase** (e.g., `Runnable`, `Serializable`).  
   - Prefix with `I` (optional, e.g., `IPayment`).  

2. **Method Declarations**:  
   - All methods are **implicitly `public abstract`** (no need to specify).  
   ```java
   interface Vehicle {
       void start();  // Automatically public abstract
   }
   ```

3. **Constants**:  
   - Fields are **implicitly `public static final`**.  
   ```java
   interface Constants {
       int MAX_SPEED = 100;  // Automatically public static final
   }
   ```

4. **Default/Static Methods (Java 8+)**:  
   - Use `default` for methods with implementations.  
   - Use `static` for utility methods.  
   ```java
   interface Logger {
       default void log(String message) {  // Default method
           System.out.println(message);
       }
       static void debug(String msg) {     // Static method
           System.out.println("[DEBUG] " + msg);
       }
   }
   ```

5. **Functional Interfaces (Java 8+)**:  
   - Should have **only one abstract method** (e.g., `Runnable`, `Comparator`).  
   ```java
   @FunctionalInterface
   interface Greeter {
       void greet(String name);  // Single abstract method
   }
   ```

---

### **35) `instanceof` Operator in Java**  
- **Purpose**: Checks if an object is an **instance of a class/interface** at runtime.  
- **Syntax**: `object instanceof Class/Interface`  
- **Returns**: `true` if the object is an instance, else `false`.  

**Example**:  
```java
Object obj = "Hello";
System.out.println(obj instanceof String);  // true
System.out.println(obj instanceof Integer); // false
```

**Use Cases**:  
- **Type checking** before casting (avoid `ClassCastException`).  
- **Polymorphism** handling.  

**Java 16+ Pattern Matching**:  
```java
if (obj instanceof String s) {  // Directly casts and assigns to 's'
    System.out.println(s.length());
}
```

---

### **36) What Does `null` Mean in Java?**  
- **Definition**: `null` is a **special literal** representing the absence of an object.  
- **Default Value**: For all **reference types** (e.g., `String`, `Object`).  
- **Key Behaviors**:  
  - Cannot call methods on `null` (throws `NullPointerException`).  
  - `null == null` evaluates to `true`.  

**Example**:  
```java
String str = null;
System.out.println(str);          // Output: null
System.out.println(str.length()); // Throws NullPointerException
```

**Handling `null` Safely**:  
1. **Null Checks**:  
   ```java
   if (str != null) {
       System.out.println(str.length());
   }
   ```
2. **Optional Class (Java 8+)**:  
   ```java
   Optional<String> optionalStr = Optional.ofNullable(str);
   optionalStr.ifPresent(s -> System.out.println(s.length()));
   ```

---

### **Summary Table**  
| Topic                     | Key Points                                                                 |
|---------------------------|---------------------------------------------------------------------------|
| **Top-Class Modifiers**   | Only `public` or default (package-private).                               |
| **Interface Standards**   | PascalCase, implicit `public abstract` methods, `default`/`static` methods allowed. |
| **`instanceof`**         | Runtime type check, returns `boolean`, used for safe casting.             |
| **`null`**               | Default for references, causes `NullPointerException` if misused.         |


### **43) What are Access Modifiers in Java?**  
**Access modifiers** control the **visibility** of classes, methods, and variables in Java. They determine which parts of your code can access these members.  

#### **Types of Access Modifiers:**  
1. **`public`** → Accessible **everywhere**.  
2. **`protected`** → Accessible within the **same package + subclasses (even in other packages)**.  
3. **`default` (no keyword)** → Accessible only within the **same package**.  
4. **`private`** → Accessible only within the **same class**.  

---

### **44) Difference Between Access Specifiers and Access Modifiers**  
| Term               | Meaning                                                                 |
|--------------------|-------------------------------------------------------------------------|
| **Access Modifier** | Official Java term (e.g., `public`, `private`, `protected`, `default`). |
| **Access Specifier** | Older term (same as access modifier). **No difference in Java.**       |

> In Java, both terms refer to the same thing. The correct term is **access modifier**.

---

### **45) Access Modifiers for Classes**  
| Modifier    | Allowed for Top-Level Class? | Allowed for Nested Class? |
|-------------|-----------------------------|--------------------------|
| **`public`** | ✅ Yes                      | ✅ Yes                   |
| **`protected`** | ❌ No                       | ✅ Yes                   |
| **`private`** | ❌ No                       | ✅ Yes                   |
| **`default`** | ✅ Yes (no keyword)         | ✅ Yes                   |

**Example:**  
```java
public class MyClass { ... }       // ✅ Top-level public class  
private class InnerClass { ... }   // ✅ Nested private class  
protected class Invalid { ... }    // ❌ Top-level protected (invalid)  
```

---

### **46) Access Modifiers for Methods**  
| Modifier    | Visibility                                                                 |
|-------------|----------------------------------------------------------------------------|
| **`public`** | Accessible **everywhere**.                                                 |
| **`protected`** | Accessible in **same package + subclasses (even in other packages)**.      |
| **`default`** | Accessible only in **same package**.                                       |
| **`private`** | Accessible only in **same class**.                                         |

**Example:**  
```java
public void show() { ... }       // Accessible everywhere  
protected void display() { ... } // Accessible in package + subclasses  
void print() { ... }             // Package-private (default)  
private void log() { ... }       // Only within the same class  
```

---

### **47) Access Modifiers for Variables**  
Same rules as methods:  

| Modifier    | Visibility                                                                 |
|-------------|----------------------------------------------------------------------------|
| **`public`** | Accessible **everywhere**.                                                 |
| **`protected`** | Accessible in **same package + subclasses**.                               |
| **`default`** | Accessible only in **same package**.                                       |
| **`private`** | Accessible only in **same class**.                                         |

**Example:**  
```java
public int x = 10;         // Accessible everywhere  
protected int y = 20;      // Accessible in package + subclasses  
int z = 30;                // Package-private (default)  
private int w = 40;        // Only within the same class  
```

---

### **48) Final Access Modifier in Java**  
The **`final`** keyword is **not an access modifier** but a **non-access modifier** that restricts further modification.  

#### **Uses of `final`:**  
1. **Final Variable** → Value cannot be changed (constant).  
   ```java
   final int MAX = 100;  
   // MAX = 200; ❌ Error (cannot reassign)  
   ```
2. **Final Method** → Cannot be **overridden** by subclasses.  
   ```java
   public final void preventOverride() { ... }  
   ```
3. **Final Class** → Cannot be **extended (inherited)**.  
   ```java
   public final class Immutable { ... }  
   // class SubClass extends Immutable { ❌ Error }  
   ```

---

### **Summary Table**  
| Concept               | Key Points                                                                 |
|-----------------------|---------------------------------------------------------------------------|
| **Access Modifiers**  | `public`, `protected`, `default`, `private` (control visibility).         |
| **Class Modifiers**   | Top-level: `public` or `default`. Nested: All 4.                          |
| **Method/Variable**   | All 4 modifiers apply.                                                    |
| **`final`**          | Makes variables constant, methods un-overridable, classes un-inheritable. |

### **49) Abstract Classes in Java**  
**Definition:**  
An **abstract class** is a class that **cannot be instantiated** (cannot create objects) and may contain **abstract methods** (methods without a body).  

**Key Points:**  
✔ Can have **both abstract and concrete methods**.  
✔ Used to provide a **common base** for subclasses.  
✔ Must be extended by a subclass to be used.  

**Example:**  
```java
abstract class Animal {
    abstract void sound();  // Abstract method (no body)
    
    void sleep() {          // Concrete method
        System.out.println("Zzz");
    }
}

class Dog extends Animal {
    void sound() {          // Must override abstract method
        System.out.println("Bark!");
    }
}
```

---

### **50) Can We Create a Constructor in an Abstract Class?**  
**✅ Yes!**  
- Abstract classes **can have constructors**, but they are **called only when a subclass is instantiated**.  
- Used to initialize common properties for subclasses.  

**Example:**  
```java
abstract class Vehicle {
    String type;
    
    Vehicle(String type) {  // Constructor
        this.type = type;
    }
}

class Car extends Vehicle {
    Car() {
        super("Car");  // Calls abstract class constructor
    }
}
```

---

### **51) Abstract Methods in Java**  
**Definition:**  
- A method **without a body** (only declaration) that must be **overridden by subclasses**.  
- Defined using the `abstract` keyword.  

**Rules:**  
✔ Can only exist in **abstract classes/interfaces**.  
✔ Subclasses **must override** them (or be declared `abstract`).  

**Example:**  
```java
abstract class Shape {
    abstract double area();  // No implementation
}

class Circle extends Shape {
    double radius;
    
    double area() {          // Must override
        return 3.14 * radius * radius;
    }
}
```

---

### **52) What is an Exception in Java?**  
**Definition:**  
An **exception** is an **unexpected event** that disrupts normal program flow (e.g., division by zero, file not found).  

**Example:**  
```java
int x = 10 / 0;  // Throws ArithmeticException
```

---

### **53) Situations Where Exceptions May Arise**  
1. **ArithmeticException** → Division by zero.  
2. **NullPointerException** → Accessing `null` object.  
3. **ArrayIndexOutOfBoundsException** → Invalid array index.  
4. **FileNotFoundException** → Missing file.  
5. **NumberFormatException** → Invalid string-to-number conversion.  

---

### **54) Exception Handling in Java**  
**Definition:**  
A mechanism to **handle runtime errors** gracefully using:  
- `try` (monitor for exceptions),  
- `catch` (handle exceptions),  
- `finally` (execute cleanup code).  

**Example:**  
```java
try {
    int x = 10 / 0;
} catch (ArithmeticException e) {
    System.out.println("Cannot divide by zero!");
} finally {
    System.out.println("Cleanup done.");
}
```

---

### **55) What is an Error in Java?**  
**Definition:**  
- **Unrecoverable** system-level problems (e.g., `OutOfMemoryError`, `StackOverflowError`).  
- Unlike exceptions, **errors should not be caught/handled**.  

**Example:**  
```java
public class Test {
    public static void main(String[] args) {
        recursiveMethod();  // Causes StackOverflowError
    }
    static void recursiveMethod() {
        recursiveMethod();
    }
}
```

---

### **56) Advantages of Exception Handling**  
1. **Prevents crashes** by handling errors gracefully.  
2. **Separates error-handling code** from business logic.  
3. **Propagates errors** up the call stack.  
4. **Grouping and differentiation** of error types.  

---

### **57) Ways to Handle Exceptions**  
1. **`try-catch`** → Handle exceptions locally.  
2. **`throws`** → Declare exceptions for the caller to handle.  
3. **`finally`** → Ensure cleanup code runs.  
4. **`try-with-resources`** → Auto-close resources (Java 7+).  

**Example of `throws`:**  
```java
void readFile() throws IOException {
    Files.readAllLines(Paths.get("file.txt"));
}
```

---

### **58) Five Keywords for Exception Handling**  
1. **`try`** → Encloses risky code.  
2. **`catch`** → Handles exceptions.  
3. **`finally`** → Executes mandatory cleanup.  
4. **`throw`** → Manually throw an exception.  
5. **`throws`** → Declares exceptions a method might throw.  

**Example of `throw`:**  
```java
if (age < 18) {
    throw new ArithmeticException("Age must be ≥18");
}
```

---

### **Summary Table**  
| Concept               | Key Points                                                                 |
|-----------------------|---------------------------------------------------------------------------|
| **Abstract Class**    | Cannot be instantiated; may have abstract/concrete methods.               |
| **Abstract Method**   | No body; must be overridden.                                              |
| **Exceptions**        | `try-catch-finally`, `throws`, `throw`.                                   |
| **Errors**           | Unrecoverable (e.g., `OutOfMemoryError`).                                 |

### **59) `try` and `catch` Keywords in Java**  
**Purpose:** Handle exceptions gracefully.  
- **`try` block:** Contains code that might throw an exception.  
- **`catch` block:** Handles the exception if it occurs.  

**Example:**  
```java
try {
    int x = 10 / 0; // Throws ArithmeticException
} catch (ArithmeticException e) {
    System.out.println("Cannot divide by zero!");
}
```

---

### **60) Can We Have a `try` Block Without a `catch` Block?**  
**✅ Yes**, but only if:  
- It has a `finally` block, or  
- The method declares the exception using `throws`.  

**Example:**  
```java
try {
    Files.readAllLines(Paths.get("file.txt"));
} finally {
    System.out.println("Cleanup");
}
```

---

### **61) Can We Have Multiple `catch` Blocks for a `try` Block?**  
**✅ Yes**, but order matters:  
- Catch **more specific exceptions first** (e.g., `ArithmeticException` before `Exception`).  

**Example:**  
```java
try {
    int[] arr = new int[5];
    arr[10] = 50; // Throws ArrayIndexOutOfBoundsException
} catch (ArithmeticException e) {
    System.out.println("Math error");
} catch (ArrayIndexOutOfBoundsException e) {
    System.out.println("Array index error");
}
```

---

### **62) Importance of `finally` Block**  
- **Guaranteed execution** (even if an exception occurs or a `return` is called).  
- Used for **cleanup** (e.g., closing files, database connections).  

**Example:**  
```java
try {
    System.out.println("Inside try");
} catch (Exception e) {
    System.out.println("Exception caught");
} finally {
    System.out.println("Always executes");
}
```

---

### **63) Can We Have Code Between `try` and `catch` Blocks?**  
**❌ No.**  
- `catch` must **immediately follow** `try`.  

**Invalid Example:**  
```java
try {
    // Code
}
System.out.println("Hello"); // ❌ Compile error
catch (Exception e) { ... }
```

---

### **64) Can We Have Code Between `try` and `finally` Blocks?**  
**❌ No.**  
- `finally` must **immediately follow** the last `catch` (or `try` if no `catch`).  

**Invalid Example:**  
```java
try {
    // Code
}
System.out.println("Hello"); // ❌ Compile error
finally { ... }
```

---

### **65) Can We Catch Multiple Exceptions in a Single `catch` Block?**  
**✅ Yes (Java 7+)**  
- Use the **pipe (`|`) symbol** to combine exceptions.  

**Example:**  
```java
try {
    // Risky code
} catch (ArithmeticException | NullPointerException e) {
    System.out.println("Caught: " + e.getClass());
}
```

---

### **66) Checked Exceptions**  
- **Checked at compile-time**. Must be handled (using `try-catch` or `throws`).  
- Examples: `IOException`, `SQLException`.  

**Example:**  
```java
try {
    Files.readAllLines(Paths.get("file.txt")); // Throws IOException (checked)
} catch (IOException e) {
    e.printStackTrace();
}
```

---

### **67) Unchecked Exceptions**  
- **Not checked at compile-time** (subclasses of `RuntimeException`).  
- Examples: `NullPointerException`, `ArrayIndexOutOfBoundsException`.  

**Example:**  
```java
String s = null;
System.out.println(s.length()); // Throws NullPointerException (unchecked)
```

---

### **68) Differences Between Checked and Unchecked Exceptions**  
| Feature               | Checked Exceptions          | Unchecked Exceptions        |  
|-----------------------|----------------------------|----------------------------|  
| **Compile-Time Check** | ✅ Yes                     | ❌ No                      |  
| **Handling Required**  | ✅ Yes (or `throws`)       | ❌ No (optional)           |  
| **Examples**          | `IOException`, `SQLException` | `NullPointerException`, `ArithmeticException` |  

---

### **69) Default Exception Handling in Java**  
- If an exception is **not caught**, the JVM:  
  1. Prints the **exception stack trace**.  
  2. **Terminates the program**.  

**Example:**  
```java
public class Main {
    public static void main(String[] args) {
        int x = 10 / 0; // Uncaught ArithmeticException
    }
}
// Output:  
// Exception in thread "main" java.lang.ArithmeticException: / by zero
```

---

### **70) `throw` Keyword**  
- **Manually throws an exception** (custom or built-in).  

**Example:**  
```java
if (age < 18) {
    throw new ArithmeticException("Age must be ≥18");
}
```

---

### **71) Can We Write Code After `throw`?**  
**❌ No.**  
- `throw` **terminates** the current block (like `return`).  

**Example:**  
```java
throw new Exception("Error");
System.out.println("Unreachable"); // ❌ Compile error
```

---

### **72) `throws` Keyword**  
- **Declares exceptions** a method might throw (used for checked exceptions).  

**Example:**  
```java
void readFile() throws IOException {
    Files.readAllLines(Paths.get("file.txt"));
}
```

---

### **73) `finally` vs `return`**  
- `finally` executes **even if `return` is called** in `try`/`catch`.  

**Example:**  
```java
try {
    return 10;
} finally {
    System.out.println("Finally runs!"); // Executes before return
}
```

---

### **74) When `finally` Does Not Execute**  
- **JVM crashes** (e.g., `System.exit(0)`).  
- **Infinite loop** in `try`/`catch`.  

**Example:**  
```java
try {
    System.exit(0); // JVM exits
} finally {
    System.out.println("Never runs");
}
```

---

### **75) Can We Use `catch` for Checked Exceptions?**  
**✅ Yes**, but it’s **optional** (unlike `throws`).  

**Example:**  
```java
try {
    throw new IOException("Checked exception");
} catch (IOException e) { // Optional for unchecked, mandatory for checked
    e.printStackTrace();
}
```

---

### **76) User-Defined Exceptions**  
- Custom exceptions **extending `Exception` or `RuntimeException`**.  

**Example:**  
```java
class InvalidAgeException extends Exception {
    InvalidAgeException(String message) {
        super(message);
    }
}

// Usage:
throw new InvalidAgeException("Age must be ≥18");
```

---

### **77) Rethrowing Exceptions**  
- **✅ Yes**, use `throw` in `catch`.  

**Example:**  
```java
try {
    // Risky code
} catch (IOException e) {
    throw e; // Rethrow
}
```

---

### **78) Nested `try` Statements**  
- **✅ Yes**, `try` blocks can be nested.  

**Example:**  
```java
try {
    try {
        int x = 10 / 0;
    } catch (ArithmeticException e) {
        System.out.println("Inner catch");
    }
} catch (Exception e) {
    System.out.println("Outer catch");
}
```

---

### **79) `Throwable` Class and Methods**  
- **Superclass** of all errors/exceptions.  
- Key methods:  
  - `getMessage()` → Returns error details.  
  - `printStackTrace()` → Prints stack trace.  

**Example:**  
```java
try {
    int x = 10 / 0;
} catch (Throwable t) {
    System.out.println(t.getMessage()); // "/ by zero"
}
```

---

### **80) `ClassNotFoundException`**  
- Raised when **a class is not found** at runtime (e.g., missing JAR).  

**Example:**  
```java
try {
    Class.forName("NonExistentClass");
} catch (ClassNotFoundException e) {
    e.printStackTrace();
}
```

---

### **81) `NoClassDefFoundError`**  
- Raised when **a compiled class is missing** during execution (e.g., deleted `.class` file).  

**Example:**  
```java
// Compiles but fails at runtime if MyClass is missing.
MyClass obj = new MyClass(); // Throws NoClassDefFoundError
```

---

### **Summary Table**  
| Concept               | Key Points                                                                 |
|-----------------------|---------------------------------------------------------------------------|
| **`try-catch`**       | Handle exceptions; `finally` always executes.                             |
| **Checked Exceptions** | Must be handled (`try-catch`/`throws`).                                   |
| **`throw`/`throws`** | `throw` raises exceptions; `throws` declares them.                        |
| **User-Defined Exceptions** | Extend `Exception` or `RuntimeException`.                              |

### **90) Java APIs That Support Threads**  
Java provides built-in APIs for multithreading:  
1. **`java.lang.Thread`** → Base class for creating/controlling threads.  
2. **`java.lang.Runnable`** → Functional interface for thread tasks.  
3. **`java.util.concurrent`** → High-level concurrency utilities:  
   - **`ExecutorService`** → Thread pool management.  
   - **`Lock`** → Advanced synchronization.  
   - **`CountDownLatch`**, **`CyclicBarrier`** → Thread coordination.  

**Example:**  
```java
// Using Thread class
Thread t1 = new Thread(() -> System.out.println("Thread running"));
t1.start();

// Using ExecutorService
ExecutorService executor = Executors.newFixedThreadPool(2);
executor.submit(() -> System.out.println("Task executed"));
```

---

### **96) Importance of Thread Scheduler in Java**  
- **Role:** The JVM’s thread scheduler decides **which thread runs next** (order is unpredictable).  
- **Policies:**  
  - **Preemptive Scheduling:** Higher-priority threads get CPU time first.  
  - **Time-Slicing:** Each thread gets a fixed time slice.  

**Key Points:**  
✔ No guarantees on execution order (platform-dependent).  
✔ Use `Thread.sleep()`, `wait()`, or `yield()` for coordination.  

---

### **98) Can We Restart a Dead Thread in Java?**  
**❌ No.**  
- A thread **dies after `run()` completes** and cannot be restarted.  
- **Workaround:** Create a new thread instance.  

**Example:**  
```java
Thread t = new Thread(() -> System.out.println("Running"));
t.start();
t.join(); // Thread dies here
t.start(); // ❌ Throws IllegalThreadStateException
```

---

### **101) What Happens If We Don’t Override `run()`?**  
- The **default `run()`** in `Thread` class does **nothing**.  
- **Result:** The thread starts but executes no task.  

**Example:**  
```java
Thread t = new Thread(); // No run() override
t.start(); // Runs but does nothing
```

---

### **102) Can We Overload `run()` in Java?**  
**✅ Yes**, but only the **no-args `run()`** is invoked by `start()`.  

**Example:**  
```java
class MyThread extends Thread {
    public void run() { System.out.println("Default run"); }
    public void run(int x) { System.out.println("Overloaded run"); }
}

MyThread t = new MyThread();
t.start(); // Calls run() → "Default run"
t.run(10); // Calls overloaded run(int) → "Overloaded run"
```

---

### **105) Purpose of Locks in Java**  
- **Ensure thread safety** by controlling access to shared resources.  
- **Types:**  
  - **Intrinsic Locks (`synchronized`)** → Built into every Java object.  
  - **Explicit Locks (`ReentrantLock`)** → More flexible than `synchronized`.  

**Example:**  
```java
// Intrinsic lock
synchronized (this) { /* Critical section */ }

// Explicit lock
Lock lock = new ReentrantLock();
lock.lock();
try { /* Critical section */ } finally { lock.unlock(); }
```

---

### **106) Ways to Achieve Synchronization in Java**  
1. **Synchronized Methods** → Lock the entire method.  
   ```java
   public synchronized void syncMethod() { ... }
   ```
2. **Synchronized Blocks** → Lock specific code sections.  
   ```java
   synchronized (obj) { /* Critical section */ }
   ```
3. **`volatile` Keyword** → Ensures visibility of changes across threads.  
   ```java
   private volatile boolean flag;
   ```
4. **Explicit Locks (`ReentrantLock`)** → More control than `synchronized`.  

---

### **107) Synchronized Methods**  
- **Lock:** The entire method locks the **object’s monitor** (for instance methods) or **class monitor** (for static methods).  
- **Usage:** Prevents concurrent access to shared data.  

**Example:**  
```java
public class Counter {
    private int count;
    public synchronized void increment() { count++; } // Thread-safe
}
```

---

### **108) When to Use Synchronized Methods?**  
- **Shared Mutable Data:** When multiple threads access/modify the same object.  
- **Critical Sections:** To avoid race conditions (e.g., bank account withdrawals).  

**Example:**  
```java
public class BankAccount {
    private double balance;
    public synchronized void withdraw(double amount) {
        if (balance >= amount) balance -= amount;
    }
}
```

---

### **Can Other Threads Execute Synchronized Methods Simultaneously?**  
**❌ No.**  
- If one thread holds the lock (executing a synchronized method), **other threads must wait** to enter **any synchronized method** of the same object.  

**Example:**  
```java
class Test {
    public synchronized void methodA() { /* Holds lock */ }
    public synchronized void methodB() { /* Waits if methodA is running */ }
}

// Thread 1: test.methodA() → Locks object  
// Thread 2: test.methodB() → Blocks until lock is released  
```

---

### **Key Takeaways**  
| Concept               | Key Points                                                                 |
|-----------------------|---------------------------------------------------------------------------|
| **Thread APIs**       | `Thread`, `Runnable`, `ExecutorService`, `Lock`.                          |
| **Thread Scheduler**  | Controls execution order (no guarantees).                                 |
| **Dead Threads**      | Cannot restart; create a new thread.                                      |
| **Synchronization**   | Use `synchronized`, `volatile`, or `ReentrantLock` for thread safety.     |
| **Synchronized Methods** | Block other threads from accessing **any** synchronized method of the same object. |

### **110) Can a Thread Access Other Synchronized Methods of the Same Object While Holding a Lock?**  
**✅ Yes!**  
- If a thread holds the lock (by executing a `synchronized` method), it can **enter other `synchronized` methods** of the **same object** without blocking.  
- This is called **reentrant synchronization**.  

**Example:**  
```java
class Test {
    public synchronized void methodA() {
        System.out.println("In methodA");
        methodB(); // Allowed (same thread already holds the lock)
    }
    public synchronized void methodB() {
        System.out.println("In methodB");
    }
}
// Thread calling methodA() can also execute methodB() without releasing the lock.
```

---

### **111) Synchronized Blocks in Java**  
- **Purpose:** Synchronize **only a block of code** (not the entire method).  
- **Syntax:**  
  ```java
  synchronized (lockObject) { /* Critical section */ }
  ```
- **Advantage:** More granular control than synchronized methods.  

**Example:**  
```java
public void increment() {
    synchronized (this) { // Locks current object
        count++;
    }
}
```

---

### **112) When to Use Synchronized Blocks?**  
**Use Cases:**  
1. **Performance Optimization:** Synchronize only critical sections, not entire methods.  
2. **Flexible Locking:** Lock on **any object** (not just `this`).  

**Advantages:**  
✔ Reduces contention by minimizing locked code.  
✔ Allows synchronization on **external objects**.  

**Example:**  
```java
private final Object lock = new Object(); // External lock

public void update() {
    // Non-critical code
    synchronized (lock) { // Critical section
        sharedResource.modify();
    }
}
```

---

### **113) Class-Level Lock**  
- **Definition:** A lock on the **class object** (applies to `static synchronized` methods/blocks).  
- **Effect:** Prevents concurrent access to **all static synchronized methods** of the class.  

**Example:**  
```java
class Counter {
    public static synchronized void staticMethod() { /* Class-level lock */ }
    public static void staticBlock() {
        synchronized (Counter.class) { /* Same as static synchronized */ }
    }
}
```

---

### **114) Can We Synchronize Static Methods?**  
**✅ Yes!**  
- Uses a **class-level lock** (not instance-level).  

**Example:**  
```java
public static synchronized void staticSyncMethod() { /* Thread-safe */ }
```

---

### **115) Can We Use Synchronized Blocks for Primitives?**  
**❌ No!**  
- Synchronized blocks require an **object reference** (primitives like `int`, `double` cannot be locks).  

**Invalid Example:**  
```java
int x = 10;
synchronized (x) { } // ❌ Compile error (primitives not allowed)
```

**Workaround:** Use a wrapper object (e.g., `Integer`):  
```java
final Integer lock = 10;
synchronized (lock) { /* Valid */ }
```

---

### **116) Thread Priorities in Java**  
- **Purpose:** Hint to the scheduler about **thread execution order** (not guaranteed).  
- **Range:** `1` (MIN_PRIORITY) to `10` (MAX_PRIORITY). Default: `5` (NORM_PRIORITY).  

**Example:**  
```java
Thread t = new Thread(() -> System.out.println("Running"));
t.setPriority(Thread.MAX_PRIORITY); // Priority = 10
t.start();
```

---

### **117) Types of Thread Priorities**  
1. **`MIN_PRIORITY` (1)** → Least priority.  
2. **`NORM_PRIORITY` (5)** → Default priority.  
3. **`MAX_PRIORITY` (10)** → Highest priority.  

---

### **118) How to Set Thread Priority?**  
Use `setPriority(int priority)`:  
```java
Thread t = new Thread(() -> {});
t.setPriority(Thread.MAX_PRIORITY); // Set to 10
```

---

### **119) If Two Threads Have the Same Priority, Which Runs First?**  
- **Depends on the JVM scheduler** (no guaranteed order).  
- May follow **FIFO**, **round-robin**, or **platform-specific policies**.  

---

### **120) Methods to Prevent Thread Execution**  
1. **`Thread.sleep(long ms)`** → Pauses the thread for a fixed time.  
2. **`Thread.yield()`** → Suggests that the thread **relinquishes CPU**.  
3. **`object.wait()`** → Releases lock and waits for `notify()`.  
4. **`thread.join()`** → Waits for another thread to die.  

**Example:**  
```java
Thread t1 = new Thread(() -> {
    Thread.sleep(1000); // Pauses for 1 second
});
t1.start();
t1.join(); // Main thread waits for t1 to finish
```

---

### **121) `yield()` Method**  
- **Purpose:** A hint to the scheduler that the thread is willing to **yield its CPU time**.  
- **Effect:** The thread moves to **runnable state**, allowing other threads to run.  

**Example:**  
```java
Thread t = new Thread(() -> {
    Thread.yield(); // May pause execution
    System.out.println("Task resumed");
});
```

---

### **122) Can a Yielded Thread Get CPU Time Again Immediately?**  
**✅ Yes, but not guaranteed.**  
- The scheduler **may re-select the same thread** if no higher-priority threads are running.  

**Example:**  
```java
Thread.yield(); // May resume execution right after
```

---

### **Summary Table**  
| Concept               | Key Points                                                                 |
|-----------------------|---------------------------------------------------------------------------|
| **Reentrant Locks**   | A thread can call other `synchronized` methods of the same object.        |
| **Synchronized Blocks** | More granular than methods; lock on any object.                          |
| **Class-Level Lock**  | Applies to `static synchronized` methods/blocks.                          |
| **Thread Priorities** | Hints (1–10) with no strict enforcement.                                 |
| **`yield()`**         | Suggests releasing CPU time (no guarantees).                             |

