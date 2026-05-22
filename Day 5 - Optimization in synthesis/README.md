# Day 5 – Optimization in Synthesis

Welcome to **Day 5** of the **RTL Design and Synthesis Workshop using SKY130 PDK**.
This session focused on how RTL coding styles affect synthesized hardware, especially regarding:

* Latch inference
* Priority logic
* Case statement behavior
* Overlapping conditions
* FOR loops
* Generate blocks
* Hardware replication during synthesis

---

# Table of Contents

1. If Case Constructs
2. Labs on Incomplete IF
3. Labs on Incomplete Overlapping CASE
4. FOR Loop and FOR Generate
5. Labs on FOR and Generate
6. Key Learnings
7. Conclusion

---

# SKY130RTL D5SK1 – If Case Constructs

Conditional constructs in Verilog are used to model combinational and sequential logic.

| Construct     | Purpose                     |
| ------------- | --------------------------- |
| `if`          | Conditional decision making |
| `if-else`     | Priority logic              |
| `case`        | Multi-way branching         |
| `casez/casex` | Wildcard matching           |

---

## IF Statement Example

```verilog
module if_example(
    input a,
    input b,
    input sel,
    output reg y
);

always @(*) begin
    if(sel)
        y = a;
    else
        y = b;
end

endmodule
```

---

## Hardware Interpretation

The synthesizer infers:

* A **2:1 multiplexer**
* `sel` acts as select line

---

# Priority Logic in IF-ELSE

`if-else` statements create **priority-based hardware**.

```verilog
if(cond1)
    y = a;
else if(cond2)
    y = b;
else
    y = c;
```

### Priority Order

```text
cond1 > cond2 > else
```

Only the highest priority matching condition executes.

---

# SKY130RTL D5SK2 – Labs on Incomplete IF

Incomplete assignments inside combinational blocks infer **latches**.

---

# SKY130RTL D5SK2 L1 – Incomplete IF Part 1

## RTL Code

```verilog
module incomplete_if(
    input i0,
    input i1,
    input sel,
    output reg y
);

always @(*) begin
    if(sel)
        y = i1;
end

endmodule
```

---

## Problem

When `sel = 0`:

* `y` is not assigned
* Previous output value must be retained

Therefore:

* Synthesizer infers a **latch**

---

## Why Latch is Inferred

The hardware needs memory to preserve the previous value.

```text
Output retains previous state
→ Memory required
→ Latch inferred
```

---

# SKY130RTL D5SK2 L2 – Incomplete IF Part 2

## Corrected Version

```verilog
module complete_if(
    input i0,
    input i1,
    input sel,
    output reg y
);

always @(*) begin
    if(sel)
        y = i1;
    else
        y = i0;
end

endmodule
```

---

## Result

* No latch inferred
* Pure combinational logic
* Synthesizes to a multiplexer

---

# Important Rule

For combinational logic:

```text
Assign outputs in ALL possible conditions
```

Otherwise latches are inferred.

---

# SKY130RTL D5SK3 – Labs on Incomplete Overlapping CASE

CASE statements can also infer latches or priority logic when improperly coded.

---

# SKY130RTL D5SK3 L1 – Incomplete CASE Part 1

## RTL Example

```verilog
module incomplete_case(
    input [1:0] sel,
    input i0,
    input i1,
    output reg y
);

always @(*) begin
    case(sel)
        2'b00 : y = i0;
        2'b01 : y = i1;
    endcase
end

endmodule
```

---

## Issue

Cases:

```text
10 and 11
```

are missing.

Hence:

* Output retains previous value
* Latch inferred

---

# SKY130RTL D5SK3 L2 – Incomplete CASE Part 2

## Using DEFAULT

```verilog
always @(*) begin
    case(sel)
        2'b00 : y = i0;
        2'b01 : y = i1;
        default : y = 1'b0;
    endcase
end
```

---

## Result

* Eliminates latch inference
* Generates complete combinational logic

---

# SKY130RTL D5SK3 L3 – Overlapping CASE Part 3

## Overlapping Conditions

```verilog
casez(sel)
    2'b1? : y = a;
    2'b?1 : y = b;
endcase
```

For some inputs, both conditions may match.

---

## Result

Synthesizer introduces:

