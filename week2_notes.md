
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


