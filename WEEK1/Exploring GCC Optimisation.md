

### ✅ 8. Exploring GCC Optimisation

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


