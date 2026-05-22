# Day 5 – Optimization in Synthesis

This section focuses on synthesis optimizations and Verilog coding styles using SKY130 standard cells.
The experiments demonstrate how different RTL constructs such as `if`, `case`, `for`, and `generate` influence synthesized hardware, simulation behavior, latch inference, and technology mapping.

The goal of this day is to understand:

* Priority logic using IF constructs
* Incomplete IF and CASE statements
* Latch inference
* Partial assignments
* Overlapping CASE conditions
* Difference between `for` loop and `for generate`
* Hardware replication using generate blocks
* RTL vs Gate-Level Simulation (GLS)
* SKY130 technology mapping using Yosys

---

# Table of Contents

1. Introduction
2. IF Constructs
3. Incomplete IF Statements
4. CASE Constructs
5. Incomplete CASE Statements
6. Partial CASE Assignment
7. Overlapping CASE Conditions
8. FOR Loop vs FOR GENERATE
9. MUX using Generate
10. DEMUX using CASE
11. DEMUX using GENERATE
12. RTL Simulation Flow
13. Synthesis Flow using Yosys
14. Gate-Level Simulation (GLS)
15. Ripple Carry Adder using Generate
16. Final Observations
17. Conclusion

---

# 1. Introduction

Synthesis optimization is an important aspect of digital design.
The Verilog coding style directly affects:

* Area
* Timing
* Hardware inference
* Power
* Scalability
* Synthesis results

Incorrect coding styles may infer:

* unwanted latches
* redundant hardware
* synthesis mismatches

Day 5 explores these concepts practically using:

* RTL simulation
* synthesized netlists
* SKY130 standard cells
* Gate-Level Simulation

---

# 2. IF Constructs

The `if-else` construct implements conditional priority logic.

---

## Important Theory

* IF statements are evaluated sequentially.
* The first true condition gets highest priority.
* Synthesized hardware behaves like cascaded multiplexers.
* Incomplete assignments can infer latches.

---

## IF Statement Example

```verilog id="4xk0zb"
always @(*) begin
    if(cond1)
        y = a;
    else if(cond2)
        y = b;
    else
        y = c;
end
```

---

# Hardware Interpretation

IF statements synthesize into:

* priority mux structures
* conditional logic networks

---

# Incomplete IF Example

```verilog id="4z0e3p"
always @(*) begin
    if(cond1)
        y = a;
    else if(cond2)
        y = b;
end
```

No assignment exists for remaining conditions.

Therefore:

* output retains previous value
* latch is inferred

---

# Sequential Logic Example

```verilog id="v31yru"
always @(posedge clk, posedge reset)
begin
    if(reset)
        count <= 3'b000;
    else if(en)
        count <= count + 1;
end
```

In sequential logic:

* retaining previous value is intentional
* implemented using flip-flops
* not considered latch inference issue

---

# IF vs CASE

| IF Statement               | CASE Statement              |
| -------------------------- | --------------------------- |
| Priority based             | Selection based             |
| Sequential evaluation      | Parallel evaluation         |
| Used for priority encoders | Used for muxes/decoders     |
| Easier to infer latches    | Cleaner for selection logic |

---

# 3. Incomplete IF Statements

Incomplete IF statements infer latches because outputs are not assigned under all conditions.

---

# Theory

When:

* an output is not assigned for every possible condition
* synthesis preserves previous output value

This creates:

* memory behavior
* latch inference

---

# Important Learning

To avoid unintended latches:

* assign outputs in all conditions
* use ELSE branch
* initialize outputs properly

---

# 4. CASE Constructs

CASE statements are used for:

* selection logic
* decoders
* multiplexers
* combinational control logic

---

# CASE Syntax

```verilog id="9pybto"
always @(*) begin
    case(sel)
        2'b00 : y = i0;
        2'b01 : y = i1;
        2'b10 : y = i2;
        default : y = i3;
    endcase
end
```

---

# Important Theory

CASE statements:

* should cover all conditions
* should include `default`
* avoid incomplete assignments

Otherwise:

* latches may be inferred
* simulation and synthesis mismatches occur

---

# 5. Incomplete CASE Statements

When CASE conditions are incomplete:

```verilog id="h5ijg8"
case(sel)
    2'b00 : y = i0;
    2'b01 : y = i1;
endcase
```

No assignment exists for:

* `2'b10`
* `2'b11`

Therefore:

* output retains old value
* latch gets inferred

---

# RTL Simulation

![Incomplete CASE RTL](incomp_case_rtl_simulation.png)

---

# Synthesized Netlist

