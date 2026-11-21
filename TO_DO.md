### edu_sbc To Do List
- Update DESIGN_DECISIONS with any changes from TO_DO list actions
- Project GitHub Repository is: https://github.com/lynchaj/edu_sbc

## Schematic
- KiCAD schematic must be strictly KiCAD version 9 compliant (grammar and syntax)
- Schematic may use metric equivalent of Imperial measurements for grid layouts and snap for compatibility with legacy symbols for example 0.10 inch = 2.54 mm, etc.
- Schematic symbol pins should align to the grid whenever possible
- Schematic grid should be as large as possible with 2.54 mm spacing preferred but 1.27 mm or smaller if required
- Schematic should have symbols centered in the drawing not overlapping with data box in lower righthand corner or drawing boundaries
- Schematic should have in general a left to right flow such that inputs are on the left and outputs are on the right to the extent possible
- All components in the BOM must be present in the schematic as a symbol
- All power nets must have PWR_FLAG symbol except those directly connected to power output pins
- All pins on symbols must either be connected to an appropriate net or a no-connect symbol
- Fix schematic load errors

## PCB layout
- KiCAD PCB Layout must be strictly KiCAD version 9 compliant (grammar and syntax)
- PCB layout may use metric equivalent of Imperial measurements for grid layouts and snap for compatibility with legacy footprints for example 0.10 inch = 2.54 mm, etc.
- PCB layout footprint pins should align to the grid whenever possible
- PCB layout grid should be as large as possible with 2.54 mm spacing preferred but 1.27 mm or smaller if required
- PCB layout should have PCB edge outline centered in the drawing not overlapping with data box in lower righthand corner or drawing boundaries (typically centered at X=150 Y=100)
- PCB layout should be spaced sufficiently to support trace routing
- PCB layout should have in general a left to right flow such that inputs are on the left and outputs are on the right to the extent possible
- Power input barrel jack should be on lefthand side of PCB towards the top
- Voltage regulator should be near top of board near the lefthand side as possible to prevent operators from accidentally touching hot regulator
- Power switch should be next to the barrel jack on the lefthand side of PCB
- Fix PCB layout load errors

## Other
- Add column to BOM with symbol library (Package) / Symbol for each component
- Verify the KiCAD symbols match the datasheets
- Verify the KiCAD footprints match the datasheets
