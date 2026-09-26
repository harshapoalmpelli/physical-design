
# Floorplanning and Placement – Asynchronous FIFO

## 1. Introduction

Floorplanning and placement are important stages in the **physical design flow of an ASIC**. After synthesis, the asynchronous FIFO netlist is converted into a physical representation by deciding the chip dimensions, core area, power distribution, I/O locations, and positions of standard cells.

The asynchronous FIFO contains separate read and write clock domains. Therefore, physical implementation must consider the placement of FIFO memory, read-side logic, write-side logic, pointer synchronization circuits, and clock-related cells.

This stage ensures that the design can be implemented efficiently while satisfying **area, timing, power, and routing requirements**.

### Physical Design Flow

```text
Synthesized FIFO Netlist
        ↓
    Floorplanning
        ↓
    Power Planning
        ↓
     Pin Placement
        ↓
       Placement
        ↓
Placement Optimization
        ↓
        CTS
        ↓
      Routing
        ↓
   Timing Analysis
````

---

## 2. Core and Die

### Core

The **core** is the internal region of the chip where the standard cells and logic components of the asynchronous FIFO are placed.

It contains elements such as:

* FIFO control logic
* Read pointer logic
* Write pointer logic
* Synchronizer registers
* Gray-code conversion logic
* Full and empty detection logic
* Data-path logic

### Die

The **die** represents the complete physical silicon area of the chip. It contains the core together with the surrounding regions required for I/O, power distribution, and other physical-design requirements.

```text
+--------------------------------+
|              DIE               |
|                                |
|      +------------------+      |
|      |                  |      |
|      |       CORE       |      |
|      |                  |      |
|      |   Async FIFO     |      |
|      |                  |      |
|      +------------------+      |
|                                |
+--------------------------------+
```

The final physical implementation is eventually represented as a layout suitable for fabrication.

---

## 3. Aspect Ratio and Utilization

### Aspect Ratio

Aspect ratio describes the physical shape of the core or die.

```text
Aspect Ratio = Height / Width
```

For example:

```text
Aspect Ratio = 1
```

indicates a square-shaped core.

An aspect ratio greater than or less than 1 produces a rectangular physical region.

### Utilization

Utilization represents the percentage of the core area occupied by placed cells.

```text
Utilization (%) =
(Cell Area / Core Area) × 100
```

Higher utilization allows more cells to fit into a smaller area, but excessive utilization can increase **routing congestion** and make timing closure more difficult.

For an asynchronous FIFO, the utilization also depends on the amount of storage, pointer logic, synchronizer registers, and control circuitry used in the implementation.

---

## 4. Floorplanning

**Floorplanning** is one of the first major steps in physical design after synthesis.

It determines the overall physical organization of the asynchronous FIFO.

### Major Floorplanning Decisions

* Core dimensions
* Die dimensions
* Aspect ratio
* Core utilization
* I/O locations
* FIFO memory location
* Read-domain logic region
* Write-domain logic region
* Power distribution requirements
* Routing resources
* Clock-domain organization

A well-designed floorplan can reduce congestion, shorten interconnects, and improve overall timing.

For an asynchronous FIFO, the floorplan should provide sufficient physical space for the logic belonging to the independent read and write clock domains.

---

## 5. Floorplan Configuration in OpenLane

OpenLane provides several configuration variables that control the floorplanning process.

Important parameters include:

```text
FP_CORE_UTIL
FP_ASPECT_RATIO
FP_SIZING
DIE_AREA
FP_IO_HMETAL
FP_IO_VMETAL
FP_IO_MODE
FP_PDN_VPITCH
FP_PDN_HPITCH
```

### Purpose of Important Variables

| Variable          | Purpose                                    |
| ----------------- | ------------------------------------------ |
| `FP_CORE_UTIL`    | Defines the target core utilization        |
| `FP_ASPECT_RATIO` | Controls the height-to-width ratio         |
| `FP_SIZING`       | Determines the floorplan sizing method     |
| `DIE_AREA`        | Specifies the die dimensions               |
| `FP_IO_HMETAL`    | Defines the horizontal metal layer for I/O |
| `FP_IO_VMETAL`    | Defines the vertical metal layer for I/O   |
| `FP_IO_MODE`      | Controls I/O placement configuration       |
| `FP_PDN_VPITCH`   | Sets vertical power-grid pitch             |
| `FP_PDN_HPITCH`   | Sets horizontal power-grid pitch           |

These parameters help establish the physical environment in which the asynchronous FIFO will be implemented.

---

## 6. OpenLane Configuration

The OpenLane configuration file contains the parameters required to run the physical design flow.

An example configuration is:

```tcl
set ::env(DESIGN_NAME) "asynchronous_fifo"

