

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


