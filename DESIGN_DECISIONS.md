# Educational Single Board Computer (EduSBC) — Project Decisions & Specifications Record

> **Status:** Schematics & PCB Layout Initialized (KiCad 9 PCB Created)
> **Last Updated:** 2026-11-22
> **Target CAD Tool:** KiCad 9 on Debian Linux 13 (amd64)
> **Project File:** `/home/lynchaj/Documents/edu_sbc/edu_sbc.kicad_pro`
> **Root Schematic:** `/home/lynchaj/Documents/edu_sbc/edu_sbc.kicad_sch`
> **PCB Layout:** `/home/lynchaj/Documents/edu_sbc/edu_sbc.kicad_pcb`
> **Bill of Materials:** `/home/lynchaj/Documents/edu_sbc/BOM.md` (and `BOM.csv`)
> **Autorouter:** FreeRouting 2.5.0 (Specctra DSN / SES workflow)
> **PCB Fabricator:** JLCPCB Standard 2-Layer Process
> **Measurement System:** **Strictly Metric (SI: mm / µm)**
> **Workspace Directory:** `/home/lynchaj/Documents/edu_sbc`

---

## 1. Project Vision & Purpose
- **Core Mission:** An open, observable Single Board Computer designed explicitly to teach **computer fundamentals, digital logic, and microarchitecture** to vocational and engineering students.
- **Application:** Basic embedded controller with CPU, RAM, ROM, parallel GPIO, and dual serial console interfaces.
- **Pedagogical Progression:**
  - **Phase 1 (Beginner):** Strictly polling-based I/O routines.
  - **Phase 2 (Advanced Microarchitecture):** Hardware interrupt service routines (Z80 Mode 1 / Mode 2) enabled via a dedicated jumper.
- **Assembly Philosophy:** Modular, progressive assembly. Students build and verify the computer one subsystem at a time across multiple laboratory sessions.
- **Target Student Profile:** Absolute beginner; assume no prior soldering, digital logic, or assembly programming experience.
- **Measurement Standard:** **100% Metric (SI)**. All PCB dimensions, trace widths, clearances, component pitches, drill sizes, keep-outs, and grid coordinates are strictly specified in millimeters (mm) or micrometers (µm).
- **File & Data Retention Policy:** All project files, schematics, PCB layouts, libraries, documentation, and downloaded datasheets must strictly reside locally on the user's PC in `/home/lynchaj/Documents/edu_sbc`.

---

## 2. System Architecture & Hardware Specifications

### 2.1 Central Processing Unit (CPU)
- **Processor:** Z80 CPU (CMOS Z84C00 preferred, 40-pin DIP, socketed, 2.54 mm pin pitch, 15.24 mm row spacing).
- **Architecture:** 8-bit data bus, 16-bit address bus, dedicated I/O address space via `/IORQ`.
- **Clock:** **4.0000 MHz** canned TTL crystal oscillator (socketed, standard DIP-8 or DIP-14 footprint with 2.54 mm pitch).
- **Interrupt & Bus Control Biasing:**
  - `/NMI`, `/WAIT`, `/BUSREQ`: Permanently pulled HIGH to $+5\text{V}$.
  - `/INT`: Tied to $+5\text{V}$ pull-up (guaranteeing it is never floating when disconnected).
  - Scope test points provided for `/WAIT` and `/INT`.
- **Hardware Interrupt Jumper (`JP_INT` - 2-pin header, 2.54 mm pitch):**
  - **Jumper Removed (Default for Initial Labs):** Z80 `/INT` is held firmly HIGH (+5V). System operates strictly in polling mode with zero risk of interrupt race conditions.
  - **Jumper Installed (Advanced Labs):** Connects the active-low inverted UART interrupt line (`/UART_INT`) to Z80 `/INT`, enabling interrupt-driven I/O.
- **Packaging & Socketing:** 100% Through-Hole DIP; dual-leaf or machined IC sockets for all active components.

### 2.2 Logic Family & Component Interfacing Standards
- **Logic Family:** **74HCT Series** (TTL-level CMOS) used for all glue logic, decoders, and inverters:
  - Ensures 100% guaranteed voltage threshold compatibility with TTL/NMOS/CMOS output levels ($V_{OH} \ge 2.4\text{V}$, $V_{IH} = 2.0\text{V}$, $V_{IL} = 0.8\text{V}$).
  - Minimizes power consumption while preserving steep CMOS switching edges and high noise margins.
- **Unused Input Termination Policy (Zero Floating CMOS Inputs):**
  - **All unused active-low inputs** must be pulled HIGH to $+5\text{V}$ (via pull-up resistors or resistor network).
  - **All unused active-high inputs** must be tied directly to GROUND.
- **Bussed Resistor Network Policy (SIP Packages, 2.54 mm pitch):**
  - Wherever **four or more common-value resistors share a common rail** (such as $+5\text{V}$ pull-ups for Z80 bus control lines, data bus pull-ups, or unused active-low pins), **use a Single In-line Package (SIP) bussed resistor network** (e.g., Bourns 4600X series, 6-pin to 10-pin common bus, 2.54 mm pitch) instead of individual axial resistors.

### 2.3 Memory Subsystem & Universal ROM Socketing
- **Memory Map (Fixed 64KB Total):**
  - **ROM:** 32KB fixed (`$0000 – $7FFF`). Mapped to low memory to accommodate Z80 power-on reset vector at `$0000`.
  - **RAM:** 32KB static RAM fixed (`$8000 – $FFFF`).
