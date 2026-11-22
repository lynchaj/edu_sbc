# Educational Z80 Single Board Computer (EduSBC) — Bill of Materials (BOM)

> **Revision:** 1.1  
> **Date:** 2026-11-22  
> **Target PCB:** Standard Eurocard (160.0 mm × 100.0 mm), 2-Layer, 1.6 mm thickness, Green Soldermask, White Silkscreen  
> **EDA Tool:** KiCad 9 (Debian Linux 13 amd64)  
> **Repository:** https://github.com/lynchaj/edu_sbc  
> **Distributor Part Numbers:** DigiKey / Mouser / Newark

---

## 1. Summary by Component Category

| Category | Total Quantity | Notes |
| :--- | :---: | :--- |
| **Integrated Circuits (Active DIP)** | 8 | Z80 CPU, 82C55 PPI, 16C550 UART, 28C256 EEPROM, AS6C62256 SRAM, 74HCT138, 74HCT139, 74HCT14 |
| **Interface & Transceiver ICs** | 2 | MAX232 RS-232 Transceiver, DS1813-5+ Reset Supervisor |
| **Voltage Regulator & Thermal** | 2 | LM7805 TO-220 +5V Regulator + Aavid Stamped Heatsink (`HS1`) |
| **Canned TTL Oscillators** | 2 | 4.0000 MHz (`X1`) and 1.8432 MHz (`X2`) in DIP-8 packages |
| **DIP IC Sockets (100% Socketed)** | 10 | 3x DIP-40 (15.24 mm), 2x DIP-28 (15.24 mm), 2x DIP-16 (7.62 mm), 1x DIP-14 (7.62 mm), 2x DIP-8 (7.62 mm) |
| **Connectors & Headers** | 4 | 5.5×2.1 mm DC Barrel Jack (`J1`), 26-pin Horizontal Box Header (`J2`), DE-9 Male Right-Angle (`J3`), 6-pin TTL Header (`J4`) |
| **Switches** | 2 | SPDT Power Slide Switch 4A rated (`SW1`), 6 mm Tactile Reset Pushbutton (`SW2`) |
| **Jumpers & Shunts** | 4 | 3x 3-pin Jumpers (`JP1`–`JP3`: ROM Type, ROM WP, GPIO Power) + 1x 2-pin Jumper (`JP4`: Interrupt Enable) + 4x Shunts |
| **Diodes & LEDs** | 3 | 1N5819 Schottky (`D1`), 1N4001 Silicon Diode (`D2`), 3.0 mm Green Power LED (`D3`) |
| **Resistors & Arrays** | 4 | 1x 10kΩ×5 Bussed SIP-6 Network (`RN1`), 3x 1.0kΩ 1/4W Axial Resistors (`R1`–`R3`) |
| **Capacitors** | 21 | 2x Electrolytic Bulk (`C1`: 220µF, `C4`: 47µF) + 19x Ceramic 50V (`C3`: 1x 0.33µF; `C2,C5–C21`: 18x 0.1µF) |
| **Test Point Loops** | 13 | Keystone Miniature Through-Hole Test Point Loops (`TP1`–`TP13`) |
| **Mechanical & Mounting Hardware** | 4 | 4x M3 × 10 mm Brass Standoffs + Screws + Rubber Feet (`H1`–`H4`) |
| **External Lab Accessories** | 3 | Qualtek `QAWA-24-12-US01` 12V 2A Wall Adapter, DTECH `DT-9008` FT232RL 6-Pin TTL Cable, DTECH `DT-5002A` USB-to-RS232 DB9 Female Cable *(Not placed as schematic symbols; noted on schematic/PCB)* |

---

## 2. Detailed Bill of Materials Grouped by Lab Subsystem

