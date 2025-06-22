
# caravel.v Analysis
![image](https://github.com/user-attachments/assets/56129171-25d7-4528-b0e2-d0c2d0f1941c)

## 1. Four Major Sub‑Modules Instantiated Inside `caravel`

The following are the four primary sub-modules instantiated inside the `caravel` top module:

1. **padframe** – Handles all I/O pads including power, ground, clock, reset, and user-defined I/Os.
2. **caravel_core** – Contains the RISC-V-based management SoC, including peripherals, memory, and project interfacing.
3. **copyright_block** – Static block used for copyright information.
4. **caravel_logo**, **caravel_motto**, **open_source**, **user_id_textblock** – Additional static/logo metadata blocks used for identification and aesthetics.

---

## 2. Signals Crossing the “Management Protect” Boundary

The following signals cross the protected management domain into the user/project domain:

- `mprj_io`, `mprj_io_in`, `mprj_io_out`, `mprj_io_oeb` – user I/Os
- Flash interface signals: `flash_csb`, `flash_clk`, `flash_io0`, `flash_io1`
- GPIO lines connected to external logic
- `clock` and `resetb` signals entering the chip and routed into internal logic
- Additional user interface signals as per padframe connectivity

These signals act as the interface between the secure management area and the user project area.

---

## 3. Synchronization of Clock and Reset

**Clock Synchronization:**
- The clock signal first enters through the `padframe` and is routed internally as `clock_core`.
- This `clock_core` signal is then fed into `caravel_core`, which serves as the clock domain for the SoC.

**Reset Synchronization:**
- The external reset signal (`resetb`) is routed into the `caravel_core` module.
- Inside `caravel_core`, the reset is synchronized (typically using flip-flop stages) and renamed as `rstb_h`, which is then distributed to internal logic.

This ensures both clock and reset are clean and glitch-free before being used by internal modules.

---

Here is a complete `README.md`-style Markdown write-up for your **Week 2** submission, based on the PDF checklist from `p1w2[1].pdf` and your debugging journey:

---

# 📁 Week 2 – Caravel SoC RTL Exploration and VCS Simulation

<details>
<summary><strong>✅ Tasks Completed</strong></summary>

### ✔️ Summary of Steps Completed:

| Step | Description                                     | Status |
| ---- | ----------------------------------------------- | ------ |
| 0    | Prerequisites checked (PDK, VCS, repo cloned)   | ✅ Done |
| 1    | Git branch creation and repo sync               | ✅ Done |
| 2    | Caravel RTL analysis and markdown documentation | ✅ Done |
| 3    | VCS hierarchy generation                        | ✅ Done |
| 4    | Deep-dive into RTL submodules                   | ✅ Done |
| 5    | First full-chip VCS compile and run             | ✅ Done |
| 6    | Documentation and folder packaging              | ✅ Done |

</details>

---

<details>
<summary><strong>📘 Week2 Notes (docs/week2_notes.md)</strong></summary>

## 1️⃣ Major Submodules in `caravel.v`

The top-level RTL file is [`rtl/caravel.v`](https://github.com/vsdip/vsdRiscvScl180/blob/main/rtl/caravel.v). The **four major submodules** instantiated inside `caravel` are:

1. **padframe** – Handles chip I/O pads (digital and analog).
2. **caravel\_core** – Contains RISC-V core, Wishbone bus, and management SoC.
3. **copyright\_block** – For silicon identification.
4. **Other logo/metadata blocks**:

   * `caravel_logo`
   * `caravel_motto`
   * `open_source`
   * `user_id_textblock`

---

## 2️⃣ Signals Crossing Management Protect Boundary

The following signals cross from the management SoC to the user region:

* **User GPIO I/O**:

  * `mprj_io`
  * `mprj_io_in`
  * `mprj_io_out`
  * `mprj_io_oeb`
* **Flash Signals**:

  * `flash_csb`
  * `flash_clk`
  * `flash_io0`
  * `flash_io1`
* **Clock and Reset**:

  * `clock`
  * `resetb`

---

## 3️⃣ Clock & Reset Synchronization

* **Clock Synchronization**:

  * Clock input enters via `padframe`
  * Routed to `caravel_core` as `clock_core`
* **Reset Synchronization**:

  * Reset (`resetb`) enters through padframe
  * Synchronized internally in `caravel_core` to `rstb_h` before distribution

---

## 4️⃣ RTL Deep Dive Summary

| Submodule              | Source File(s)               | Notes                                                            |
| ---------------------- | ---------------------------- | ---------------------------------------------------------------- |
| `padframe`             | `rtl/padframe.v`             | Two pad variants: `gpio_pad`, `flash_pad`                        |
| `housekeeping`         | `rtl/housekeeping_spi.v`     | SPI clk: `spi_clk` <br> CSB pin: `spi_csb` <br> Default mode: 0  |
| `mgmt_core_wrapper`    | `rtl/mgmt_core_wrapper.v`    | Wishbone data width: 32-bit <br> IRQ fan-in: 8 signals           |
| `user_project_wrapper` | `rtl/user_project_wrapper.v` | GPIO controlled: 38 <br> Clock source: `mprj_clk` from mgmt core |

</details>

---

<details>
<summary><strong>🛠️ VCS Compilation & Hierarchy Generation</strong></summary>

## 5️⃣ First Full-Chip Compile

### ✅ Compilation & Simulation Steps:

```bash
# Step 1: Create filelist
find rtl -name "*.v" > synopsys/filelist.f

# Step 2: Compile RTL
vlogan -full64 -sverilog +incdir+rtl -f synopsys/filelist.f

# Step 3: Elaborate with testbench
vcs -full64 -debug_access+all -sverilog \
     +lint=all -timescale=1ns/1ps \
     -f synopsys/filelist.f tb/caravel_boot_tb.v \
     -l compile.log -o simv

# Step 4: Run simulation
./simv +clk_period=10ns -l run.log
```

📸 Screenshot of last lines of `run.log`: ✅ Attached
📄 Log files: `compile.log`, `run.log` — ✅ Present in submission
🔁 Encountered & resolved error:

* `caravel_boot_tb.v` was initially missing — manually downloaded
* Missing modules like `copyright_block` — fetched from upstream repo
* VCS timescale errors — resolved by adding consistent `timescale` directive

---

## 6️⃣ Hierarchy Dump

```bash
vcs -full64 -sverilog -debug_access+none \
     -f synopsys/filelist.f -top vsdcaravel -kdb -o simv

# Verdi failed due to license issues, so used TCL method
echo 'write -hierarchy hierarchy.txt' > generate_hierarchy.tcl
dve -full64 -vpd vcdplus.vpd -command generate_hierarchy.tcl
```

✅ `docs/hierarchy.txt` was manually generated and moved from `csrc/DVEfiles`

</details>

---

<details>
<summary><strong>💡 Challenges Faced & Solutions</strong></summary>

### 🔧 Challenges & Fixes

| Issue                                                       | Solution                                                             |
| ----------------------------------------------------------- | -------------------------------------------------------------------- |
| Missing files like `caravel_boot_tb.v`, `copyright_block.v` | Downloaded from caravel-lite repo                                    |
| Verdi license errors                                        | Switched to using `vcs -kdb` and `dve`                               |
| `Illegal \`timescale\` error                                | Ensured all modules had `\`timescale 1ns/1ps\` at the top            |
| `Undefined macro` or `identifier not declared`              | Fixed by passing `+define+...` and adding `user_defines.v`           |
| `Cannot find cell in liblist`                               | Added missing dummy wrappers and black-box modules                   |
| `Illegal array port connection`                             | Adjusted `dm[...]` slice size for correct multiple of instance count |
| VCS warnings flooding output                                | Used `-suppress` or `grep Error:` to filter logs                     |

</details>

---
![WhatsApp Image 2025-06-18 at 15 36 12_17b1361e](https://github.com/user-attachments/assets/01798624-38de-4ee5-9dd8-3513948d35cd)

![WhatsApp Image 2025-06-18 at 15 37 54_64653b46](https://github.com/user-attachments/assets/f2e2e135-b6f2-4658-bade-b66187e37b69)
