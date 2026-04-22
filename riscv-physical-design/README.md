# RISC-V Single-Cycle Processor – Physical Implementation

This repository contains the physical design implementation of a 32-bit Single-Cycle RISC-V Processor using the OpenLane automated RTL-to-GDSII flow, targeting the SkyWater 130nm (`sky130`) open-source PDK.

## Project Overview

The objective of this project was to take a Verilog RTL design of a RISC-V processor and successfully drive it through the entire Physical Design flow to generate a manufacturing-ready GDSII layout. 

### Key Achievements (As mentioned in my Resume):
- **Complete Physical Design Flow**: Executed the RTL-to-GDSII flow using the OpenLane automated toolchain.
- **Technology Node**: SkyWater 130nm open-source PDK (`sky130A`).
- **Floorplanning & Placement**: Achieved a core utilization of **45%** while maintaining routability and standard cell density targets.
- **Timing Closure**: Satisfied all setup and hold timing constraints. Analyzed post-CTS (Clock Tree Synthesis) timing reports to identify and fix critical path violations, improving overall design closure quality.
- **Signoff & Verification**: Validated design integrity by running Design Rule Checks (DRC) and Layout vs. Schematic (LVS) checks using Magic and Netgen, ensuring **zero violations** in the final GDSII layout.

## Repository Structure

```text
├── rtl/                # Verilog source files for the RISC-V processor
├── openlane/           # OpenLane configuration files (e.g., config.json)
├── reports/            # Selected reports from OpenLane runs
│   ├── routing/        # Post-routing and post-CTS timing reports
│   └── signoff/        # DRC and LVS verification reports
├── images/             # Screenshots of the layout and floorplan (Add your GDS screenshots here)
└── README.md           # This file
```

## Physical Design Flow Steps

### 1. Synthesis
- Tool: `Yosys`
- Mapped Verilog RTL to `sky130` standard cells.
- Optimized for delay and area.

### 2. Floorplanning & Power Planning (PDN)
- Tool: `OpenROAD`, `TritonRoute`
- Set core utilization to 45%.
- Inserted decoupling capacitors and tap cells.
- Generated the Power Delivery Network (PDN) with concentric rings and vertical/horizontal stripes.

### 3. Placement
- Tool: `RePlAce`, `OpenDP`
- Global and detailed placement of standard cells.

### 4. Clock Tree Synthesis (CTS)
- Tool: `TritonCTS`
- Synthesized the clock tree to minimize skew and insertion delay.
- Post-CTS timing analysis was performed to resolve any setup/hold violations.

### 5. Routing
- Tool: `FastRoute`, `TritonRoute`
- Global and detailed routing of signal nets.

### 6. Signoff (DRC & LVS)
- **DRC (Design Rule Check)**: Used `Magic` to ensure the layout meets foundry manufacturing rules. Zero violations found.
- **LVS (Layout vs Schematic)**: Used `Netgen` to ensure the physical layout perfectly matches the synthesized gate-level netlist. Circuits matched uniquely.

## How to Run

If you have OpenLane installed, you can reproduce this build by placing the `riscv_core` folder into your OpenLane `designs` directory and running:

```bash
make mount
./flow.tcl -design riscv_core
```

## Future Work
- Implementing a pipelined architecture to improve the maximum operating frequency.
- Adding data and instruction caches.
