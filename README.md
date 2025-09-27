<p align="center">
  <img src="https://img.shields.io/badge/⚡%20VSD--IAT-vlsisystemdesign.com-8A2BE2?style=for-the-badge&logo=riscv&logoColor=white" alt="vsd-iat banner">
</p>

<p align="center">
  <img src="https://img.shields.io/github/repo-size/TheRootNode/vsd-iat?color=blue&label=Repo%20Size&style=flat-square" alt="repo size">
  <img src="https://img.shields.io/github/license/TheRootNode/vsd-iat?color=brightgreen&style=flat-square" alt="license">
  <img src="https://img.shields.io/github/last-commit/TheRootNode/vsd-iat?color=yellow&style=flat-square" alt="last commit">
</p>

<p align="center">
It’s not just a repo of code and configs, but a playground for <b> experiments, notes, logs, and artifacts .</b> <br> <i>📖 Documenting the process in an open, transparent, and reproducible way.</i>
</p>

<p align="center">
  <img src="./Tools_installation/assets/sysinfo.png" alt="System Information" width="600">
</p>

---
<details>
<summary> Tools Installation & Environment Setup </summary>
  <hr style="border-top: 1px dotted #bbb;" />
🔹 Yosys – Logic Synthesis Tool
  
An open-source synthesis framework used to convert Verilog RTL into gate-level netlists. It’s the backbone of open-source digital design flows and works with standard cell libraries like Sky130.

```bash
sudo apt-get update
git clone https://github.com/YosysHQ/yosys.git
cd yosys
sudo apt install make build-essential clang bison flex \
    libreadline-dev gawk tcl-dev libffi-dev git \
    graphviz xdot pkg-config python3 libboost-system-dev \
    libboost-python-dev libboost-filesystem-dev zlib1g-dev
make config-gcc
make
sudo make install
```
<p align="center">
  <img src="./Tools_installation/assets/yosys.png" alt="System Information" width="600">
</p>

<hr style="border-top: 1px dotted #bbb;" />

🔹 Icarus Verilog (iverilog) – RTL Simulation

Icarus Verilog is a Verilog simulator that lets you compile and run HDL code, often paired with GTKWave for waveform viewing.

```bash
sudo apt-get update
sudo apt-get install iverilog
```
<p align="center">
  <img src="./Tools_installation/assets/iverilog.png" alt="System Information" width="600">
</p>

<hr style="border-top: 1px dotted #bbb;" />

🔹 GTKWave – Waveform Viewer

GTKWave is a signal waveform viewer used to visualize VCD/EVCD files generated during simulation.

```bash
sudo apt-get update
sudo apt-get install gtkwave
```
<p align="center">
  <img src="./Tools_installation/assets/gtkwave.png" alt="System Information" width="600">
</p>
<hr style="border-top: 1px dotted #bbb;" />

🔹 Ngspice – Circuit Simulator

Ngspice is an analog/mixed-signal circuit simulator, widely used for SPICE netlists, device models, and transistor-level verification.

```bash
# Download ngspice (ngspice-xx.tar.gz) from SourceForge

tar -xvzf ngspice-xx.tar.gz
cd ngspice-xx
mkdir release && cd release
../configure --with-x --with-readline=yes --disable-debug
make
sudo make install

```
<p align="center">
  <img src="./Tools_installation/assets/ngspice.png" alt="System Information" width="600">
</p>
<hr style="border-top: 1px dotted #bbb;" />

🔹 Magic VLSI – Layout Editor

Magic is a classic VLSI layout editor and DRC tool, used for viewing, editing, and validating IC layouts.

```bash
sudo apt-get install m4 tcsh csh libx11-dev tcl-dev tk-dev \
    libcairo2-dev mesa-common-dev libglu1-mesa-dev libncurses-dev
git clone https://github.com/RTimothyEdwards/magic
cd magic
./configure
make
sudo make install
```
<p align="center">
  <img src="./Tools_installation/assets/magic.png" alt="System Information" width="600">
</p>
<hr style="border-top: 1px dotted #bbb;" />

🔹 OpenLane – RTL to GDSII Flow

OpenLane is a fully automated digital design flow (built on OpenROAD + Yosys + Magic + KLayout) that takes RTL → GDSII. It’s the main open-source toolchain for tapeouts.

