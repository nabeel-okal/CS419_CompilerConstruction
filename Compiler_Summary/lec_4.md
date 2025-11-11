### 🧩 **CS419 — Compiler Construction (Lecture 4) Summary**

---

#### 📘 **Overview**

Lecture 4 continues from Lecture 3 and dives deeper into the **lexical analysis** process, specifically focusing on **token patterns**, **regular expressions**, and **language operations**. It explains how tokens are represented, how their patterns are specified mathematically, and introduces **regular definitions and shorthand notations** commonly used in compiler design.

---

### 1️⃣ **Token Rules**

In most programming languages, tokens fall into these main categories:

|**Token Type**|**Description**|**Example**|
|---|---|---|
|**Keywords**|Reserved words with fixed meaning|`if`, `while`, `return`|
|**Operators**|Can be individual or grouped|`<`, `<=`, `>`, `>=`, `+`, `-`|
|**Identifiers**|Variable/function names|`sum`, `value`, `x`|
|**Constants**|Numeric or string values|`42`, `"Hello"`|
|**Punctuation**|Structural symbols|`(`, `)`, `;`, `{`, `}`|

💡 _Example:_  
In the expression `a = b + 2;`, the tokens are: `<id, a> <=> <id, b> <+> <num, 2> <;>`.

---

### 2️⃣ **Token Hidden Information**

Every **identifier token (`id`)** is associated with extra information stored in the **symbol table**:

- Its **lexeme** (actual text, e.g., `E` or `sum`).
    
- Its **data type**, **memory address**, or **scope**.
    
- Thus, the token `<id>` acts like a **pointer** to its symbol table entry.
    

This helps the compiler quickly find all details about an identifier during later phases (like type checking).

---

### 3️⃣ **Token Example**

Expression:

`E = M * 2`

✅ **Token Stream:**

`<id, ‘E’> <=> <id, ‘M’> <*> <num, 2>`

- Each `<id>` points to a symbol table entry (for variables `E` and `M`).
    
- `<num, 2>` is a constant token representing a literal value.
    

---

### 4️⃣ **Specification of Token Patterns**

The compiler uses **formal languages** and **regular expressions** to describe valid token structures.

- **Alphabet (Σ):**  
    A finite set of characters.  
    Example:  
    `{0, 1}` (binary), or ASCII characters.
    
- **String:**  
    A finite sequence of symbols from Σ.  
    Example: `x = 5;` is a string of ASCII characters.
    
- **Language:**  
    A _set of strings_ defined over an alphabet that follows certain rules.  
    Example: All valid variable names in C (like `count`, `value`, `x1`).
    

---

### 5️⃣ **String Operations**

|**Operation**|**Meaning**|**Example**|
|---|---|---|
|**Concatenation (xy)**|Joins two strings.|`"AB"` + `"CD"` → `"ABCD"`|
|**Exponentiation (sⁱ)**|Repeats a string _i_ times.|`"A"² = "AA"`, `"A"³ = "AAA"`|
|**Empty string (ε)**|Represents "no characters".|Length = 0|

---

### 6️⃣ **Language Operations**

Let L and M be languages:

|**Operation**|**Definition**|**Example**|
|---|---|---|
|**Union (L ∪ M)**|Strings in L or M.|`{a,b}` ∪ `{b,c}` = `{a,b,c}`|
|**Concatenation (LM)**|Every string in L followed by every string in M.|`{a,b}{c,d}` = `{ac, ad, bc, bd}`|
|**Exponentiation (Lⁱ)**|Repeat L _i_ times.|`{a}² = {aa}`|
|**Kleene Closure (L*)**|Zero or more repetitions.|`{a}`* = `{ε, a, aa, aaa, …}`|
|**Positive Closure (L⁺)**|One or more repetitions.|`{a}`⁺ = `{a, aa, aaa, …}`|

---

### 7️⃣ **Language Operations — Examples**

Let:

- **L = {A–Z, a–z}** (letters)
    
- **D = {0–9}** (digits)
    

Then:

|**Expression**|**Meaning**|
|---|---|
|**L ∪ D**|Any single letter or digit (total 62).|
|**LD**|A letter followed by a digit (like `A0`, `b2`).|
|**L⁴**|All 4-letter strings (e.g., `code`, `main`).|
|**L(L ∪ D)***|Strings starting with a letter and followed by letters/digits (→ typical identifier pattern).|
|**D⁺**|One or more digits (→ integers like `123`, `9`).|

---

### 8️⃣ **Regular Expressions (REs)**

Regular Expressions describe the structure of **token patterns** in a concise, mathematical way.

- **Example (C identifiers):**
    
    `letter (letter | _ | digit)*`
    
    - Must start with a letter.
        
    - Followed by any number of letters, underscores, or digits.
        

**Meaning of symbols:**

|**Symbol**|**Meaning**|
|---|---|
|`|`|
|`*`|Zero or more repetitions|
|`()`|Grouping|
|`ε`|Empty string|

---

### 9️⃣ **Regular Expression Examples**

Let Σ = {a, b}

|**RE**|**Language Represented**|
|---|---|
|`a|b`|
|`(a|b)(a|
|`a*`|`{ε, a, aa, aaa, …}`|
|`(a|b)*`|
|`a*b`|`{b, ab, aab, aaab, …}`|

🧠 Used in compilers to define how identifiers, numbers, strings, and operators are recognized by the **lexer**.

---

### 🔟 **Regular Definitions**

Instead of rewriting long expressions, we **name** regular patterns.

**Example (C identifiers):**

`letter → A | B | … | Z | a | b | … | z digit  → 0 | 1 | … | 9 id     → letter (letter | _ | digit)*`

💡 **Note:** Recursive definitions (like `digits → digit digits`) are _illegal_ — because regular expressions cannot define infinite recursion.

---

### 11️⃣ **Notational Shorthands**

Common shorthand notations simplify regular expressions:

|**Shorthand**|**Meaning**|
|---|---|
|`r*`|Zero or more repetitions (Kleene closure).|
|`r+`|One or more repetitions (positive closure).|
|`r?`|Zero or one instance (optional).|
|`[a-z]`|Range of characters (`a` to `z`).|

**Example:**

`digit → [0-9] num → digit+ (. digit+)? (E (+|-)? digit+)?`

This defines numbers with optional decimals and exponents (e.g., `42`, `3.14`, `2.5E+3`).

---

### 🧠 **Key Takeaways**

- **Tokens** represent meaningful program units (keywords, identifiers, literals).
    
- **Symbol tables** store hidden info (type, address, etc.) for each identifier.
    
- **Formal languages and regular expressions** describe valid token patterns.
    
- **Kleene and positive closures** are used for repetition.
    
- **Regular definitions** and **shorthands** simplify pattern writing — essential for building **lexical analyzers** in compilers.