- **Memory Address Decoding:**
  - Decoded using `A15` qualified with `/MREQ`:
    - `A15 = 0` and `/MREQ = 0` $\to$ `/ROM_CS` (Active Low).
    - `A15 = 1` and `/MREQ = 0` $\to$ `/RAM_CS` (Active Low).
  - Implemented cleanly using a socketed **74HCT139** (dual 2-to-4 decoder) or discrete 74HCT gates.
  - Zero wait states required at 4 MHz ($T_{cyc} = 250$ ns; 55 ns SRAM / 120 ns EEPROM).
- **Universal 28-Pin ROM Socket Configuration (EPROM 27C256 vs. EEPROM 28C256):**
  - Controlled by two adjacent 2.54 mm pitch jumpers to eliminate risk of shorting address lines to $+5\text{V}$:
  - **1. ROM Type Jumper (`JP_ROM_TYPE` - 3-pin, 2.54 mm pitch):**
    - **Pins 1–2 (EEPROM 28C256 Mode):** Socket Pin 1 connects to address line $A14$; Socket Pin 27 routes to `JP_ROM_WP`.
    - **Pins 2–3 (EPROM 27C256 Mode):** Socket Pin 1 connects to $+5\text{V}$ ($V_{PP}$); Socket Pin 27 routes directly to address line $A14$.
  - **2. In-Circuit Reprogramming Protection Jumper (`JP_ROM_WP` - 3-pin, 2.54 mm pitch):**
    - Active when 28C256 EEPROM mode is selected:
      - **Pins 1–2 (Write-Protect):** Ties $\overline{WE}$ (Pin 27) HIGH to $+5\text{V}$ (hardware write lock).
      - **Pins 2–3 (Write-Enable):** Connects $\overline{WE}$ (Pin 27) to Z80 $\overline{WR}$ strobe for in-circuit flashing.

### 2.4 Parallel I/O Subsystem
- **IC:** 82C55 / 8255 Programmable Peripheral Interface (PPI) (40-pin DIP, socketed, 2.54 mm pitch, 15.24 mm row spacing).
- **I/O Port Address Allocation:**
  - Base address: `$00 – $03` (Port A, Port B, Port C, Control Register).
  - Decoded via **74HCT138** (socketed) qualified by `/IORQ` and `/M1` (ignoring interrupt acknowledge cycles).
- **Reset Inversion:** 8255 requires active-HIGH reset; driven via inverted `/RESET` from 74HCT14 Schmitt trigger inverter.
- **External GPIO Connector:**
  - Form factor: **26-pin (13×2 / 2×13) horizontal / right-angle 2.54 mm pitch shrouded box header** pointing outward over the PCB edge.
  - **Exact Pinout Map (Odd / Even Interleaved):**
    - **Odd Pins 1, 3, 5, 7, 9, 11, 13, 15:** Port A (PA0, PA1, PA2, PA3, PA4, PA5, PA6, PA7)
    - **Even Pins 2, 4, 6, 8, 10, 12, 14, 16:** Port B (PB0, PB1, PB2, PB3, PB4, PB5, PB6, PB7)
    - **Pins 17 – 24:** Port C (17: PC0, 18: PC1, 19: PC2, 20: PC3, 21: PC4, 22: PC5, 23: PC6, 24: PC7)
    - **Pin 25:** Dedicated Ground (`GND`)
    - **Pin 26:** Selectable Power/Ground via 3-pin jumper (`JP_GPIO_PWR`, 2.54 mm pitch):
      - Pins 1–2 (Default): **VCC (+5V)** delivered to Pin 26.
      - Pins 2–3: **Ground (GND)** delivered to Pin 26.

### 2.5 Serial Communications & Hardware Handshaking
- **IC:** 16C550 / 16550 UART with FIFO (40-pin DIP, socketed, 2.54 mm pitch, 15.24 mm row spacing).
- **Interrupt Output Inversion:**
  - 16550 active-high `INTR` (Pin 30) is inverted via a spare 74HCT14 inverter to generate active-low `/UART_INT`.
  - Routed to Pin 1 of `JP_INT` (Z80 `/INT` connection).
- **I/O Port Address Allocation:**
  - Base address: `$08 – $0F` (8 registers: RBR/THR, IER, IIR/FCR, LCR, MCR, LSR, MSR, SCR).
  - Decoded via 74HCT138 qualified by `/IORQ` and `/M1`.
  - Unused chip selects (CS0, CS1) tied to $+5\text{V}$; `/CS2` driven by decoded select.
