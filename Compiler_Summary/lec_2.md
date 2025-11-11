### 🧩 **CS419 — Compiler Construction (Lecture 2) Summary**

---

#### 📘 **Overview**

This lecture introduces **fundamental programming language concepts** that every compiler designer must understand — how variables, scope, and parameters behave during compilation and execution. It explains **static vs dynamic decisions**, **environments and states**, **scoping rules**, and **parameter passing mechanisms** with clear examples.

---

### 1️⃣ **Programming Language Basics**

Main topics covered:

- Static and dynamic decision policies
    
- Environments and states
    
- Static scope and block structure
    
- Parameter passing mechanisms
    

These concepts are foundational for how compilers analyze and manage variables and memory during program execution.

---

### 2️⃣ **Static and Dynamic Decisions**

- **Static Policy:**
    
    - Decisions made **at compile time**.
        
    - The compiler determines variable types, memory allocation, and possible errors **before execution**.
        
    - Errors found here are called **compile-time errors**.
        
    - Example: In C++, declaring `int x; x = "Hello";` will cause a compile-time error because `"Hello"` is not an integer.
        
- **Dynamic Policy:**
    
    - Decisions made **at runtime**.
        
    - The compiler postpones certain checks (like type or memory decisions) until the program is **executed**.
        
    - Example: In Python, variable types are decided dynamically when the program runs.
        

---

### 3️⃣ **Environments and States**

These define how a program connects **variable names** to **memory locations** and **values**.

- **Environment:**  
    Maps **variable names → memory locations**.  
    Example: variable `x` might be mapped to memory address `0xA12F`.
    
- **State:**  
    Maps **memory locations → actual stored values**.  
    Example: the location `0xA12F` holds the value `5`.
    

🧠 **Analogy:**  
Think of _Environment_ as a **contacts list** (names → phone numbers)  
and _State_ as the **current owner** of those phone numbers (numbers → people).

---

### 4️⃣ **Example — Local vs Global Variables**

```cpp
int i;  // global i

void f() {
    int i; // local i
    i = 3; // uses local i
}
x = i + 1; // uses global i
```

Explanation:

- Inside the function `f`, `i` refers to the **local variable**.
    
- Outside the function, `i` refers to the **global variable**.
    

🧩 This demonstrates **scope resolution**: local variables override global ones within their block.

---

### 5️⃣ **Static Scope and Block Structure**

- **Static Scope:**  
    The scope of a variable is known **by its position** in the source code.
    
    - **Implicit:** Determined automatically by where the variable is declared.
        
    - **Explicit:** Controlled using keywords like `public`, `private`, `protected` (as in C++ or Java).
        

**Block Structure:**  
A **block** is a sequence of declarations and statements surrounded by `{ }`.  
Blocks can be **nested**, and inner blocks can redefine variables.

Example:

```cpp
main() {
 int a = 1, b = 1;
 {
   int b = 2;
   {
     int a = 3;
     cout << a << b;
   }
   {
     int b = 4;
     cout << a << b;
   }
   cout << a << b;
 }
 cout << a << b;
}
```

🔍 **Explanation:**

|Variable|Scope|
|---|---|
|`int a = 1`|From Block 1 to Block 3|
|`int b = 1`|From Block 1 to Block 2|
|`int b = 2`|From Block 2 to Block 4|
|`int a = 3`|Only Block 3|
|`int b = 4`|Only Block 4|

🧠 Rule:  
If a variable is not found in the current block, the program **searches upward** in the enclosing blocks.

---

### 6️⃣ **Parameter Passing Mechanisms**

There are several ways to pass parameters from one function to another:

#### **1. Call-by-Value**

- A copy of the variable’s **value** is passed.
    
- Changes made inside the function **don’t affect** the original variable.
    

Example:

```cpp
void by_value(int a) { a += 10; }

int x = 40;
by_value(x); // x = 40 (unchanged)
```

---

#### **2. Call-by-Reference**

- The **address** of the original variable is passed.
    
- Changes made inside the function **affect** the original variable.
    
- Common in C++ using the `&` symbol.
    

Example:

```cpp
void by_ref(int &a) { a += 10; }

int x = 40;
by_ref(x); // x = 50
```

---

#### **3. Call-by-Pointer**

- Similar to reference, but uses **pointers** explicitly (`*` and `&` operators).
    
- The pointer may be **null** or point to invalid memory, unlike references.
    

Example:

```cpp
void by_pointer(int *a) { (*a) += 10; }

int x = 40;
by_pointer(&x); // x = 50
```

---

### 7️⃣ **Practical Example**

```cpp
int x = 40;
by_value(x);    // x = 40
by_pointer(&x); // x = 50
by_ref(x);      // x = 60
```

💡 **Final Result:**  
`x = 60`

Explanation:

- `by_value` makes no real change.
    
- `by_pointer` modifies the original variable using its address.
    
- `by_ref` continues the modification directly by reference.
    

---

### 🧠 **Key Takeaways**

- **Static vs Dynamic:** Compile-time vs runtime decisions.
    
- **Environment & State:** Connect names to memory and values.
    
- **Scope:** Determines variable visibility (local overrides global).
    
- **Parameter Passing:**
    
    - Value → copy only
        
    - Reference → modifies directly
        
    - Pointer → modifies directly but less safe