```bash
sudo apt-get update && sudo apt-get upgrade -y
sudo apt install -y build-essential python3 python3-venv python3-pip make git \
    apt-transport-https ca-certificates curl software-properties-common

# Install Docker
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | \
  sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] \
https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io

# Test Docker
sudo docker run hello-world

# Add user to docker group
sudo groupadd docker
sudo usermod -aG docker $USER
sudo reboot

# After reboot
docker run hello-world

# Install OpenLane
cd $HOME
git clone https://github.com/The-OpenROAD-Project/OpenLane
cd OpenLane
make
make test
```
A successful run of the `spm` design using **Sky130A PDK**.  
Key outputs are stored under [`Tools_installation/assets/openlane-spm/`](Tools_installation/assets/openlane-spm/).

- [Final GDSII (Magic)](Tools_installation/assets/openlane-spm/gds/spm.gds)
- [Final LEF](Tools_installation/assets/openlane-spm/lef/spm.lef)
- [Final Verilog](Tools_installation/assets/openlane-spm/verilog/gl/spm.v)
</details>

---
## <font color=""> 𝐖𝐞𝐞𝐤 𝟏</font>
<details>
<summary> <font color="#00f7ffff">Day 1-Introduction to Verilog RTL design and Synthesis </font></summary>
 <details>
  <summary> <font color="skyblue">Introduction to open-source simulator iverilog </font></summary>

  ### 2-SKY130RTL D1SK1 L1 Introduction to iverilog design test bench

  #### 🔹 Simulator
  A **simulator** is a software tool that mimics how an RTL design behaves over time without fabricating hardware.  
  - Inputs: Design (DUT) + Testbench  
  - Processes: Compiles and executes the RTL  
  - Outputs: Logs, reports, and waveform files (showing how signals evolve)  

  *Example:* iverilog compiles the RTL, and vvp executes it, generating a `.vcd` file viewable in GTKWave.  

  ---

  #### 🔹 Design (DUT)
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

  #### 🔹 Testbench

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
 #### 🔹 General Simulation Flow

  ```
+----------------+        +-----------+        +-----------------------------+
|  Design (RTL)  | -----> | Simulator | -----> | Logs / Waveforms / Coverage |
+----------------+        +-----------+        +-----------------------------+
           ^
           |
+----------------+
|   Testbench    |
+----------------+
  ```
  ---
####  🔹 Testbench Block Diagram

  ```
 +--------------------- Testbench ----------------------+
|                                                      |
|  +--------------------+       +-------------------+  |
|  | Stimulus Generator | ----> |   Design (DUT)    |  |
|  +--------------------+       +-------------------+  |
|                                   |                  |
|                                   v                  |
|                        +-------------------------+   |
|                        |     Stimulus Observer   |   |
|                        +-------------------------+   |
|                                                      |
+------------------------------------------------------+
  ```
  ---
####  🔹 Iverilog + GTKWave Simulation Flow

  ```
+------------+       +----------+       +-----------+       +---------+
|  RTL (v)   | ----> | iverilog | ----> |  vvp run  | --->  |  VCD    |
+------------+       +----------+       +-----------+       +---------+
       ^                                                       |
       |                                                       v
+------------+                                            +---------+
| Testbench  |------------------------------------------> | GTKWave |
+------------+                                            +---------+

  ```
  ---
####  🔹 Example Workflow with Open-Source Tools
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
  <summary><font color="skyblue">Labs using iverilog and gtkwave</font></summary>
  <details>
    <summary><font color="#b69b72">Introduction to lab</font></summary>

###  Sky130 RTL Design and Synthesis Workshop (Working Repo)

