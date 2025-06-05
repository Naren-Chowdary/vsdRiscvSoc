

### ✅ Exploring GCC Optimisation

**`hello.c` Source Code:**

```c
int add(int a, int b) {
    int result = a + b;
    return result;
}

int main() {
    int sum = add(5, 10);
    while (1);  // Infinite loop
    return 0;
}
```

---

**Compile to assembly with no optimization:**

```bash
riscv64-unknown-elf-gcc -march=rv32imac -mabi=ilp32 -O0 -S -o hello_O0.s hello.c
```

**Compile to assembly with high optimization:**

```bash
riscv64-unknown-elf-gcc -march=rv32imac -mabi=ilp32 -O2 -S -o hello_O2.s hello.c
```

---

**Compare output:**

```bash
code hello_O0.s hello_O2.s
```

or

```bash
diff hello_O0.s hello_O2.s
```

---

**Expected Differences:**

| Feature          | `-O0`                              | `-O2`                                  |
| ---------------- | ---------------------------------- | -------------------------------------- |
| Function `add()` | Separate function with call/return | Inlined into `main()`                  |
| Instructions     | More, direct 1-to-1 C translation  | Fewer, optimized                       |
| Stack usage      | Full stack frame setup             | May eliminate frame or reuse registers |
| Dead code        | Retained                           | Removed                                |

---

**Why It Matters:**

* `-O0` is ideal for debugging with GDB (preserves variables, structure).
* `-O2` is used in real firmware for speed and smaller code size.
Great catch, Naren — let me now **clearly explain** the three key optimizations mentioned in the task doc:

---

### 🧠 GCC Optimisations Explained

#### ✅ 1. Dead-Code Elimination

> **What it is:** The compiler removes any code or variables that are never used or have no effect on program output.

**Example:**

```c
int unused = 42;     // Not used anywhere
return 0;
```

* With `-O0`: This line **stays** in the assembly output.
* With `-O2`: The `unused` variable is **completely removed** from the `.s` file.

🔍 **Why:** To reduce code size and improve performance.

---

#### ✅ 2. Register Allocation

> **What it is:** The compiler tries to **keep values in registers** (like `t0`, `a0`, etc.) instead of RAM/stack to speed up access.

**Example:**

```c
int result = a + b;
```

* With `-O0`: `a` and `b` may be loaded/stored using memory (e.g., stack).
* With `-O2`: `a` and `b` are often kept in registers throughout.

🔍 **Why:** Registers are faster than RAM — better performance.

---

#### ✅ 3. Function Inlining

> **What it is:** The compiler replaces a function call with the actual function code to avoid the cost of a `call` and `return`.

**Example:**

```c
int add(int a, int b) { return a + b; }

int main() {
    int sum = add(2, 3);
}
```

* With `-O0`: You'll see a `call add` in assembly.
* With `-O2`: The call to `add` disappears — it gets **inlined into `main()`**.

🔍 **Why:** Reduces function-call overhead and may allow further optimization.