### Subsystem 1: Power Supply, Protection & Indicators (Lab 1)
| Item | Ref Des | Qty | Value / Part | Description | Symbol Library / Symbol | Package / Footprint | Manufacturer | Manufacturer Part Number (MPN) | DigiKey | Mouser | Newark | Datasheet |
| :---: | :--- | :---: | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | `J1` | 1 | 5.5 × 2.1 mm | DC Power Barrel Jack, Center-Positive, Horizontal Through-Hole | `Connector:Barrel_Jack_Switch` | `Connector_BarrelJack:BarrelJack_Horizontal` | CUI Devices | PJ-102AH | CP-102AH-ND | 490-PJ-102AH | 16M4548 | [PDF](Datasheets/CUI_PJ-102AH.pdf) |
| 2 | `SW1` | 1 | SPDT 4A 28V | Power Slide Switch, SPDT, 4.70 mm Pitch, Rated 4A @ 28VDC | `Switch:SW_SPDT` | `edu_sbc:SW_Slide_Power_SPDT` | C&K / Littelfuse | V80212S205QE | CKN9127-ND | 611-V80212S205QE | 97K2284 | — |
| 3 | `D1` | 1 | 1N5819 | Schottky Barrier Diode, 40V 1A, Reverse Voltage Protection | `Diode:1N5819` | `Diode_THT:D_DO-41_SOD81_P10.16mm_Horizontal` | Vishay | 1N5819-E3/54 | 1N5819-E3/54GI-ND | 625-1N5819-E3/54 | 57AC8766 | [PDF](Datasheets/Vishay_1N5819.pdf) |
| 4 | `C1` | 1 | 220 µF 25V | Aluminum Electrolytic Bulk Input Capacitor, Low ESR, Radial | `Device:C_Polarized` | `Capacitor_THT:CP_Radial_D8.0mm_P3.50mm` | Nichicon | UVR1E221MED | 493-1082-ND | 647-UVR1E221MED | 48AC4862 | — |
| 5 | `C2` | 1 | 100 nF 50V | Monolithic Ceramic Input Bypass Capacitor, X7R, 5.0 mm Pitch | `Device:C` | `Capacitor_THT:C_Disc_D5.0mm_W2.5mm_P5.00mm` | KEMET | C320C104K5R5TA | 399-4467-1-ND | 80-C320C104K5R | 89K8172 | — |
| 6 | `U1` | 1 | LM7805 | Linear Voltage Regulator +5V 1.5A, TO-220 | `Regulator_Linear:LM7805_TO220` | `Package_TO_SOT_THT:TO-220-3_Vertical` | STMicroelectronics | L7805CV | 497-1443-5-ND | 511-L7805CV | 89K1171 | [PDF](Datasheets/ST_L78_Series_Voltage_Regulators.pdf) |
| 7 | `HS1` | 1 | Heatsink TO-220 | Stamped Aluminum Heatsink, Bolt-On, Thermal Resist 13.2°C/W | `Mechanical:Heatsink` | `Heatsink:Heatsink_Aavid_577202B00000G_TO-220_Vertical` | Aavid / Boyd | 577202B00000G | 577202B00000G-ND | 532-577202B00G | 34M6519 | [PDF](Datasheets/Aavid_Boyd_5770_TO220_Heatsink.pdf) |
| 8 | `C3` | 1 | 0.33 µF 50V | Monolithic Ceramic Regulator Input Stability Cap, 5.0 mm Pitch | `Device:C` | `Capacitor_THT:C_Disc_D5.0mm_W2.5mm_P5.00mm` | KEMET | C320C334K5R5TA | 399-4482-1-ND | 80-C320C334K5R | 89K8178 | — |
| 9 | `C4` | 1 | 47 µF 16V | Aluminum Electrolytic Output Bulk Capacitor, Radial | `Device:C_Polarized` | `Capacitor_THT:CP_Radial_D6.3mm_P2.50mm` | Nichicon | UVR1C470MDD | 493-1051-ND | 647-UVR1C470MDD | 74R3966 | — |
| 10 | `C5` | 1 | 100 nF 50V | Monolithic Ceramic Regulator Output Bypass Cap, 5.0 mm Pitch | `Device:C` | `Capacitor_THT:C_Disc_D5.0mm_W2.5mm_P5.00mm` | KEMET | C320C104K5R5TA | 399-4467-1-ND | 80-C320C104K5R | 89K8172 | — |
| 11 | `D2` | 1 | 1N4001 | Standard Silicon Rectifier Diode 50V 1A (Reverse Discharge) | `Diode:1N4001` | `Diode_THT:D_DO-41_SOD81_P10.16mm_Horizontal` | Vishay | 1N4001-E3/54 | 1N4001-E3/54GI-ND | 625-1N4001-E3 | 57AC8749 | [PDF](Datasheets/Vishay_1N4001.pdf) |
| 12 | `R1` | 1 | 1.0 kΩ | Metal Film Resistor 1/4W 1% Axial (Power LED Limiter) | `Device:R` | `Resistor_THT:R_Axial_DIN0207_L6.3mm_D2.5mm_P10.16mm_Horizontal` | Yageo | MFR-25FTE52-1K | 1.0KXBK-ND | 603-MFR-25FTE52-1K | 38K0344 | — |
| 13 | `D3` | 1 | Green LED | 3.0 mm (T-1) Through-Hole Green Diffused Power LED | `Device:LED` | `LED_THT:LED_D3.0mm` | Kingbright | WP7104GD | 754-1264-ND | 604-WP7104GD | 83K4724 | [PDF](Datasheets/Kingbright_WP7104GD_Green_LED.pdf) |
| 14 | `EXT_PWR` | 1 | QAWA-24-12-US01 | 12V DC 2A Wall Adapter, 5.5×2.1 mm Center-Positive Barrel Plug | *N/A — External Accessory (Schematic/PCB Note)* | External Wall Adapter | Qualtek | QAWA-24-12-US01 | QAWA-24-12-US01-ND | 702-QAWA-24-12-US01 | 28AC7789 | [PDF](Datasheets/QAWA-24%20Series.pdf) |

