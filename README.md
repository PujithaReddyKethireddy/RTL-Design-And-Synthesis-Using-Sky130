# RTL Design And Synthesis Workshop Using SKY130

<div align="center">

![RTL](https://img.shields.io/badge/RTL-Design-blue?style=for-the-badge)
![Verilog](https://img.shields.io/badge/Verilog-HDL-orange?style=for-the-badge)
![Yosys](https://img.shields.io/badge/Yosys-Synthesis-green?style=for-the-badge)
![SKY130](https://img.shields.io/badge/SKY130-OpenSource-red?style=for-the-badge)

</div>

---


# 📌 About The Workshop

Welcome to the **RTL Design And Synthesis Workshop Using SKY130** repository.

This repository contains complete hands-on documentation, RTL designs, simulations, synthesis results, waveform analysis, Gate-Level Simulation (GLS), synthesis optimization techniques, and SKY130 standard-cell based ASIC design flow using open-source EDA tools.

The workshop demonstrates the complete RTL-to-Gate-Level design flow using:

* Verilog HDL
* Icarus Verilog
* GTKWave
* Yosys
* SKY130 Standard Cell Libraries
* Open-source ASIC synthesis flow

---

# 🎯 Workshop Objectives

The main objectives of this workshop are:

* Understand Verilog RTL design fundamentals
* Learn RTL simulation and waveform verification
* Understand synthesis using Yosys
* Explore SKY130 timing libraries and standard cells
* Learn combinational and sequential optimizations
* Perform Gate-Level Simulation (GLS)
* Understand synthesis-simulation mismatches
* Analyze latch inference and coding styles
* Build scalable RTL designs using generate blocks
* Understand synthesis-aware RTL coding practices

---

# 🧭 Complete RTL Design Flow

```text
Specification
      ↓
RTL Design (Verilog)
      ↓
RTL Simulation using Icarus Verilog
      ↓
Waveform Verification using GTKWave
      ↓
Logic Synthesis using Yosys
      ↓
Technology Mapping using SKY130
      ↓
Gate-Level Netlist Generation
      ↓
Gate-Level Simulation (GLS)
      ↓
Optimization & Verification
```

---

# 📂 Repository Structure

```text
RTL-Design-And-Synthesis-Workshop-Using-SKY130/
│
├── Day 1 - Introduction to Verilog RTL design and Synthesis/
├── Day 2 - Timing libs, hierarchical vs flat synthesis and efficient flop coding styles/
├── Day 3 - Combinational and sequential optimizations/
├── Day 4 - GLS, blocking vs non-blocking and Synthesis-Simulation mismatch/
├── Day 5 - Optimization in synthesis/
│
└── README.md
```

---

# 📚 Table of Contents

* [About The Workshop](#-about-the-workshop)
* [Workshop Objectives](#-workshop-objectives)
* [RTL Design Flow](#-complete-rtl-design-flow)
* [Repository Structure](#-repository-structure)
* [Prerequisites](#-prerequisites)
* [Installation](#️-installation)
* [Tools Used](#️-tools-used)
* [Workshop Structure](#-workshop-structure)
* [Quick Start](#-quick-start)
* [Major Concepts Covered](#-major-concepts-covered)
* [Important Comparisons](#-important-comparisons)
* [Skills Demonstrated](#-skills-demonstrated)
* [Author](#-author)
* [Acknowledgements](#-acknowledgements)
* [Conclusion](#-conclusion)

---

# 📖 Prerequisites

Before starting this workshop, basic understanding of the following concepts is helpful:

* Digital Electronics
* Logic Gates
* Flip-Flops
* Linux Terminal Commands
* Verilog HDL Basics
* Digital Logic Design

---

# 🛠️ Installation

## Install Icarus Verilog

```bash
sudo apt install iverilog
```

---

## Install GTKWave

```bash
sudo apt install gtkwave
```

---

## Install Yosys

```bash
sudo apt install yosys
```

---

## Install Git

```bash
sudo apt install git
```

---

## Clone Workshop Repository

```bash
git clone https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git
```

---

# 🖥️ Tools Used

| Tool                      | Purpose                 |
| ------------------------- | ----------------------- |
| Icarus Verilog (iverilog) | RTL & GLS Simulation    |
| GTKWave                   | Waveform Analysis       |
| Yosys                     | Logic Synthesis         |
| SKY130 PDK                | Standard Cell Libraries |
| Linux Terminal            | Command Execution       |
| Git & GitHub              | Version Control         |

---

# 📂 Workshop Structure

## Day 1 – Introduction to Verilog RTL Design and Synthesis

Topics Covered:

* Simulator, Design, and Testbench
* RTL Simulation using Icarus Verilog
* Waveform Analysis using GTKWave
* Introduction to Logic Synthesis
* SKY130 Standard Cell Libraries
* Timing Concepts
* Synthesis using Yosys
* Gate-Level Netlist Generation

---

## Day 2 – Timing Libraries, Hierarchical vs Flat Synthesis & Efficient Flip-Flop Coding Styles

Topics Covered:

* Liberty Timing Libraries
* SKY130 PDK Overview
* Standard Cell Characterization
* Drive Strength Analysis
* Hierarchical vs Flat Synthesis
* Logic Optimization
* Flip-Flop Coding Styles
* Technology Mapping

---

## Day 3 – Combinational and Sequential Optimizations

Topics Covered:

* Constant Propagation
* Boolean Simplification
* Dead Logic Elimination
* Multi-Module Optimization
* Sequential Optimization
* Counter Optimization
* Register Optimization
* Optimization-aware RTL Coding

---

## Day 4 – GLS, Blocking vs Non-Blocking & Synthesis-Simulation Mismatch

Topics Covered:

* Gate-Level Simulation (GLS)
* RTL vs GLS Verification
* Missing Sensitivity List
* Blocking vs Non-Blocking Assignments
* Synthesis-Simulation Mismatch
* Correct RTL Coding Styles
* Gate-Level Verification

---

## Day 5 – Optimization in Synthesis

Topics Covered:

* IF Constructs
* CASE Constructs
* Incomplete IF and CASE Statements
* Latch Inference
* FOR vs FOR GENERATE
* MUX & DEMUX Design
* Generate-based Hardware Replication
* Ripple Carry Adder using Generate
* Gate-Level Simulation

---

# 🚀 Quick Start

## RTL Simulation

```bash
iverilog design.v testbench.v
./a.out
gtkwave waveform.vcd
```

---

## Yosys Synthesis

```bash
yosys

read_liberty -lib sky130_fd_sc_hd__tt_025C_1v80.lib

read_verilog design.v

synth -top module_name

abc -liberty sky130_fd_sc_hd__tt_025C_1v80.lib

show
```

---

## Gate-Level Simulation (GLS)

```bash
iverilog primitives.v sky130_fd_sc_hd.v design_netlist.v testbench.v

./a.out

gtkwave waveform.vcd
```

---

# 🧠 Major Concepts Covered

## RTL Simulation

* Functional verification
* Waveform analysis
* Testbench development
* Signal monitoring

---

## Logic Synthesis

* RTL to Gate-Level conversion
* Technology mapping
* Logic optimization
* Standard-cell inference

---

## Timing Libraries

* Liberty `.lib` files
* Setup timing
* Hold timing
* PVT analysis
* Drive strength optimization

---

## Combinational Optimization

* Constant propagation
* Boolean simplification
* Dead logic elimination
* Multi-module optimization

---

## Sequential Optimization

* Register optimization
* Counter optimization
* Sequential constant propagation
* Flip-flop inference

---

## Gate-Level Simulation (GLS)

* Netlist verification
* Post-synthesis validation
* RTL vs GLS comparison
* Functional equivalence checking

---

## RTL Coding Styles

* Blocking vs Non-blocking assignments
* Sensitivity list importance
* IF vs CASE constructs
* Latch inference avoidance
* Generate-based scalable RTL

---

# 📊 Important Comparisons

| Comparison                       | Observation                        |
| -------------------------------- | ---------------------------------- |
| RTL vs GLS                       | GLS uses synthesized hardware      |
| Blocking vs Non-Blocking         | Sequential vs Parallel behavior    |
| Hierarchical vs Flat Synthesis   | Debugging vs Optimization tradeoff |
| Complete vs Incomplete CASE      | No latch vs Latch inference        |
| FOR vs FOR GENERATE              | Behavioral vs Structural           |
| Optimized vs Non-Optimized Logic | Reduced area and gate count        |
| Fast Cells vs Slow Cells         | Speed vs Area/Power tradeoff       |

---

# 📸 Repository Highlights

✅ Day-wise structured documentation
✅ RTL + GLS Verification
✅ SKY130 Standard Cell Mapping
✅ Waveform Analysis
✅ Gate-Level Netlists
✅ Yosys Synthesis Flow
✅ Optimization Examples
✅ Timing Library Analysis
✅ Generate-based RTL Design
✅ Latch Inference Analysis
✅ Beginner-Friendly Explanations
✅ Open-Source ASIC Design Flow

---

# 🧪 Skills Demonstrated

This repository demonstrates practical understanding of:

* Verilog RTL Design
* RTL Simulation
* Waveform Analysis
* Logic Synthesis
* SKY130 Standard Cell Mapping
* Timing Library Analysis
* Gate-Level Simulation (GLS)
* Synthesis Optimization
* Sequential Logic Design
* Combinational Logic Design
* Generate Constructs
* ASIC Design Fundamentals
* Linux-based ASIC Flow
* Yosys Synthesis Flow
* Synthesis-aware RTL Coding

---

# 👩‍💻 Author

## Kethireddy Pujitha Reddy

Electronics & Communication Engineering Student

Interested in:

* VLSI Design
* RTL Design
* ASIC Design Flow
* Verilog HDL
* Open-Source Silicon
* Digital Electronics

---

# 🙏 Acknowledgements

Special thanks to:

* Kunal Ghosh
* VSD (VLSI System Design) Platform
* SKY130 Open PDK Contributors
* Yosys Development Team
* Icarus Verilog Contributors
* Open-Source VLSI Community

---


# 📌 Conclusion

This workshop provided strong practical understanding of:

* RTL Design
* Logic Synthesis
* Timing Libraries
* Optimization Techniques
* Gate-Level Simulation
* SKY130 Standard Cells
* Synthesis-aware RTL Coding
* Generate-based Hardware Design

Through hands-on labs, waveform analysis, synthesized netlists, optimization experiments, and GLS verification, this repository establishes a strong foundation in RTL design and synthesis using open-source ASIC tools.

---

<div align="center">

# ⭐ If you found this repository useful, consider giving it a star ⭐

</div>
