# RISC-V Internship Log

---

# RISC-V Internship Log

<details>
<summary><strong>---</strong></summary>



</details>

<details>
<summary><strong>📸 Output & Screenshots</strong></summary>

_Add screenshots here._

</details>
---

## 1. Install & Sanity-Check the Toolchain

<details>
<summary><strong><details></strong></summary>

<summary><strong>🧾 Instructions</strong></summary>

</details>

<details>
<summary><strong>🛠️ RISC-V Toolchain Setup Guide (RV32IMAC)</strong></summary>

This guide explains how to unpack the RISC-V toolchain, configure your environment, and verify that everything is working correctly.

---

</details>

<details>
<summary><strong>📦 1. Unpack the Toolchain</strong></summary>

Open a terminal and run:

```bash
tar -xvzf riscv-toolchain-rv32imac-x86_64-ubuntu.tar.gz
```

This will extract a directory, typically named `opt` or `riscv-toolchain`, containing the toolchain files.

---

</details>

<details>
<summary><strong>📁 2. Locate the bin/ Directory</strong></summary>

Navigate to the extracted directory:

```bash
cd path/to/extracted/folder/opt/riscv/bin
```

Replace `path/to/extracted/folder` with the actual path where the toolchain was extracted.

Then list the contents:

```bash
ls
```

You should see binaries like:

- `riscv32-unknown-elf-gcc`
- `riscv32-unknown-elf-objdump`
- `riscv32-unknown-elf-gdb`

---

</details>

<details>
<summary><strong>🌐 3. Add Toolchain to Your PATH</strong></summary>



</details>

<details>
<summary><strong>🔹 Temporary (for current terminal session):</strong></summary>

```bash
export PATH="$PWD:$PATH"
```

</details>

<details>
<summary><strong>🔹 Permanent (recommended):</strong></summary>

To make the change permanent:

**For Bash users:**

```bash
echo 'export PATH="/home/naren/Desktop/VSD/opt/riscv/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

**For Zsh users:**

```bash
echo 'export PATH="/home/naren/Desktop/VSD/opt/riscv/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
```

---

</details>

<details>
<summary><strong>📸 Output & Screenshots</strong></summary>

_Add screenshots here._

</details>
---

## 2. Compile Hello, RISC V

<details>
<summary><strong><details></strong></summary>

<summary><strong>🧾 Instructions</strong></summary>

</details>

<details>
<summary><strong>🖥️ Compile "Hello, RISC-V"</strong></summary>

This section shows how to write and compile a simple RISC-V program using the `riscv32-unknown-elf-gcc` toolchain for the `rv32imc` target.

</details>

<details>
<summary><strong>📄 Step 1: Create the C Source File</strong></summary>

Create a file called `hello.c` with the following content:
````markdown



#include <stdio.h>

int main() {
    printf("Hello, RISC-V!\n");
    return 0;
}
````

---

</details>

<details>
<summary><strong>⚙️ Step 2: Compile for RV32IMC</strong></summary>

Use the following command to compile the code into a RISC-V ELF binary:

```bash
riscv32-unknown-elf-gcc -march=rv32imc -mabi=ilp32 -o hello.elf hello.c
```

---

</details>

<details>
<summary><strong>📸 Output & Screenshots</strong></summary>

_Add screenshots here._

</details>
---

## 3. From C to Assembly

<details>
<summary><strong><details></strong></summary>

<summary><strong>🧾 Instructions</strong></summary>

Here's a clean and structured **Markdown** version of your instructions with the `.s` file generation and explanation request:

---

</details>

<details>
<summary><strong>🛠️ Generate Assembly and Understand Function Structure</strong></summary>

This section explains how to generate the assembly (`.s`) file for a simple C program targeting RISC-V (`rv32imc`), and what the **function prologue** and **epilogue** mean.

---

</details>

<details>
<summary><strong>📄 Step 1: Source File – `hello.c`</strong></summary>

```c
#include <stdio.h>