- **Baud Rate Clock:** Dedicated **1.8432 MHz** canned TTL crystal oscillator (socketed, standard DIP-8 or DIP-14 footprint, 2.54 mm pitch) feeding `RCLK` and `BAUDOUT` for 0% baud rate error from 9600 to 115200 bps.
- **Reset Inversion:** 16550 requires active-HIGH reset; driven in parallel with 8255 from inverted `/RESET`.
- **Dual Physical Interfaces (Exclusive Operational Policy):**
  1. **RS-232 Port:** Standard **DE-9 (DB9) Male right-angle connector with DTE pinout** driven by onboard MAX232 transceiver (socketed DIP-16) with four 0.1µF charge pump capacitors.
     - **Supported Handshake Signals (5-wire connection):**
       - Pin 2: `RXD` (Data from host PC)
       - Pin 3: `TXD` (Data to host PC)
       - Pin 5: `GND` (Signal ground)
       - Pin 7: `RTS` (Request to send from 16550)
       - Pin 8: `CTS` (Clear to send from host PC)
     - Pins 1 (DCD), 4 (DTR), 6 (DSR), 9 (RI) left open / unpopulated.
  2. **TTL Serial Port:** Standard 2.54 mm pitch horizontal / right-angle 6-pin header (`Connector_PinHeader_2.54mm:PinHeader_1x06_P2.54mm_Horizontal`) pointing off the board edge, designed to mate directly with the standard **DTECH USB to TTL Serial 5V Adapter Cable with FTDI FT232RL Chip (6-Pin 0.1" Female Socket Header)**:
     - **Header Pinout & Mating Map:**
       - **Pin 1 (GND):** System Ground (`GND`) $\leftrightarrow$ DTECH Pin 1 (Black, GND)
       - **Pin 2 (CTS):** Driven by 16C550 `/RTS` (Pin 32) $\rightarrow$ DTECH Pin 2 (Brown, CTS in to PC)
       - **Pin 3 (VCC):** System `+5V` $\leftrightarrow$ DTECH Pin 3 (Red, VCC)
       - **Pin 4 (TXD):** Received from DTECH Pin 4 (Orange, TXD out of PC) $\rightarrow$ 16C550 `SIN` (Pin 10, RX)
       - **Pin 5 (RXD):** Driven by 16C550 `SOUT` (Pin 11, TX) $\rightarrow$ DTECH Pin 5 (Yellow, RXD in to PC)
       - **Pin 6 (RTS):** Received from DTECH Pin 6 (Green, RTS out of PC) $\rightarrow$ 16C550 `/CTS` (Pin 36)
     - Fully compatible with DTECH 6ft FT232RL cables available on Amazon and eBay.
- **Exclusive Operational Mandate & Silkscreen Warning:**
  - **The schematic and PCB silkscreen must prominently state that the operator can connect either TTL Serial (J4) OR DE-9 RS-232 (J3), but NEVER both at once.**
  - Silkscreen text on `F.Silk` adjacent to the serial connectors: `CONNECT EITHER TTL (J4) OR RS-232 (J3) - NOT BOTH`.
- **Contention Prevention Isolation:**
  - Series isolation resistors (1kΩ) placed between the MAX232 receiver outputs (`R1OUT` / `R2OUT`) and the 16550 `SIN` (RX) / `CTS` handshake lines to prevent driver damage if an operator accidentally plugs in both cables.

### 2.6 Power Supply, Protection & Decoupling Architecture
- **System Current Draw Budget:**
  - Onboard Computer Core (Z80 CMOS, 82C55, 16C550, ROM, SRAM, 74HCT logic, dual oscillators, LED): **~140 mA** max.
  - External GPIO $V_{CC}$ Current Allocation: **250 mA** max.
  - **Total 5V System Current:** **~390 mA (0.4 A – 0.5 A peak)**.
  - Since the LM7805 is a linear regulator, input current through the power switch `SW1` equals the total system load ($\approx 0.4\text{ A} - 0.5\text{ A}$ @ 12 VDC).
- **Power Switch Specification (`SW1` - Option B Confirmed):**
  - **Type:** Power Slide Switch (SPDT, through-hole).
  - **Contact Rating:** Minimum **1.0 A – 4.0 A @ 12VDC/28VDC** (e.g., C&K V802 series, NKK SS-12 power series, or E-Switch EG power series).
  - **Footprint:** Dedicated project footprint `edu_sbc:SW_Slide_Power_SPDT` (stored in local library `/home/lynchaj/Documents/edu_sbc/edu_sbc.pretty/`), featuring 4.70 mm terminal pitch, sturdy 1.30 mm drill holes, 2.40 mm pads (0.55 mm annular ring), and clear `ON` / `OFF` silkscreen legends readable when installed on the left PCB edge.
- **Input Source & Wall Adapter Specification:**
  - **Standard Adapter:** **Qualtek QAWA-24-12-US01** (or exact equivalent)
  - **Output:** 12.0 VDC @ 2.0 A (24W regulated/unregulated wall mount adapter)
  - **Plug:** 5.5 mm Outer Diameter × 2.1 mm Inner Diameter center-positive barrel plug (available on DigiKey and Mouser).
  - Operating input voltage range supported by board: 9.0 VDC to 15.0 VDC.
- **Input Jack:** 5.5 mm OD × 2.1 mm ID center-positive barrel jack located on the left edge.
- **Power Switch:** Miniature SPDT slide or toggle power switch placed directly after the barrel jack on the left edge.
- **Reverse Voltage Protection:** Series Schottky diode (1N5819 or 1N5822, 1A–3A through-hole) immediately following the power switch to protect the entire board against reverse polarity.
- **Input Stage Filtering (Pre-Regulator):**
  - **Bulk Capacitor:** 100µF – 220µF electrolytic (25V/35V rated) across the raw input rail.
  - **High-Frequency Bypass:** 100nF (0.1µF) 50V monolithic ceramic capacitor mounted directly at the barrel jack / diode output.
- **Voltage Regulation (7805 Stage):**
  - Onboard regulator footprint: TO-220 (LM7805) with **dedicated physical clearance for a bolt-on / clip-on stamped aluminum heatsink** (or drop-in switching regulator like Murata OKI-78SR) providing +5V @ up to 1.5A safely from 15V inputs.
  - **Regulator Input Capacitor:** 0.33µF – 1µF ceramic or low-ESR electrolytic placed immediately at the 7805 `VIN` pin.
  - **Regulator Output Capacitor:** 10µF – 47µF electrolytic bulk + 0.1µF ceramic placed immediately at the 7805 `VOUT` pin to maintain loop stability and transient response.
  - **Reverse Bias Protection:** 1N4001/1N4148 diode across regulator (anode to `VOUT`, cathode to `VIN`) to protect against discharge when input power is switched off.
- **Power Indicator:** Dedicated **3.0 mm Green LED** with 1kΩ current-limiting resistor located on the left edge after the 5V regulator.
- **Component-Level Bypass Capacitors:**
  - Dedicated **100nF (0.1µF) monolithic ceramic bypass capacitor mounted as close to $V_{CC}$ as practical** for every active IC and oscillator.

### 2.7 Reset & Supervisory Circuitry
- **Reset Supervisor:** **Dallas/Maxim DS1813-5+ in TO-92 footprint**.
  - Active-low `/RESET` open-drain output with internal pull-up resistor.
  - Precise 5V brownout threshold (4.62V nominal).
  - Integrated push-button debounce circuitry directly connected to the manual reset switch.
- **Manual Reset Switch:** 6.0 mm × 6.0 mm tactile momentary pushbutton switch located on the left edge.
- **Inverter Stage:** 74HCT14 Schmitt trigger (socketed) to generate active-HIGH `RESET` required by 8255 PPI and 16550 UART.

### 2.8 Laboratory Instrumentation & Test Points
- Dedicated, clearly labeled Keystone through-hole test point loops:
  - `GND` (multiple points for scope ground clips)
  - `+5V`
  - `CPU_CLK` (4.0000 MHz square wave)
  - `UART_CLK` (1.8432 MHz square wave)
  - `/RESET` (active-low)
  - `RESET_HIGH` (active-high)
  - Key Z80 bus strobes: `/MREQ`, `/IORQ`, `/RD`, `/WR`, `/M1`, `/WAIT`, `/INT`

---

## 3. Schematic & PCB Drafting Standards (Strict Zero-Overlap & Installed-Readability)

### 3.1 Schematic Capture Standards
- **Zero Symbol & Text Overlap:** Absolute prohibition on overlapping schematic symbols, text labels, pin designators, pin names, or junction dots.
- **100% Explicit Net Labeling:** **Every net must be explicitly labeled** (e.g., `A0`–`A15`, `D0`–`D7`, `nRD`, `nWR`, `nMREQ`, `nIORQ`, `nM1`, `CPU_CLK`, `UART_CLK`, `PA0`–`PA7`, `PB0`–`PB7`, `PC0`–`PC7`, `TXD_TTL`, `RXD_TTL`, etc.).
- **Unambiguous Wiring & Busses:**
  - Wires and buses may be used for illustration and signal routing.
  - **No 4-way cross-junctions with connection dots**. All junctions must use distinct, staggered 3-way T-junctions.
  - Crossing wires without connection must have clear perpendicular clearance so non-connections are immediately obvious.

### 3.2 PCB Layout & Silkscreen Visibility Standards ("Installed Readability")
- **Zero Footprint & Text Overlap:** Absolute prohibition on overlapping footprints, silkscreen text, pads, or silkscreen artwork on the PCB.
- **Installed Readability Mandate:**
  - Silkscreen labels must remain **100% visible and readable even when components and sockets are fully installed**.
  - Reference designators (e.g., `U1`, `R1`, `C1`) and Component Values (e.g., `Z84C0004PEG`, `10k`, `100nF`) located on the `F.Fab` layer must be converted and positioned on the `F.Silk` layer **immediately adjacent to (outside) the component footprint outline**.
  - Component text must be placed as close to the component as practical without colliding with neighboring parts, test points, or solder pads.
  - Pin 1 indicators, power polarity (`+`/`-`), jumper options (`WP`/`WE`, `VCC`/`GND`, `27C`/`28C`), and switch states (`ON`/`OFF`) must be clearly silkscreened outside component bodies.
  - Minimum silkscreen text height: **1.000 mm**, line width: **0.150 mm**.

---

## 4. Physical Layout, Routing Rules & Ergonomics (Metric Units)

```
+-------------------------------------------------------------------------------+
| [Hole]                                                               [Hole]   |
|                                                     [TTL Serial Header]       |
| [DC Barrel]                                         [26-Pin GPIO Header] ---> |
| [Pwr Switch]                                                                  |
| [Pwr LED]              Z80 CPU      ROM / RAM                                 |
| [Reset Btn]            8255 PPI     74HCT Decoders  [DE-9 RS-232 Male]  --->  |
| [TO-220 + HS]          16550 UART   Dual Clocks     [JP_INT Jumper]           |
|                                                                               |
| [Hole]                                                               [Hole]   |
+-------------------------------------------------------------------------------+
  <--- LEFT EDGE: POWER & RESET --->                  <--- RIGHT / TOP-RIGHT: I/O --->
```

- **Board Orientation & Form Factor:** Standard Eurocard format: **160.000 mm (Width) × 100.000 mm (Height)** in landscape/horizontal orientation.
- **Component Placement Orientation:**
  - All DIP ICs, sockets, and discrete components placed **horizontally** (Pin 1 to the left).
  - All silkscreen reference designators and text oriented horizontally (left-to-right) for readability on a lab bench.
- **Desk Mounting:** Standard **3.200 mm diameter mounting holes** in **all 4 corners** with generous **6.500 mm diameter keep-outs** for M3 standoffs/screws.
- **Left Edge (Power & Control Zone):** DC barrel jack (5.5 mm × 2.1 mm), power switch, 3.0 mm Green LED, Reset switch & DS1813, TO-220 7805 regulator with heatsink clearance.
- **Right Edge / Top-Right (User & Lab I/O Zone):** 26-pin (13×2) horizontal 2.54 mm pitch GPIO header, DE-9 Male RS-232, 6-pin 2.54 mm pitch TTL serial header.
- **PCB Routing & Net Classes (For FreeRouting & KiCad - All Metric):**
  - **Signal Net Trace Width:** **0.1000 mm**.
  - **Power Net Trace Width:** **0.3000 mm** (3× signal width).
  - **Clearance (Trace-to-Trace, Trace-to-Pad):** **0.1270 mm**.
  - **Via Size:** **0.800 mm Pad Diameter / 0.400 mm Drill Diameter**.
  - **Component Through-Hole Annular Rings:** **$\ge 0.500$ mm** for novice soldering durability.
  - Ground plane pour on top/bottom layers (`F.Cu` and `B.Cu`) after FreeRouting completes.

---

## 5. Fabrication & EDA Toolchain Specifications

### 5.1 EDA & Autorouting Workflow
- **EDA Suite:** **KiCad 9** on Debian Linux 13 (amd64) configured exclusively with **Metric (mm)** display and grid settings.
- **Symbol & Footprint Library Policy:**
  - **Priority:** Strictly utilize **built-in KiCad 9 standard symbols and footprints** for all components whenever possible.
  - **Notification Mandate:** Any component that cannot use a standard built-in symbol or footprint must be explicitly flagged and reported to the user.
  - **Power Switch (`SW1`):** Identified as the primary candidate requiring a dedicated custom footprint (or verification against built-in `Button_Switch_THT:SW_Slide_1P2T_CK_OS102011MS2Q` / `EG1218`). If a custom footprint is required, it will be stored locally in the project repository in a local `edu_sbc.pretty` library.
  - All ICs, DIP sockets, resistors, capacitors, diodes, LEDs, barrel jack, DE-9, IDC headers, and test points map 100% to KiCad 9 standard built-in libraries (`Package_DIP`, `Connector_IDC`, `Connector_Dsub`, `Connector_BarrelJack`, `Button_Switch_THT`, `Package_TO_SOT_THT`, `Resistor_THT`, `Capacitor_THT`, `Diode_THT`, `LED_THT`, `TestPoint`).
- **Autorouter:** **FreeRouting 2.5.0**:
  - Export: KiCad 9 PCB Editor $\to$ `File` $\to$ `Export` $\to$ `Specctra DSN...` (`edu_sbc.dsn`).
  - Route: FreeRouting 2.5.0 imports `edu_sbc.dsn`, routes 2 layers (`F.Cu`, `B.Cu`) respecting Metric Net Classes (0.1000 mm / 0.3000 mm) and Keepout Zones, and writes `edu_sbc.ses`.
  - Import: KiCad 9 PCB Editor $\to$ `File` $\to$ `Import` $\to$ `Specctra Session...` (`edu_sbc.ses`).
  - Post-Route: Add solid Ground Copper Pours on Top and Bottom layers, run DRC.

### 5.2 JLCPCB Manufacturing Parameters (Metric)
- **Board Type:** Standard 2-Layer Rigid PCB.
- **Dimensions:** **160.000 mm × 100.000 mm** (Standard Eurocard).
- **Base Material:** Standard FR-4 (TG130–140).
- **PCB Thickness:** **1.600 mm** (common standard thickness).
- **Copper Weight:** Standard **35 µm** (1 oz) outer layers (`F.Cu` and `B.Cu`).
- **Soldermask Color:** **Green** (both sides).
- **Silkscreen Color:** **White** on **Front (`F.Silkscreen`) and Back (`B.Silkscreen`)**.
- **Surface Finish:** Standard HASL (with lead or lead-free / RoHS).
- **Minimum Drill / Annular Ring:** Fully compliant with JLCPCB standard capabilities ($>0.300$ mm drill, $>0.130$ mm annular ring).

---

## 6. Subsystem Assembly & Verification Flow (Multi-Session Labs)
Students build and verify the computer across 6 structured lab sessions:
1. **Lab 1 — Power Supply, Protection & Indicator:** Solder DC barrel jack, power switch, reverse protection diode, input bulk/bypass caps, 7805 regulator with heatsink and capacitors, and 3.0 mm Green LED. *Test: Verify clean +5.0V with DMM before installing any sockets.*
2. **Lab 2 — Reset Supervisor & Dual Oscillators:** Solder DS1813 supervisor, reset button, 74HCT14 inverter socket, 4.0000 MHz CPU oscillator socket, and 1.8432 MHz UART oscillator socket. *Test: Insert ICs/oscillators and verify reset pulse and both clock square waves on oscilloscope.*
3. **Lab 3 — CPU Core & "Free-Run" Verification:** Solder Z80 socket, pull-up resistor network (SIP), bypass cap, and test point loops. *Test: Insert Z80, tie data bus to `0x00` (NOP instructions), observe A0-A15 counting on scope/logic analyzer.*
4. **Lab 4 — Address Decoding & Memory:** Solder 74HCT138/139 decoder sockets, SRAM socket, ROM universal socket, bypass caps, and `JP_ROM_TYPE` / `JP_ROM_WP` jumpers. *Test: Read test program from ROM, verify RAM read/write cycles.*
5. **Lab 5 — Parallel I/O (8255 PPI):** Solder 8255 socket, bypass cap, 26-pin (13×2) horizontal 2.54 mm pitch GPIO box header, and `JP_GPIO_PWR` jumper. *Test: Run simple LED chaser test on Port A, B, and C.*
6. **Lab 6 — Serial Subsystem (16550 & MAX232):** Solder 16550 UART socket, MAX232 socket, bypass caps, DE-9 male connector (with RX, TX, CTS, RTS, GND), TTL header with isolation resistors, and `JP_INT` jumper. *Test:*
   - *Part A (Polling):* Jumper removed; verify interactive console using polled status.
   - *Part B (Interrupts):* Install `JP_INT` jumper; verify interrupt service routine on incoming serial data.

---

## 7. Session Change Log
- **2026-11-19 (Session 1 - Kickoff):** Initialized project repository and established "comprehension before CAD" principle.
- **2026-11-19 (Session 1 - Architecture Defined):** Confirmed Z80 CPU, 32KB ROM / 32KB RAM, 8255 PPI, 16550 UART, 9-15V input with 7805 and 3mm green LED, reset supervisor, and 160x100mm Eurocard format.
- **2026-11-19 (Session 1 - Detailed Specifications Confirmed):**
  - Confirmed 4.0000 MHz TTL CPU oscillator and 1.8432 MHz TTL UART oscillator.
  - Confirmed 26-pin horizontal/right-angle box header for GPIO.
  - Confirmed omission of DIN 41612 connector (standalone benchtop board).
  - Confirmed universal 28-pin ROM socket configuration with 3-pin write-protect jumper.
  - Confirmed 74HCT138 I/O decode ($00-$03 for 8255, $08-$0F for 16550).
- **2026-11-19 (Session 1 - Final Clarifications Confirmed):**
  - Standard DE-9 male connector with DTE pinout.
  - Onboard power switch after barrel jack.
  - Dedicated scope test point loops for GND, +5V, clocks, reset, and bus strobes.
  - DS1813-5+ reset supervisor in TO-92 footprint.
  - Reset inversion stage (74HCT14) for active-HIGH 8255 and 16550 reset inputs.
- **2026-11-19 (Session 1 - Ergonomics, Sockets & Desk Mounting):**
  - Mandated comprehensive IC and oscillator socketing for beginner lab assembly protection.
  - Established physical layout: Left edge for Power/Switch/LED/Reset; Right/Top-Right edge for DE-9/GPIO/TTL.
  - Added 4-corner mounting holes for securing the board to a lab desk.
- **2026-11-19 (Session 1 - Power Integrity & Decoupling Mandates):**
  - Formulated strict decoupling rules: dedicated 100nF ceramic bypass capacitor placed as close to $V_{CC}$ as practical for every active device.
  - Formulated DC input protection: reverse Schottky diode, input bulk electrolytic cap, and input bypass ceramic cap.
  - Defined voltage regulator stability network: dedicated input and output capacitor pairs plus reverse discharge diode.
- **2026-11-19 (Session 1 - Orientation, Contention Protection, Trace Widths & Local Storage):**
  - Established horizontal PCB orientation (160×100 mm) with horizontal component placement and text for lab bench readability.
  - Added series isolation resistors between MAX232 and TTL serial header to prevent bus contention.
  - Formally specified Net Classes: **0.1000 mm** for signals, **0.3000 mm** (3×) for power nets.
  - Documented strict local retention policy: all datasheets and project files to remain locally in the working directory on the user's PC.
- **2026-11-19 (Session 1 - Polling Model, Logic Family & Pin Termination Rules):**
  - Mandated **74HCT (TTL-level CMOS)** logic family for all glue logic to guarantee voltage threshold compatibility with Z80/PPI/UART.
  - Formulated strict unused pin termination: all unused active-low inputs pulled HIGH via 10kΩ; all unused active-high inputs tied to GROUND (zero floating CMOS pins).
- **2026-11-19 (Session 1 - Interrupt Jumper Specification):**
  - Added 2-pin `JP_INT` jumper between inverted UART interrupt line and Z80 `/INT` pin.
  - Dedicated 10kΩ pull-up resistor directly on the Z80 `/INT` line ensures it remains safely pulled HIGH (non-floating) when the jumper is removed for polling-only labs.
- **2026-11-19 (Session 1 - Bussed Resistor Network Policy):**
  - Mandated common-bus SIP resistor networks (e.g., Bourns 4600X series) whenever $\ge 4$ common-value resistors share a common rail.
- **2026-11-19 (Session 1 - Toolchain & JLCPCB Fabrication Parameters):**
  - Documented KiCad 9 on Debian Linux 13 (amd64) and FreeRouting 2.5.0 DSN/SES workflow.
  - Specified JLCPCB fabrication: 1.6 mm thickness, FR-4 material, 1 oz copper, Green soldermask, White silkscreen (Front and Back).
  - Defined DRC rules and via dimensions (0.800 mm pad / 0.400 mm drill) matching FreeRouting and JLCPCB.
- **2026-11-19 (Session 1 - Schematic & PCB Drafting Quality Standards):**
  - Zero-overlap mandate for all schematic symbols, text, wires, footprints, pads, and silkscreen text.
  - 100% net labeling requirement across all schematic sheets.
  - Elimination of ambiguous 4-way cross-junctions; explicit staggered T-junctions only.
  - "Installed-Readability" mandate: Reference designator, component value, and orientation markings placed on `F.Silk` outside footprint outlines so components are identifiable even after sockets and ICs are installed.
- **2026-11-19 (Session 1 - Specification Validation & Audit Alignment):**
  - Resolved universal ROM socket ambiguity with separate `JP_ROM_TYPE` (27C256 vs 28C256) and `JP_ROM_WP` (Write-Protect) jumpers, preventing $A14$ short circuits.
  - Confirmed 5-wire RS-232 hardware handshaking (`RXD`, `TXD`, `CTS`, `RTS`, `GND`) with remaining DE-9 pins left open.
  - Formalized 26-pin (13×2) horizontal GPIO header pinout with odd/even Port A / Port B interleaving.
  - Confirmed TO-220 footprint heatsink clearance for safe 15V thermal dissipation.
- **2026-11-19 (Session 1 - Strict Metric Measurement Mandate):**
  - Mandated 100% Metric (SI: mm / µm) units across all board dimensions, trace widths, clearances, drills, component pitches (2.540 mm / 15.240 mm), and silkscreen text heights.
- **2026-11-19 (Session 1 - KiCad 9 Project Creation):**
  - Generated `edu_sbc.kicad_pro` with metric design rules, JLCPCB 2-layer constraints, FreeRouting DSN export configuration, and Net Classes (`Default`: 0.100 mm / `Signal`: 0.100 mm / `Power`: 0.300 mm, clearance 0.127 mm, via 0.800/0.400 mm).
- **2026-11-19 (Session 1 - Complete Schematic Capture):**
  - Created root schematic `edu_sbc.kicad_sch` with hierarchical architecture linking 5 distinct subsystem sheets matching the 6 lab sessions.
  - Implemented `01_Power_Clock.kicad_sch` (Barrel jack, reverse diode, bulk/bypass caps, 7805 regulator with heatsink, 3mm green LED, DS1813-5+ reset supervisor, 74HCT14 inverter, dual 4MHz/1.8432MHz TTL canned oscillators, test point loops TP1-TP6).
  - Implemented `02_CPU_Bus.kicad_sch` (Z80 CMOS CPU, SIP-6 10k resistor network pull-up, full address A0-A15 and data D0-D7 bus breakouts, test point loops TP7-TP13).
  - Implemented `03_Memory_Decode.kicad_sch` (74HCT139 memory decoder, 74HCT138 I/O decoder with /IORQ and /M1 gating, 28-pin universal ROM socket with JP_ROM_TYPE and JP_ROM_WP jumpers, 62256 32KB SRAM).
  - Implemented `04_Parallel_IO.kicad_sch` (82C55 PPI, 26-pin 13x2 horizontal box header with odd/even interleaved pinout, JP_GPIO_PWR jumper).
  - Implemented `05_Serial_UART.kicad_sch` (16C550 UART, MAX232, 5-wire DE-9 male connector with RTS/CTS, 6-pin TTL serial header, 1k contention isolation resistors, JP_INT interrupt jumper).
  - Enforced 100% net labeling, zero symbol/text overlap, and strict metric pin pitches across all sheets.
- **2026-11-19 (Session 1 - Hardware Ecosystem Alignment: DTECH Cable & QAWA Adapter):**
  - Explicitly mapped `J4` 6-pin TTL header to the DTECH FT232RL 6-pin female socket cable (Pin 1 GND, Pin 2 CTS, Pin 3 VCC, Pin 4 TXD, Pin 5 RXD, Pin 6 RTS).
  - Explicitly specified standard AC/DC wall adapter: Qualtek `QAWA-24-12-US01` (12V DC, 2A, 5.5mm OD x 2.1mm ID center-positive barrel plug, DigiKey MPN QAWA-24-12-US01).
- **2026-11-19 (Session 1 - Exclusive Serial Port Mandate & Silkscreen Warning):**
  - Mandated prominent schematic text and PCB silkscreen warning at `J3` / `J4`: operator can connect either TTL Serial or DE-9 RS-232, but never both simultaneously. Silkscreen text: `CONNECT EITHER TTL (J4) OR RS-232 (J3) - NOT BOTH`.
- **2026-11-19 (Session 1 - Power Slide Switch Option B & Custom Footprint Creation):**
  - Confirmed Option B (Power slide switch rated 1A–4A @ 28VDC/125VAC).
  - Created local KiCad footprint library `edu_sbc.pretty` and configured `fp-lib-table`.
  - Created custom footprint `SW_Slide_Power_SPDT.kicad_mod` (4.70 mm terminal pitch, 1.30 mm drills, 2.40 mm pads with 0.55 mm annular ring, with explicit `ON` and `OFF` silkscreen legends placed outside the body).
- **2026-11-19 (Session 1 - Complete Bill of Materials Generation):**
  - Created detailed Markdown Bill of Materials `BOM.md` grouped across the 6 lab sessions with exact manufacturer part numbers, descriptions, footprints, and DigiKey order codes.
  - Created uploadable spreadsheet `BOM.csv` for bulk ordering on DigiKey or Mouser.
  - Sockets provided for 100% of ICs and oscillators; hardware flow control and wall adapter accessories included.
- **2026-11-19 (Session 1 - KiCad 9 PCB Layout Initialization):**
  - Created `edu_sbc.kicad_pcb` with standard Eurocard 160.000 mm × 100.000 mm board boundary on `Edge.Cuts`.
  - Placed 4 corner M3 mounting holes (3.2 mm drill, 6.5 mm keepout) at 5.0 mm edge offsets.
  - Established physical floorplan: Left edge Power & Reset zone, Center Computing/Memory core (horizontal DIP IC orientation), Right edge I/O zone (26-pin horizontal GPIO, DE-9 male, 6-pin horizontal TTL).
  - Placed prominent warning silkscreen at J3/J4: `CONNECT EITHER TTL (J4) OR RS-232 (J3) - DO NOT CONNECT BOTH`.
  - Configured 2-layer JLCPCB stackup (1.6 mm FR-4, 35 µm copper, green soldermask, white silkscreen) and Net Classes (0.100 mm signal, 0.300 mm power, 0.127 mm clearance, 0.8/0.4 mm vias).
- **2026-11-19 (Session 1 - KiCad 9 Built-In Symbol & Footprint Policy):**
  - Established strict priority for built-in KiCad 9 symbols and footprints across all ICs, DIP sockets, passives, connectors, and test points.
  - Mandated explicit notification for any part requiring custom assets; identified Power Switch (`SW1`) as the candidate for custom footprint evaluation.
- **2026-11-21 (Target Environment Clarification & BOM Serial Cable Addition):**
  - Clarified target PC host environment specification to Debian Linux 13 (amd64) (correcting previous typo of Debian Linux 9).
  - Added DTECH 10 ft USB to RS232 DB9 Female FTDI serial adapter cable to `BOM.md` and `BOM.csv` (Item `EXT_RS232`) for direct PC connection to the DE-9 RS-232 serial interface (`J3`).
- **2026-11-21 (Schematic ERC Cleanup & Zero-Violation Compliance):**
  - Resolved all 9 ERC violations in `01_Power_Clock.kicad_sch`: removed dangling unconnected wire stubs and isolated internal power labels.
  - Retained all primary global labels (`+5V`, `GND`, `nRESET`, `RESET_HIGH`, `UART_INTR`, `nUART_INT`, `CPU_CLK`, `UART_CLK`) and architectural notes.
  - Re-ran KiCad 9 ERC check across the full hierarchical schematic: verified 0 errors, 0 warnings.
- **2026-11-22 (KiCad 9 Schematic Syntax Fixes, 06_Parallel_IO Sheet & v4 Bundle):**
  - Diagnosed KiCad 9 schematic load errors in `edu_sbc_schematics.zip`: literal newline characters inside double-quoted `(text "...")` blocks in `edu_sbc.kicad_sch` (lines 195–196) and `01_Power_Clock.kicad_sch` (lines 3380–3383), and invalid `(shape power_in)` / `(shape power_out)` tokens on `(global_label ...)` elements across sub-sheets (which only accept `input`, `output`, `bidirectional`, `tri_state`, or `passive`).
  - Patched quoted multi-line strings to escaped `\n` sequences and normalized `(global_label ...)` shapes across the schematics while preserving symbol pins, wires, and layout alignment.
  - Added the missing Parallel I/O schematic `06_Parallel_IO.kicad_sch` (`U9` `CP82C55A-5Z` DIP-40, `C15` 100nF bypass capacitor, `J2` 26-pin horizontal box header with odd/even `PA0`–`PA7` / `PB0`–`PB7` interleaving, `PC0`–`PC7`, `GND`, and `GPIO_PWR_SELECT`, `JP3` 1x3 power jumper, and `PWR_FLAG` symbols on `+5V` and `GND`) and registered it as a hierarchical sheet in `edu_sbc.kicad_sch` clear of the A3 title block.
  - Documented external accessories (`Qualtek QAWA-24-12-US01`, `DTECH DT-9008`, `DTECH DT-5002A`) as schematic/PCB notes rather than BOM schematic symbols, and packaged the 7-file schematic set into `edu_sbc_schematics-v4.zip`.
- **2026-11-22 (PCB Layout Load Error Fix in `edu_sbc.kicad_pcb`):**
  - Diagnosed KiCad 9 PCB load error (`Expecting '(' in .../edu_sbc.kicad_pcb, line 236, offset 2`) caused by unsupported `;;` comment lines in `.kicad_pcb` S-expression syntax.
  - Removed the `;;` comment lines in `edu_sbc.kicad_pcb` and updated `TO_DO.md` to mark PCB layout load errors resolved.