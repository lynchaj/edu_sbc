(kicad_sch
	(version 20250114)
	(generator "eeschema")
	(generator_version "9.0")
	(uuid "00000000-0000-0000-0000-000065000000")
	(paper "A3")
	(title_block
		(title "Educational Z80 Single Board Computer (EduSBC)")
		(date "2026-11-22")
		(rev "1.2")
		(company "Educational Architecture Series")
		(comment 1 "Hierarchical Root Schematic - 6 Lab Progression (100% Metric / 2.54 mm Grid)")
		(comment 2 "External Accessories (Not BOM Symbols): Qualtek QAWA-24-12-US01, DTECH DT-9008, DTECH DT-5002A")
	)
	(lib_symbols)
	(sheet
		(at 25.4 25.4)
		(size 76.2 38.1)
		(fields_autoplaced yes)
		(stroke
			(width 0.1524)
			(type solid)
		)
		(fill
			(color 0 0 0 0.0000)
		)
		(uuid "00000000-0000-0000-0000-000065000010")
		(property "Sheetname" "Power, Clock & Reset"
			(at 25.4 24.13 0)
			(effects
				(font
					(size 1.524 1.524)
					(bold yes)
				)
				(justify left bottom)
			)
		)
		(property "Sheetfile" "01_Power_Clock.kicad_sch"
			(at 25.4 64.77 0)
			(effects
				(font
					(size 1.27 1.27)
					(italic yes)
				)
				(justify left top)
			)
		)
	)
	(sheet
		(at 109.22 25.4)
		(size 76.2 38.1)
		(fields_autoplaced yes)
		(stroke
			(width 0.1524)
			(type solid)
		)
		(fill
			(color 0 0 0 0.0000)
		)
		(uuid "00000000-0000-0000-0000-000065000020")
		(property "Sheetname" "Z80 CPU & System Bus"
			(at 109.22 24.13 0)
			(effects
				(font
					(size 1.524 1.524)
					(bold yes)
				)
				(justify left bottom)
			)
		)
		(property "Sheetfile" "02_CPU_Bus.kicad_sch"
			(at 109.22 64.77 0)
			(effects
				(font
					(size 1.27 1.27)
					(italic yes)
				)
				(justify left top)
			)
		)
	)
	(sheet
		(at 193.04 25.4)
		(size 76.2 38.1)
		(fields_autoplaced yes)
		(stroke
			(width 0.1524)
			(type solid)
		)
		(fill
			(color 0 0 0 0.0000)
		)
		(uuid "00000000-0000-0000-0000-000065000030")
		(property "Sheetname" "Memory & Address Decode"
			(at 193.04 24.13 0)
			(effects
				(font
					(size 1.524 1.524)
					(bold yes)
				)
				(justify left bottom)
			)
		)
		(property "Sheetfile" "03_Memory_Decode.kicad_sch"
			(at 193.04 64.77 0)
			(effects
				(font
					(size 1.27 1.27)
					(italic yes)
				)
				(justify left top)
			)
		)
	)
	(sheet
		(at 68.58 96.52)
		(size 76.2 38.1)
		(fields_autoplaced yes)
		(stroke
			(width 0.1524)
			(type solid)
		)
		(fill
			(color 0 0 0 0.0000)
		)
		(uuid "00000000-0000-0000-0000-000065000040")
		(property "Sheetname" "Parallel I/O (82C55 PPI)"
			(at 68.58 95.25 0)
			(effects
				(font
					(size 1.524 1.524)
					(bold yes)
				)
				(justify left bottom)
			)
		)
		(property "Sheetfile" "04_Parallel_IO.kicad_sch"
			(at 68.58 135.89 0)
			(effects
				(font
					(size 1.27 1.27)
					(italic yes)
				)
				(justify left top)
			)
		)
	)
	(sheet
		(at 152.4 96.52)
		(size 76.2 38.1)
		(fields_autoplaced yes)
		(stroke
			(width 0.1524)
			(type solid)
		)
		(fill
			(color 0 0 0 0.0000)
		)
		(uuid "00000000-0000-0000-0000-000065000050")
		(property "Sheetname" "Serial Communications (UART & RS232)"
			(at 152.4 95.25 0)
			(effects
				(font
					(size 1.524 1.524)
					(bold yes)
				)
				(justify left bottom)
			)
		)
		(property "Sheetfile" "05_Serial_UART.kicad_sch"
			(at 152.4 135.89 0)
			(effects
				(font
					(size 1.27 1.27)
					(italic yes)
				)
				(justify left top)
			)
		)
	)
	(text "EDUCATIONAL SINGLE BOARD COMPUTER (EDUSBC) - HIERARCHY & NOTES\n\nAll subsystem sheets share nets via explicit Global Labels:\n- Address Bus: A0..A15 | Data Bus: D0..D7\n- Bus Control: nMREQ, nIORQ, nRD, nWR, nM1, nRFSH, nHALT, nWAIT, nINT, nNMI, nBUSREQ, nBUSACK\n- Clocks & Reset: CPU_CLK (4.0000 MHz), UART_CLK (1.8432 MHz), nRESET, RESET_HIGH, nUART_INT\n- Power Rails: V_IN_RAW, V_PROT, +5V, GND\n- External Accessories (Excluded from Schematic Symbols; see Note on Schematic & PCB):\n  * Qualtek QAWA-24-12-US01 (12V DC 2A Wall Adapter)\n  * DTECH DT-9008 (USB-to-TTL 5V FT232RL 6-Pin Cable)\n  * DTECH DT-5002A (USB-to-RS232 DB9 Female Cable)"
		(at 25.4 149.86 0)
		(effects
			(font
				(size 1.524 1.524)
				(bold yes)
			)
			(justify left top)
		)
		(uuid "00000000-0000-0000-0000-000065000099")
	)
)