int main() {
    printf("Hello, RISC-V!\n");
    return 0;
}
```

---

</details>

<details>
<summary><strong>⚙️ Step 2: Generate Assembly</strong></summary>

Use the following command to generate the assembly file (`hello.s`):

```bash
riscv32-unknown-elf-gcc -S -O0 hello.c
```

* `-S` tells GCC to output assembly instead of object code.
* `-O0` disables optimizations so you can see the raw function structure.

This creates a file named `hello.s`.

---

</details>

<details>
<summary><strong>📸 Output & Screenshots</strong></summary>

_Add screenshots here._

</details>
---

## 4. HEX Dump and Disassembly

<details>
<summary><strong><details></strong></summary>

<summary><strong>🧾 Instructions</strong></summary>

---

</details>

<details>
<summary><strong>🧰 Convert and Disassemble RISC-V ELF</strong></summary>

This guide shows how to convert your compiled ELF binary into a raw hex file and disassemble it for analysis.

---

</details>

<details>
<summary><strong>🔄 Step 1: Disassemble the ELF File</strong></summary>

```bash
riscv32-unknown-elf-objdump -d hello.elf > hello.dump
```

This generates a human-readable disassembly in `hello.dump`.

---

</details>

<details>
<summary><strong>🔃 Step 2: Generate Intel HEX Format</strong></summary>

```bash
riscv32-unknown-elf-objcopy -O ihex hello.elf hello.hex
```

This converts the ELF into an Intel HEX format file (`hello.hex`), often used for flashing embedded devices.

---

</details>

<details>
<summary><strong>🧐 Understanding the Disassembly Output</strong></summary>

A line in the disassembly typically looks like this:

```
00000000 <main>:
   0:  1141        addi   sp,sp,-16
   2:  c606        sw     ra,12(sp)
   ...
```

Each column represents:

| Column           | Meaning                                                                     |
| ---------------- | --------------------------------------------------------------------------- |
| `0:`             | **Address offset** within the function (e.g., 0 bytes from start of `main`) |
| `1141`           | **Raw machine code** (hex representation of the instruction)                |
| `addi sp,sp,-16` | **Mnemonic + operands** — the actual instruction being executed             |

---

</details>

<details>
<summary><strong>📸 Output & Screenshots</strong></summary>

_Add screenshots here._

</details>
---

## 5. ABI & Register Cheat Sheet

<details>
<summary><strong><details></strong></summary>

<summary><strong>🧾 Instructions</strong></summary>

</details>

<details>
<summary><strong>🧠 RV32 Integer Registers & Calling Convention</strong></summary>



</details>

<details>
<summary><strong>📋 Register Table</strong></summary>

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

</details>

<details>
<summary><strong>📸 Output & Screenshots</strong></summary>

_Add screenshots here._

</details>
---

## 6. Stepping With GDB

<details>
<summary><strong><details></strong></summary>

<summary><strong>🧾 Instructions</strong></summary>

</details>

<details>
<summary><strong>📸 Output & Screenshots</strong></summary>

_Add screenshots here._

</details>
---

## 7. Running Under An Emulator

<details>
<summary><strong><details></strong></summary>

<summary><strong>🧾 Instructions</strong></summary>

</details>

<details>
<summary><strong>📸 Output & Screenshots</strong></summary>

_Add screenshots here._

</details>
---

## 8. Exploring GCC Optimisation

<details>
<summary><strong><details></strong></summary>

<summary><strong>🧾 Instructions</strong></summary>

</details>

<details>
<summary><strong>✅ Exploring GCC Optimisation</strong></summary>

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

</details>

<details>
<summary><strong>🧠 GCC Optimisations Explained</strong></summary>



</details>

<details>
<summary><strong>✅ 1. Dead-Code Elimination</strong></summary>

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

</details>

<details>
<summary><strong>✅ 2. Register Allocation</strong></summary>

> **What it is:** The compiler tries to **keep values in registers** (like `t0`, `a0`, etc.) instead of RAM/stack to speed up access.

**Example:**

```c
int result = a + b;
```

* With `-O0`: `a` and `b` may be loaded/stored using memory (e.g., stack).
* With `-O2`: `a` and `b` are often kept in registers throughout.

🔍 **Why:** Registers are faster than RAM — better performance.

---

</details>

<details>
<summary><strong>📸 Output & Screenshots</strong></summary>

_Add screenshots here._

</details>
---

## 9. Inline Assembly (Read `cycle` CSR)

<details>
<summary><strong><details></strong></summary>

<summary><strong>🧾 Instructions</strong></summary>

</details>

<details>
<summary><strong>📸 Output & Screenshots</strong></summary>

_Add screenshots here._

</details>
---

## 10. MMIO GPIO Toggle Using Volatile Pointers

<details>
<summary><strong><details></strong></summary>

<summary><strong>🧾 Instructions</strong></summary>

</details>

<details>
<summary><strong>📸 Output & Screenshots</strong></summary>

_Add screenshots here._

</details>