set ::env(VERILOG_FILES) \
"./designs/asynchronous_fifo/src/asynchronous_fifo.v"

set ::env(SDC_FILE) \
"./designs/asynchronous_fifo/src/asynchronous_fifo.sdc"

set ::env(CLOCK_PERIOD) "5.000"

set ::env(CLOCK_PORT) "wr_clk"

set ::env(CLOCK_NET) $::env(CLOCK_PORT)
```

For an asynchronous FIFO, the design may contain two independent clocks:

```text
wr_clk → Write Clock Domain

rd_clk → Read Clock Domain
```

Therefore, the timing constraints must account for the two clock domains.

### Configuration Parameters

* `DESIGN_NAME` – specifies the name of the design.
* `VERILOG_FILES` – specifies the RTL source file.
* `SDC_FILE` – specifies the timing constraint file.
* `CLOCK_PERIOD` – defines the clock period used for the relevant timing constraint.
* `CLOCK_PORT` – identifies the clock input port.
* `CLOCK_NET` – identifies the clock network.

> **Note:** The exact OpenLane configuration depends on the installed OpenLane version and the timing constraints used for the asynchronous FIFO.

---

## 7. Pre-Placed Cells and Blocks

Certain cells or blocks may need to be assigned fixed physical locations before the automated placement process.

These are commonly referred to as **pre-placed cells or blocks**.

For an asynchronous FIFO, such structures may include:

* FIFO memory blocks
* Large memory macros
* Clock-related cells
* Synchronizer-related structures
* Interface blocks
* Other fixed IP blocks

Pre-placement helps the tool organize the remaining standard cells around these fixed regions.

```text
+---------------------------+
| Standard Cells            |
|                           |
|     +-------------+       |
|     | FIFO Memory |       |
|     |    Block    |       |
|     +-------------+       |
|                           |
| Standard Cells            |
+---------------------------+
```

The physical location of the memory and surrounding control logic can influence routing distance and congestion.

---

## 8. Power Planning

Power planning establishes the power distribution network required to deliver a stable supply voltage to all cells.

The primary power signals are:

```text
VDD → Power Supply
VSS → Ground
```

The power distribution network generally consists of:

```text
VDD / VSS
    ↓
Power Rings
    ↓
Power Straps
    ↓
Standard Cell Rails
    ↓
Logic Cells
```

For an asynchronous FIFO, the power network supplies:

* Write-domain logic
* Read-domain logic
* Synchronizer circuits
* Pointer logic
* FIFO memory
* Control logic

A properly designed power network helps reduce:

* IR voltage drop
* Ground bounce
* Supply noise
* Power integrity problems

It also ensures that the FIFO logic receives adequate power during operation.

---

## 9. Decoupling Capacitors

**Decoupling capacitors**, also called **decap cells**, are used to reduce fluctuations in the local power supply.

When a large number of cells switch simultaneously, the instantaneous current demand can cause temporary voltage variations.

A decoupling capacitor stores charge and can supply it locally when required.

```text
       VDD
        |
     +------+
     | Decap|
     | Cell |
     +------+
        |
     Circuit
        |
       VSS