![Incomplete CASE Netlist](incomp_case_netlist.png)

---

# Observations

1. Output retains previous value when uncovered CASE conditions occur.
2. Synthesis inferred `$DLATCH_P_` cell due to incomplete assignment.
3. The netlist clearly proves latch inference in combinational logic.

---

# 6. Complete CASE Statement

A complete CASE statement removes latch inference.

---

# RTL Simulation

![Complete CASE RTL](comp_case_rtl_simulation.png)

---

# Synthesized Netlist

![Complete CASE Netlist](comp_case_netlist.png)

---

# Observations

1. All input combinations are properly covered.
2. No latch is inferred in synthesized hardware.
3. Netlist maps cleanly into mux-based combinational logic.

---

# 7. Partial CASE Assignment

Partial assignments happen when:

* some outputs are assigned
* others remain unassigned in specific branches

This also infers latches.

---

# RTL Simulation

![Partial CASE Assignment RTL](partial_case_assign_rtl_simulation.png)

---

# Synthesized Netlist

![Partial CASE Assignment Netlist](partial_case_assign_netlist.png)

---

# Observations

1. Some outputs remain unassigned in selected CASE branches.
2. Synthesis inferred latches to preserve previous values.
3. `$DLATCH_P_` cells appear clearly in synthesized hardware.

---

# 8. Overlapping CASE Conditions

Improper CASE coding can create overlapping or ambiguous conditions.

---

# RTL Simulation

![Bad CASE RTL](bad_case_rtl_simulation.png)

---

# Synthesized Netlist

![Bad CASE Netlist](bad_case_netlist.png)

---

# Observations

1. Overlapping conditions may produce unpredictable outputs.
2. RTL expectation and synthesized hardware may differ.
3. Such coding styles should be avoided in practical RTL design.

---

# 9. FOR Loop vs FOR GENERATE

| FOR Loop                      | FOR GENERATE                   |
| ----------------------------- | ------------------------------ |
| Used inside always block      | Used outside always block      |
| Behavioral construct          | Structural construct           |
| Evaluates logic               | Replicates hardware            |
| Does not instantiate hardware | Instantiates repeated hardware |
| Used for iteration            | Used for scalable hardware     |

---

# FOR Loop Example

```verilog id="j5z2rr"
always @(*) begin
    for(i=0;i<8;i=i+1)
        ...
end
```

---

# FOR GENERATE Example

```verilog id="6ng8mj"
generate
    for(i=0;i<8;i=i+1)
    begin
        and u1(...);
    end
endgenerate
```

---

# Important Difference

FOR loop:

* evaluates logic behaviorally

FOR GENERATE:

* physically replicates hardware during synthesis

---

# 10. MUX using Generate

MUX implemented using iterative FOR logic.

---

# RTL Simulation

![MUX Generate RTL](mux_generate_rtl_simulation.png)

---

# Synthesized Netlist

![MUX Generate Netlist](mux_generate_netlist.png)

---

# Observations

1. FOR logic evaluates multiple selection conditions.
2. Missing assignments infer latch behavior.
3. Netlist shows mux implementation along with inferred latch structure.

---

# 11. DEMUX using CASE

DEMUX implemented using CASE statement.

---

# RTL Simulation

![DEMUX CASE RTL](demux_case_rtl_simulation.png)

---

# Synthesized Netlist

![DEMUX CASE Netlist](demux_case_netlist.png)

---

# Observations

1. CASE statement synthesizes into decoder-like hardware.
2. Output activation depends entirely on select input.
3. Multiple SKY130 gates and buffers are used during mapping.

---

# 12. DEMUX using GENERATE

DEMUX implemented using generate constructs.

---

# RTL Simulation

![DEMUX Generate RTL](demux_generate_rtl_simulation.png)

---

# Synthesized Netlist

![DEMUX Generate Netlist](demux_generate_netlist.png)

---

# Observations

1. Generate construct replicated repetitive hardware efficiently.
2. Hardware structure becomes scalable and modular.
3. Generate-based implementation simplifies larger designs.

---

# 13. RTL Simulation Flow

RTL simulation verifies functional correctness before synthesis.

---

# Compilation

```bash id="27p8m3"
iverilog design.v tb_design.v
```

---

# Run Simulation

```bash id="xt2t6k"
./a.out
```

---

# Open GTKWave

```bash id="6iw3wo"
gtkwave tb_design.vcd
```

---

# RTL Simulation Purpose

RTL simulation helps verify:

* logic correctness
* waveform behavior
* functional implementation

before hardware synthesis.

---

# 14. Synthesis Flow using Yosys

