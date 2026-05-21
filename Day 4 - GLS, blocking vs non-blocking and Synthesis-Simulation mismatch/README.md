# Day 4 – GLS, Blocking vs Non-Blocking & Synthesis-Simulation Mismatch

Welcome to Day 4 of the **RTL Design and Synthesis Workshop using SKY130 PDK**.
This day focused on understanding how synthesized hardware behaves compared to RTL simulation and how improper Verilog coding styles can create mismatches between simulation and actual synthesized hardware.

---

# Table of Contents

* Gate Level Simulation (GLS)
* Synthesis-Simulation Mismatch
* Blocking vs Non-Blocking Assignments
* Labs and Experiments

  * Ternary Operator MUX
  * Good MUX vs Bad MUX
  * Blocking Assignment Caveat
* RTL vs Synthesized Netlists
* Gate-Level Schematics
* Simulation vs GLS Comparison
* Key Learnings

---

# 1. Gate Level Simulation (GLS)

## What is GLS?

GLS stands for:

# Gate Level Simulation

It is the process of simulating the synthesized gate-level netlist instead of RTL code.

In GLS:

* synthesized netlist becomes the DUT
* same testbench used for RTL can be reused
* synthesized hardware functionality is verified

---

# Why GLS is Needed

GLS helps to:

* verify synthesized hardware behavior
* compare RTL vs synthesized implementation
* detect synthesis-simulation mismatches
* validate logic correctness after synthesis
* analyze timing-aware behavior

---

# GLS Flow

```text
RTL / Synthesized Netlist
            ↓
        Icarus Verilog
            ↓
           VCD
            ↓
         GTKWave
```

---

# Types of GLS

## Functional GLS

* Logic-only verification
* Zero/unit delay simulation

## Timing GLS

* Uses SDF timing annotation
* Checks setup/hold timing behavior

---

# Important Observation

Simulator works based on:

# activity/events

Synthesizer focuses mainly on:

* hardware inference
* logic optimization

This difference can cause:

# Synthesis-Simulation Mismatch

---

# 2. Synthesis-Simulation Mismatch

A synthesis-simulation mismatch occurs when:

```text
RTL Simulation ≠ Synthesized Hardware Behavior
```

---

# Common Reasons

* Missing sensitivity list
* Incorrect blocking/non-blocking assignments
* Improper combinational coding style
* Ambiguous RTL coding
* Tool interpretation differences

---

# 3. Blocking vs Non-Blocking Assignments

---

# Blocking Assignment (`=`)

## Characteristics

* Executes sequentially
* Order matters
* Immediate assignment

## Used For

# Combinational Logic

## Example

```verilog
always @(*)
    y = a & b;
```

---

# Non-Blocking Assignment (`<=`)

## Characteristics

* Executes in parallel
* Updates scheduled together
* Mimics flip-flop behavior

## Used For

# Sequential Logic

## Example

```verilog
always @(posedge clk)
    q <= d;
```

---

# Comparison Table

| Blocking (`=`)              | Non-Blocking (`<=`)         |
| --------------------------- | --------------------------- |
| Sequential execution        | Parallel execution          |
| Immediate update            | Scheduled update            |
| Used in combinational logic | Used in sequential logic    |
| Order dependent             | Order independent           |
| Infers combinational gates  | Infers flip-flops/registers |

---

# 4. Labs & Experiments

---

# Lab 1 – Ternary Operator MUX

## RTL Code

```verilog
module ternary_operator_mux (
    input i0,
    input i1,
    input sel,
    output y
);

assign y = sel ? i1 : i0;

endmodule
```

---

# Observation

The ternary operator:

```verilog
condition ? TRUE : FALSE
```

implements:

# 2:1 Multiplexer

Behavior:

* `sel = 1` → `y = i1`
* `sel = 0` → `y = i0`

---

# RTL vs Synthesized Netlist

![RTL vs Synthesized Netlist](ternary_operator_mux_rtl_vs_synthesized_netlist.png)

### Observation

* RTL converted into SKY130 standard cells
* Yosys inferred `sky130_fd_sc_hd__mux2_1`
* Synthesized hardware matches intended RTL functionality

---

# Gate-Level Schematic

![Gate Level Netlist](ternary_operator_mux_netlist.png)

### Observation

* Inputs connected to mux standard cell
* Proper hardware inference observed
* SKY130 mux cell successfully generated

---

# RTL vs GLS Waveform

![Simulation vs Synthesis](ternary_operator_mux_simulation_vs_synthesis.png)

### Observation

* RTL and GLS outputs match correctly
* Functional verification successful
* No synthesis mismatch observed

---

# Lab 2 – Bad MUX Example

## RTL Code

```verilog
module bad_mux (
    input i0,
    input i1,
    input sel,
    output reg y
);

always @(sel)
begin
    if(sel)
        y <= i1;
    else
        y <= i0;
end

endmodule
```

---

# Problems in RTL

## Missing Sensitivity List

The always block triggers only when:

```text
sel changes
```

Changes in:

* `i0`
* `i1`