This repository is a **working clone** of the  **[Sky130 RTL Design and Synthesis Workshop](https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop)**  
by [Kunal Ghosh](https://github.com/kunalg123).  

## 📂 Steps to Setup

#### 1️⃣ Create a workspace directory
```bash
mkdir vsdworkshop
cd vsdworkshop
```
#### 2️⃣ Clone the workshop repo
```bash
git clone https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git
```
This creates a folder:
```bash
sky130RTLDesignAndSynthesisWorkshop/
```
#### 3️⃣ Explore the repo structure
```bash
cd sky130RTLDesignAndSynthesisWorkshop/
ls
```
```bash
lib/    my_lib/    README.md    verilog_files/    yosys_run.sh
```
<p align="center">
<img src="./Week-1/Day-1/Labs_using_iverilog_and_gtkwave/lab.png">
</p>
</details>

<details>
    <summary><font color="#b69b72">Introduction iverilog gtkwave part1</font></summary>
    
### Verilog Simulation of MUX (Sky130 Workshop)


### 📂 Files Used
- **Design File:** `good_mux.v`  
- **Testbench:** `tb_good_mux.v`  
- **Generated Output:** `tb_good_mux.vcd`  

---

### ⚙️ Steps to Run
<p align="center">
<img src="./Week-1/Day-1/Labs_using_iverilog_and_gtkwave/terminal.png">
</p>

#### 1️⃣ Compile Design + Testbench
```bash
iverilog good_mux.v tb_good_mux.v
```
#### 2️⃣ Run Simulation
```bash
./a.out
```
Output:
```bash
VCD info: dumpfile tb_good_mux.vcd opened for output.
tb_good_mux.v:23: $finish called at 300000 (1ps)
```
#### 3️⃣ Open Waveform in GTKWave
```bash
gtkwave tb_good_mux.vcd
```
<p align="center">
<img src="./Week-1/Day-1/Labs_using_iverilog_and_gtkwave/mux.png">
</p>
i0 → Input 0 of the MUX (wire) |
i1 → Input 1 of the MUX (wire) |
sel → Select line (wire)       |
y → Output of the MUX (reg)    |

### Multiplexer Truth Table

For a 2:1 multiplexer:

| sel | y  |
| --- | -- |
| 0   | i0 |
| 1   | i1 |

## Waveform Explanation (0 → 300 ns)

| Time Range (approx) | sel | Output Behavior (y) | Explanation |
|---------------------|-----|---------------------|-------------|
| 0 – 100 ns          | 0   | y = i0              | When `sel=0`, output follows input `i0`. |
| 100 – 200 ns        | 1   | y = i1              | When `sel=1`, output follows input `i1`. |
| 200 – 300 ns        | 0   | y = i0              | `sel` switches back to 0, so output follows `i0` again. |

✅ This confirms that the **good_mux** design is functioning correctly.  

</details>

<details>
    <summary><font color="#b69b72">Introduction iverilog gtkwave part2</font></summary>

To view or edit the design files:
```bash
<Location of the verilog_files>
gedit good_mux.v
gedit tb_good_mux.v
```
<p align="center">
<img src="./Week-1/Day-1/Labs_using_iverilog_and_gtkwave/module.png">
</p>

| Aspect        | Details                                                                 |
|---------------|-------------------------------------------------------------------------|
| **Inputs**    | `i0`, `i1` (data), `sel` (select line)                                  |
| **Output**    | `y` → declared as **reg** (since assigned inside an `always` block)     |
| **Logic**     | - If `sel = 1` → `y = i1` <br> - If `sel = 0` → `y = i0`                 |
| **Coding Style** | - Uses `always @(*)` → proper combinational block <br> - No latches inferred |

<p align="center">
<img src="./Week-1/Day-1/Labs_using_iverilog_and_gtkwave/tb.png">
</p>

| Aspect              | Details                                                                 |
|---------------------|-------------------------------------------------------------------------|
| **Simulation time** | Runs for **300 ns**, then finishes (`#300 $finish;`)                    |
| **Stimulus (sel)**  | Toggles every **75 ns** → switches output between `i0` and `i1`         |
| **Stimulus (i0)**   | Toggles every **10 ns** → fastest changing input                        |
| **Stimulus (i1)**   | Toggles every **55 ns** → medium speed input                            |
| **Waveform dump**   | Dumps to file **`tb_good_mux.vcd`**, can be viewed in **GTKWave**       |

</details>

</details>
<details>
  <summary><font color="skyblue">Introduction to Yosys and Logic synthesis</font></summary>

  <details>
    <summary><font color="#b69b72">Introduction to yosys</font></summary>
 
  ### 1️⃣ Synthesizer

  - Tool used for converting RTL to netlist  
  - **Yosys** is the synthesizer used in this course

```
+---------+            +-----------+            +-------------+
| DESIGN  | ---------> |           | ---------> |  netlist.v  |
+---------+            |   Yosys   |            +-------------+
|  .lib   | ---------> |           |
+---------+            +-----------+

```
 ### 2️⃣ Yosys Setup

- **read_verilog** → load the RTL design  
- **read_liberty** → load the standard cell library (.lib)  
- **write_verilog** → export synthesized netlist

```
   [read_verilog]                               [write_verilog]
        |                                             ^
        v                                             |
+---------+            +-----------+            +-------------+
| DESIGN  | ---------> |           | ---------> |  netlist.v  |
+---------+            |   Yosys   |            +-------------+
|  .lib   | ---------> |           |
+---------+            +-----------+
        ^
        |
  [read_liberty]

```
### 3️⃣ Verify the Synthesis

- After synthesis, the generated **netlist** is simulated with the same **testbench**.  
- **Icarus Verilog (iverilog)** compiles the design and produces a **VCD file**.  
- **GTKWave** is used to visualize the waveforms.

```
+-----------+            +-----------+            +-----------+
|  NETLIST  | ---------> |           | ---------> |  vcd file |
+-----------+            | iverilog  |            +-----------+
| Testbench | ---------> |           |
+-----------+            +-----------+
                               |
                               v
                         +-----------+
                         |  GTKWave  |
                         +-----------+

```
  </details>

  <details>
    <summary><font color="#b69b72">7-SKY130RTL D1SK3 L2 introduction to logic synthesis part1</font></summary>
  </details>

  <details>
    <summary><font color="#b69b72">8-SKY130RTL D1SK3 L3 introduction to logic synthesis part2</font></summary>
  </details>

</details>

<details>
  <summary><font color="skyblue">Labs using Yosys and Sky130 PDKs</font></summary>

  <details>
    <summary><font color="#b69b72">9-SKY130RTL D1SK4 L1 Lab3 Yosys 1 good mux Part1</font></summary>
  </details>

  <details>
    <summary><font color="#b69b72">10-SKY130RTL D1SK4 L2 Lab3 Yosys 1 good mux Part2</font></summary>
  </details>

  <details>
    <summary><font color="#b69b72">11-SKY130RTL D1SK4 L3 Lab3 Yosys 1 good mux Part3</font></summary>
  </details>
</details>
</details>
---310
<details>
<summary> <font color="#00ff6aff">Day 2-Introduction to Verilog RTL design and Synthesis </font></summary>
 <details>
  <summary> <font color="skyblue">Introduction to open-source simulator iverilog </font></summary>

  ### 2-SKY130RTL D1SK1 L1 Introduction to iverilog design test bench

  #### 🔹 Simulator
  A **simulator** is a software tool that mimics how an RTL design behaves over time without fabricating hardware.  
  - Inputs: Design (DUT) + Testbench  
  - Processes: Compiles and executes the RTL  
  - Outputs: Logs, reports, and waveform files (showing how signals evolve)  

  *Example:* iverilog compiles the RTL, and vvp executes it, generating a `.vcd` file viewable in GTKWave.  

  ---

  #### 🔹 Design (DUT)
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

  #### 🔹 Testbench

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
 #### 🔹 General Simulation Flow

  ```mermaid
  flowchart LR
      A[Design (RTL)] --> C[Simulator]
      B[Testbench] --> C[Simulator]
      C --> D[Outputs: logs / waveforms / coverage]
  ```
  ---
####  🔹 Testbench Block Diagram

  ```mermaid
  flowchart LR
      subgraph TB[Testbench]
          A[Stimulus Generator] --> B[Design (DUT)]
          B --> C[Stimulus Observer]
      end

  ```
  ---
####  🔹 Iverilog + GTKWave Simulation Flow

  ```mermaid
  flowchart TB
      A[Design (RTL)] --> C[iverilog]
      B[Testbench] --> C[iverilog]
      C --> D[vcd file]
      D --> E[GTKWave]
  ```
  ---
####  🔹 Example Workflow with Open-Source Tools
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
  <summary><font color="skyblue">Labs using iverilog and gtkwave</font></summary>
  <details>
    <summary><font color="#b69b72">3-SKY130RTL D1SK2 L1 Lab1 introduction to lab</font></summary>
  </details>

  <details>
    <summary><font color="#b69b72">4-SKY130RTL D1SK2 L2 Lab2 Introduction iverilog gtkwave part1</font></summary>
  </details>

  <details>
    <summary><font color="#b69b72">5-SKY130RTL D1SK2 L3 Lab2 Introduction iverilog gtkwave part2</font></summary>
  </details>

</details>
<details>
  <summary><font color="skyblue">Introduction to Yosys and Logic synthesis</font></summary>

  <details>
    <summary><font color="#b69b72">6-SKY130RTL D1SK3 L1 Introduction to yosys</font></summary>
  </details>

  <details>
    <summary><font color="#b69b72">7-SKY130RTL D1SK3 L2 introduction to logic synthesis part1</font></summary>
  </details>

  <details>
    <summary><font color="#b69b72">8-SKY130RTL D1SK3 L3 introduction to logic synthesis part2</font></summary>
  </details>

</details>

<details>
  <summary><font color="skyblue">Labs using Yosys and Sky130 PDKs</font></summary>

  <details>
    <summary><font color="#b69b72">9-SKY130RTL D1SK4 L1 Lab3 Yosys 1 good mux Part1</font></summary>
  </details>

  <details>
    <summary><font color="#b69b72">10-SKY130RTL D1SK4 L2 Lab3 Yosys 1 good mux Part2</font></summary>
  </details>

  <details>
    <summary><font color="#b69b72">11-SKY130RTL D1SK4 L3 Lab3 Yosys 1 good mux Part3</font></summary>
  </details>
</details>
</details>