Synthesis converts RTL into gate-level hardware.

---

# Start Yosys

```bash id="zht0rz"
yosys
```

---

# Read Liberty File

```tcl id="mkt54x"
read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```

---

# Read Verilog

```tcl id="hllmnd"
read_verilog design.v
```

---

# Synthesize Design

```tcl id="q8zk24"
synth -top design
```

---

# Technology Mapping

```tcl id="zy3jdn"
abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```

---

# Write Netlist

```tcl id="5mjlwm"
write_verilog design_netlist.v
```

---

# Important Learning

Synthesis maps RTL into:

* SKY130 standard cells
* gates
* multiplexers
* buffers
* latches
* combinational networks

---

# 15. Gate-Level Simulation (GLS)

GLS verifies synthesized hardware behavior.

Unlike RTL simulation:

* GLS uses synthesized netlist
* includes standard cell timing behavior
* validates synthesis correctness

---

# GLS Compilation

```bash id="9z5oxw"
iverilog \
../my_lib/verilog_model/primitives.v \
../my_lib/verilog_model/sky130_fd_sc_hd.v \
design_netlist.v \
tb_design.v
```

---

# Run GLS

```bash id="99oyjlwm"
./a.out
```

---

# Open Waveform

```bash id="6n1n8q"
gtkwave tb_design.vcd
```

---

# RTL vs GLS

| RTL Simulation          | GLS                         |
| ----------------------- | --------------------------- |
| Behavioral              | Gate-level                  |
| Faster                  | More realistic              |
| Uses RTL                | Uses synthesized netlist    |
| No standard cells       | Uses SKY130 standard cells  |
| Functional verification | Post-synthesis verification |

---

# Important GLS Learning

GLS helps identify:

* synthesis mismatches
* latch inference
* incorrect hardware mapping
* post-synthesis behavior differences

---

# 16. Ripple Carry Adder using Generate

A Ripple Carry Adder (RCA) was implemented using:

* Full Adder module
* Generate blocks
* Hierarchical design
* Structural replication

This experiment demonstrates scalable hardware generation using FOR GENERATE.

---

# RCA RTL Design

![RCA RTL](rca_fa_rtl.png)

---

# RCA RTL Simulation

![RCA RTL Simulation](rca_simulation.png)

---

# RCA Synthesized Netlist

![RCA Netlist](rca_netlist.png)

---

# RCA Gate-Level Simulation

![RCA GLS](rca_netlist_simulation.png)

---

# Observations

1. Generate block replicated full adder hardware multiple times.
2. Carry propagation between stages is visible in the synthesized structure.
3. GLS output matches RTL simulation confirming correct synthesis behavior.

---

# RCA Hardware Understanding

The synthesized netlist clearly shows:

* hierarchical module instantiation
* repeated full adder blocks
* carry chain propagation
* structural hardware replication

This demonstrates how generate blocks help build scalable architectures efficiently.

---

# Final Observations

| Topic              | Observation                      |
| ------------------ | -------------------------------- |
| Incomplete IF      | Infers latch                     |
| Incomplete CASE    | Infers latch                     |
| Partial Assignment | Creates memory behavior          |
| Complete CASE      | Removes latch inference          |
| FOR Loop           | Behavioral iteration             |
| FOR GENERATE       | Hardware replication             |
| GLS                | Verifies synthesized hardware    |
| RCA Generate       | Demonstrates scalable design     |
| SKY130 Mapping     | Converts RTL into standard cells |

---

# Key Learnings

* Incomplete IF and CASE statements infer latches.
* CASE statements should include default assignments.
* Partial assignments create memory behavior.
* FOR loops evaluate logic behaviorally.
* FOR GENERATE replicates hardware structurally.
* Generate blocks simplify scalable hardware design.
* GLS verifies synthesized hardware correctness.
* SKY130 standard cells are mapped during synthesis.
* RTL and synthesized hardware may differ for poor coding styles.
* Proper RTL coding avoids unintended latch inference.

---

# Conclusion

Day 5 provided a strong understanding of:

* synthesis-aware RTL coding
* latch inference
* combinational vs sequential behavior
* generate-based scalable design
* RTL vs Gate-Level Simulation
* SKY130 technology mapping

The experiments demonstrated how Verilog coding style directly impacts synthesized hardware.

The day covered:

* IF constructs
* CASE constructs
* incomplete assignments
* overlapping conditions
* FOR loops
* FOR GENERATE
* MUX/DEMUX implementation
* hierarchical RCA design
* synthesis flow
* GLS verification

This establishes a strong foundation in RTL design optimization and synthesis using SKY130 technology.
