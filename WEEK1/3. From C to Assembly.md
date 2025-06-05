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

