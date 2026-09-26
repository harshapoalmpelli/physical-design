
# OpenLane Physical Design Flow – Asynchronous FIFO

## 📌 Project Overview

This project documents a practical **RTL-to-GDSII ASIC implementation flow** using an **Asynchronous FIFO**, the **OpenLane flow**, and the **Sky130 PDK**.

The work covers the transformation of an asynchronous FIFO digital design from Verilog RTL into a technology-mapped netlist and then into a physical layout. The major implementation stages include RTL design, synthesis, floorplanning, power planning, placement, clock-tree synthesis, routing, static timing analysis, physical verification, and GDSII generation.

The asynchronous FIFO is designed to transfer data safely between two independent clock domains. The design uses separate read and write clocks and employs Gray-code pointer synchronization to reduce the risk of incorrect pointer interpretation across clock domains.

### Complete Flow

```text
RTL Design
    ↓
Logic Synthesis
    ↓
Gate-Level Netlist
    ↓
Floorplanning
    ↓
Power Planning
    ↓
Placement
    ↓
Clock Tree Synthesis
    ↓
Routing
    ↓
Static Timing Analysis
    ↓
Physical Verification
    ↓
Signoff
    ↓
GDSII
````

---

## 1. Asynchronous FIFO

An **Asynchronous FIFO** is a memory-based digital circuit used to transfer data between two independent clock domains.

Unlike a synchronous FIFO, an asynchronous FIFO uses separate clocks for the write and read operations.

The main components of the FIFO include:

* Write-side control logic
* Read-side control logic
* FIFO memory
* Write pointer
* Read pointer
* Gray-code conversion logic
* Clock-domain synchronizers
* Full detection logic
* Empty detection logic
* Reset circuitry

The basic organization can be represented as:

```text
             WRITE CLOCK DOMAIN
                    │
             Write Control
                    │
             Write Pointer
                    │
             Gray Code
                    │
             Synchronizer
                    │
                    ▼
              READ CLOCK DOMAIN
                    │
              Read Control
                    │
              Read Pointer
                    │
             Gray Code
                    │
             Synchronizer
                    │
                    ▼

             FIFO MEMORY
              ▲       ▲
              │       │
          Write Data  Read Data
```

The RTL describes the intended behavior of the FIFO before technology-specific implementation.

---

## 2. Sky130 PDK

A **Process Design Kit (PDK)** provides the technology information required by EDA tools to implement a digital circuit for a specific semiconductor process.

This project uses the **SkyWater SKY130** technology.

The PDK provides information such as:

* Standard-cell libraries
* Technology layers
* Cell dimensions
* Timing models
* Physical cell information
* Manufacturing design rules
* Routing constraints
* Technology-specific layout information

Using the PDK, synthesis and physical-design tools can select suitable standard cells and construct the physical implementation of the FIFO.

---

## 3. OpenLane

**OpenLane** is an open-source automated RTL-to-GDSII implementation flow.

It integrates several open-source EDA tools and provides a structured process for implementing digital designs.

A simplified OpenLane process is:

```text
RTL
 ↓
Logic Synthesis
 ↓
Floorplanning
 ↓
Placement
 ↓
Clock Tree Synthesis
 ↓
Routing
 ↓
Timing / Signoff Checks
 ↓
GDSII
```

For the asynchronous FIFO, OpenLane is used to transform the Verilog RTL into a physical layout.

---

## 4. RTL Design

**Register Transfer Level (RTL)** describes the behavior and data movement of a digital circuit using registers, combinational logic, and sequential logic.

For the asynchronous FIFO, Verilog RTL represents:

* Write-side logic
* Read-side logic
* Memory interface
* Binary pointers
* Gray-code pointers
* Synchronizer registers
* Full detection
* Empty detection

The basic transformation is:

```text
Verilog RTL
     ↓
Logic Synthesis
     ↓