---

### Subsystem 2: Reset Supervisor, Inverter & Dual Clocks (Lab 2)
| Item | Ref Des | Qty | Value / Part | Description | Symbol Library / Symbol | Package / Footprint | Manufacturer | Manufacturer Part Number (MPN) | DigiKey | Mouser | Newark | Datasheet |
| :---: | :--- | :---: | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 15 | `U2` | 1 | DS1813-5+ | 5V Voltage Reset Supervisor with Pushbutton Debounce, Active-Low | `Power_Supervisor:DS1813` | `Package_TO_SOT_THT:TO-92_Inline` | Analog Devices / Maxim | DS1813-5+ | DS1813-5+-ND | 700-DS1813-5 | 33C8403 | [PDF](Datasheets/Dallas_Maxim_DS1813_Reset_Supervisor.pdf) |
| 16 | `SW2` | 1 | Tactile Switch | 6.0 mm × 6.0 mm Momentary Tactile Pushbutton Switch, 5 mm Height | `Switch:SW_Push` | `Button_Switch_THT:SW_PUSH_6mm_H5mm` | Omron | B3F-1000 | SW400-ND | 653-B3F-1000 | 36K8703 | [PDF](Datasheets/Omron_B3F_Tactile_Switch.pdf) |
| 17 | `C6` | 1 | 100 nF 50V | Monolithic Ceramic Bypass Capacitor for U2 | `Device:C` | `Capacitor_THT:C_Disc_D5.0mm_W2.5mm_P5.00mm` | KEMET | C320C104K5R5TA | 399-4467-1-ND | 80-C320C104K5R | 89K8172 | — |
| 18 | `U3` | 1 | 74HCT14 | Hex Schmitt Trigger Inverter, High-Speed CMOS with TTL Inputs | `74xx:74HCT14` | `Package_DIP:DIP-14_W7.62mm` | Texas Instruments | SN74HCT14N | 296-1600-5-ND | 595-SN74HCT14N | 08F9083 | [PDF](Datasheets/TI_SN74HCT14.pdf) |
| 19 | `SKT_U3` | 1 | DIP-14 Socket | 14-Pin DIP IC Socket, Dual-Leaf Phosphor Bronze Contacts | *Mechanical / Socket (Combined w/ U3)* | `Package_DIP:DIP-14_W7.62mm` | TE Connectivity | 1-2199298-2 | A100188-ND | 571-1-2199298-2 | 16M4542 | — |
| 20 | `C7` | 1 | 100 nF 50V | Monolithic Ceramic Bypass Capacitor for U3 (Pin 14) | `Device:C` | `Capacitor_THT:C_Disc_D5.0mm_W2.5mm_P5.00mm` | KEMET | C320C104K5R5TA | 399-4467-1-ND | 80-C320C104K5R | 89K8172 | — |
| 21 | `X1` | 1 | 4.0000 MHz | Canned TTL Crystal Oscillator, 5V, Half/Full Size DIP-8 | `Oscillator:XO_DIP8` | `Oscillator:Oscillator_DIP-8` | ECS Inc. | ECS-2100AX-040 | XC1088-ND | 520-2100AX-040 | 41K2842 | [PDF](Datasheets/ECS_2100_Oscillator.pdf) |
| 22 | `SKT_X1` | 1 | DIP-8 Socket | 8-Pin DIP Socket for Oscillator X1 | *Mechanical / Socket (Combined w/ X1)* | `Package_DIP:DIP-8_W7.62mm` | TE Connectivity | 1-2199298-1 | A100187-ND | 571-1-2199298-1 | 16M4541 | — |
| 23 | `C8` | 1 | 100 nF 50V | Monolithic Ceramic Bypass Capacitor for X1 | `Device:C` | `Capacitor_THT:C_Disc_D5.0mm_W2.5mm_P5.00mm` | KEMET | C320C104K5R5TA | 399-4467-1-ND | 80-C320C104K5R | 89K8172 | — |
| 24 | `X2` | 1 | 1.8432 MHz | Canned TTL Crystal Oscillator, 5V, Half/Full Size DIP-8 (Baud Clock) | `Oscillator:XO_DIP8` | `Oscillator:Oscillator_DIP-8` | ECS Inc. | ECS-2100AX-018 | XC1086-ND | 520-2100AX-018 | 41K2841 | [PDF](Datasheets/ECS_2100_Oscillator.pdf) |
| 25 | `SKT_X2` | 1 | DIP-8 Socket | 8-Pin DIP Socket for Oscillator X2 | *Mechanical / Socket (Combined w/ X2)* | `Package_DIP:DIP-8_W7.62mm` | TE Connectivity | 1-2199298-1 | A100187-ND | 571-1-2199298-1 | 16M4541 | — |
| 26 | `C9` | 1 | 100 nF 50V | Monolithic Ceramic Bypass Capacitor for X2 | `Device:C` | `Capacitor_THT:C_Disc_D5.0mm_W2.5mm_P5.00mm` | KEMET | C320C104K5R5TA | 399-4467-1-ND | 80-C320C104K5R | 89K8172 | — |
| 27 | `TP1`..`TP6` | 6 | Keystone Loop | Miniature Through-Hole Scope Test Point Loops (GND, +5V, CLKs, Reset) | `Connector:TestPoint` | `TestPoint:TestPoint_Loop_D2.54mm_Drill1.0mm` | Keystone Electronics | 5000 (Black), 5001 (Red), 5002 (White) | 36-5000-ND / 36-5001-ND | 534-5000 | 36M7320 | — |