* Priority logic
* Additional combinational hardware

This may increase:

* Area
* Delay

---

# SKY130RTL D5SK3 L4 – Overlapping CASE Part 4

## Key Observation

Poorly written case statements can create:

* Latches
* Priority encoders
* Unnecessary hardware

Careful RTL coding avoids optimization issues.

---

# SKY130RTL D5SK4 – FOR Loop and FOR Generate

Verilog supports iterative constructs for scalable hardware design.

---

# FOR Loop vs GENERATE

| Feature            | FOR Loop            | GENERATE                    |
| ------------------ | ------------------- | --------------------------- |
| Used Inside        | `always` block      | Outside procedural block    |
| Purpose            | Repeated evaluation | Hardware replication        |
| Synthesizer Action | Loop unrolling      | Multiple hardware instances |

---

# SKY130RTL D5SK4 L1 – FOR Loop Part 1

## Procedural FOR Loop

```verilog
integer i;

always @(*) begin
    for(i = 0; i < 4; i = i + 1)
        out[i] = a[i] & b[i];
end
```

---

## Synthesized Logic

Equivalent hardware:

```text
out[0] = a[0] & b[0]
out[1] = a[1] & b[1]
out[2] = a[2] & b[2]
out[3] = a[3] & b[3]
```

The loop is fully unrolled during synthesis.

---

# SKY130RTL D5SK4 L2 – FOR GENERATE Part 2

## Generate Block Example

```verilog
genvar i;

generate
    for(i = 0; i < 4; i = i + 1) begin
        and(out[i], a[i], b[i]);
    end
endgenerate
```

---

## Result

* Replicates hardware
* Instantiates multiple AND gates

Useful for:

* Ripple carry adders
* Array structures
* Parameterized RTL

---

# SKY130RTL D5SK4 L3 – FOR GENERATE Part 3

## Key Difference

### FOR Loop

```text
Evaluates logic repeatedly
```

### GENERATE

```text
Replicates actual hardware
```

---

# SKY130RTL D5SK5 – Labs on FOR Loop and GENERATE

Hands-on labs demonstrated synthesis behavior of loops and generate blocks.

---

# SKY130RTL D5SK5 L1 – Lab FOR Loop Part 1

Observation:

* FOR loops inside `always` blocks become repeated combinational logic.

---

# SKY130RTL D5SK5 L2 – Lab FOR GENERATE Part 2

Observation:

* Generate blocks instantiate multiple hardware copies.

---

# SKY130RTL D5SK5 L3 – Lab FOR and GENERATE Part 3

Key learning:

* Generate constructs improve RTL scalability and readability.

---

# SKY130RTL D5SK5 L4 – Lab FOR and GENERATE Part 4

Final synthesis observations:

* Loops are completely unrolled
* No runtime looping hardware exists
* Larger loops increase hardware area

---

# Common RTL Coding Mistakes

| Mistake                   | Effect           |
| ------------------------- | ---------------- |
| Missing ELSE              | Latch inference  |
| Missing DEFAULT           | Latch inference  |
| Overlapping CASE          | Priority logic   |
| Partial assignment        | Unwanted latches |
| Excessive generate blocks | Larger area      |

---

# Key Learnings

| Topic            | Learning               |
| ---------------- | ---------------------- |
| Incomplete IF    | Infers latch           |
| Incomplete CASE  | Infers latch           |
| DEFAULT case     | Prevents latch         |
| Overlapping CASE | Creates priority logic |
| FOR loop         | Loop unrolling         |
| GENERATE         | Hardware replication   |

---

# Important Takeaways

* Every combinational output must be assigned in all conditions.
* Incomplete conditional statements infer latches.
* CASE statements should include `default`.
* Overlapping CASE conditions create priority hardware.
* FOR loops are unrolled during synthesis.
* GENERATE blocks replicate hardware structures.

---

# Conclusion

Day 5 focused on how RTL coding style directly impacts synthesized hardware.
Through various labs, we observed how incomplete conditions infer latches, how overlapping cases create priority logic, and how loops expand into replicated hardware structures during synthesis.

Understanding these concepts is essential for writing clean, optimized, and synthesis-friendly RTL for ASIC implementation using the SKY130 PDK.