Technology-Mapped Netlist
```

The FIFO RTL is independent of the physical layout technology at this stage.

---

## 5. FIFO Pointer Architecture

The asynchronous FIFO uses separate pointers for the two clock domains.

The write pointer operates using the write clock, while the read pointer operates using the read clock.

```text
Write Clock
    ↓
Write Binary Pointer
    ↓
Gray Code Conversion
    ↓
Write Pointer Synchronizer
    ↓
Read Clock Domain
```

Similarly:

```text
Read Clock
    ↓
Read Binary Pointer
    ↓
Gray Code Conversion
    ↓
Read Pointer Synchronizer
    ↓
Write Clock Domain
```

The Gray-code representation changes only one bit between consecutive pointer values.

This property helps reduce ambiguity when a multi-bit pointer crosses from one clock domain to another.

---

## 6. FIFO Memory

The FIFO contains memory locations used to temporarily store data during clock-domain transfer.

The write operation stores data into the location addressed by the write pointer.

The read operation retrieves data from the location addressed by the read pointer.

A simplified representation is:

```text
              FIFO MEMORY

       ┌─────────────────────┐
       │ Memory Location 0   │
       ├─────────────────────┤
       │ Memory Location 1   │
       ├─────────────────────┤
       │ Memory Location 2   │
       ├─────────────────────┤
       │ Memory Location 3   │
       ├─────────────────────┤
       │        ...          │
       ├─────────────────────┤
       │ Memory Location N   │
       └─────────────────────┘
              ▲       ▲
              │       │
        Write Address Read Address
```

The memory provides the storage mechanism between the independent write and read operations.

---

## 7. Logic Synthesis

Logic synthesis transforms the FIFO RTL description into a **gate-level netlist**.

During synthesis, the RTL is interpreted and mapped to standard cells available in the selected technology library.

```text
RTL Description
      ↓
   Synthesis
      ↓
Technology-Mapped Netlist
```

The synthesized design may contain cells such as:

* AND gates
* OR gates
* NAND gates
* NOR gates
* Inverters
* Buffers
* Multiplexers
* Flip-flops
* Synchronizer registers
* Combinational logic cells

### Synthesis Result

The synthesis stage produces the gate-level representation required for physical implementation.

---

## 8. Gate-Level Netlist

A **gate-level netlist** represents the FIFO after synthesis using technology-specific cells and their connections.

It contains:

* Standard cells
* Flip-flops
* Synchronizer registers
* Combinational logic
* FIFO control logic
* Primary inputs
* Primary outputs
* Clock connections
* Reset connections
* Internal signal connections

The transformation can be represented as:

```text
RTL
 ↓
Synthesis
 ↓
Library Cells
 ↓
Cell Interconnections
 ↓
Gate-Level Netlist
```

The generated netlist becomes the logical input for the physical-design stages.

---

## 9. Floorplanning

**Floorplanning** establishes the initial physical organization of the FIFO design.

During this stage, the physical dimensions and placement regions are defined.

Important floorplan elements include:

* Die dimensions
* Core dimensions
* Standard-cell placement area
* I/O locations
* Placement boundaries
* Clock regions
* Power distribution regions

A good floorplan helps reduce:

* Routing congestion
* Interconnect length
* Timing problems
* Area overhead

The physical organization established during floorplanning affects later implementation stages.

---

## 10. Power Distribution Network

The **Power Distribution Network (PDN)** supplies power and ground connections to the standard cells.

A typical power network contains:

* VDD rails
* VSS rails
* Power rings
* Power straps
* Standard-cell power connections

A simplified representation is:

```text
VDD
 │
 ├── Power Ring
 │
 ├── Power Straps
 │
 └── Standard-Cell Rails


VSS
 │
 ├── Power Ring
 │
 ├── Power Straps
 │
 └── Standard-Cell Rails
