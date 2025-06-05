

## 🧠 RV32 Integer Registers & Calling Convention

### 📋 Register Table

| Register | ABI Name | Description / Role                            |
| -------- | -------- | --------------------------------------------- |
| x0       | zero     | Constant 0                                    |
| x1       | ra       | Return address                                |
| x2       | sp       | Stack pointer                                 |
| x3       | gp       | Global pointer                                |
| x4       | tp       | Thread pointer                                |
| x5       | t0       | Temporary register (caller-saved)             |
| x6       | t1       | Temporary register (caller-saved)             |
| x7       | t2       | Temporary register (caller-saved)             |
| x8       | s0/fp    | Saved register / frame pointer (callee-saved) |
| x9       | s1       | Saved register (callee-saved)                 |
| x10      | a0       | Function argument / return value              |
| x11      | a1       | Function argument / return value              |
| x12      | a2       | Function argument                             |
| x13      | a3       | Function argument                             |
| x14      | a4       | Function argument                             |
| x15      | a5       | Function argument                             |
| x16      | a6       | Function argument                             |
| x17      | a7       | Function argument                             |
| x18      | s2       | Saved register (callee-saved)                 |
| x19      | s3       | Saved register (callee-saved)                 |
| x20      | s4       | Saved register (callee-saved)                 |
| x21      | s5       | Saved register (callee-saved)                 |
| x22      | s6       | Saved register (callee-saved)                 |
| x23      | s7       | Saved register (callee-saved)                 |
| x24      | s8       | Saved register (callee-saved)                 |
| x25      | s9       | Saved register (callee-saved)                 |
| x26      | s10      | Saved register (callee-saved)                 |
| x27      | s11      | Saved register (callee-saved)                 |
| x28      | t3       | Temporary register (caller-saved)             |
| x29      | t4       | Temporary register (caller-saved)             |
| x30      | t5       | Temporary register (caller-saved)             |
| x31      | t6       | Temporary register (caller-saved)             |

---

### 📚 Calling Convention Summary

* **`a0–a7`**: Used for function **arguments and return values**.
* **`s0–s11`**: **Callee-saved**: if a function uses these, it must restore them before returning.
* **`t0–t6`**: **Caller-saved**: not preserved across function calls.