```

In an asynchronous FIFO, switching activity can occur independently in the read and write clock domains.

### Benefits

* Reduces supply-voltage fluctuations
* Improves power integrity
* Helps reduce local noise
* Provides temporary local charge during switching activity
* Supports stable operation of clock-domain logic

---

## 10. Pin Placement

**Pin placement** determines the physical locations of input and output pins around the chip.

For an asynchronous FIFO, important interface signals may include:

```text
wr_clk
rd_clk
wr_en
rd_en
wr_data
rd_data
rst
full
empty
```

The placement of these pins affects routing length, congestion, and timing.

Pins can generally be positioned along:

* Left side
* Right side
* Top side
* Bottom side

Clock-related pins require special attention because the clock networks have a significant effect on timing.

Good pin placement helps:

* Reduce routing distance
* Reduce congestion
* Improve timing
* Simplify routing
* Improve connectivity between external interfaces and FIFO logic

---

## 11. Placement Blockages

A **placement blockage** is a region where standard cells are not allowed to be placed.

Blockages may be created to protect:

* FIFO memory macros
* Fixed macros
* Power structures
* Special routing regions
* Clock-related areas
* Reserved physical regions

Example:

```text
+---------------------------+
| Standard Cells            |
|                           |
|     +-------------+       |
|     |   BLOCKED   |       |
|     | FIFO MEMORY |       |
|     |    AREA     |       |
|     +-------------+       |
|                           |
| Standard Cells            |
+---------------------------+
```

Placement blockages provide better control over cell distribution and help avoid conflicts with important physical structures.

For the asynchronous FIFO, they can help maintain physical separation around memory or other fixed structures.

---

## 12. Standard Cell Placement

After floorplanning and power planning, the standard cells are assigned physical locations inside the core.

The placement process attempts to achieve:

* Shorter interconnects
* Lower congestion
* Better timing
* Legal cell positions
* Efficient area utilization
* Better connectivity between related cells
* Efficient organization of read and write clock domains

### Placement Flow

```text
Synthesized FIFO Netlist
        ↓
Global Placement
        ↓
Legalization
        ↓
Detailed Placement
        ↓
Placement Optimization
```

### Global Placement

Global placement determines approximate locations for cells while optimizing wire length and congestion.

### Legalization

Legalization moves cells into valid positions according to the physical placement rules.

### Detailed Placement

Detailed placement performs local adjustments to improve the quality of the placement.

For an asynchronous FIFO, the placement process must accommodate:

```text
Write Clock Domain
        ↓
Write Pointer Logic
        ↓
FIFO Memory
        ↓
Read Pointer Logic
        ↓
Read Clock Domain
```

Synchronizer registers must also be physically implemented as part of the corresponding clock-domain logic.

---

## 13. Placement Optimization

After the initial placement, optimization is performed to improve the physical and timing characteristics of the design.

The tool considers parameters such as:

* Wire length
* Capacitance
* Delay
* Congestion
* Setup timing
* Hold timing
* Cell density
* Clock distribution

If necessary, the tool may resize cells, move cells, or insert additional buffers.

### Buffer and Repeater Insertion

Long interconnects can introduce significant delay and signal degradation.

Buffers or repeaters can be inserted along long paths:

```text
Source Cell
     |
     | Long Wire
     |
   Buffer
     |
     | Long Wire
     |