```

The PDN ensures that the FIFO cells receive reliable power and ground connections throughout the implemented core.

---

## 11. Placement

**Placement** assigns physical locations to the standard cells inside the floorplanned core.

Placement can be divided into two main stages.

### Global Placement

Global placement determines approximate cell positions while considering:

* Cell density
* Timing
* Interconnect length
* Routing congestion

### Detailed Placement

Detailed placement adjusts the positions of cells so that they satisfy legal placement requirements.

For an asynchronous FIFO, placement also affects the physical relationship between:

* Write-domain logic
* Read-domain logic
* Synchronizer registers
* FIFO memory
* Clock networks

A well-optimized placement can help:

* Reduce routing congestion
* Shorten interconnects
* Improve timing
* Improve area utilization
* Simplify routing

---

## 12. Clock Tree Synthesis

**Clock Tree Synthesis (CTS)** creates the physical clock distribution network required to deliver clock signals to sequential elements.

Since an asynchronous FIFO has two independent clock domains, separate clock networks are required.

```text
                 Write Clock
                     │
                  Buffer
                 /      \
             Buffer    Buffer
               │          │
          Write FFs    Write FFs


                 Read Clock
                     │
                  Buffer
                 /      \
             Buffer    Buffer
               │          │
           Read FFs     Read FFs
```

CTS primarily aims to:

* Control clock skew
* Manage clock latency
* Provide adequate clock drive
* Distribute clock signals reliably

The independent clock networks are an important feature of asynchronous FIFO physical implementation.

---

## 13. Routing

After placement and clock-tree construction, the cells are physically connected using metal layers.

Routing establishes physical connections for:

* Data signals
* Write-clock signals
* Read-clock signals
* Reset signals
* Pointer signals
* Synchronizer signals
* Control signals
* Power connections

Routing can be divided into two stages.

### Global Routing

Global routing determines approximate paths and evaluates available routing resources.

### Detailed Routing

Detailed routing creates the actual metal and via connections while following the technology design rules.

```text
Placed Standard Cells
        ↓
Global Routing
        ↓
Detailed Routing
        ↓
Physically Connected FIFO
```

---

## 14. Gray-Code Synchronization

Gray-code synchronization is an important part of asynchronous FIFO design.

Binary counters may change multiple bits simultaneously.

For example:

```text
Binary:

0111
 ↓
1000
```

Several bits change at the same time.

In Gray code, consecutive values differ by only one bit.

This makes Gray-coded pointers suitable for clock-domain synchronization.

The process is:

```text
Binary Pointer
      ↓
Gray Code Converter
      ↓
Synchronizer
      ↓
Other Clock Domain
```

Two-stage synchronizer registers are commonly used to reduce the probability of metastability propagating into the receiving clock domain.

---

## 15. Full and Empty Detection

The FIFO must determine whether it can accept additional data or whether data is available for reading.

### Empty Condition

The FIFO is considered empty when the synchronized write pointer matches the read pointer.

```text
Read Pointer
     │
     ▼
Compare with
     │
     ▼
Synchronized Write Pointer
     │
     ▼
EMPTY
```

### Full Condition

The FIFO is considered full when the next write pointer reaches the corresponding read-pointer position indicating that the FIFO capacity has been reached.

```text
Write Pointer
     │
     ▼
Compare with
     │
     ▼
Synchronized Read Pointer
     │
     ▼
FULL
```

These conditions prevent invalid read and write operations.

---

## 16. Static Timing Analysis

**Static Timing Analysis (STA)** evaluates whether the implemented FIFO satisfies its timing requirements.

Unlike functional simulation, STA examines timing paths systematically.

Important timing parameters include:

* Clock period
* Cell delay
* Net delay
* Setup time
* Hold time
* Clock skew
* Signal slew
* Slack

### Understanding Slack

Slack represents the available timing margin on a path.

```text
Positive Slack
      ↓
Timing Requirement Met


Negative Slack
      ↓
