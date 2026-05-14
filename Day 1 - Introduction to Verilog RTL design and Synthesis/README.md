# Day 1 – Introduction to Verilog RTL Design and Synthesis

Day 1 focused on understanding the complete RTL design flow using open-source VLSI tools.

The goal was to learn:

* How Verilog RTL describes digital hardware
* How simulation verifies functionality
* How synthesis converts RTL into gate-level hardware
* How Sky130 standard cells are used during synthesis

---

# 1. Understanding Simulator, Design, and Testbench

Before implementing any digital hardware physically, the design must first be verified in software.
In RTL design flow, three important components are used:

* Design
* Simulator
* Testbench

These components help ensure that the digital circuit behaves correctly before synthesis and hardware fabrication.

---

## Simulator

A simulator is a software tool used to verify whether the RTL design behaves according to the required functionality.

Instead of directly fabricating hardware, the design is first simulated by applying different input combinations and observing the outputs.

### What does the simulator do?

* Applies input signals to the design
* Continuously monitors changes in inputs
* Evaluates outputs whenever inputs change
* Detects logical and functional errors
* Verifies whether the design satisfies the required specification

### Important Observation

* If input changes → output gets evaluated
* If input does not change → output remains the same

Simulation helps engineers debug and verify the circuit before actual hardware implementation.

### Why is simulation important?

Simulation helps:

* Save hardware cost
* Detect design mistakes early
* Verify logic functionality
* Debug circuits easily using waveforms

---

## Design

The design is the actual Verilog RTL code implementing the required digital logic functionality.

RTL (Register Transfer Level) describes how the digital circuit behaves.

### RTL Representation

```text
RTL Code → Digital Logic Circuit
```

The RTL code defines:

* Inputs
* Outputs
* Internal logic behavior
* Data flow inside the circuit

### Example Used in This Lab

The `good_mux.v` file implements a 2:1 multiplexer.

* When `sel = 0` → output follows `i0`
* When `sel = 1` → output follows `i1`

### RTL Design File


![RTL Design File](good_mux.v.png)

---

## Testbench

A testbench is used to apply stimulus (test vectors) to the RTL design and verify whether the outputs are correct.

The testbench acts like a virtual environment around the design.

### Functions of Testbench

* Generates input signals
* Applies different test conditions
* Observes output behavior
* Automates functional verification

### Verification Flow

```text
Stimulus Generator → Design → Output Observer
```

### Why is a testbench required?

RTL code alone cannot confirm whether the circuit works correctly.

The testbench helps:

* Verify functionality automatically
* Apply multiple input combinations
* Check expected outputs
* Observe signal transitions during simulation

### Important Notes

* A design may contain one or more primary inputs and outputs
* The testbench itself does not have primary inputs or outputs
* The testbench wraps around the design and drives the simulation

### Testbench Overview

![Testbench Overview](TESTBENCH.png)

### Testbench File

![Testbench File](tb_good_mux.v.png)

After verifying the RTL functionality using simulation and testbench, the next step is converting the RTL design into gate-level hardware using synthesis tools like Yosys.


# 2. RTL Simulation using Iverilog and GTKWave

## Simulation Flow

```text
Design + Testbench  
↓  
Iverilog  
↓  
VCD File  
↓  
GTKWave
````

### Why are we doing simulation?

Simulation helps verify whether the RTL design behaves correctly before synthesis and hardware implementation.

### Why GTKWave?

GTKWave is used to visualize waveform activity stored inside `.vcd` files.

Waveforms help observe:

* Input changes
* Output transitions
* Timing behavior
* Functional correctness

### Simulation Flow Diagram

![Simulation Flow](simulation_flow.png)

---

## Commands Used

### Clone the Workshop Repository

```bash
git clone https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git
```

#### Purpose

* Downloads the complete workshop files from GitHub
* Includes Verilog designs, testbenches, and libraries
* Creates a local working directory for the lab

---

### Move to Verilog Files Directory

```bash
cd sky130RTLDesignAndSynthesisWorkshop/verilog_files
```

#### Purpose

* Enters the directory containing Verilog source files
* Provides access to design and testbench files
* Required before compilation and simulation

---

### Compile the Design and Testbench

```bash
iverilog good_mux.v tb_good_mux.v
```

#### Purpose

* Compiles both RTL design and testbench
* Checks Verilog syntax correctness
* Creates simulation executable (`a.out`)
* Prepares the design for simulation

#### Files Used

* `good_mux.v` → RTL design
* `tb_good_mux.v` → Testbench

---

### Run the Simulation

```bash
./a.out
```

#### Purpose

* Executes the compiled simulation
* Applies stimulus from the testbench
* Evaluates design outputs
* Generates waveform dump file (`.vcd`)

#### Output Generated

* `tb_good_mux.vcd`

---

### View Waveforms using GTKWave

```bash
gtkwave tb_good_mux.vcd
```

#### Purpose

* Opens waveform viewer
* Displays signal transitions with time
* Helps verify RTL functionality visually
* Used for debugging and analysis

---

## Verilog Design – good_mux

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
        y <= i1;
    else
        y <= i0;
end

endmodule
```

