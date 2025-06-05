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