Timing Violation
```

For an asynchronous FIFO, timing analysis must consider the separate clock domains and their associated timing constraints.

**OpenSTA** can be used for static timing analysis.

### STA Report

The STA report provides information about critical paths and timing margins.

---

## 17. Physical Verification

After routing, the physical implementation must be checked against technology requirements.

Two important physical verification checks are:

* Design Rule Check (DRC)
* Layout Versus Schematic (LVS)

### Design Rule Check (DRC)

DRC verifies that the physical layout follows the manufacturing rules.

Typical checks include:

* Minimum metal width
* Minimum spacing
* Via requirements
* Layer restrictions
* Metal geometry constraints

### Layout Versus Schematic (LVS)

LVS compares the extracted physical connectivity with the intended circuit representation.

The purpose is to verify that the physical layout corresponds to the expected netlist.

---

## 18. Signoff

**Signoff** is the final verification stage before the physical layout is accepted.

The implementation can be checked for:

* Timing compliance
* Routing correctness
* Design-rule compliance
* Layout consistency
* Power connectivity
* Netlist consistency
* Clock-domain implementation

After successful verification, the final physical database can be generated in **GDSII** format.

```text
Physical Implementation
        ↓
Verification
        ↓
Signoff
        ↓
GDSII
```

---

## 19. OpenLane Execution

OpenLane can be launched using the flow script provided with the installation.

For an interactive session, a typical command is:

```bash
./flow.tcl -interactive
```

The design can then be prepared using the appropriate command supported by the installed OpenLane version:

```tcl
prep -design asynchronous_fifo
```

> **Note:** OpenLane commands and configuration options can vary between releases. The commands used should match the OpenLane version installed in the development environment.

The exact command sequence should therefore be verified against the installed OpenLane release.

---

## 20. Project Directory Structure

A typical OpenLane project can be organized approximately as follows:

```text
openlane/
│
├── designs/
│   └── asynchronous_fifo/
│       ├── config.tcl
│       ├── src/
│       │   └── *.v
│       └── runs/
│
├── flow.tcl
├── scripts/
└── configuration files
```

The main design directory contains the RTL source and configuration information.

The `src` directory contains the Verilog source files.

The configuration file controls important implementation parameters.

The `runs` directory contains generated outputs and reports from individual implementation runs.

---

## 21. Design Statistics

The physical-design flow produces several statistics that help evaluate the implemented FIFO.

Important parameters include:

| Parameter         | Meaning                                           |
| ----------------- | ------------------------------------------------- |
| **Total Cells**   | Overall number of cells in the synthesized design |
| **Flip-Flops**    | Number of sequential storage elements             |
| **Wires**         | Number of logical connections                     |
| **Wire Bits**     | Number of individual wire bits                    |
| **Area**          | Physical area occupied by the implementation      |
| **Utilization**   | Portion of core area occupied by cells            |
| **Timing**        | Timing information of the implemented design      |
| **Clock Domains** | Number of independent clock domains               |
| **FIFO Depth**    | Number of storage locations                       |
| **Data Width**    | Number of bits transferred per FIFO entry         |

### Design Statistics

The final implementation reports can be used to record the actual values obtained from the OpenLane run.

---

## 22. FIFO Pointer and Storage Parameters

The functionality of an asynchronous FIFO depends on several important architectural parameters.

### FIFO Depth

FIFO depth represents the number of entries that can be stored.

For example:

```text
FIFO Depth = 16
```

means that the FIFO can store up to 16 data entries.

### Data Width

Data width represents the number of bits in each FIFO entry.

For example:

```text
Data Width = 8 bits
```

means each FIFO location stores 8 bits.

### Address Width

For a FIFO depth of \(2^N\):

```text
Address Width = N
```

For example:

```text
FIFO Depth = 16
Address Width = 4
```

The pointer architecture also requires additional pointer information for full and empty detection.

---

## 23. Complete Physical Design Flow

The complete asynchronous FIFO ASIC implementation process can be summarized as:

```text
                         RTL
                          ↓
                    FIFO Architecture
                          ↓
                    Logic Synthesis
                          ↓
                   Gate-Level Netlist
                          ↓
                     Floorplanning
                          ↓
                  Power Distribution
                          ↓
                       Placement
                          ↓
                Clock Tree Synthesis
                          ↓
                       Routing
                          ↓
              Static Timing Analysis
                          ↓
                 Physical Verification
                          ↓
                       Signoff
                          ↓
                        GDSII
