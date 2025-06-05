

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

