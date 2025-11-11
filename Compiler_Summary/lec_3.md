### 🧩 **CS419 — Compiler Construction (Lecture 3) Summary**

---

#### 📘 **Overview**

Lecture 3 explains how a **simple one-pass compiler** works — especially focusing on the **front-end (lexical, syntax, and semantic analysis)** and how it connects with the **back-end** that generates code. It introduces the **tools used to build compilers**, explains **tokens, lexemes, and patterns**, and ends by outlining the **Java compiler project** that combines all these steps.

---

### 1️⃣ **Compiler Phases Recap**

The compilation process is divided into **phases**, each transforming the program into a new form:

1. **Lexical Analysis (Scanner):** Breaks code into tokens.
    
2. **Syntax Analysis (Parser):** Analyzes grammatical structure.
    
3. **Semantic Analysis:** Ensures meaning consistency (types, scope, etc.).
    
4. **Intermediate Code Generation:** Produces machine-like code.
    
5. **Code Optimization (optional):** Makes the code faster/smaller.
    
6. **Code Generation:** Produces assembly or machine code.
    

---

### 2️⃣ **Grouping Phases into Passes**

Phases are grouped into **passes** — a _pass_ is one complete traversal of the source code.

- **Front-End Pass:**  
    Includes _lexical analysis_, _syntax analysis_, _semantic analysis_, and _intermediate code generation_.  
    → It **analyzes** and **translates** the program into an intermediate representation.
    
- **Back-End Pass:**  
    Includes _code optimization_ and _code generation_.  
    → It **converts** the intermediate code into **machine code** and improves performance.
    

🧠 **Tip:** A “one-pass compiler” does both analysis and synthesis in a single read of the code.

---

### 3️⃣ **Front-End Pass Example (A = B + 60)**

|**Phase**|**Purpose**|**Output Example**|
|---|---|---|
|**Lexical Analyzer (Scanner)**|Removes spaces & splits input into tokens.|‘A’, ‘=’, ‘B’, ‘+’, ‘60’|
|**Syntax Analyzer (Parser)**|Builds a **parse tree** showing structure.|Tree with root ‘=’ and children ‘A’ and ‘+’|
|**Semantic Analyzer**|Checks types & meaning; builds an **abstract syntax tree (AST)**.|Converts 60 → float if needed (`inttofloat(60)`)|
|**Intermediate Code Generator**|Produces machine-like 3-address code.|`t1 = inttofloat(60)`  <br>`t2 = B + t1`  <br>`A = t2`|

💡 **Note:** This intermediate code is easier for later translation and optimization.

---

### 4️⃣ **Back-End Pass Example**

|**Phase**|**Goal**|**Sample Output**|
|---|---|---|
|**Code Optimization**|Simplifies code for speed or size.|`A = B + 60.0`|
|**Code Generation**|Converts intermediate code to assembly.|`MOVF #60.0,r1`  <br>`ADDF r1,r2`  <br>`MOVF r2,A`|
|**Peephole Optimization**|Replaces small instruction sequences with faster ones.|`ADDF #60.0,r2`  <br>`MOVF r2,A`|

🧩 Example instruction meaning:  
`ADDF` = Add floating-point numbers.

---

### 5️⃣ **Compiler-Construction Tools**

Modern compilers use automated tools for each phase:

|**Tool**|**Purpose**|
|---|---|
|**Scanner Generator (Lex)**|Creates lexical analyzers to detect tokens (e.g., `if`, `while`, identifiers).|
|**Parser Generator (YACC / Bison)**|Generates syntax analyzers based on grammar rules.|
|**Syntax-Directed Translation Engine**|Associates translation actions with grammar rules.|
|**Automatic Code Generator**|Converts intermediate code into machine instructions.|
|**Dataflow Engine**|Improves performance by analyzing how data moves in code.|

These tools make compiler design faster, modular, and less error-prone.

---

### 6️⃣ **Lexical Analyzer (Scanner)**

**Main tasks:**

- Remove spaces and comments.
    
- Encode constants as tokens (like numbers or strings).
    
- Recognize keywords (`if`, `while`, etc.).
    
- Identify and store variable names in a **symbol table**.
    

**Role:**

- Reads source characters → groups them into **lexemes** (meaningful text parts).
    
- Produces **tokens** for each lexeme.
    
- Communicates with the **symbol table**:
    
    - Inserts new identifiers.
        
    - Retrieves data for known identifiers.
        

---

### 7️⃣ **Lexemes, Tokens, and Patterns**

|**Term**|**Definition**|**Example**|
|---|---|---|
|**Lexeme**|Actual text from source code.|`abc`, `y`, `31`|
|**Token**|A category + optional value that represents a lexeme.|`<id, pointer>`, `<num, 31>`, `<=>`|
|**Pattern**|Rule describing valid lexemes.|_letter followed by letters or digits_ (for identifiers)|

💡 Example program:

`y = 31 + 28 * x;`

→ Tokens: `<id, y>`, `<=>`, `<num, 31>`, `<+>`, `<num, 28>`, `<*>`, `<id, x>`

---

### 8️⃣ **Lexeme Examples Table**

| **Token Type** | **Description**             | **Example**         |
| -------------- | --------------------------- | ------------------- |
| `if`           | Reserved word               | `if`                |
| `id`           | Identifier (letters/digits) | `Pi`, `score`, `D2` |
| `num`          | Numeric constant            | `3.14`, `0`, `6.2`  |
| `string`       | Text in quotes              | `"CS419"`           |
| `comparison`   | Comparison operators        | `<=`, `!=`, `>`     |

---

### 9️⃣ **Lexical Analyzer ↔ Parser Interaction**

1. **Parser** requests next token.
    
2. **Lexical Analyzer**:
    
    - Reads characters.
        
    - Identifies next lexeme & token.
        
    - Returns token to parser.
        
3. **Error Handling:**  
    The lexical analyzer links compiler errors to specific locations in the source code.
    

---

### 🔟 **Why Separate Lexical & Syntax Phases?**

- **Simplifies design:** Parser doesn’t handle comments or spacing.
    
- **Improves efficiency:** Lexical analyzers can buffer and process input faster.
    
- **Increases portability:** Device-specific handling (like file input) is kept inside the lexical analyzer.
    

---

### 11️⃣ **Building a Simple Java Compiler (Project Goal)**

You’ll build a **front-end Java compiler** that performs:

- **Lexical analysis** (tokenization)
    
- **Syntax analysis** (parsing)
    
- **Semantic analysis** (meaning verification)
    
- **Intermediate code generation** (translation to JVM-compatible format)
    

🧱 **Core Components:**

- Lexical & syntax analyzers
    
- Syntax definition (BNF grammar)
    
- Syntax-directed translation
    
- Symbol tables
    
- Intermediate code generation
    

---

### 🧠 **Key Takeaways**

- **Front-End Pass:** Analyzes and translates source code to intermediate form.
    
- **Back-End Pass:** Optimizes and generates final machine code.
    
- **Lexemes, tokens, and patterns** form the foundation of lexical analysis.
    
- **Separation** between lexical and syntax analysis simplifies compiler design.
    
- The final project focuses on developing a **Java front-end compiler** using these concepts.