---

### Subsystem 3: CPU Core & Bus Control (Lab 3)
| Item | Ref Des | Qty | Value / Part | Description | Symbol Library / Symbol | Package / Footprint | Manufacturer | Manufacturer Part Number (MPN) | DigiKey | Mouser | Newark | Datasheet |
| :---: | :--- | :---: | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 28 | `U4` | 1 | Z84C0004PEG | Z80 CMOS Microprocessor 4.0 MHz, 8-bit, 40-Pin DIP | `CPU:Z80CPU` | `Package_DIP:DIP-40_W15.24mm` | Zilog / Littelfuse | Z84C0004PEG | 269-4674-ND | 692-Z84C0004PEG | 87F8548 | [PDF](Datasheets/Zilog_Z84C00_Product_Specification.pdf) |
| 29 | `SKT_U4` | 1 | DIP-40 Socket | 40-Pin DIP IC Socket, 15.24 mm Row Spacing, Machined/Stamped | *Mechanical / Socket (Combined w/ U4)* | `Package_DIP:DIP-40_W15.24mm` | TE Connectivity | 1-2199298-6 | A100192-ND | 571-1-2199298-6 | 16M4546 | — |
| 30 | `C10` | 1 | 100 nF 50V | Monolithic Ceramic Bypass Capacitor for U4 (Pin 11) | `Device:C` | `Capacitor_THT:C_Disc_D5.0mm_W2.5mm_P5.00mm` | KEMET | C320C104K5R5TA | 399-4467-1-ND | 80-C320C104K5R | 89K8172 | — |
| 31 | `RN1` | 1 | 10 kΩ × 5 | Bussed Resistor Network (SIP-6, 2.54 mm pitch), Pin 1 Common VCC | `Device:R_Network05` | `Resistor_THT:R_Array_SIP6` | Bourns | 4606X-101-103LF | 4606X-101-103LF-ND | 652-4606X-1LF-10K | 14M0660 | [PDF](Datasheets/Bourns_4600X.pdf) |
| 32 | `TP7`..`TP13` | 7 | Keystone Loop | Scope Test Point Loops (nMREQ, nIORQ, nRD, nWR, nM1, nWAIT, nINT) | `Connector:TestPoint` | `TestPoint:TestPoint_Loop_D2.54mm_Drill1.0mm` | Keystone Electronics | 5003 (Orange) / 5002 (White) | 36-5003-ND | 534-5003 | 36M7323 | — |

