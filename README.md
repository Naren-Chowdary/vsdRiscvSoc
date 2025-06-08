# RISC-V Internship Log

## 🛠️ Objective

These tasks are focused on building a complete, low-level understanding of **RISC-V-based system design**, simulation, and software bring-up. The goal was to remove the abstraction layers imposed by modern OS environments and experience what it means to program hardware directly, from the ground up.

---

## 1. Install & Sanity-Check the Toolchain

<details>
<summary><strong>🧾 Instructions</strong></summary>

# 🛠️ RISC-V Toolchain Setup Guide (RV32IMAC)

This guide explains how to unpack the RISC-V toolchain, configure your environment, and verify that everything is working correctly.

---

## 📦 1. Unpack the Toolchain

Open a terminal and run:

```bash
tar -xvzf riscv-toolchain-rv32imac-x86_64-ubuntu.tar.gz
```

This will extract a directory, typically named `opt` or `riscv-toolchain`, containing the toolchain files.

---

## 📁 2. Locate the bin/ Directory

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

## 🌐 3. Add Toolchain to Your PATH

### 🔹 Temporary (for current terminal session):

```bash
export PATH="$PWD:$PATH"
```

### 🔹 Permanent (recommended):

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

## ✅ 4. Verify Installation

Run the following commands to ensure the toolchain is working:

```bash
riscv32-unknown-elf-gcc --version
riscv32-unknown-elf-objdump --version
riscv32-unknown-elf-gdb --version
```

</details>

<details>
<summary><strong>📸 Output & Screenshots</strong></summary>

