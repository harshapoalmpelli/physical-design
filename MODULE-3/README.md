# CMOS Inverter — Fabrication, Simulation, Layout & Verification

<p align="center">
  <b>CMOS Technology • Device Fabrication • SPICE Simulation • Layout • Verification</b>
</p>

<p align="center">
  A detailed study of CMOS inverter design covering transistor formation,
  fabrication stages, circuit simulation, physical layout and verification.
</p>

---

## 📚 Table of Contents

1. [CMOS Inverter](#1--cmos-inverter)
2. [CMOS Device Structure](#2--cmos-device-structure)
3. [Substrate Selection](#3--substrate-selection)
4. [N-Well Formation](#4--n-well-formation)
5. [Active Region and Isolation](#5--active-region-and-isolation)
6. [Gate Oxide and Polysilicon Gate](#6--gate-oxide-and-polysilicon-gate)
7. [Source and Drain Formation](#7--source-and-drain-formation)
8. [LDD and Spacer Formation](#8--ldd-and-spacer-formation)
9. [Contact Formation](#9--contact-formation)
10. [Silicidation](#10--silicidation)
11. [Metal Interconnection](#11--metal-interconnection)
12. [CMOS Inverter Operation](#12--cmos-inverter-operation)
13. [Switching Threshold Voltage](#13--switching-threshold-voltage)
14. [SPICE CMOS Inverter Simulation](#14--spice-cmos-inverter-simulation)
15. [Layout and Physical Verification](#15--layout-and-physical-verification)

---

## 🎯 Project Objectives

The CMOS inverter is used as a basic example to understand the complete VLSI design process, starting from transistor fabrication and ending with physical verification.

The major objectives are:

* Understand the structure and operation of NMOS and PMOS transistors.
* Study the important steps involved in CMOS fabrication.
* Examine how a CMOS inverter performs logical inversion.
* Analyze the voltage transfer characteristics of the inverter.
* Perform DC and operating-point simulation using SPICE.
* Understand the conversion of a circuit schematic into physical layout.
* Study the purpose of DRC and LVS in physical verification.
* Establish a complete flow from fabrication concepts to verified layout.

---

# 1. 🔲 CMOS Inverter

A CMOS inverter is one of the most basic building blocks used in digital integrated circuits. It consists of two complementary MOS transistors: one PMOS and one NMOS.

The PMOS transistor provides the pull-up path, while the NMOS transistor provides the pull-down path.

The gates of both devices are connected to the same input signal. Their drains are joined together to produce the output.

### Circuit

```text
                         VDD
                          │
                     ┌─────────┐
              VIN ───┤  PMOS   │
                     └────┬────┘
                          │
                          ├──────── VOUT
                          │
                     ┌────┴────┐
              VIN ───┤  NMOS   │
                     └────┬────┘
                          │
                         GND
```

### Truth Table

| VIN | PMOS | NMOS | VOUT |
| :-: | :--: | :--: | :--: |
|  0  |  ON  |  OFF | HIGH |
|  1  |  OFF |  ON  |  LOW |

Thus, the output is always the complement of the input:

$$
V_{OUT} = \overline{V_{IN}}
$$

The complementary operation of the two transistors makes CMOS inverters highly useful as fundamental digital logic cells.

---

# 2. 🧱 CMOS Device Structure

CMOS technology uses both NMOS and PMOS transistors to achieve complementary switching.

In a conventional CMOS process, the two devices are fabricated in different body regions so that each transistor obtains the required substrate or well connection.

### Device Arrangement

| Device |      Body Region     | Source / Drain |
| :----: | :------------------: | :------------: |
|  PMOS  |        N-Well        |       P+       |
|  NMOS  | P-Well / P-Substrate |       N+       |

### Basic Structure

```text
                 PMOS                         NMOS

              P+     P+                   N+     N+
               │      │                    │      │
          ┌────┴──────┴────┐          ┌────┴──────┴────┐
          │     N-WELL     │          │   P-WELL /     │
          │                │          │  P-SUBSTRATE   │
          └────────────────┘          └────────────────┘

                       P-SUBSTRATE
```

### Major Device Regions

A CMOS transistor structure contains several important physical regions:

* N-Well
* P-Well or P-Substrate
* Active silicon region
* Gate oxide
* Polysilicon gate
* Source
* Drain
* Contacts
* Metal interconnects

These regions collectively determine the electrical and physical behavior of the fabricated device.

---

# 3. 🟫 Substrate Selection

The silicon substrate acts as the foundation on which the different transistor regions are constructed.

For the CMOS process considered here, a **P-type silicon substrate** is selected.

### P-Type Substrate

```text
┌──────────────────────────────────────┐
│                                      │
│             P-SUBSTRATE              │
│                                      │
│            Silicon Wafer             │
│                                      │
└──────────────────────────────────────┘
```

### Role of the Substrate

The substrate provides the base material required for several fabrication operations, including:

* Formation of wells
* Formation of NMOS devices
* Electrical isolation
* Creation of transistor regions
* Supporting subsequent fabrication steps

The choice of substrate is therefore an important initial step in CMOS processing.

---

# 4. 🟦 N-Well Formation

An N-Well is introduced into the P-type substrate to provide the body region required for PMOS transistor fabrication.

The well also electrically separates the PMOS body from the surrounding substrate.

### Basic N-Well Structure

```text
              N-WELL
        ┌─────────────────┐
        │                 │
        │    N-type       │
        │     Region      │
        │                 │
        └─────────────────┘
────────────────────────────────
           P-SUBSTRATE
────────────────────────────────
```

### General N-Well Process

```text
P-SUBSTRATE
     │
     ▼
PHOTORESIST
     │
     ▼
PHOTOLITHOGRAPHY
     │
     ▼
N-WELL IMPLANTATION
     │
     ▼
ANNEALING
     │
     ▼
N-WELL FORMATION
```

During this process, selected areas of the substrate are patterned and doped to produce the required N-type well.

The completed N-Well becomes the body region in which the PMOS transistor is fabricated.

---

# 5. 🟩 Active Region and Isolation

The **active region** represents the portion of silicon where the transistor is physically created.

This region contains the source, channel and drain areas.

Isolation regions are placed between neighboring devices to prevent unwanted electrical interaction.

### Active Region

```text
       ISOLATION          ACTIVE REGION          ISOLATION

┌─────────────┐     ┌────────────────────┐     ┌─────────────┐
│             │     │                    │     │             │
│             │     │ SOURCE → CHANNEL   │     │             │
│             │     │             → DRAIN│     │             │
└─────────────┘     └────────────────────┘     └─────────────┘
```

The fundamental transistor arrangement can be represented as:

```text
SOURCE ───── CHANNEL ───── DRAIN
```

The active region therefore defines where current can flow through the MOS device.

Isolation is equally important because it prevents adjacent devices from becoming unintentionally connected.

---

# 6. ⚡ Gate Oxide and Polysilicon Gate

The gate structure is formed above the active silicon region.

A thin insulating oxide layer separates the gate from the silicon. A polysilicon layer is then deposited and patterned to create the transistor gate.

### MOS Gate Structure

```text
                  POLYSILICON
                       │
                ┌─────────────┐
                │    GATE     │
                └─────────────┘
────────────────────────────────
                 GATE OXIDE
────────────────────────────────
        SOURCE     CHANNEL     DRAIN
          │            │          │
         N+/P+                   N+/P+
────────────────────────────────
                 SILICON
```

The gate controls whether a conductive channel is established between the source and drain.

### Important Parameters

| Parameter | Description          |
| :-------: | -------------------- |
|     W     | Transistor width     |
|     L     | Channel length       |
|    tox    | Gate oxide thickness |
|    VTH    | Threshold voltage    |

These parameters have a significant effect on transistor current, switching behavior and overall inverter performance.

---

# 7. 🔵 Source and Drain Formation

Source and drain regions are created by introducing suitable dopants into selected areas of the active silicon.

Ion implantation followed by thermal processing is commonly used for this purpose.

## NMOS Source and Drain

An NMOS transistor contains N+ source and drain regions.

```text
       N+ SOURCE                    N+ DRAIN
           │                            │
           ▼                            ▼

    ┌──────────┐                ┌──────────┐
    │          │                │          │
────┴──────────┴────────────────┴──────────┴────
                 NMOS CHANNEL
────────────────────────────────────────────────
                    P-REGION
```

## PMOS Source and Drain

A PMOS transistor uses P+ source and drain regions.

```text
       P+ SOURCE                    P+ DRAIN
           │                            │
           ▼                            ▼

    ┌──────────┐                ┌──────────┐
    │          │                │          │
────┴──────────┴────────────────┴──────────┴────
                 PMOS CHANNEL
────────────────────────────────────────────────
                    N-WELL
```

### Purpose

The source and drain provide the terminals through which the transistor current enters and leaves the device.

The type of doping determines whether the device operates as an NMOS or PMOS transistor.

---

# 8. 🟨 LDD and Spacer Formation

**LDD** stands for **Lightly Doped Drain**.

LDD regions are lightly doped extensions positioned close to the transistor channel. They are introduced to control the electric field near the drain.

### Simplified LDD Process

```text
STEP 1 — GATE FORMATION

             POLY
              │
──────────────┼──────────────
              │
────────────────────────────


STEP 2 — LIGHT IMPLANTATION

          N-       N-
──────────┐         ┌──────────
          │   POLY  │
──────────┴─────────┴──────────


STEP 3 — SPACER FORMATION

              ││
          ┌──────┐
──────────┤ POLY ├──────────
          └──────┘


STEP 4 — HEAVY IMPLANTATION

         N+          N+
─────────┐            ┌────────
         │            │
─────────┴────────────┴────────
```

### Benefits of LDD

The LDD structure provides several advantages:

* Decreases the electric-field intensity near the drain.
* Helps control hot-carrier effects.
* Improves long-term transistor reliability.
* Supports stable MOS transistor operation.

Spacers are used during the later implantation step to control the position of the heavily doped source and drain regions.

---

# 9. 🔗 Contact Formation

Contacts provide vertical electrical connections between the transistor regions and the metal interconnection system.

A contact can connect diffusion or polysilicon to an upper metal layer.

### Contact Structure

```text
                 METAL
────────────────────────────
                  │
                CONTACT
                  │
────────────────────────────
            DIFFUSION / POLY
```

### Contacts May Connect

* Source
* Drain
* Polysilicon gate
* Well
* Substrate

Correct contact placement is necessary to establish the intended electrical connectivity of the layout.

Improper contacts can result in disconnected or incorrectly connected circuit nodes.

---

# 10. 🔶 Silicidation

Silicidation is a fabrication technique used to reduce resistance in selected silicon and polysilicon regions.

A metal is deposited and thermally reacted with silicon to create a low-resistance metal silicide layer.

### Silicidation Process

```text
Metal Deposition
       │
       ▼
Thermal Annealing
       │
       ▼
Metal + Silicon Reaction
       │
       ▼
Silicide Formation
       │
       ▼
Removal of Unreacted Metal
       │
       ▼
Low-Resistance Region
```

### Main Benefits

Silicidation can lower resistance in:

* Source regions
* Drain regions
* Polysilicon gate regions

Reduced resistance helps improve current conduction and circuit performance.

---

# 11. 🛣️ Metal Interconnection

After transistor formation, metal layers are used to connect devices and provide supply and signal paths.

Different metal layers can be connected using vias.

### Interconnect Structure

```text
                 METAL 2
────────────────────────────────
                  │
                 VIA
                  │
────────────────────────────────
                 METAL 1
────────────────────────────────
                  │
                CONTACT
                  │
────────────────────────────────
              DIFFUSION / POLY
```

### Main Interconnection Elements

| Element       | Function                                   |
| ------------- | ------------------------------------------ |
| Contact       | Connects a device region to metal          |
| Metal 1       | Provides local routing                     |
| Via           | Joins two metal layers                     |
| Metal 2       | Provides higher-level routing              |
| Higher Metals | Used for extended signal and power routing |

Metal interconnects are essential for connecting the PMOS and NMOS devices into a functional CMOS circuit.

---

# 12. 🔄 CMOS Inverter Operation

The CMOS inverter works through complementary switching between its PMOS and NMOS transistors.

When one transistor conducts, the other transistor is turned off.

## Input LOW

When:

$$
V_{IN}=0
$$

the PMOS transistor is turned ON and the NMOS transistor is turned OFF.

```text
             VDD
              │
           ┌─────┐
           │ PMOS│
           └─────┘
              │
              ├────── VOUT ≈ VDD
              │
           ┌─────┐
           │ NMOS│
           └─────┘
              │
             GND
```

Therefore:

```text
VIN = 0  →  VOUT = 1
```

The output is pulled toward the supply voltage.

---

## Input HIGH

When:

$$
V_{IN}=V_{DD}
$$

the PMOS becomes OFF and the NMOS becomes ON.

```text
             VDD
              │
           ┌─────┐
           │ PMOS│
           └─────┘
              │
              ├────── VOUT ≈ 0
              │
           ┌─────┐
           │ NMOS│
           └─────┘
              │
             GND
```

Therefore:

```text
VIN = 1  →  VOUT = 0
```

The output is pulled down toward ground.

### Overall Switching Behavior

| Input | PMOS | NMOS | Output |
| :---: | :--: | :--: | :----: |
|  LOW  |  ON  |  OFF |  HIGH  |
|  HIGH |  OFF |  ON  |   LOW  |

This complementary switching produces the NOT logic function.

---

# 13. 📈 Switching Threshold Voltage

The switching point of a CMOS inverter is generally represented by the symbol:

$$
V_M
$$

At the switching threshold, the input and output voltages are approximately equal:

$$
V_{IN}=V_{OUT}=V_M
$$

At this point, the magnitudes of the PMOS and NMOS drain currents are equal:

$$
I_{DP}=-I_{DN}
$$

### Voltage Transfer Characteristic

```text
VOUT
 │
 │───────────────
 │               \
 │                \
 │                 \
 │                  \
 │                   ─────────
 │
 └────────────────────────────── VIN
                    │
                   VM
```

The VTC illustrates how the output voltage changes as the input voltage moves from LOW to HIGH.

### Importance of the Switching Threshold

The switching threshold is useful when studying:

* Logic transitions
* Noise margins
* Input and output voltage levels
* Inverter switching behavior
* CMOS digital logic performance

The exact threshold depends on factors such as transistor dimensions, threshold voltages and device characteristics.

---

# 14. 🧪 SPICE CMOS Inverter Simulation

SPICE simulation allows the electrical characteristics of a CMOS inverter to be examined before implementing the circuit physically.

It can be used to determine the operating point, DC transfer curve and switching behavior.

## MOSFET Syntax

The general MOS transistor syntax is:

```text
Mname Drain Gate Source Bulk Model W=... L=...
```

---

## PMOS Definition

```spice
M1 out in vdd vdd PMOS W=0.375u L=0.25u
```

### PMOS Connections

| Terminal | Connection |
| -------- | ---------- |
| Drain    | out        |
| Gate     | in         |
| Source   | vdd        |
| Bulk     | vdd        |

---

## NMOS Definition

```spice
M2 out in 0 0 NMOS W=0.375u L=0.25u
```

### NMOS Connections

| Terminal | Connection |
| -------- | ---------- |
| Drain    | out        |
| Gate     | in         |
| Source   | GND        |
| Bulk     | GND        |

---

## Load Capacitor

An output load capacitor can be included using:

```spice
Cload out 0 10f
```

Therefore:

$$
C_{LOAD}=10fF
$$

The capacitor represents the loading associated with the inverter output node.

---

## Supply Voltage

The power supply can be defined as:

```spice
Vdd vdd 0 2.5
```

Thus:

$$
V_{DD}=2.5V
$$

---

## Input Voltage

The input source is defined as:

```spice
Vin in 0 2.5
```

This provides the voltage source used for DC analysis.

---

## Operating-Point Analysis

The operating point can be obtained using:

```spice
.op
```

The `.op` command determines the DC conditions of the circuit at the specified bias values.

---

## DC Sweep

The inverter transfer response can be obtained using:

```spice
.dc Vin 0 2.5 0.05
```

The input voltage is varied according to the following values:

| Parameter        | Value  |
| ---------------- | ------ |
| Starting Voltage | 0 V    |
| Final Voltage    | 2.5 V  |
| Increment        | 0.05 V |

The resulting simulation can be used to observe the relationship between VIN and VOUT.

---

## Model File

A transistor model can be included using:

```spice
.include tsmc_025um_model.mod
```

The model file name and transistor model names depend on the technology library or PDK being used.

---

## Complete SPICE Example

```spice
* CMOS Inverter DC Analysis

M1 out in vdd vdd PMOS W=0.375u L=0.25u
M2 out in 0   0   NMOS W=0.375u L=0.25u

Cload out 0 10f

Vdd vdd 0 2.5
Vin in  0 2.5

.op

.dc Vin 0 2.5 0.05

.include tsmc_025um_model.mod

.end
```

This netlist describes the basic CMOS inverter required for DC simulation.

---

# 15. 🖥️ Layout and Physical Verification

After electrical analysis, the circuit can be translated into a physical layout.

Before creating the layout, the schematic is analyzed using the transistor models and SPICE simulation.

## Pre-Layout Analysis

The following parameters can be examined:

* DC transfer characteristic
* Switching threshold
* Logic HIGH level
* Logic LOW level
* Current
* Power consumption
* Output load capacitance
* Delay-related behavior

### Pre-Layout Flow

```text
Schematic
    │
    ▼
SPICE Simulation
    │
    ▼
DC Analysis
    │
    ▼
Switching Threshold
    │
    ▼
Pre-Layout Analysis
    │
    ▼
Physical Layout
```

The purpose of this stage is to ensure that the circuit behavior is understood before physical implementation.

---

# 🧰 Magic Layout Tool

**Magic** is a VLSI layout editor that can be used to create, inspect and verify integrated-circuit layouts.

A typical command for launching Magic with OpenGL display is:

```bash
magic -d OGL
```

Magic can be used for:

* Creating physical layouts
* Editing layout geometries
* Defining different technology layers
* Connecting transistor regions
* Inspecting physical geometry
* Running DRC
* Extracting circuit information
* Supporting LVS verification

---

# 🔍 Layout Elements

A CMOS inverter layout contains several physical layers and electrical connections.

| Layout Element       | Purpose                          |
| -------------------- | -------------------------------- |
| N-Well               | Provides PMOS body region        |
| P-Substrate / P-Well | Provides NMOS body region        |
| Active               | Defines source and drain regions |
| Polysilicon          | Forms the transistor gate        |
| Contact              | Connects device regions to metal |
| Metal                | Provides electrical routing      |
| VDD                  | Positive supply                  |
| GND                  | Ground connection                |
| VIN                  | Input signal                     |
| VOUT                 | Output signal                    |

### Simplified Physical Layout

```text
                         VDD
                          │
                 ┌─────────────────┐
                 │      PMOS       │
                 │     N-WELL      │
                 └────────┬────────┘
                          │
                          ├────────── VOUT
                          │
                 ┌────────┴────────┐
                 │      NMOS       │
                 │ P-SUB / P-WELL  │
                 └─────────────────┘
                          │
                         GND

                 VIN → POLYSILICON GATE
```

The physical arrangement must maintain the required connections between the input, output, power and ground nodes.

---

# ✅ DRC and LVS Verification

Once the layout has been completed, physical verification is performed.

Two important verification procedures are:

1. **DRC — Design Rule Check**
2. **LVS — Layout Versus Schematic**

---

## DRC — Design Rule Check

DRC determines whether the layout satisfies the manufacturing rules defined by the selected technology.

Typical design rules include:

* Minimum layer width
* Minimum spacing
* Contact dimensions
* Layer overlap
* Enclosure requirements
* Geometrical restrictions

### DRC Flow

```text
LAYOUT
   │
   ▼
 DRC
   │
   ▼
PASS / ERRORS
```

A successful DRC indicates that no unresolved design-rule violations remain in the checked layout.

---

## LVS — Layout Versus Schematic

LVS compares the circuit extracted from the physical layout with the original schematic.

Its purpose is to determine whether the physical implementation represents the intended circuit.

### LVS Flow

```text
SCHEMATIC                    LAYOUT
    │                           │
    │                           │
    └──────────┬────────────────┘
               │
               ▼
              LVS
               │
               ▼
        MATCH / MISMATCH
```

If the layout and schematic have matching device and connectivity information, the LVS result indicates that the physical circuit corresponds to the intended schematic.

---

# 🔁 Complete CMOS Design Flow

The complete CMOS inverter development process can be represented as:

```text
                    CMOS FABRICATION
                           │
                           ▼
                       SUBSTRATE
                           │
                           ▼
                     WELL FORMATION
                           │
                           ▼
                      ACTIVE REGION
                           │
                           ▼
                       GATE OXIDE
                           │
                           ▼
                    POLYSILICON GATE
                           │
                           ▼
                  SOURCE / DRAIN IMPLANT
                           │
                           ▼
                      LDD / SPACER
                           │
                           ▼
                      SILICIDATION
                           │
                           ▼
                   CONTACT FORMATION
                           │
                           ▼
                  METAL INTERCONNECTION
                           │
                           ▼
                    SPICE SIMULATION
                           │
                           ▼
                    PRE-LAYOUT ANALYSIS
                           │
                           ▼
                       MAGIC LAYOUT
                           │
                           ▼
                           DRC
                           │
                           ▼
                           LVS
                           │
                           ▼
                  VERIFIED CMOS LAYOUT
```

This flow connects the device-level fabrication process with circuit-level simulation and physical-design verification.

---

# 📌 Key Takeaways

| Topic          | Main Concept                                 |
| -------------- | -------------------------------------------- |
| CMOS           | Combines complementary NMOS and PMOS devices |
| PMOS           | Normally fabricated inside an N-Well         |
| NMOS           | Fabricated in a P-type body region           |
| Gate           | Controls channel formation                   |
| Source / Drain | Provide the transistor current paths         |
| LDD            | Helps reduce the drain electric field        |
| Silicide       | Helps lower parasitic resistance             |
| Contact        | Provides vertical electrical connection      |
| Metal          | Provides circuit interconnection             |
| SPICE          | Used for electrical circuit simulation       |
| `.op`          | Performs operating-point analysis            |
| `.dc`          | Performs DC sweep analysis                   |
| VM             | Represents inverter switching threshold      |
| Magic          | Used for physical layout                     |
| DRC            | Checks manufacturing design rules            |
| LVS            | Compares layout connectivity with schematic  |

---

# 🛠️ Tools Used

| Tool         | Application                        |
| ------------ | ---------------------------------- |
| **SPICE**    | Electrical circuit simulation      |
| **Magic**    | Physical layout development        |
| **CMOS PDK** | Technology and device information  |
| **Linux**    | VLSI design environment            |
| **Git**      | Version control                    |
| **GitHub**   | Source-code and project repository |

---

# 🧾 Verification Checklist

Before finalizing the CMOS inverter layout, verify the following:

* [ ] PMOS is positioned inside the required N-Well.
* [ ] NMOS is located in the required P-type region.
* [ ] VIN is correctly connected to both transistor gates.
* [ ] PMOS and NMOS drains are connected to the VOUT node.
* [ ] VDD is correctly connected to the PMOS supply.
* [ ] GND is correctly connected to the NMOS side.
* [ ] Contacts are correctly placed.
* [ ] Metal routes provide the intended electrical connections.
* [ ] No unresolved DRC violations are present.
* [ ] LVS confirms correspondence between the layout and schematic.

---

# 🚀 Conclusion

The CMOS inverter provides a compact example through which the major stages of VLSI design can be studied.

The process begins with the selection of a silicon substrate and continues through well formation, active-region definition, gate formation, source/drain implantation, LDD and spacer formation, silicidation, contacts and metal routing.

After fabrication concepts are understood, the electrical circuit can be modeled and analyzed using SPICE. The DC response and switching threshold provide useful information about the inverter's behavior.

The circuit can then be converted into a physical layout using a VLSI layout editor such as Magic. Physical verification is finally performed using DRC and LVS.

### Complete Learning Path

```text
FABRICATION
     ↓
DEVICE FORMATION
     ↓
CMOS INVERTER
     ↓
SPICE SIMULATION
     ↓
PRE-LAYOUT ANALYSIS
     ↓
PHYSICAL LAYOUT
     ↓
DRC
     ↓
LVS
     ↓
FINAL VERIFIED DESIGN
```

The concepts covered in this module provide a foundation for further study in:

* VLSI Design
* CMOS Digital Circuit Design
* Physical Design
* ASIC Design
* SPICE-Based Simulation
* Standard Cell Development
* IC Layout Design

---

<p align="center">
  <b>CMOS Inverter | VLSI Design & Physical Verification</b>
</p>

<p align="center">
  Fabrication → Device Formation → Simulation → Layout → Verification
</p>
