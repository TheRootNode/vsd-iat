<details>
<summary> Day 1-Introduction to Verilog RTL design and Synthesis </summary>


<details>
<summary> Introduction to open-source simulator iverilog </summary>
## 2-SKY130RTL D1SK1 L1 Introduction to iverilog design test bench

### 🔹 Simulator
A **simulator** is a software tool that mimics how an RTL design behaves over time without fabricating hardware.  
- Inputs: Design (DUT) + Testbench  
- Processes: Compiles and executes the RTL  
- Outputs: Logs, reports, and waveform files (showing how signals evolve)  

*Example:* iverilog compiles the RTL, and vvp executes it, generating a `.vcd` file viewable in GTKWave.  

---

### 🔹 Design (DUT)
The **design** is the RTL description of the hardware circuit, written in Verilog, VHDL, or SystemVerilog.  
- Defines structure and functionality.  
- Synthesizable into gates and layout.  

Example:  
```verilog
module and_gate(input a, input b, output y);
    assign y = a & b;
endmodule
```
---

🔹 Testbench

A testbench is a non-synthesizable RTL environment used to verify the design.

Generates stimulus signals.
Monitors outputs.
Dumps activity for waveform analysis.

Example:
```verilog
module and_gate_tb;
    reg a, b;
    wire y;

    and_gate dut (.a(a), .b(b), .y(y));

    initial begin
        $dumpfile("and_gate.vcd");
        $dumpvars(0, and_gate_tb);

        a=0; b=0; #10;
        a=0; b=1; #10;
        a=1; b=0; #10;
        a=1; b=1; #10;

        $finish;
    end
endmodule

```
---
🔹 General Simulation Flow
```mermaid
flowchart LR
    A[Design (RTL)] --> C[Simulator]
    B[Testbench] --> C[Simulator]
    C --> D[Outputs: logs / waveforms / coverage]
```
---
🔹 Testbench Block Diagram
```mermaid
flowchart LR
    subgraph TB[Testbench]
        A[Stimulus Generator] --> B[Design (DUT)]
        B --> C[Stimulus Observer]
    end

```
---
🔹 Iverilog + GTKWave Simulation Flow
```mermaid
flowchart TB
    A[Design (RTL)] --> C[iverilog]
    B[Testbench] --> C[iverilog]
    C --> D[vcd file]
    D --> E[GTKWave]
```
---
🔹 Example Workflow with Open-Source Tools
```bash
# Step 1: Compile design + testbench
iverilog -o sim.out design.v testbench.v

# Step 2: Run simulation
vvp sim.out

# Step 3: View waveforms
gtkwave design.vcd

```
</details>

<details>
<summary> Labs using iverilog and gtkwave </summary>
<details>
<summary> 3-SKY130RTL D1SK2 L1 Lab1 introduction to lab </summary>

</details>

<details>
<summary> 4-SKY130RTL D1SK2 L2 Lab2 Introduction iverilog gtkwave part1 </summary>

</details>

<details>
<summary> 5-SKY130RTL D1SK2 L3 Lab2 Introduction iverilog gtkwave part2 </summary>

</details>
</details>

<details>
<summary> Introduction to Yosys and Logic synthesis </summary>
<details>
<summary> 6-SKY130RTL D1SK3 L1 Introduction to yosys </summary>

</details>

<details>
<summary> 7-SKY130RTL D1SK3 L2 introduction to logic synthesis part1 </summary>

</details>

<details>
<summary> 8-SKY130RTL D1SK3 L3 introduction to logic synthesis part2 </summary>

</details>
</details>

<details>
<summary> Labs using Yosys and Sky130 PDKs </summary>
<details>
<summary> 9-SKY130RTL D1SK4 L1 Lab3 Yosys 1 good mux Part1 </summary>

</details>

<details>
<summary> 10-SKY130RTL D1SK4 L2 Lab3 Yosys 1 good mux Part2 </summary>

</details>

<details>
<summary> 11-SKY130RTL D1SK4 L3 Lab3 Yosys 1 good mux Part3 </summary>

</details>
</details>
</detils>