---

### Subsystem 4: Address Decoding & Memory Subsystem (Lab 4)
| Item | Ref Des | Qty | Value / Part | Description | Symbol Library / Symbol | Package / Footprint | Manufacturer | Manufacturer Part Number (MPN) | DigiKey | Mouser | Newark | Datasheet |
| :---: | :--- | :---: | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 33 | `U5` | 1 | 74HCT139 | Dual 2-to-4 Line Decoder, High-Speed CMOS with TTL Inputs | `74xx:74LS139` *(or `74xx:74HCT139`)* | `Package_DIP:DIP-16_W7.62mm` | Texas Instruments | SN74HCT139N | 296-1603-5-ND | 595-SN74HCT139N | 08F9082 | [PDF](Datasheets/TI_SN74HCT139.pdf) |
| 34 | `SKT_U5` | 1 | DIP-16 Socket | 16-Pin DIP IC Socket, 7.62 mm Row Spacing | *Mechanical / Socket (Combined w/ U5)* | `Package_DIP:DIP-16_W7.62mm` | TE Connectivity | 1-2199298-3 | A100189-ND | 571-1-2199298-3 | 16M4543 | — |
| 35 | `C11` | 1 | 100 nF 50V | Monolithic Ceramic Bypass Capacitor for U5 (Pin 16) | `Device:C` | `Capacitor_THT:C_Disc_D5.0mm_W2.5mm_P5.00mm` | KEMET | C320C104K5R5TA | 399-4467-1-ND | 80-C320C104K5R | 89K8172 | — |
| 36 | `U6` | 1 | 74HCT138 | 3-to-8 Line Decoder / Demultiplexer, High-Speed CMOS TTL Inputs | `74xx:74LS138` *(or `74xx:74HCT138`)* | `Package_DIP:DIP-16_W7.62mm` | Texas Instruments | SN74HCT138N | 296-1602-5-ND | 595-SN74HCT138N | 08F9081 | [PDF](Datasheets/TI_SN74HCT138.pdf) |
| 37 | `SKT_U6` | 1 | DIP-16 Socket | 16-Pin DIP IC Socket, 7.62 mm Row Spacing | *Mechanical / Socket (Combined w/ U6)* | `Package_DIP:DIP-16_W7.62mm` | TE Connectivity | 1-2199298-3 | A100189-ND | 571-1-2199298-3 | 16M4543 | — |
| 38 | `C12` | 1 | 100 nF 50V | Monolithic Ceramic Bypass Capacitor for U6 (Pin 16) | `Device:C` | `Capacitor_THT:C_Disc_D5.0mm_W2.5mm_P5.00mm` | KEMET | C320C104K5R5TA | 399-4467-1-ND | 80-C320C104K5R | 89K8172 | — |
| 39 | `U7` | 1 | AT28C256-15PU | 32KB (256Kbit) Parallel EEPROM 150ns, 28-Pin DIP (or 27C256 EPROM) | `Memory_EEPROM:28C256` | `Package_DIP:DIP-28_W15.24mm` | Microchip Technology | AT28C256-15PU | AT28C256-15PU-ND | 556-AT28C256-15PU | 68K3217 | [PDF](Datasheets/Microchip_AT28C256.pdf) |
| 40 | `SKT_U7` | 1 | DIP-28 Socket | 28-Pin Universal ROM Socket, 15.24 mm Row Spacing | *Mechanical / Socket (Combined w/ U7)* | `Package_DIP:DIP-28_W15.24mm` | TE Connectivity | 1-2199298-5 | A100191-ND | 571-1-2199298-5 | 16M4545 | — |
| 41 | `C13` | 1 | 100 nF 50V | Monolithic Ceramic Bypass Capacitor for U7 (Pin 28) | `Device:C` | `Capacitor_THT:C_Disc_D5.0mm_W2.5mm_P5.00mm` | KEMET | C320C104K5R5TA | 399-4467-1-ND | 80-C320C104K5R | 89K8172 | — |
| 42 | `JP1` | 1 | JP_ROM_TYPE | 3-Pin 1×3 2.54 mm Vertical Pin Header + Gold Shorting Jumper | `Connector_Generic:Conn_01x03` *(or `Jumper:Jumper_3_Bridged12`)* | `Connector_PinHeader_2.54mm:PinHeader_1x03_P2.54mm_Vertical` | Würth Elektronik | 61300311121 + 60900213410 | 732-5316-ND / 732-2259-ND | 710-61300311121 | 48P9813 | — |
| 43 | `JP2` | 1 | JP_ROM_WP | 3-Pin 1×3 2.54 mm Vertical Pin Header + Gold Shorting Jumper | `Connector_Generic:Conn_01x03` *(or `Jumper:Jumper_3_Bridged12`)* | `Connector_PinHeader_2.54mm:PinHeader_1x03_P2.54mm_Vertical` | Würth Elektronik | 61300311121 + 60900213410 | 732-5316-ND / 732-2259-ND | 710-61300311121 | 48P9813 | — |
| 44 | `U8` | 1 | AS6C62256-55PCN | 32KB (256Kbit) Low Power Static RAM 55ns, 28-Pin DIP | `Memory_RAM:AS6C62256` *(or `Memory_RAM:62256`)* | `Package_DIP:DIP-28_W15.24mm` | Alliance Memory | AS6C62256-55PCN | 1450-1196-ND | 913-AS6C62256-55PCN | 88W2822 | [PDF](Datasheets/Alliance_AS6C62256_SRAM.pdf) |
| 45 | `SKT_U8` | 1 | DIP-28 Socket | 28-Pin DIP IC Socket, 15.24 mm Row Spacing | *Mechanical / Socket (Combined w/ U8)* | `Package_DIP:DIP-28_W15.24mm` | TE Connectivity | 1-2199298-5 | A100191-ND | 571-1-2199298-5 | 16M4545 | — |
| 46 | `C14` | 1 | 100 nF 50V | Monolithic Ceramic Bypass Capacitor for U8 (Pin 28) | `Device:C` | `Capacitor_THT:C_Disc_D5.0mm_W2.5mm_P5.00mm` | KEMET | C320C104K5R5TA | 399-4467-1-ND | 80-C320C104K5R | 89K8172 | — |