```

Each stage transforms the design into a more detailed physical representation.

---

## 24. Tools, Learning Outcomes and Conclusion

### Tools and Technologies

| Tool / Technology | Main Role                                  |
| ----------------- | ------------------------------------------ |
| **Verilog HDL**   | RTL description of asynchronous FIFO       |
| **OpenLane**      | Automated RTL-to-GDSII implementation flow |
| **Yosys**         | RTL synthesis                              |
| **OpenROAD**      | Physical-design implementation             |
| **OpenSTA**       | Static timing analysis                     |
| **Sky130 PDK**    | Technology and standard-cell information   |
| **Magic**         | Layout verification                        |
| **Netgen**        | LVS verification                           |
| **GDSII**         | Final physical layout database             |

### Key Learning Outcomes

This project provides practical exposure to the major stages involved in digital ASIC physical design.

The main concepts covered include:

* Asynchronous FIFO architecture
* Dual-clock-domain design
* FIFO memory organization
* Binary pointers
* Gray-code conversion
* Clock-domain synchronization
* Full and empty detection
* Verilog RTL design
* Logic synthesis
* Technology-mapped netlists
* Standard-cell implementation
* Floorplanning
* Power distribution
* Cell placement
* Clock Tree Synthesis
* Global routing
* Detailed routing
* Static Timing Analysis
* Setup and hold timing
* Slack analysis
* Design Rule Checking
* Layout Versus Schematic verification
* Signoff
* GDSII generation

### Project Outcome

The asynchronous FIFO RTL is processed through the major stages of an open-source ASIC implementation flow.

The overall transformation can be represented as:

```text
Verilog RTL
     ↓
FIFO Synthesis
     ↓
Gate-Level Netlist
     ↓
Physical Implementation
     ↓
Timing Analysis
     ↓
Physical Verification
     ↓
Final Layout
```

The project demonstrates how an asynchronous FIFO described at the RTL level can be transformed into a physical chip layout using **Sky130 technology** and an **OpenLane-based implementation flow**.

### Final Project Flow

```text
RTL
 ↓
FIFO Architecture
 ↓
Logic Synthesis
 ↓
Gate-Level Netlist
 ↓
Floorplan
 ↓
Power Planning
 ↓
Placement
 ↓
Clock Tree Synthesis
 ↓
Routing
 ↓
Static Timing Analysis
 ↓
Physical Verification
 ↓
Signoff
 ↓
GDSII
```

This sequence represents the progression from a functional asynchronous FIFO description to a physical layout database.

### Conclusion

The project provides practical exposure to the **RTL-to-GDSII ASIC physical-design process** using an asynchronous FIFO and open-source EDA tools.

The implementation begins with Verilog RTL and proceeds through FIFO architecture design, synthesis, netlist generation, floorplanning, power planning, placement, clock-tree construction, routing, timing analysis, and physical verification.

The work demonstrates the importance of clock-domain synchronization, Gray-code pointer transfer, FIFO full and empty detection, and physical implementation considerations.

It also provides practical familiarity with tools such as **Yosys, OpenROAD, OpenSTA, Magic, Netgen, OpenLane, and the Sky130 PDK**.

### Final Takeaway

```text
RTL
 ↓
FIFO Architecture
 ↓
Netlist
 ↓
Physical Design
 ↓
Timing & Physical Verification
 ↓
Signoff
 ↓
GDSII
```

**RTL → FIFO Logic → Netlist → Physical Implementation → Verification → GDSII**

```
```
