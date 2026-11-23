
# Educational Single Board Computer (EduSBC)

> **Status:** Schematics & PCB Layout Initialized (KiCad 9 PCB Created)  
> **Last Updated:** 2026-11-22  
> **Measurement Standard:** Strictly Metric (SI: mm / µm)

EduSBC is an open, through-hole Single Board Computer designed to teach computer fundamentals, digital logic, and microarchitecture to beginners—assuming no prior soldering, digital logic, or assembly programming experience. Students build and bring up the board in stages across six lab sessions, moving from polling-based I/O to hardware interrupt service routines (Z80 Mode 1 / Mode 2) via a dedicated jumper.

## Key Hardware & Toolchain Summary

* **CPU & Clocks:** Z80 CMOS CPU (Z84C00 preferred, DIP-40 socketed) with a 4.0000 MHz canned TTL CPU oscillator and a 1.8432 MHz canned TTL UART oscillator.
* **Memory & Glue Logic:** Fixed 64 KB map (32 KB ROM at `$0000–$7FFF` via a universal 28-pin socket supporting 27C256 EPROM or 28C256 EEPROM, and 32 KB SRAM at `$8000–$FFFF`), decoded with 74HCT glue logic.
* **I/O Interfaces:**
  * **Parallel I/O:** 82C55 / 8255 PPI (DIP-40 socketed) at I/O ports `$00–$03` wired to a 26-pin (13×2) horizontal 2.54 mm box header (`JP_GPIO_PWR` selects `+5V` or `GND` on Pin 26).
  * **Serial I/O:** 16C550 / 16550 UART (DIP-40 socketed) at I/O ports `$08–$0F` with both a DE-9 male RS-232 port (MAX232, 5-wire handshaking) and a 6-pin 2.54 mm TTL header (compatible with DTECH FT232RL 5V cables). *Connect either TTL (`J4`) or RS-232 (`J3`), never both simultaneously.*
* **Power & Reset:** 9–15 VDC barrel jack input (standard 12 VDC / 2 A Qualtek `QAWA-24-12-US01` adapter), SPDT power slide switch, reverse Schottky protection, 7805 linear regulator with heatsink clearance, 3.0 mm green power LED, DS1813-5+ reset supervisor, and 74HCT14 reset inverter.
* **Form Factor & CAD:** 160.000 mm × 100.000 mm Eurocard (2-layer JLCPCB spec), designed in **KiCad 9** on **Debian Linux 13 (amd64)** with **FreeRouting 2.5.0** (Specctra DSN / SES workflow).

## Repository Structure

* `edu_sbc.kicad_pro` / `edu_sbc.kicad_prl` — KiCad 9 project and local settings.
* `edu_sbc.kicad_sch` and `01_*.kicad_sch` – `06_*.kicad_sch` — Hierarchical schematic sheets (`root_schematic_edu_sbc_kicad_sch.md` provides schematic notes).
* `edu_sbc.kicad_pcb` and `edu_sbc-drc.rpt` — 2-layer PCB layout and DRC report.
* `edu_sbc.pretty/` and `fp-lib-table` — Local footprint library (including `SW_Slide_Power_SPDT.kicad_mod`) and footprint library table.
* `BOM.md` and `BOM.csv` — Bill of materials grouped by lab session and formatted for component ordering.
* `DESIGN_DECISIONS.md` — Detailed hardware specifications, drafting/routing rules, lab build sequence, and session change log.
* `TO_DO.md` and `LICENSE` — Active task checklist and repository license.

## Getting Started & Workflow

1. Review `DESIGN_DECISIONS.md` for architecture constraints, metric design rules, and the six-lab progressive assembly plan.
2. Check `TO_DO.md` for current schematic, layout, BOM, and verification tasks.
3. Keep project files and datasheets stored locally in your working directory (default local workspace: `/home/lynchaj/Documents/edu_sbc`).
```[cite: 1, 2, 3]