Destination Cell
```

These buffers help improve signal integrity and reduce the impact of long interconnects.

For an asynchronous FIFO, optimization may be particularly important for paths involving:

* Pointer logic
* Gray-code conversion
* Synchronizer registers
* FIFO control signals
* Data paths
* Clock-related connections

---

## 14. Placement Statistics

After placement, OpenLane provides various statistics that help evaluate the quality of the physical implementation.

The actual values should be obtained from the asynchronous FIFO OpenLane run.

Example format:

```text
Total Instances      : <value>
Fixed Instances      : <value>
Nets                 : <value>
Design Area          : <value> um²
Utilization          : <value> %
Utilization Padded   : <value> %
Rows                 : <value>
```

### Important Placement Metrics

| Parameter          | Description                               |
| ------------------ | ----------------------------------------- |
| Total Instances    | Total number of cell instances            |
| Fixed Instances    | Number of instances with fixed locations  |
| Nets               | Number of electrical connections          |
| Design Area        | Physical area occupied by the design      |
| Utilization        | Percentage of available area occupied     |
| Utilization Padded | Utilization considering placement padding |
| Rows               | Number of standard-cell placement rows    |
| Wire Length        | Estimated interconnect length             |
| Displacement       | Movement of cells during optimization     |

These statistics are useful for identifying potential problems with **area, congestion, placement quality, and timing**.

---

## 15. Floorplanning and Placement Results

### Floorplanning Result

The floorplanning stage establishes the physical boundaries of the asynchronous FIFO and defines the regions where cells, memory structures, power networks, and other physical structures will be placed.

The resulting floorplan provides the physical framework for the remaining implementation stages.

### Placement Result

After placement, standard cells are distributed within the core while considering timing, congestion, and connectivity.

A conceptual placement can be represented as:

```text
+--------------------------------+
|              DIE               |
|                                |
|  +--------------------------+  |
|  |          CORE            |  |
|  |                          |  |
|  | Write     FIFO    Read   |  |
|  | Logic    Memory   Logic  |  |
|  |                          |  |
|  |  Sync Logic / Control    |  |
|  |                          |  |
|  +--------------------------+  |
|                                |
+--------------------------------+
```

The final placement provides the physical foundation for Clock Tree Synthesis and routing.

---

## 16. Key Learnings

Through the floorplanning and placement stage of the **Asynchronous FIFO design**, the following concepts were studied:

```text
Core
Die
Aspect Ratio
Utilization
Floorplanning
Floorplan Configuration
Pre-Placed Cells
Power Planning
Decoupling Capacitors
Pin Placement
Placement Blockages
Standard Cell Placement
Placement Optimization
Placement Statistics
Clock-Domain Organization
FIFO Memory Placement
```

The major purpose of this stage is to convert the synthesized logical FIFO design into a physically organized layout that is suitable for further implementation.

The physical arrangement of the read and write clock domains, FIFO memory, synchronizer logic, and control circuitry can affect routing and timing characteristics.

---

## 17. Physical Design Flow Covered

The overall flow completed so far is:

```text
RTL
 ↓
Synthesis
 ↓
Gate-Level Netlist
 ↓
Floorplanning
 ↓
Power Planning
 ↓
Pin Placement
 ↓
Standard Cell Placement
 ↓
Placement Optimization
```

The next major stages of the physical design flow are:

```text
Clock Tree Synthesis (CTS)
        ↓
Routing
        ↓
Parasitic Extraction
        ↓
Static Timing Analysis
        ↓
Physical Verification
        ↓
Final Layout
```

For the asynchronous FIFO, the remaining stages must consider the independent read and write clock domains along with their timing constraints.

---

## 18. Conclusion

Floorplanning and placement are essential steps in converting a synthesized asynchronous FIFO netlist into a physically implementable chip layout.

The floorplan determines the physical organization of the design, while placement assigns actual locations to standard cells. Power planning, pin placement, blockages, and placement optimization further improve the quality of the physical implementation.

The asynchronous FIFO design contains multiple important physical-design structures, including FIFO memory, write-domain logic, read-domain logic, pointer-generation logic, synchronizer circuits, and clock-related elements.

The design has therefore progressed from the synthesized netlist toward a physically organized implementation, providing the foundation for the next stages of **Clock Tree Synthesis, routing, and timing analysis**.

### Final Flow

```text
Asynchronous FIFO RTL
        ↓
      Synthesis
        ↓
Gate-Level Netlist
        ↓
    Floorplanning
        ↓
   Power Planning
        ↓
    Pin Placement
        ↓
Standard Cell Placement
        ↓
Placement Optimization
        ↓
       CTS
        ↓
     Routing
        ↓
 Timing Analysis
        ↓
Physical Verification
        ↓
      GDSII
```

**Asynchronous FIFO RTL → Netlist → Floorplanning → Placement → CTS → Routing → Verification → GDSII**

```
```