---

### Subsystem 5: Parallel I/O Subsystem (Lab 5)
| Item | Ref Des | Qty | Value / Part | Description | Symbol Library / Symbol | Package / Footprint | Manufacturer | Manufacturer Part Number (MPN) | DigiKey | Mouser | Newark | Datasheet |
| :---: | :--- | :---: | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 47 | `U9` | 1 | CP82C55A-5Z | CMOS Programmable Peripheral Interface (PPI) 24 I/O Lines, 40-Pin DIP | `Interface:8255` *(or `Interface:82C55A`)* | `Package_DIP:DIP-40_W15.24mm` | Renesas / Intersil | CP82C55A-5Z | CP82C55A-5Z-ND | 968-CP82C55A-5Z | 34C1518 | [PDF](Datasheets/82C55A.pdf) |
| 48 | `SKT_U9` | 1 | DIP-40 Socket | 40-Pin DIP IC Socket, 15.24 mm Row Spacing | *Mechanical / Socket (Combined w/ U9)* | `Package_DIP:DIP-40_W15.24mm` | TE Connectivity | 1-2199298-6 | A100192-ND | 571-1-2199298-6 | 16M4546 | — |
| 49 | `C15` | 1 | 100 nF 50V | Monolithic Ceramic Bypass Capacitor for U9 (Pin 26) | `Device:C` | `Capacitor_THT:C_Disc_D5.0mm_W2.5mm_P5.00mm` | KEMET | C320C104K5R5TA | 399-4467-1-ND | 80-C320C104K5R | 89K8172 | — |
| 50 | `J2` | 1 | 26-Pin Box Header | 26-Pin (13×2) 2.54 mm Horizontal Right-Angle Shrouded Box Header | `Connector_Generic:Conn_02x13_Odd_Even` | `Connector_IDC:IDC-Header_2x13_P2.54mm_Horizontal` | Amphenol / FCI | 71600-026LF | 609-3252-ND | 649-71600-026LF | 24M1629 | — |
| 51 | `JP3` | 1 | JP_GPIO_PWR | 3-Pin 1×3 2.54 mm Vertical Pin Header + Gold Shorting Jumper | `Connector_Generic:Conn_01x03` *(or `Jumper:Jumper_3_Bridged12`)* | `Connector_PinHeader_2.54mm:PinHeader_1x03_P2.54mm_Vertical` | Würth Elektronik | 61300311121 + 60900213410 | 732-5316-ND / 732-2259-ND | 710-61300311121 | 48P9813 | — |