### RTL Design File

![RTL Design File](good_mux.v.png)

---

### Observation

* When `sel = 0`, output follows `i0`
* When `sel = 1`, output follows `i1`
* Output changes immediately whenever inputs change
* Multiplexer functionality verified successfully

---

## GTKWave Output

The waveform output confirms the correct working of the 2:1 multiplexer.

### Waveform Observation

* `sel` controls which input reaches the output
* Output `y` follows either `i0` or `i1`
* Signal transitions match expected RTL behavior

![GTKWave Output](output_of_tb_good_mux.vcd.png)

---

```
```
# 3. Introduction to Logic Synthesis

RTL code only describes the behavior of a digital circuit.
However, real hardware implementation requires actual logic gates and standard cells.

Synthesis is the process of converting RTL Verilog code into gate-level hardware representation.

Real hardware implementation requires:

* Logic gates
* Standard cells
* Physical hardware realization

During synthesis, the RTL design and standard cell library are provided to Yosys, which converts the RTL description into a gate-level netlist.

---

## Synthesis Flow

![Synthesis Flow](yosys_setup.png)

---

## Why Synthesis is Required

RTL code cannot be directly fabricated into hardware.

The synthesis tool:

* Reads the RTL Verilog code
* Understands the logic functionality
* Optimizes the logic implementation
* Maps the design into available standard cells
* Generates a gate-level netlist

The generated netlist represents the actual hardware implementation of the design.

---

## Standard Cell Library

A standard cell library (`.lib`) contains definitions of real hardware cells used during implementation.

These cells include:

* AND gates
* OR gates
* NAND gates
* NOR gates
* Multiplexers
* Buffers
* Inverters
* Flip-flops

The library also contains:

* Timing information
* Power information
* Area information
* Drive strengths of cells

These standard cells act as the building blocks for digital chip design.

---

## Why Different Cell Flavors Exist

Different digital circuits require optimization tradeoffs between:

* Speed
* Power
* Area

The same logic gate may exist in multiple versions with different characteristics.

Example:

* Fast AND gate
* Medium-speed AND gate
* Slow AND gate

This allows the synthesis tool to choose cells according to timing requirements.

---

## Fast Cells vs Slow Cells

### Fast Cells

Fast cells:

* Reduce propagation delay
* Improve circuit performance
* Charge/discharge capacitance quickly
* Use wider transistors
* Consume more power
* Occupy larger silicon area

### Slow Cells

Slow cells:

* Consume lower power
* Occupy smaller area
* Use narrower transistors
* Have larger delay
* Help fix hold timing issues

The synthesis tool selects suitable cells depending on timing and design constraints.

---

## Timing Concepts

The operating speed of a digital circuit depends on the delay through the combinational logic path between flip-flops.

### Setup Timing

`Tclk > Tcq-A + Tcomb + Tsetup-B`

This condition ensures that data reaches the destination flip-flop before the next clock edge.

If setup timing is violated:

* Incorrect data may get captured
* Circuit may fail at higher frequencies

---

### Hold Timing

`Thold-B < Tcq-A + Tcomb`

This condition ensures that data remains stable for a short duration after the clock edge.

If hold timing is violated:

* Old data may get corrupted
* Unstable behavior may occur

---

### Maximum Operating Frequency

`fmax = 1 / Tclk(min)`

The clock period determines the maximum speed at which the circuit can operate.