will not update output correctly.

---

# Incorrect Assignment Type

Using:

```verilog
<=
```

inside combinational logic is not recommended.

Combinational logic should use:

```verilog
=
```

---

# Correct RTL

```verilog
always @(*)
begin
    if(sel)
        y = i1;
    else
        y = i0;
end
```

---

# RTL vs Synthesized Netlist

![Bad MUX RTL vs Synthesized](bad_mux_rtl_vs_synthesized_netlist.png)

### Observation

* Synthesizer still infers proper mux hardware
* Hardware implementation remains correct
* Sensitivity list issue mainly affects simulation

---

# Simulation vs Synthesis

![Bad MUX Simulation vs Synthesis](bad_mux_simulation_vs_synthesis.png)

### Observation

* RTL simulation may fail to update output properly
* GLS behaves as expected
* Demonstrates synthesis-simulation mismatch

---

# Synthesized Schematic

![Bad MUX Netlist](bad_mux_netlist.png)

### Observation

* SKY130 mux cell inferred successfully
* Hardware generated correctly despite poor coding style

---

# Lab 3 – Good MUX Example

## RTL Code

```verilog
module good_mux (
    input i0,
    input i1,
    input sel,
    output reg y
);

always @(*)
begin
    if(sel)
        y = i1;
    else
        y = i0;
end

endmodule
```

---

# Why This Works

`@(*)` automatically includes:

* `i0`
* `i1`
* `sel`

This ensures:

* proper combinational logic
* correct output updates
* accurate simulation behavior

---

# RTL vs Synthesized Netlist

![Good MUX RTL vs Synthesized](good_mux_rtl_vs_synthesized_netlist.png)

### Observation

* Clean combinational RTL style
* Correct mux inference
* Synthesized netlist matches RTL intent

---

# Simulation vs GLS

![Good MUX Simulation vs Synthesis](good_mux_simulation_vs_synthesis.png)

### Observation

* RTL and GLS outputs match correctly
* No mismatch observed
* Proper combinational behavior verified

---

# Gate-Level Schematic

![Good MUX Netlist](good_mux_netlist.png)

### Observation

* SKY130 mux standard cell inferred correctly
* Functional hardware implementation verified

---

# Lab 4 – Blocking Assignment Caveat

## Problematic RTL

```verilog
module blocking_caveat (
    input a,
    input b,
    input c,
    output reg d
);

reg x;

always @(*)
begin
    d = x & c;
    x = a | b;
end

endmodule
```

---

# Problem

Execution occurs sequentially:

1. `d` uses old value of `x`
2. `x` updates afterward

Simulation becomes:

# order dependent

---

# Correct RTL

```verilog
always @(*)
begin
    x = a | b;
    d = x & c;
end
```

---

# Observation

Intermediate variables should always be assigned before usage.

---

# RTL vs Synthesized Netlist

![Blocking Caveat RTL vs Synthesized](blocking_caveat_rtl_vs_synthesized_netlist.png)

### Observation

* Synthesis optimizes logic into combinational gates
* Hardware still synthesized correctly
* Statement ordering affects simulation more than synthesis

---

# Simulation vs GLS

![Blocking Caveat Simulation vs Synthesis](blocking_caveat_simulation_vs_synthesis.png)

### Observation

* RTL simulation depends on statement ordering
* GLS behaves according to optimized hardware
* Blocking assignment caveat demonstrated clearly

---

# Gate-Level Schematic

![Blocking Caveat Netlist](blocking_caveat_netlist.png)

### Observation

* Optimized combinational logic generated
* Synthesized hardware simplified successfully

---

# Yosys Synthesis Flow

```text
read_liberty
read_verilog
synth -top
abc -liberty
write_verilog
```

---

# Tools Used

* Icarus Verilog (iverilog)
* GTKWave
* Yosys
* SKY130 Standard Cell Library
* GVim

---

# Repository Contents

This Day 4 repository includes:

* RTL Codes
* Testbenches
* RTL Simulation Waveforms
* GLS Waveforms
* RTL vs Synthesized Netlists
* Gate-Level Schematics
* SKY130 Standard Cell Implementations
* Handwritten Notes
* Lab Outputs and Observations

---

# Key Learnings

* GLS verifies synthesized hardware behavior.
* Missing sensitivity lists can cause simulation mismatch.
* Synthesizer and simulator interpret RTL differently.
* Blocking assignments are order dependent.
* Non-blocking assignments are preferred for sequential logic.
* `always @(*)` should be used for combinational logic.
* Proper RTL coding style improves predictability and hardware correctness.
* SKY130 standard cells are inferred during synthesis.

---

# Summary

Day 4 provided practical understanding of:

* Gate Level Simulation (GLS)
* Synthesized netlist behavior
* RTL vs synthesized hardware comparison
* Blocking and non-blocking assignments
* Simulation mismatch causes
* Verilog coding pitfalls
* Proper RTL coding practices using SKY130 libraries

along with hands-on labs, waveform analysis, synthesized netlists, and gate-level schematics.
