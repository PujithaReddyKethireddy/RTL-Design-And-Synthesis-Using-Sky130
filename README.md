
# RTL Design And Synthesis Workshop Using SKY130

<div align="center">

![RTL](https://img.shields.io/badge/RTL-Design-blue?style=for-the-badge)
![Verilog](https://img.shields.io/badge/Verilog-HDL-orange?style=for-the-badge)
![Yosys](https://img.shields.io/badge/Yosys-Synthesis-green?style=for-the-badge)
![SKY130](https://img.shields.io/badge/SKY130-OpenSource-red?style=for-the-badge)

</div>

---

# 📌 About The Workshop

Welcome to the **RTL Design And Synthesis Workshop Using SKY130**.

This repository contains complete hands-on documentation, RTL designs, simulations, synthesis results, optimizations, Gate-Level Simulation (GLS), waveform analysis, and SKY130 standard-cell based ASIC design flow using open-source EDA tools.

This workshop focuses on:

- Verilog RTL Design
- RTL Simulation using Icarus Verilog
- Waveform Analysis using GTKWave
- Logic Synthesis using Yosys
- SKY130 Standard Cell Libraries
- Timing Libraries and Liberty Files
- Combinational and Sequential Optimization
- Gate-Level Simulation (GLS)
- Blocking vs Non-Blocking Assignments
- Latch Inference
- Generate Blocks and Scalable RTL Design

---

# 📚 Table of Contents

- [About This Workshop](#-about-the-workshop)
- [Prerequisites](#-prerequisites)
- [Tools Used](#️-tools-used)
- [Workshop Structure](#-workshop-structure)
- [RTL Design Flow](#-rtl-design-flow)
- [Important Concepts Covered](#-important-concepts-covered)
- [Common Commands Used](#️-common-commands-used)
- [Major Comparisons](#-major-comparisons)
- [Skills Gained](#-skills-gained)
- [Author](#-author)
- [Acknowledgements](#-acknowledgements)
- [Conclusion](#-conclusion)

---

# 📖 Prerequisites

- Basic understanding of digital electronics
- Familiarity with logic gates and flip-flops
- Basic Linux command-line knowledge
- Verilog HDL basics
- Installed tools:
  - `iverilog`
  - `gtkwave`
  - `yosys`
  - `git`

---

# 🛠️ Tools Used

| Tool | Purpose |
|---|---|
| Icarus Verilog (iverilog) | RTL & GLS Simulation |
| GTKWave | Waveform Analysis |
| Yosys | Logic Synthesis |
| SKY130 PDK | Standard Cell Library |
| Linux Terminal | Command Execution |
| Git & GitHub | Version Control |

---

# 📂 Workshop Structure

The workshop is organized day-wise, each with a dedicated folder and README:

- [Day 1: Introduction to Verilog RTL Design & Synthesis](./Day1/README.md)
- [Day 2: Timing Libraries, Hierarchical vs Flat Synthesis & Efficient Flip-Flop Coding](./Day2/README.md)
- [Day 3: Combinational and Sequential Optimization](./Day3/README.md)
- [Day 4: Gate-Level Simulation (GLS), Blocking vs Non-Blocking in Verilog & Synthesis-Simulation Mismatch](./Day4/README.md)
- [Day 5: Optimization in Synthesis](./Day5/README.md)

Each day’s README includes:

- Detailed theory explanations
- Step-by-step practical labs
- RTL codes and screenshots
- Waveform analysis
- Synthesized netlists
- Observations and conclusions
- Optimization techniques
- SKY130 technology mapping

---

# 🔄 RTL Design Flow

```text
Specification
      ↓
RTL Design (Verilog)
      ↓
RTL Simulation
      ↓
Waveform Verification
      ↓
Logic Synthesis using Yosys
      ↓
Technology Mapping
      ↓
Gate-Level Netlist
      ↓
Gate-Level Simulation (GLS)
      ↓
Optimization & Verification
````

---

# 🧠 Important Concepts Covered

## RTL Simulation

RTL simulation verifies functionality before hardware implementation.

### Key Learnings

* Testbench writing
* Functional verification
* Waveform analysis
* Debugging RTL behavior

---

## Logic Synthesis

Synthesis converts RTL into gate-level hardware using SKY130 standard cells.

### Key Learnings

* Technology mapping
* Logic optimization
* Standard-cell inference
* Timing-aware synthesis

---

## Timing Libraries

The SKY130 Liberty `.lib` files contain:

* Timing information
* Area information
* Leakage power
* Capacitance data
* Transition delays

### Important Concepts

* PVT Analysis
* Setup timing
* Hold timing
* Drive strength optimization

---

## Combinational Optimization

Covered techniques:

* Constant propagation
* Boolean simplification
* Dead logic elimination
* Multi-module optimization

---

## Sequential Optimization

Covered techniques:

* Register optimization
* Counter optimization
* Sequential constant propagation
* Flip-flop inference

---

## Gate-Level Simulation (GLS)

GLS verifies synthesized hardware functionality.

### GLS Flow

```bash
iverilog primitives.v sky130_fd_sc_hd.v synthesized_netlist.v testbench.v
./a.out
gtkwave waveform.vcd
```

### Importance

* Detect synthesis mismatch
* Verify synthesized hardware
* Validate gate-level functionality

---

## Blocking vs Non-Blocking Assignments

### Blocking (`=`)

* Sequential execution
* Mainly used for combinational logic

### Non-Blocking (`<=`)

* Parallel execution
* Mainly used for sequential logic

---

## Latch Inference

Incomplete IF/CASE statements infer latches because outputs retain previous values.

### Important Learning

Always assign outputs under all conditions to avoid unintended latches.

---

## FOR vs FOR GENERATE

| FOR Loop                 | FOR GENERATE              |
| ------------------------ | ------------------------- |
| Behavioral               | Structural                |
| Used inside always block | Used outside always block |
| Logic iteration          | Hardware replication      |

---

# 🖥️ Common Commands Used

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

## Gate-Level Simulation

```bash
iverilog  primitives.v sky130_fd_sc_hd.v design_netlist.v testbench.v

./a.out

gtkwave waveform.vcd
```

---

# 📊 Major Comparisons

| Comparison                       | Observation                     |
| -------------------------------- | ------------------------------- |
| RTL vs GLS                       | GLS uses synthesized hardware   |
| Blocking vs Non-Blocking         | Sequential vs Parallel behavior |
| Hierarchical vs Flat Synthesis   | Debuggability vs Optimization   |
| Complete vs Incomplete CASE      | No latch vs Latch inference     |
| FOR vs FOR GENERATE              | Behavioral vs Structural        |
| Optimized vs Non-Optimized Logic | Reduced gates and area          |

---

# 🚀 Skills Gained

* Verilog RTL Coding
* Logic Synthesis
* Gate-Level Simulation
* Timing Library Understanding
* Sequential Logic Design
* Optimization Techniques
* Yosys Synthesis Flow
* Waveform Analysis
* Linux-based ASIC Flow
* SKY130 Standard Cell Mapping

---

# 🌟 Repository Highlights

✅ Complete Day-wise Documentation
✅ RTL + GLS Verification
✅ Waveform Analysis
✅ Synthesized Netlists
✅ Optimization Examples
✅ SKY130 Technology Mapping
✅ Gate-Level Schematics
✅ Beginner Friendly Structure
✅ Open-Source ASIC Design Flow

---

# 👩‍💻 Author

## pujitha reddy keth

Electronics & Communication Engineering Student

Interested in:

* VLSI Design
* RTL Design
* ASIC Design Flow
* Digital Electronics
* Verilog HDL
* Open-Source Silicon

---

# 🙏 Acknowledgements

Special thanks to:

* Workshop Mentors & Instructors
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
* Synthesis-Aware Coding Styles

Through hands-on labs, waveform analysis, synthesized netlists, optimization experiments, and GLS verification, this repository establishes a strong foundation in RTL design and synthesis using open-source ASIC tools.

---

<div align="center">

# ⭐ If you found this repository useful, consider giving it a star ⭐

</div>
```