Smaller clock period:

* Higher operating frequency
* Higher performance requirements

---

## Important Observations

* Faster cells help meet setup timing requirements
* Slower cells help avoid hold timing violations
* Using only fast cells increases power and area
* Using only slow cells may reduce performance

Hence, synthesis tools balance:

* Timing
* Area
* Power

while selecting cells from the standard cell library.

---

After understanding synthesis concepts and timing behavior, the next step is performing RTL synthesis using Yosys and generating the gate-level netlist.

# 4. Synthesis using Yosys

Yosys is an open-source synthesis tool used to convert RTL Verilog code into gate-level netlists using standard cell libraries.

During synthesis:
- RTL functionality is analyzed
- Logic is optimized
- Standard cells are selected
- Hardware netlist is generated

---

## Yosys Setup Flow

![Yosys Setup](synthesis.png)

---

## Commands Used

### Start Yosys

```bash
yosys
````

#### Purpose

* Launches the Yosys synthesis environment
* Allows execution of synthesis commands
* Initializes synthesis flow

---

### Read Liberty File

```bash
read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```

#### Purpose

* Loads Sky130 standard cell library
* Provides timing and cell information
* Makes hardware cells available for synthesis

#### Observation

* Library contains multiple hardware cell types
* Cells include gates, buffers, multiplexers, and flip-flops

---

### Read Verilog Design

```bash
read_verilog good_mux.v
```

#### Purpose

* Reads RTL Verilog design into Yosys
* Converts RTL into internal representation
* Prepares design for synthesis

---

### Synthesize Top Module

```bash
synth -top good_mux
```

#### Purpose

* Performs RTL synthesis
* Optimizes logic implementation
* Identifies the top module of the design
* Generates intermediate synthesized logic

---

### Technology Mapping

```bash
abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```

#### Purpose

* Maps synthesized RTL into Sky130 standard cells
* Performs logic optimization
* Selects appropriate hardware cells from library

---

### View Synthesized Circuit

```bash
show
```

#### Purpose

* Displays graphical representation of synthesized circuit
* Helps visualize gate-level implementation
* Shows mapped standard cells

### Synthesized Netlist Diagram

![Synthesized Netlist](netlist.png)

---

### Generate Netlist

```bash
write_verilog good_mux_netlist.v
```

#### Purpose

* Generates gate-level Verilog netlist
* Stores synthesized hardware implementation
* Used for post-synthesis verification

---

### Generate Simplified Netlist

```bash
write_verilog -noattr good_mux_netlist.v
```

#### Purpose

* Removes synthesis attributes
* Produces cleaner and more readable netlist
* Easier for analysis and debugging

---

# 5. Post-Synthesis Verification

After synthesis, the generated gate-level netlist is verified again using the same testbench.

This ensures that synthesis has not changed the functionality of the design.

---

## Verification Flow

![Post-Synthesis Verification](verify_the_system.png)

---

## Why Post-Synthesis Verification is Required

During synthesis:

* Logic gets optimized
* Gates may be rearranged internally
* Different standard cells may be selected

Even after optimization, the output behavior must remain identical to RTL simulation.

Post-synthesis verification helps:

* Confirm functional correctness
* Compare RTL and synthesized outputs
* Detect synthesis-related issues
* Validate gate-level implementation

---

## Observation

* Synthesized netlist produced correct functionality
* Output waveforms matched RTL simulation results
* Multiplexer behavior remained unchanged after synthesis

---

# Tools Used

* Iverilog
* GTKWave
* Yosys
* Sky130 Standard Cell Library
* GVim

---

# Day 1 Summary

In Day 1, I learned the complete RTL-to-gate-level design flow using open-source VLSI tools.

### Key Learnings

* Understanding simulator, design, and testbench
* RTL simulation using Iverilog
* Waveform analysis using GTKWave
* Introduction to logic synthesis
* Importance of standard cell libraries
* Setup and hold timing concepts
* Fast cells vs slow cells
* Synthesis using Yosys
* Technology mapping using Sky130 libraries
* Gate-level netlist generation
* Post-synthesis verification

This day provided the foundation for understanding how Verilog RTL is converted into actual digital hardware implementation.

---

# Workshop Repository

Original workshop repository:

[https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop](https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop)

```
```