![image](https://github.com/user-attachments/assets/4b74a2c0-7e89-49ac-8640-2667047ae4f9)


</details>

---

## 2. Compile Hello, RISC V

<details>
<summary><strong>🧾 Instructions</strong></summary>

## 🖥️ Compile "Hello, RISC-V"

This section shows how to write and compile a simple RISC-V program using the `riscv32-unknown-elf-gcc` toolchain for the `rv32imc` target.



### 📄 Step 1: Create the C Source File

Create a file called `hello.c` with the following content:
````markdown



#include <stdio.h>

int main() {
    printf("Hello, RISC-V!\n");
    return 0;
}
````

---

### ⚙️ Step 2: Compile for RV32IMC

Use the following command to compile the code into a RISC-V ELF binary:

```bash
riscv32-unknown-elf-gcc -march=rv32imc -mabi=ilp32 -o hello.elf hello.c
```

---

### 🔍 Step 3: Verify the ELF File

Check that the output ELF file is for 32-bit RISC-V:

```bash
file hello.elf
```

Expected output should include:

```
ELF 32-bit LSB executable, UCB RISC-V, RVC, soft-float ABI, ...
```

This confirms that the binary was successfully compiled for the RV32IMC architecture.

---

```

```

</details>

<details>
<summary><strong>📸 Output & Screenshots</strong></summary>

![image](https://github.com/user-attachments/assets/5346adc5-d046-4273-a028-0245e8d59145)


</details>

---

## 3. From C to Assembly

<details>
<summary><strong>🧾 Instructions</strong></summary>

Here's a clean and structured **Markdown** version of your instructions with the `.s` file generation and explanation request:

---

## 🛠️ Generate Assembly and Understand Function Structure

This section explains how to generate the assembly (`.s`) file for a simple C program targeting RISC-V (`rv32imc`), and what the **function prologue** and **epilogue** mean.

---

### 📄 Step 1: Source File – `hello.c`

```c
#include <stdio.h>

int main() {
    printf("Hello, RISC-V!\n");
    return 0;
}
```

---

### ⚙️ Step 2: Generate Assembly

Use the following command to generate the assembly file (`hello.s`):

```bash
riscv32-unknown-elf-gcc -S -O0 hello.c
```

* `-S` tells GCC to output assembly instead of object code.
* `-O0` disables optimizations so you can see the raw function structure.

This creates a file named `hello.s`.

---

### 🧩 Step 3: Understand the Prologue and Epilogue

Open `hello.s` and look for lines like these inside the `main` function:

```assembly
addi    sp,sp,-16       # Allocate 16 bytes on the stack
sw      ra,12(sp)       # Save return address (ra) at offset 12
```

These lines are part of the **function prologue** — setup code that:

* Adjusts the stack pointer (`sp`)
* Saves important registers (like `ra`, the return address) to the stack

At the end of the function, you’ll see the **epilogue**:

```assembly
lw      ra,12(sp)       # Restore return address
addi    sp,sp,16        # Deallocate stack space
ret                     # Return to caller
```

These reverse the prologue steps, restoring the original state before returning.

---

</details>

<details>
<summary><strong>📸 Output & Screenshots</strong></summary>

![image](https://github.com/user-attachments/assets/43dcf14d-3a6b-4889-8f9f-d7843ac178e3)

![image](https://github.com/user-attachments/assets/b4c2522a-2f81-48d0-9e41-4e4541ebb78e)

</details>

---

## 4. HEX Dump and Disassembly

<details>
<summary><strong>🧾 Instructions</strong></summary>

---

## 🧰 Convert and Disassemble RISC-V ELF

This guide shows how to convert your compiled ELF binary into a raw hex file and disassemble it for analysis.

---

### 🔄 Step 1: Disassemble the ELF File

```bash
riscv32-unknown-elf-objdump -d hello.elf > hello.dump
```

This generates a human-readable disassembly in `hello.dump`.

---

### 🔃 Step 2: Generate Intel HEX Format

```bash
riscv32-unknown-elf-objcopy -O ihex hello.elf hello.hex
```

This converts the ELF into an Intel HEX format file (`hello.hex`), often used for flashing embedded devices.

---

### 🧐 Understanding the Disassembly Output

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

### 🧪 Example Instruction Breakdown

**Instruction:**

```
   0:  1141        addi   sp,sp,-16
```

* **Address**: `0:` → First instruction in `main`
* **Opcode**: `1141` → Binary encoding of the instruction
* **Mnemonic**: `addi` → Add Immediate
* **Operands**: `sp, sp, -16` → Subtracts 16 from the stack pointer (`sp`), creating stack space (prologue setup)

---

</details>

<details>
<summary><strong>📸 Output & Screenshots</strong></summary>

![image](https://github.com/user-attachments/assets/68b57372-a03a-4e51-bc3b-905d5b441cf4)
![image](https://github.com/user-attachments/assets/4f5b4ef1-8944-48ad-87a5-54c50389688e)
![image](https://github.com/user-attachments/assets/d25db213-1570-46ba-9777-c71ee90767de)


</details>

---

## 5. ABI & Register Cheat Sheet

<details>
<summary><strong>🧾 Instructions</strong></summary>

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

</details>


## 6. Stepping With GDB

<details>
<summary><strong>🧾 Instructions</strong></summary>

### ✅ 6. Stepping with GDB

**Command to launch GDB:**

```
riscv64-unknown-elf-gdb hello.elf`
```

**Inside GDB:**

```gdb
(gdb) target sim                # Use the built-in RISC-V simulator
(gdb) load                      # Load the ELF into simulated memory
(gdb) break main                # Set breakpoint at main()
(gdb) run                       # Start execution
(gdb) info registers            # Inspect all general-purpose registers
(gdb) disassemble               # View disassembly at current PC
(gdb) stepi                     # Step one instruction
(gdb) continue                  # Resume execution until the end
(gdb) quit                      # Exit GDB
```

**Expected Output:**

```
Breakpoint 1, main () at hello.c:4
4       printf("Hello, RISC-V!\n");
```

* `load` is essential to load your program into memory.
* If `break main` fails, use the address shown by disassembly:

  ```gdb
  (gdb) break *0x10118
  ```

</details>

<details>
<summary><strong>📸 Output & Screenshots</strong></summary>

![image](https://github.com/user-attachments/assets/cc102f63-dc88-48dd-a2fe-64da362e2ada)
![image](https://github.com/user-attachments/assets/e179c793-03d9-4ed5-a02b-ef1e9cb06d70)


</details>

---

## 7. Running Under An Emulator

<details>
<summary><strong>🧾 Instructions</strong></summary>

### ✅ Running Under an Emulator

**Minimal `hello.c` for bare-metal QEMU run:**

```c
int main() {
    while (1);  // Infinite loop — prevents program from exiting
    return 0;
}
```

---

**Compile without standard libraries:**

```bash
riscv64-unknown-elf-gcc -march=rv32imac -mabi=ilp32 -nostdlib -o hello.elf hello.c
```

> `-nostdlib` ensures the binary is truly bare-metal with no standard C runtime.

---

**Run using QEMU (no BIOS):**

```bash
qemu-system-riscv32 -nographic -machine sifive_e -kernel hello.elf -bios none
```

---

**Explanation of flags:**

* `-nographic` → sends all output to terminal (no GUI).
* `-machine sifive_e` → emulates SiFive E-class RISC-V board.
* `-kernel hello.elf` → loads your compiled ELF.
* `-bios none` → skips firmware (runs your ELF directly).

---

**Expected Behavior:**

* Program runs silently (infinite loop).
* No crash = ✅ success.
* To exit QEMU:
  Press `Ctrl + A`, then `X`.

</details>

<details>
<summary><strong>📸 Output & Screenshots</strong></summary>

![image](https://github.com/user-attachments/assets/660ccbf1-5af3-4c3c-a3c8-e7dddcd7de13)

![image](https://github.com/user-attachments/assets/475afff3-875c-4f55-896f-363d60c612d2)

</details>

---

## 8. Exploring GCC Optimisation

<details>
<summary><strong>🧾 Instructions</strong></summary>

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

</details>

<details>
<summary><strong>📸 Output & Screenshots</strong></summary>

![image](https://github.com/user-attachments/assets/981649f0-be96-4a1f-9c7b-1cbcd977b1d4)
![image](https://github.com/user-attachments/assets/20534496-c117-4dbf-8d39-0d60718bfd9a)


</details>

---

## 9. Inline Assembly (Read `cycle` CSR)

<details>
<summary><strong>🧾 Instructions</strong></summary>

### ✅ 9. Inline Assembly (Read `cycle` CSR)

**`hello.c`**

```c
#include <stdint.h>

int main() {
    uint32_t cycle;
    asm volatile ("csrr %0, cycle" : "=r"(cycle));
    while (1);
    return 0;
}
```

**Compile with debug info:**

```bash
riscv64-unknown-elf-gcc -march=rv32imac -mabi=ilp32 -O0 -g -o hello.elf hello.c
```

**Launch GDB:**

```bash
riscv64-unknown-elf-gdb hello.elf
```

**Inside GDB:**

```gdb
(gdb) target sim
(gdb) load
(gdb) break main
(gdb) run
(gdb) x/w &cycle         # Inspect contents of the 'cycle' variable
(gdb) info registers     # Optional: see where it was stored
```

**Disassemble `main()` to view the generated instructions:**

```gdb
(gdb) disassemble main
```

or use `objdump` outside GDB:

```bash
riscv64-unknown-elf-objdump -d hello.elf > hello.dump
```



**Explanation:**

* `csrr %0, cycle` reads the current CPU cycle count into a register.
* The result is stored in the C variable `cycle` using inline assembly.
* `-g` is required for GDB to understand variable names like `cycle`.
* `x/w &cycle` shows the value of `cycle` in memory after the instruction executes.

</details>

<details>
<summary><strong>📸 Output & Screenshots</strong></summary>

![image](https://github.com/user-attachments/assets/df8e5950-af39-45cb-bce9-57b749f4a957)
![image](https://github.com/user-attachments/assets/1f9b372f-15c6-4595-ba3b-af497c64aa9c)
![image](https://github.com/user-attachments/assets/da22d8a0-b05c-4bdf-b32f-779835ce1510)
![image](https://github.com/user-attachments/assets/3196df89-8dba-4a9d-9ee9-c80cfbf0802e)


</details>

---

## 10. MMIO GPIO Toggle Using Volatile Pointers

<details>
<summary><strong>🧾 Instructions</strong></summary>

### 10. MMIO GPIO Toggle Using Volatile Pointers

**Minimal Bare-Metal Snippet:**

```c
int main() {
    volatile uint32_t *gpio = (uint32_t *)0x10012000; *gpio = 0x1;
    while (1);
    return 0;
}
```



**Explanation:**

* `volatile` prevents the compiler from **optimizing away** the store to the GPIO memory address.
* The pointer cast `(uint32_t *)0x10012000` treats the MMIO base address as a pointer to a 32-bit register.
* Writing `*gpio = 0x1;` sets GPIO pin 0 high (toggle can be added later).



**Memory Alignment:**

* `0x10012000` is **4-byte aligned**, which is correct for a `uint32_t` (32-bit value).
* Unaligned memory access can cause **hardware exceptions** on RISC-V.
* MMIO registers must always be accessed with correctly sized and aligned operations.

</details>

<details>
<summary><strong>📸 Output & Screenshots</strong></summary>

![image](https://github.com/user-attachments/assets/887acfb6-fb9a-4e06-965b-f24b2d2067bb)
![image](https://github.com/user-attachments/assets/3a6f0583-3b38-441d-ba7d-12ece71451cd)


</details>



---

## 11. Linker Script 101

<details>
<summary><strong>🧾 Instructions</strong></summary>

## 🛠️ Objective

Create a minimal linker script that places:

* `.text` section at address `0x00000000` (Flash)
* `.data` section at address `0x10000000` (SRAM)

For the `RV32IMC` RISC-V target (bare-metal, no OS).

---

## 📦 Step 1: Create the C Source File

```bash
nano main.c
```

Paste this minimal code:

```c
int x = 42;

int main() {
    x += 1;
    while (1);
    return 0;
}
```

Save and exit.

---

## 📁 Step 2: Create the Linker Script

```bash
nano link.ld
```

Paste this content:

```ld
SECTIONS {
  .text 0x00000000 : {
    *(.text*)
  }

  .data 0x10000000 : {
    *(.data*)
  }
}
```

Save and exit.

---

## 🌐 Step 3: Compile with the Custom Linker Script

```bash
riscv64-unknown-elf-gcc -march=rv32imac -mabi=ilp32 -nostdlib -T link.ld -o output.elf main.c
```

This compiles your program without the standard library and uses your custom memory layout.

---

## ✅ Step 4: Verify Section Placement

Use `objdump` to confirm that the sections are correctly placed:

```bash
riscv64-unknown-elf-objdump -h output.elf
```

You should see something like:

```
Idx Name          Size      VMA       LMA       File off  Algn
  0 .text         xxxx      00000000  00000000  ...
  1 .data         xxxx      10000000  10000000  ...
```

This confirms:

* `.text` is at `0x00000000` (Flash)
* `.data` is at `0x10000000` (SRAM)

---

## 💡 Flash vs. SRAM — Why Are They Separated?

| Memory Type | Section | Properties                             |
| ----------- | ------- | -------------------------------------- |
| Flash       | `.text` | Non-volatile, read-only, stores code   |
| SRAM        | `.data` | Volatile, read-write, stores variables |

* Flash retains your program across resets — perfect for `.text`.
* SRAM is writable and fast — perfect for `.data`.

</details>

<details>
<summary><strong>📸 Output & Screenshots</strong></summary>

![image](https://github.com/user-attachments/assets/99fc06c3-5bc4-4b9b-a810-22f5c01e15b2)


</details>


---


---

## 12. Start-up Code & `crt0`

<details>
<summary><strong>🧾 Instructions</strong></summary>

## 🛠️ Objective

Understand what `crt0.S` (C Runtime Zero) does in a bare-metal RISC-V system and how it prepares the environment before `main()` runs.

---

## 🔍 What is `crt0.S`?

`crt0.S` is the **startup assembly code** that runs **before `main()`** in a bare-metal system. It does not rely on any OS or standard C runtime — it provides the **minimum initialization** needed to run a C program on hardware (or a simulator like QEMU).

---

## ⚙️ Responsibilities of `crt0.S`

Typical tasks performed:

1. **Set up the stack pointer**

   * Bare-metal systems don’t have a stack set up automatically.
   * You typically point it to the top of a memory region (e.g., `0x80004000` or similar).

2. **Zero the `.bss` section**

   * `.bss` holds uninitialized global/static variables.
   * Must be zeroed manually, usually using two symbols defined in the linker script:

     ```asm
     la a0, __bss_start
     la a1, __bss_end
     loop:
       beq a0, a1, done
       sw zero, 0(a0)
       addi a0, a0, 4
       j loop
     done:
     ```

3. **Call `main()`**

   * It’s a normal `call main` instruction.

4. **Infinite loop after `main()` returns**

   * Prevents the program from running into unknown memory.

   ```asm
   call main
   hang:
     j hang
   ```

---

## 📦 Where Do You Get `crt0.S`?

Options include:

* **Write your own**, based on the minimal tasks above.
* Use examples from:

  * **Newlib (`libgloss`)**

    * Folder: `newlib/libgloss/riscv/crt0.S`
  * **SiFive SDKs or other BSPs (Board Support Packages)** for your target.
  * **Minimal embedded repos**, e.g., [riscv-blink](https://github.com/sifive/riscv-blink)

These are often customized for:

* Memory layout
* Peripheral initialization
* Optional features like interrupt support

---

## 📄 Minimal Example of `crt0.S`

```asm
.section .init
.globl _start
_start:
    la sp, _stack_top         # Setup stack pointer

    # Zero out .bss
    la a0, __bss_start
    la a1, __bss_end
bss_clear:
    bge a0, a1, bss_done
    sw zero, 0(a0)
    addi a0, a0, 4
    j bss_clear
bss_done:

    call main                 # Call C main()

hang:
    j hang                    # Infinite loop after main returns
```

---

## 🧠 Why This Matters

Bare-metal systems do **not** have:

* An OS to initialize memory
* A C runtime to set up globals or the stack

So `crt0.S` acts as your **bare-metal bootstrapping code**, and it must be tailored to your:

* CPU architecture (e.g., RV32IMAC)
* Linker script memory map
* Toolchain expectations

</details>

<details>
<summary><strong>📸 Output & Screenshots</strong></summary>

![image](https://github.com/user-attachments/assets/937d9bd7-1818-4380-b310-016f53694943)
![image](https://github.com/user-attachments/assets/fbc52086-97be-4f8c-9ec6-4aea9aeb84fa)
![image](https://github.com/user-attachments/assets/2028e74c-a734-4eb0-9810-c92b92934c48)
![image](https://github.com/user-attachments/assets/9400e289-78b0-4d24-a331-7bcc57585db3)


</details>

---


## 13. Interrupt Primer

<details>
<summary><strong>🧾 Instructions</strong></summary>

### ❓ Objective

Demonstrate how to enable the **machine-timer interrupt (MTIP)** and write a simple handler in C/assembly.

---

### 🧰 Steps Summary

- Set up `mtime` and `mtimecmp` registers using memory-mapped I/O (CLINT).
- Enable the **MTIE** bit in the `mie` CSR.
- Enable the **MIE** bit in the `mstatus` CSR.
- Set the `mtvec` register to point to your interrupt handler.
- Write a C handler using `__attribute__((interrupt))`.
- Add `UART` prints inside the handler to show periodic timer output in QEMU.

---

### 🧾 Minimal Code Snippets

**Timer Handler in C:**
```c
void __attribute__((interrupt)) timer_handler(void) {
    *mtimecmp = *mtime + 100000;
    interrupt_counter++;
    uart_puts("Timer fired: ");
    uart_putint(interrupt_counter);
    uart_putc('\n');
}
````

**Interrupt Setup:**

```c
write_csr(mtvec, (uint32_t)timer_handler);
write_csr(mie, read_csr(mie) | (1 << 7));       // Enable MTIE
write_csr(mstatus, read_csr(mstatus) | (1 << 3)); // Enable MIE
```

**Startup Assembly (`crt0.S`):**

```asm
.section .text
.globl _start
_start:
  la sp, stack_top
  call main
  j .
```

**Linker Script Snippet:**

```ld
SECTIONS {
  . = 0x80000000;
  .text : { *(.text*) }
  .data : { *(.data*) }
  .bss  : { *(.bss*) }
}
```

**Build Command:**

```bash
riscv32-unknown-elf-gcc -Wall -O2 -ffreestanding -nostdlib -mabi=ilp32 -march=rv32imac \
  -o mtip.elf mtip.c crt0.S -T link.ld
```

**Run in QEMU:**

```bash
qemu-system-riscv32 -nographic -machine virt -bios none -kernel mtip.elf -serial mon:stdio
```

---

### 🧠 Why It Matters

* Essential for preemptive multitasking and real-time systems.
* Lays the foundation for writing OS-level features on RISC-V.
* Helps understand core concepts of CSRs, CLINT, and trap handling in bare-metal environments.

</details>

<details>
<summary><strong>📸 Output & Screenshots</strong></summary>


![image](https://github.com/user-attachments/assets/9b65fc49-e209-4d7f-ae5f-f2ee07ddb5e8)
![image](https://github.com/user-attachments/assets/98c9a6e0-51b6-496e-8124-3c894a04c83c)
![Image](https://github.com/user-attachments/assets/808acfef-bc75-48ef-8501-395b1bdec3d5)



</details>


---


## 14. rv32imac vs rv32imc – What’s the “A”?

<details>
<summary><strong>🧾 Instructions</strong></summary>

### ❓ What is the “A” Extension?

The `'A'` extension in `rv32imac` stands for **Atomic Instructions**, and it's what differentiates `rv32imac` from `rv32imc`. These instructions are crucial in concurrent and multi-core systems where multiple threads or cores access shared memory.

---

### 🧠 Why Is It Useful?

The `A` extension adds **atomic read-modify-write instructions** to the instruction set. These are essential for:

- Building **lock-free data structures**
- Implementing **mutexes, semaphores, and spinlocks**
- Writing **OS kernels** or **thread-safe embedded code**
- Preventing **race conditions** during memory access

---

### 🧩 Instructions Introduced

The extension introduces the following atomic instructions:

| Instruction   | Description                             |
|---------------|-----------------------------------------|
| `lr.w`        | Load-Reserved (32-bit)                  |
| `sc.w`        | Store-Conditional (32-bit)              |
| `amoadd.w`    | Atomic Add                              |
| `amoswap.w`   | Atomic Swap                             |
| `amoor.w`     | Atomic OR                               |
| `amoand.w`    | Atomic AND                              |
| `amoxor.w`    | Atomic XOR                              |
| `amomin.w`    | Atomic Minimum (signed)                 |
| `amomax.w`    | Atomic Maximum (signed)                 |
| `amominu.w`   | Atomic Minimum (unsigned)               |
| `amomaxu.w`   | Atomic Maximum (unsigned)               |

These allow **atomic memory updates** without disabling interrupts or using heavy locks.

---

### 📌 Summary

If you're writing:
- Bare-metal firmware for a **single-core system**: `rv32imc` is usually enough.
- A **multi-threaded application** or **OS kernel**: You need `rv32imac` for atomic ops.

</details>



## 15. Atomic Test Program

<details>
<summary><strong>🧾 Instructions</strong></summary>

### ❓ Objective

Provide a **two-thread mutex example** (pseudo-threads inside `main`) using `lr.w` / `sc.w` atomic primitives on **RV32** architecture.

---

### 🔒 Approach: Spinlock using `lr.w` / `sc.w`

- The lock is implemented as a **spinlock** using RISC-V's `lr.w` (load-reserved) and `sc.w` (store-conditional) instructions.
- Only one "pseudo-thread" (loop block in main) can enter the critical section at a time.

---

### 🔐 Spinlock Code (C + Inline ASM)

```c
volatile int lock = 0;

void acquire_lock(volatile int *lock) {
    int tmp;
    do {
        asm volatile (
            "lr.w %[tmp], (%[addr]);\n"
            "bnez %[tmp], 1f;\n"
            "li %[tmp], 1;\n"
            "sc.w %[tmp], %[tmp], (%[addr]);\n"
            "1:"
            : [tmp] "=&r" (tmp)
            : [addr] "r" (lock)
            : "memory"
        );
    } while (tmp != 0);  // Retry if store-conditional failed
}

void release_lock(volatile int *lock) {
    *lock = 0;
}
````

---

### 🤖 Simulated Two Threads in `main()`

```c
int shared_counter = 0;

int main() {
    for (int i = 0; i < 5; i++) { // Thread 1
        acquire_lock(&lock);
        shared_counter++;
        uart_puts("Thread 1 incremented counter: ");
        uart_putint(shared_counter);
        uart_putc('\n');
        release_lock(&lock);
    }

    for (int i = 0; i < 5; i++) { // Thread 2
        acquire_lock(&lock);
        shared_counter++;
        uart_puts("Thread 2 incremented counter: ");
        uart_putint(shared_counter);
        uart_putc('\n');
        release_lock(&lock);
    }

    while (1);
}
```

* Use UART prints to observe mutex effectiveness in QEMU (no overlapping prints or skipped values).
* Ensure `-march=rv32im` or `rv32imac` when compiling, as `lr.w` / `sc.w` are from the `'A'` extension.

---

### 🧠 Why It Matters

* Demonstrates **real-world use** of atomic primitives (`lr.w`, `sc.w`).
* Teaches **synchronization** and **concurrency** on a bare-metal system.
* Foundation for **multi-core** safe programming and OS-level synchronization.

</details>

<details>
<summary><strong>📸 Output & Screenshots</strong></summary>

![image](https://github.com/user-attachments/assets/bca50249-592c-4097-a98f-be4529d6a0a1)
![image](https://github.com/user-attachments/assets/e9e5643a-3c90-489e-86a6-89aba5598e02)
![image](https://github.com/user-attachments/assets/c8e77007-b6c0-4b25-af66-8f6ebe59ef19)
![image](https://github.com/user-attachments/assets/74579d36-b1f5-4145-92f9-08f4cb903f55)
![Image](https://github.com/user-attachments/assets/13a77314-a8c2-4c9d-b45a-a1333f6991b6)


</details>


---

## 16. Using Newlib printf Without an OS

<details>
<summary><strong>🧾 Instructions</strong></summary>

## 🛠️ Objective

Enable the use of `printf()` in a bare-metal RISC-V environment by retargeting the `_write` syscall to send characters to a memory-mapped UART.

---

## 📦 Requirements

* Memory-mapped UART output (e.g., at `0x10000000`)
* Newlib for standard C support (e.g., `printf`)
* Your own `syscalls.c` implementing at least `_write` and `_sbrk`
* Custom `link.ld` defining `_end`
* Startup file (`crt0.S`) for bare-metal boot

---

## 📄 Code Snippets

### 🔧 `syscalls.c`

```c
#define UART0_ADDR 0x10000000UL

int _write(int fd, char *buf, int len) {
    volatile char *uart = (char *)UART0_ADDR;
    for (int i = 0; i < len; i++) {
        uart[0] = buf[i];
    }
    return len;
}

void* _sbrk(int incr) {
    extern char _end;
    static char *heap_end = &_end;
    char *prev_heap_end = heap_end;
    heap_end += incr;
    return (void *)prev_heap_end;
}
```

---

### 🔗 `link.ld` (add `_end` after `.bss`)

```ld
.bss : {
  *(.bss*)
  _end = .;
}
```

---

### 🚀 `main.c`

```c
#include <stdio.h>

int main() {
    printf("Hello from printf over UART!\n");
    while (1);
    return 0;
}
```

---

### 📦 Startup `crt0.S`

```asm
.section .text
.global _start
_start:
    la sp, stack_top
    call main
    j .

.section .bss
.space 4096
stack_top:
```

---

### 🧱 Build Command

```bash
riscv32-unknown-elf-gcc -Wall -O2 -ffreestanding -nostdlib \
  -mabi=ilp32 -march=rv32imac \
  -o printf.elf main.c syscalls.c crt0.S -T link.ld -lc -lgcc
```

---

### 🏃‍♂️ Run on QEMU

```bash
qemu-system-riscv32 -nographic -machine virt -bios none \
  -kernel printf.elf -serial mon:stdio
```

</details>

<details>
<summary><strong>📸 Output & Screenshots</strong></summary>

![image](https://github.com/user-attachments/assets/b79c6526-43b0-41ae-9703-19b9e207ac6c)
![image](https://github.com/user-attachments/assets/57b1d3ef-7df9-4cd7-8516-306c8933c2e3)
![image](https://github.com/user-attachments/assets/53dba741-cb46-4c45-8251-b099b68a25e9)
![image](https://github.com/user-attachments/assets/e17c90e4-3ece-4f91-92b2-6099d262e4ac)
![Image](https://github.com/user-attachments/assets/cfe2b36e-a654-48d1-a493-a2eadbbbe73f)



This confirms that `printf()` is internally using `_write()`, and it's been redirected to the UART peripheral.

</details>


---

## 17: Endianness Check (Little vs Big Endian)

<details>
<summary><strong>🧾 Instructions</strong></summary>

### 🛠️ Objective

Determine the endianness of a RISC-V RV32 system using a union trick to inspect byte ordering.

---

### 🧠 Background

In little-endian systems (like RV32 by default), the least significant byte of a multi-byte value is stored at the **lowest** memory address.

---

### 📄 main.c

```c
#include <stdio.h>
#include <stdint.h>

int main() {
    union {
        uint32_t value;
        uint8_t bytes[4];
    } test;

    test.value = 0x01020304;

    printf("Byte 0: 0x%02x\n", test.bytes[0]);
    printf("Byte 1: 0x%02x\n", test.bytes[1]);
    printf("Byte 2: 0x%02x\n", test.bytes[2]);
    printf("Byte 3: 0x%02x\n", test.bytes[3]);

    while (1);
    return 0;
}
```

---

### ⚙️ Compilation Command

```bash
riscv32-unknown-elf-gcc -Wall -O2 -ffreestanding -nostdlib \
  -mabi=ilp32 -march=rv32imac_zicsr \
  -o endian.elf main.c syscalls.c crt0.S -T link.ld -lc -lgcc
```

---

### 🚀 Run on QEMU

```bash
qemu-system-riscv32 -nographic -machine virt -bios none \
  -kernel endian.elf -serial mon:stdio
```

---

### 📄 link.ld

```ld
ENTRY(_start)

SECTIONS {
  . = 0x80000000;

  .text : {
    *(.init)
    *(.text*)
  }

  .rodata : {
    *(.rodata*)
  }

  .data : {
    *(.data*)
  }

  .bss : {
    __bss_start = .;
    *(.bss*)
    *(COMMON)
    __bss_end = .;
    . = ALIGN(4);
    _stack_top = . + 0x1000;
    _end = .;
  }
}
```

---

### 📄 crt0.S (Minimal Startup)

```asm
.section .init
.global _start
_start:
    la a0, __bss_start
    la a1, __bss_end
1:
    beq a0, a1, 2f
    sw zero, 0(a0)
    addi a0, a0, 4
    j 1b
2:
    call main
1:  j 1b
```

---

### 📄 syscalls.c (Minimal Syscalls)

```c
#include <sys/stat.h>
#include <stdint.h>

extern int _end;
static char *heap_end = (char *)&_end;

caddr_t _sbrk(int incr) {
    char *prev_heap_end = heap_end;
    heap_end += incr;
    return (caddr_t)prev_heap_end;
}

int _write(int file, char *ptr, int len) {
    for (int i = 0; i < len; i++) {
        volatile char *uart = (char *)0x10000000;
        *uart = ptr[i];
    }
    return len;
}

int _read(int file, char *ptr, int len) { return 0; }
int _close(int file) { return -1; }
int _fstat(int file, struct stat *st) { return 0; }
int _lseek(int file, int ptr, int dir) { return 0; }
int _isatty(int file) { return 1; }
```

</details>

<details>
<summary><strong>📸 Output & Screenshots</strong></summary>

![image](https://github.com/user-attachments/assets/b79c6526-43b0-41ae-9703-19b9e207ac6c)
![image](https://github.com/user-attachments/assets/ae2367a4-98a6-4460-8432-94044d66f326)
![image](https://github.com/user-attachments/assets/9544f93b-d59e-4ac4-929f-b86896c0f573)
![image](https://github.com/user-attachments/assets/4f3d237a-8aaf-4e95-ae64-0dfd2a370c0e)
![image](https://github.com/user-attachments/assets/b2764563-76aa-49be-a364-f862d7f495c9)

---

### 🧾 Explanation

| Memory Address | Byte Label | Value |
| -------------- | ---------- | ----- |
| `&bytes[0]`    | LSB        | 0x04  |
| `&bytes[1]`    |            | 0x03  |
| `&bytes[2]`    |            | 0x02  |
| `&bytes[3]`    | MSB        | 0x01  |

This confirms **little-endian** byte ordering: LSB is stored at the lowest memory address.

</details>

---


## 🧾 Summary




## 📘 What is RISC-V?

* RISC-V is an **open Instruction Set Architecture (ISA)** — a blueprint that defines the instructions a processor can execute.
* ❌ It is **not** a processor, Verilog code, or a physical chip.
* ✅ It **can** be implemented in hardware using languages like Verilog.
* This decoupling is critical — like a programming language is separate from its compiler, RISC-V is separate from the CPU that implements it.

---

## 📅 Why We Did These Tasks

Each weekly task was designed to teach **core concepts** behind bare-metal embedded systems using RISC-V.

| Task(s) | Concept Learned                                                                     |
| ------- | ----------------------------------------------------------------------------------- |
| 1–2     | Installed the RISC-V toolchain, wrote and compiled basic C programs.                |
| 3–4     | Used `objdump` and `readelf` to inspect ELF files and analyze register usage.       |
| 5       | Observed stack behavior by examining disassembly and runtime register changes.      |
| 6–7     | Used memory-mapped I/O to print characters by writing directly to UART.             |
| 8–9     | Explored how to control hardware directly without an OS.                            |
| 10–11   | Wrote custom linker scripts and minimal startup code (`crt0.S`).                    |
| 12      | Understood full startup sequence: setting up stack, zeroing `.bss`, calling `main`. |
| 13      | Enabled timer interrupts using `mtime` and `mtimecmp` and wrote a custom ISR.       |
| 14      | Compared `RV32IMC` vs `RV32IMAC` — explained the Atomic `'A'` extension.            |
| 15      | Built a spinlock using `lr.w` / `sc.w` to simulate two-thread mutex.                |
| 16      | Retargeted `_write()` to send `printf()` output to UART without an OS.              |
| 17      | Verified **little-endian** behavior on RV32 using a union trick in C.               |

---

## 🧠 Final Reflection

Through these tasks, we developed a **full-stack perspective** on bare-metal RISC-V systems. Key takeaways:

* Built and ran RISC-V programs without any operating system.
* Controlled UART and hardware via memory-mapped I/O.
* Handled timer interrupts using low-level CSRs.
* Wrote startup code (`crt0.S`) and linker scripts (`link.ld`) manually.
* Explored atomic memory operations and how concurrency is handled in hardware.
* Implemented `printf()` in a bare-metal context by writing a custom `_write()`.

---

## 🎯 Why This Matters

We didn’t just learn RISC-V as an architecture — we **experienced** the process of bringing up a real system based on it. This included:

* Toolchain usage
* Bootstrapping the runtime
* Writing interrupt handlers
* Understanding processor state
* Debugging with disassembly
* And finally, interacting with real hardware (UART)

This foundation is **essential** for embedded systems, OS kernel development, device driver design, and silicon bring-up.

</details>


---


