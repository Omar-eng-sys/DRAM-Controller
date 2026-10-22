# DRAM Controller

**A Verilog HDL design and simulation project for Computer Architecture.**

![Status](https://img.shields.io/badge/status-planning-blue)
![Language](https://img.shields.io/badge/HDL-Verilog-orange)
![Implementation](https://img.shields.io/badge/implementation-simulation--only-purple)

## Overview

This project aims to design, simulate, and verify an **educational asynchronous DRAM controller** using Verilog HDL. The controller will coordinate memory read/write requests, multiplex row and column addresses, generate appropriately timed RAS/CAS control signals, schedule refresh operations, and report its status to the requesting system.

The project is currently in the **planning and architecture** phase. Features described below are *planned*, not yet implemented or verified.

> **Scope:** Simulation only. No physical DRAM chip, FPGA, or other hardware is required. The memory device will be represented by a separate behavioral model. Timing parameters will be explicitly defined for the educational model and must not be interpreted as validated specifications for a commercial DRAM device.

## Project Requirements

1. **Address separation and multiplexing** — split a memory address into row and column fields and present each field at the correct stage of a transaction.
2. **RAS/CAS generation and timing** — generate active-low row/column strobes and coordinate them with address setup, read/write control, and cycle recovery.
3. **Periodic refresh** — schedule refresh operations and arbitrate between refresh and normal memory requests.
4. **Status reporting** — indicate whether the controller is ready, busy, refreshing, or completing a transaction.

The simulation will also demonstrate **read and write operations** and verify data integrity.

## Proposed Educational Memory Configuration

| Parameter | Proposed value |
| --- | --- |
| DRAM type | Asynchronous DRAM (behavioral model) |
| Address width | 8 bits |
| Row / column address | 4 bits / 4 bits |
| Data width | 8 bits |
| Memory organization | 16 rows × 16 columns × 8 bits |
| Total capacity | 256 bytes |
| Controller clock | 10 MHz (100 ns period) |
| Operations | Read, write, periodic refresh |

**These are preliminary design choices.** Exact timing constants, refresh policy, transaction handshakes, and signal behavior will be finalized in the design specification before implementation.

## High-Level Architecture

```text
           Requesting System / Testbench
          address, write data, read/write
                       |
                       v
           +-------------------------+
           |     DRAM Controller     |
           |-------------------------|
           | Main FSM / Arbitration  |
           | Address Register & MUX  |
           | Timing Control          |
           | Refresh Scheduler       |
           | Status / Data Interface |
           +------------+------------+
                        |
             RAS#, CAS#, WE#, address,
                  read/write data
                        |
                        v
           +-------------------------+
           |  Behavioral DRAM Model  |
           |    16 × 16 × 8 bits     |
           +-------------------------+
```

The **controller** and **DRAM model** will be separate Verilog components so that controller behavior can be tested against an independently implemented memory interface.

## Planned Modules

| Module | Responsibility |
| --- | --- |
| `dram_controller.v` | Main finite-state machine, request handling, and arbitration |
| `address_mux.v` | Row/column address selection |
| `timing_unit.v` | Control-signal sequencing and timing counters |
| `refresh_controller.v` | Refresh scheduling and row tracking |
| `dram_model.v` | Behavioral DRAM model used for verification |
| `dram_controller_tb.v` | Automated integration testbench |

Module boundaries and filenames may change during architecture review.

## Tools

- **Verilog HDL** — hardware description language
- **Icarus Verilog** — compilation and simulation
- **GTKWave** — waveform inspection
- **VS Code / Antigravity** — code editing
- **Git** — version control and team collaboration

The workflow is intended to support **Linux, Windows, and macOS**. The core simulation can be run from a terminal wherever Icarus Verilog is installed.

## Planned Repository Structure

```text
DRAM-Controller/
├── rtl/                     # Controller RTL source files
│   ├── dram_controller.v
│   ├── address_mux.v
│   ├── timing_unit.v
│   └── refresh_controller.v
├── tb/                      # Testbench and DRAM behavioral model
│   ├── dram_model.v
│   └── dram_controller_tb.v
├── sim/                     # Simulation scripts and generated waveforms
├── docs/                    # Design specification, diagrams, and test results
└── README.md
```

*This is the target layout; some files and directories may not exist yet.*

## Simulation Workflow (Once Implemented)

From the repository root, compile the RTL and testbench using Icarus Verilog:

```bash
mkdir -p sim
iverilog -g2012 -s dram_controller_tb -o sim/dram_tb.vvp rtl/*.v tb/*.v
vvp sim/dram_tb.vvp
```

If the testbench generates a VCD file at `sim/dram_tb.vcd`, inspect it with:

```bash
gtkwave sim/dram_tb.vcd
```

> These commands describe the **planned workflow** and will work only after the corresponding source files, testbench top module, and VCD output have been implemented.

## Verification Plan

The testbench is planned to cover:

- Reset and idle/ready behavior
- Row and column address multiplexing
- Read/write correctness and data integrity
- RAS/CAS sequencing and timing constraints
- Automatic refresh scheduling and execution
- Read/write requests arriving during refresh
- Busy, ready, refresh-active, and completion status
- Consecutive transactions and refresh under sustained traffic

Waveform captures and test results will be added after the design passes verification.

## Development Roadmap

- [ ] Finalize the design specification and timing assumptions
- [ ] Define module interfaces and controller state diagram
- [ ] Implement and unit-test address multiplexing
- [ ] Implement the main controller FSM and timing logic
- [ ] Implement refresh scheduling and arbitration
- [ ] Implement the independent behavioral DRAM model
- [ ] Build automated tests and inspect waveforms
- [ ] Integrate and verify the complete simulation
- [ ] Prepare the technical report and presentation

## Team Workflow

The project is developed by a **five-member team**. Work will be divided by modules after the architecture and shared interfaces are agreed upon. Each contributor should document assumptions, test their changes, and submit work for integration review.

## Academic Context

**Course:** Computer Architecture  
**Project:** DRAM Controller Design and Simulation  

Reference textbooks:

- *Logic and Computer Design Fundamentals*, 5th Edition.
- *Computer Architecture: A Quantitative Approach*, 5th Edition.

## Project Status

🚧 **Planning / Architecture Design** — implementation and verification are pending.