---

### Subsystem 6: Serial Communications Subsystem (Lab 6)
| Item | Ref Des | Qty | Value / Part | Description | Symbol Library / Symbol | Package / Footprint | Manufacturer | Manufacturer Part Number (MPN) | DigiKey | Mouser | Newark | Datasheet |
| :---: | :--- | :---: | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 52 | `U10` | 1 | TL16C550CN | Asynchronous Communications Element (UART) with 16-Byte FIFO, 40-Pin DIP | `Interface_UART:TL16C550C` *(or `16550`)* | `Package_DIP:DIP-40_W15.24mm` | Texas Instruments | TL16C550CN | 296-1798-5-ND | 595-TL16C550CN | 08F7965 | [PDF](Datasheets/TI_TL16C550C.pdf) |
| 53 | `SKT_U10` | 1 | DIP-40 Socket | 40-Pin DIP IC Socket, 15.24 mm Row Spacing | *Mechanical / Socket (Combined w/ U10)* | `Package_DIP:DIP-40_W15.24mm` | TE Connectivity | 1-2199298-6 | A100192-ND | 571-1-2199298-6 | 16M4546 | — |
| 54 | `C16` | 1 | 100 nF 50V | Monolithic Ceramic Bypass Capacitor for U10 (Pin 40) | `Device:C` | `Capacitor_THT:C_Disc_D5.0mm_W2.5mm_P5.00mm` | KEMET | C320C104K5R5TA | 399-4467-1-ND | 80-C320C104K5R | 89K8172 | — |
| 55 | `U11` | 1 | MAX232N | Dual RS-232 Driver/Receiver with ±10V Charge Pump, 16-Pin DIP | `Interface_UART:MAX232` | `Package_DIP:DIP-16_W7.62mm` | Texas Instruments | MAX232N | 296-1402-5-ND | 595-MAX232N | 08F7925 | [PDF](Datasheets/TI_MAX232.pdf) |
| 56 | `SKT_U11` | 1 | DIP-16 Socket | 16-Pin DIP IC Socket, 7.62 mm Row Spacing | *Mechanical / Socket (Combined w/ U11)* | `Package_DIP:DIP-16_W7.62mm` | TE Connectivity | 1-2199298-3 | A100189-ND | 571-1-2199298-3 | 16M4543 | — |
| 57 | `C17` | 1 | 100 nF 50V | Monolithic Ceramic Bypass Capacitor for U11 (Pin 16) | `Device:C` | `Capacitor_THT:C_Disc_D5.0mm_W2.5mm_P5.00mm` | KEMET | C320C104K5R5TA | 399-4467-1-ND | 80-C320C104K5R | 89K8172 | — |
| 58 | `C18`..`C21` | 4 | 100 nF 50V | Monolithic Ceramic Charge Pump Capacitors for MAX232 | `Device:C` | `Capacitor_THT:C_Disc_D5.0mm_W2.5mm_P5.00mm` | KEMET | C320C104K5R5TA | 399-4467-1-ND | 80-C320C104K5R | 89K8172 | — |
| 59 | `R2`, `R3` | 2 | 1.0 kΩ | Metal Film Resistors 1/4W 1% (Serial Receiver Contention Isolation) | `Device:R` | `Resistor_THT:R_Axial_DIN0207_L6.3mm_D2.5mm_P10.16mm_Horizontal` | Yageo | MFR-25FTE52-1K | 1.0KXBK-ND | 603-MFR-25FTE52-1K | 38K0344 | — |
| 60 | `J3` | 1 | DE-9 Male R/A | D-Sub 9-Pin Male Right-Angle PCB Connector, DTE Pinout, Boardlocks | `Connector:DB9_Male` | `Connector_Dsub:DSUB-9_Male_Horizontal_P2.77x2.84mm_EdgePinOffset9.90mm_Housed_MountingHolesOffset11.32mm` | NorComp | 172-E09-103R001 | 172-E09-103R001-ND | 585-172-E09-103R001 | 87F6675 | [PDF](Datasheets/NorComp_172-E_Series_DE9.pdf) |
| 61 | `J4` | 1 | 6-Pin TTL R/A | 6-Pin 1×6 2.54 mm Horizontal Right-Angle Male Header (DTECH Mating) | `Connector_Generic:Conn_01x06` | `Connector_PinHeader_2.54mm:PinHeader_1x06_P2.54mm_Horizontal` | Würth Elektronik | 61300611021 | 732-5327-ND | 710-61300611021 | 48P9816 | — |
| 62 | `JP4` | 1 | JP_INT | 2-Pin 1×2 2.54 mm Vertical Pin Header + Gold Shorting Jumper | `Connector_Generic:Conn_01x02` *(or `Jumper:Jumper_2_Open`)* | `Connector_PinHeader_2.54mm:PinHeader_1x02_P2.54mm_Vertical` | Würth Elektronik | 61300211121 + 60900213410 | 732-5315-ND / 732-2259-ND | 710-61300211121 | 48P9812 | — |
| 63 | `EXT_SER` | 1 | DTECH DT-9008 | USB to TTL 5V Serial Cable with FTDI FT232RL Chip (6-Pin Female Socket) | *N/A — External Accessory (Schematic/PCB Note)* | External USB-to-TTL Serial Cable (Amazon / eBay) | DTECH | DT-9008 | Available on Amazon/eBay | Non-Catalog | Non-Catalog | [PDF](Datasheets/DS_TTL-232R_CABLES.pdf) |
| 64 | `EXT_RS232` | 1 | DTECH DT-5002A | USB to RS-232 DB9 Female Serial Adapter Cable, 10 ft, FTDI Chipset | *N/A — External Accessory (Schematic/PCB Note)* | External USB-to-RS232 DB9 Female Cable (Amazon / eBay) | DTECH | DT-5002A | Available on Amazon/eBay | Non-Catalog | Non-Catalog | — |

---

### Mechanical & Workstation Mounting Hardware
| Item | Ref Des | Qty | Value / Part | Description | Symbol Library / Symbol | Package / Footprint | Manufacturer | Manufacturer Part Number (MPN) | DigiKey | Mouser | Newark | Datasheet |
| :---: | :--- | :---: | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 65 | `H1`..`H4` | 4 | M3 Standoffs | M3 × 10 mm Hex Female/Female Brass Standoffs + M3 Screws + Rubber Feet | `Mechanical:MountingHole` *(or `MountingHole_Pad`)* | `MountingHole:MountingHole_3.2mm_M3` | Keystone Electronics | 8400 (Standoff) + 9900 (Screw) | 36-8400-ND | 534-8400 | 36M8400 | — |
