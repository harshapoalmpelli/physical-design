
# Timing Analysis & Clock Tree Synthesis

<p align="center">
  <b>Timing Characterization • Static Timing Analysis • Clock Distribution • Signal Integrity</b>
</p>

<p align="center">
  A practical exploration of cell timing, delay characterization, setup and hold checks,
  clock uncertainty, clock tree synthesis, skew, crosstalk and timing verification using the SKY130 flow.
</p>

---

## 📚 Table of Contents

1. [Timing Modelling](#1--timing-modelling)
2. [Delay Tables](#2--delay-tables)
3. [Input Slew and Output Load](#3--input-slew-and-output-load)
4. [Setup Timing Analysis](#4--setup-timing-analysis)
5. [Hold Timing Analysis](#5--hold-timing-analysis)
6. [Clock Jitter and Uncertainty](#6--clock-jitter-and-uncertainty)
7. [Clock Tree Synthesis](#7--clock-tree-synthesis)
8. [Clock Skew and Clock Latency](#8--clock-skew-and-clock-latency)
9. [Crosstalk and Signal Integrity](#9--crosstalk-and-signal-integrity)
10. [Clock Shielding](#10--clock-shielding)
11. [Ideal Clock vs Real Clock](#11--ideal-clock-vs-real-clock)
12. [Static Timing Analysis using OpenSTA](#12--static-timing-analysis-using-opensta)
13. [WNS and TNS](#13--wns-and-tns)
14. [Important OpenLane and OpenROAD Commands](#14--important-openlane-and-openroad-commands)
15. [Module 4 Key Takeaways](#15--module-3-key-takeaways)

---

# 1. ⏱️ Timing Modelling

Timing modelling is an essential step in digital physical design because the delay of a standard cell is not a constant value.

The propagation delay of a cell changes according to the speed of the input transition and the amount of load connected to its output.

### 🔹 Basic Concept

```text
              Input Transition
                     │
                     ▼
              ┌─────────────┐
              │ Standard    │
              │    Cell     │
              └──────┬──────┘
                     │
                     ▼
                 Cell Delay
                     ▲
                     │
                Output Load
```

The relationship can be expressed as:

**Cell Delay = f(Input Slew, Output Load)**

### 🔹 Important Parameters

| Parameter       | Description                                            |
| :-------------- | :----------------------------------------------------- |
| **Input Slew**  | Rate at which the input signal changes                 |
| **Output Load** | Capacitance driven by the cell                         |
| **Cell Delay**  | Time required for the output to respond                |
| **Transition**  | Time taken for the signal to move between logic levels |

### 🔹 Why Timing Modelling is Required?

Physical-design tools need timing information for standard cells under different input and loading conditions.

Instead of repeatedly performing transistor-level simulations for every path, cells are characterized beforehand and their timing information is stored inside technology timing libraries.

These timing models are used by:

* Synthesis tools
* Static Timing Analysis tools
* Placement optimization tools
* Routing optimization tools

### 💡 Key Idea

> **The delay of a standard cell is primarily determined by the input transition and the output load.**

A slow input transition or a large output capacitance generally causes an increase in cell delay.

---

# 2. 📊 Delay Tables

Standard-cell timing information is commonly represented using lookup tables.

These tables provide delay values corresponding to different combinations of input slew and output capacitance.

### 🔹 Simplified Delay Table

| Input Slew ↓ / Output Load → |          Low |       Medium |         High |
| :--------------------------- | -----------: | -----------: | -----------: |
| **Fast**                     |    Low Delay |    Low Delay | Medium Delay |
| **Medium**                   |    Low Delay | Medium Delay |   High Delay |
| **Slow**                     | Medium Delay |   High Delay |   High Delay |

### 🔹 Working Principle

```text
Input Slew ─────────►
                     │
                     ▼
                ┌───────────┐
                │   Delay   │
                │   Table   │
                └───────────┘
                     ▲
                     │
Output Load ─────────┘
```

The timing engine determines the appropriate delay by using the measured input transition and the output load of the cell.

### 💡 Key Idea

> **An increase in load together with a slower input transition generally produces a larger cell delay.**

---

# 3. ⚡ Input Slew and Output Load

Input slew and output load are two major factors that influence the timing characteristics of a standard cell.

## 🔹 Input Slew

Input slew describes how rapidly an input signal changes from one logic state to another.

```text
Voltage
  │
  │              ┌────────
  │             /
  │            /
  │           /
  │──────────┘
  │
  └──────────────────────► Time
```

A signal with a short transition time has a fast slew, whereas a signal that takes longer to change has a slow slew.

### 🔹 Output Load

Output load refers to the capacitive load that must be driven by the cell.

```text
              Standard Cell
                   │
                   ▼
              ┌─────────┐
              │  Load   │
              │    C    │
              └─────────┘
```

When the capacitance increases, the cell requires more time to charge or discharge the output node.

### 🔹 Relationship

For lower slew and lower load:

```text
Input Slew ↓
Output Load ↓
      │
      ▼
Cell Delay ↓
```

For higher slew and higher load:

```text
Input Slew ↑
Output Load ↑
      │
      ▼
Cell Delay ↑
```

### 📌 Summary

| Condition             | Cell Delay |
| :-------------------- | :--------: |
| Fast Slew + Low Load  |     Low    |
| Fast Slew + High Load |  Moderate  |
| Slow Slew + Low Load  |  Moderate  |
| Slow Slew + High Load |    High    |

> **Both input transition and output loading have a direct influence on standard-cell delay.**

---

# 4. 🕐 Setup Timing Analysis

**Setup timing analysis** verifies that data reaches the receiving flip-flop sufficiently before the active clock edge.

A typical sequential path consists of:

```text
Launch FF
    │
    ▼
Combinational Logic
    │
    ▼
Capture FF
```

The data launched by the first flip-flop must travel through the combinational logic and arrive at the capture flip-flop within the allowed timing interval.

### 🔹 Setup Requirement

For an ideal clock:

```text
Data Delay < Clock Period - Setup Time
```

When clock uncertainty is considered:

```text
Available Time
=
Clock Period
- Setup Time
- Setup Uncertainty
```

### 🔹 Setup Slack

```text
Setup Slack
=
Required Time - Arrival Time
```

### 🔹 Timing Condition

```text
Setup Slack > 0
       │
       ▼
  Timing Passed
```

```text
Setup Slack < 0
       │
       ▼
  Setup Violation
```

### 🔹 Example

Assume:

```text
Clock Period   = 1 ns
Setup Time     = 0.10 ns
Uncertainty    = 0.05 ns
```

The available timing window becomes:

```text
1 - 0.10 - 0.05
= 0.85 ns
```

Therefore, the data path must complete within **0.85 ns** under these conditions.

### 💡 Key Idea

> **Setup analysis verifies that data does not reach the capture element too late.**

---

# 5. 🔒 Hold Timing Analysis

**Hold timing analysis** verifies that the data remains unchanged for the required duration immediately after the active clock edge.

```text
                   Hold Window
                       │
                       ▼
Clock ────────────────┼────────────
                       │
Data  ────────────────┴────────────
                       │
                 Must remain stable
```

The main concern is that data should not arrive at the capture flip-flop too quickly.

### 🔹 Hold Requirement

For an ideal clock:

```text
Data Delay > Hold Time
```

For a real clock:

```text
O + d₁ > H + d₂
```

Where:

* **O** = Data path delay
* **d₁** = Launch clock delay
* **d₂** = Capture clock delay
* **H** = Hold time

### 🔹 Hold Slack

```text
Hold Slack
=
Arrival Time - Required Time
```

### 🔹 Timing Condition

```text
Hold Slack > 0
       │
       ▼
   Timing Passed
```

```text
Hold Slack < 0
       │
       ▼
   Hold Violation
```

### 🔹 Setup vs Hold

| Setup                                      | Hold                                         |
| :----------------------------------------- | :------------------------------------------- |
| Checks late-arriving data                  | Checks early-arriving data                   |
| Maximum-delay analysis                     | Minimum-delay analysis                       |
| Evaluated with respect to the capture edge | Evaluated immediately after the capture edge |
| Setup time is important                    | Hold time is important                       |

### 💡 Key Idea

> **Hold analysis ensures that data does not reach the receiving flip-flop earlier than permitted.**

---

# 6. ⚠️ Clock Jitter and Uncertainty

In an actual digital system, clock edges may not occur at precisely the expected instants.

This variation in the clock edge position is called **clock jitter**.

```text
Expected Clock Edge
        │
        ▼
────────┼────────
     ← Jitter →
```

The actual clock edge may shift slightly forward or backward in time.

## 🔹 Clock Uncertainty

Clock uncertainty represents the timing margin reserved for variations associated with the clock.

It may include:

* Clock jitter
* Clock variation
* Other clock-related uncertainties

### 🔹 Effect on Setup Timing

```text
Available Time
=
Clock Period
- Setup Time
- Clock Uncertainty
```

Therefore:

```text
Clock Uncertainty ↑
        │
        ▼
Timing Margin ↓
```

### 🔹 Importance

Because the exact clock arrival time cannot always be guaranteed, the entire clock period cannot be treated as usable timing space.

Including uncertainty makes the timing analysis more conservative and realistic.

### 💡 Key Idea

> **Clock jitter and uncertainty decrease the timing margin available for data propagation.**

---

# 7. 🌳 Clock Tree Synthesis

**Clock Tree Synthesis (CTS)** creates a physical clock distribution network between the clock source and sequential elements such as flip-flops.

A clock source may have to drive hundreds or thousands of sequential elements.

A direct connection can result in:

* High fanout
* High capacitance
* Increased delay
* Unequal clock arrival times

CTS addresses these issues by constructing a structured clock network, generally using buffers.

### 🔹 Basic Clock Tree

```text
                    Clock Source
                         │
                       Buffer
                         │
                  ┌──────┴──────┐
                  │             │
               Buffer        Buffer
                 │               │
            ┌────┴────┐     ┌────┴────┐
            │         │     │         │
           FF1       FF2    FF3       FF4
```

### 🔹 Objectives of CTS

The CTS process attempts to:

* Minimize clock skew
* Manage clock latency
* Balance clock paths
* Improve clock transition quality
* Drive high-capacitance loads
* Preserve clock signal integrity

### 🔹 TritonCTS

Within the OpenLane/OpenROAD environment, **TritonCTS** performs Clock Tree Synthesis.

The CTS stage can be initiated using:

```tcl
run_cts
```

### 💡 Key Idea

> **CTS builds a balanced clock network so that the clock can reach sequential elements with controlled skew, latency and transition.**

---

# 8. 📐 Clock Skew and Clock Latency

## 🔹 Clock Skew

Clock skew is the difference between the arrival times of a clock edge at two different sequential elements.

```text
                 Clock Source
                      │
                ┌─────┴─────┐
                │           │
               FF1         FF2
                │           │
               t₁           t₂
```

Therefore:

```text
Clock Skew = t₂ - t₁
```

Ideally:

```text
Clock Skew ≈ 0
```

### 🔹 Why Skew Matters?

Excessive clock skew can influence:

* Setup timing
* Hold timing
* Available timing margin
* Sequential circuit operation

---

## 🔹 Clock Latency

Clock latency is the time required for a clock signal to travel from its source to the destination sequential element.

It can include:

```text
Clock Latency
=
Buffer Delay
+
Wire Delay
+
RC Effects
```

### 🔹 Clock Network

```text
Clock Source
     │
     ▼
   Buffer
     │
     ▼
    Wire
     │
     ▼
   Buffer
     │
     ▼
  Flip-Flop
```

### 💡 Key Idea

> **A well-designed clock network aims to make clock arrival predictable and balanced across sequential elements.**

---

# 9. 🔊 Crosstalk and Signal Integrity

When two physically adjacent interconnects interact through coupling capacitance, the resulting unwanted interaction is known as **crosstalk**.

```text
Aggressor Wire
══════════════════════════
          ↕
   Coupling Capacitance
          ↕
Victim Wire
══════════════════════════
```

A transition on the aggressor line can introduce an unwanted disturbance on the victim line.

### 🔹 Effects of Crosstalk

Crosstalk may result in:

* Noise
* Glitches
* Delay changes
* Timing variation
* Clock disturbance
* Signal integrity issues

### 🔹 Crosstalk and Delay

```text
Normal Delay
     +
Crosstalk Effect
     │
     ▼
Actual Delay
```

This effect is especially important for critical signals such as clock networks.

### 🔹 Signal Integrity

Signal integrity refers to maintaining the intended quality and behavior of signals as they travel through the physical interconnect.

Important factors include:

* Coupling
* Crosstalk
* Noise
* Delay variation
* Transition degradation

### 💡 Key Idea

> **Physical coupling between nearby wires can alter both signal quality and timing behavior.**

---

# 10. 🛡️ Clock Shielding

The clock is a highly timing-sensitive signal. Coupling from nearby signal wires can introduce noise and unwanted timing variations.

Clock shielding is a routing technique used to reduce this coupling.

### 🔹 Simplified Structure

```text
       Shield          Clock          Shield
══════════════       ═══════       ══════════════
      GND               CLK              GND
```

The shield can be connected to a stable supply such as:

```text
VDD
or
GND
```

### 🔹 Benefits

Clock shielding can help:

* Lower coupling
* Reduce crosstalk
* Reduce noise
* Protect the clock signal
* Improve timing stability

### 💡 Key Idea

> **Sensitive signals such as clocks can require dedicated routing and shielding to reduce unwanted interference.**

---

# 11. 🔄 Ideal Clock vs Real Clock

Timing analysis can be performed with an idealized clock before CTS and with a propagated physical clock after CTS.

## 🔹 Ideal Clock

Before CTS, the clock can be considered ideal.

```text
              Clock
                │
          ┌─────┴─────┐
          │           │
         FF1         FF2
```

The physical delay of the clock distribution network is not considered.

This representation is useful during the early stages of timing analysis.

---

## 🔹 Real Clock

After CTS, the clock travels through the physical clock network.

```text
              Clock
                │
              Buffer
                │
               Wire
                │
          ┌─────┴─────┐
       Buffer       Buffer
          │             │
         FF1           FF2
```

The analysis now considers:

* Clock buffers
* Clock wire delay
* Clock latency
* Clock skew
* Parasitic effects

### 🔹 Comparison

| Feature         | Ideal Clock |   Real Clock   |
| :-------------- | :---------: | :------------: |
| Clock Buffers   |      ❌      |        ✅       |
| Wire Delay      |      ❌      |        ✅       |
| Clock Latency   |  Idealized  |    Included    |
| Clock Skew      |  Idealized  |      Real      |
| RC Effects      |      ❌      |        ✅       |
| Timing Accuracy | Preliminary | More realistic |

### 🔄 Transition

```text
Ideal Clock
     │
     ▼
Pre-CTS Timing Analysis
     │
     ▼
Clock Tree Synthesis
     │
     ▼
Real / Propagated Clock
     │
     ▼
Post-CTS Timing Analysis
```

### 💡 Key Idea

> **Propagated-clock analysis gives a more realistic timing picture because the physical clock network is included.**

---

# 12. 🔬 Static Timing Analysis using OpenSTA

**Static Timing Analysis (STA)** is used to determine whether the timing requirements of a digital design are satisfied.

OpenSTA analyzes timing paths without requiring functional simulation vectors.

### 🔹 Typical Timing Path

```text
Launch Flip-Flop
       │
       ▼
Combinational Logic
       │
       ▼
Capture Flip-Flop
```

OpenSTA can determine:

* Arrival time
* Required time
* Slack
* Setup timing
* Hold timing
* Clock skew
* Critical timing paths

### 🔹 Slack

The basic timing relationship is:

```text
Slack = Required Time - Arrival Time
```

### 🔹 Timing Result

```text
Positive Slack
      │
      ▼
Timing Passed
```

```text
Zero Slack
      │
      ▼
Timing Met
```

```text
Negative Slack
      │
      ▼
Timing Violation
```

### 🔹 Propagated Clock

After CTS, the physical clock network can be propagated during timing analysis.

This enables the timing engine to account for:

* Clock buffer delay
* Clock wire delay
* Clock latency
* Clock skew

### 💡 Key Idea

> **OpenSTA evaluates timing paths to determine whether setup and hold requirements are satisfied.**

---

# 13. 📈 WNS and TNS

Two important metrics used to evaluate timing quality are:

* **WNS — Worst Negative Slack**
* **TNS — Total Negative Slack**

---

## 🔹 WNS — Worst Negative Slack

WNS represents the smallest slack value among the analyzed timing paths.

```text
WNS = Minimum Slack
```

### Example

```text
Path 1 → +0.20 ns
Path 2 → -0.05 ns
Path 3 → -0.15 ns
```

Therefore:

```text
WNS = -0.15 ns
```

A negative WNS indicates that at least one analyzed timing path violates its timing requirement.

---

## 🔹 TNS — Total Negative Slack

TNS represents the sum of the negative slack values from all violating paths.

```text
TNS = Sum of Negative Slack Values
```

### Example

```text
Path 1 → -0.05 ns
Path 2 → -0.10 ns
Path 3 → -0.15 ns
```

Therefore:

```text
TNS = -0.30 ns
```

### 🔹 Desired Timing Condition

```text
WNS ≥ 0
```

and

```text
TNS = 0
```

These conditions indicate that there are no negative-slack timing paths.

### 💡 Key Idea

> **WNS identifies the most critical timing violation, whereas TNS represents the combined negative slack of violating paths.**

---

# 14. 💻 Important OpenLane and OpenROAD Commands

The following commands are useful during the physical-design and timing-analysis flow.

## 🔹 Run Synthesis

```tcl
run_synthesis
```

---

## 🔹 Run Floorplan

```tcl
run_floorplan
```

---

## 🔹 Run Placement

```tcl
run_placement
```

---

## 🔹 Run Clock Tree Synthesis

```tcl
run_cts
```

---

## 🔹 Check Synthesis Strategy

```tcl
echo $::env(SYNTH_STRATEGY)
```

---

## 🔹 Enable Synthesis Buffering

```tcl
set ::env(SYNTH_BUFFERING) 1
```

---

## 🔹 Enable Synthesis Sizing

```tcl
set ::env(SYNTH_SIZING) 1
```

---

## 🔹 Start OpenROAD

```bash
openroad
```

---

## 🔹 Generate Timing Report

```tcl
report_checks -path_delay min_max \
-format full_clock_expanded \
-digits 4
```

---

## 🔹 Report Setup Clock Skew

```tcl
report_clock_skew -setup
```

---

## 🔹 Report Hold Clock Skew

```tcl
report_clock_skew -hold
```

### 🔹 Important Timing Values

When examining a timing report, the following parameters are important:

| Parameter         | Meaning                                        |
| :---------------- | :--------------------------------------------- |
| **Arrival Time**  | Time at which data reaches the endpoint        |
| **Required Time** | Permitted timing limit for data arrival        |
| **Slack**         | Difference between required and actual arrival |
| **Data Delay**    | Delay introduced by the data path              |
| **Clock Delay**   | Delay through the clock network                |
| **Clock Skew**    | Difference between clock arrival times         |
| **WNS**           | Worst slack in the timing analysis             |
| **TNS**           | Combined negative slack                        |

---

# 15. 🎯 Module 4 Key Takeaways

### 🧠 Major Concepts

* Standard-cell delay depends on **input slew and output load**.
* Delay tables contain characterized timing information.
* Setup analysis checks for **late-arriving data**.
* Hold analysis checks for **early-arriving data**.
* Clock jitter represents variation in clock-edge timing.
* Clock uncertainty reduces the timing margin.
* Clock Tree Synthesis creates the physical clock distribution network.
* TritonCTS performs Clock Tree Synthesis in the OpenLane/OpenROAD flow.
* Clock skew represents differences in clock arrival time.
* Clock latency represents the time required for the clock to reach a sequential element.
* Crosstalk results from coupling between nearby interconnects.
* Crosstalk can modify both delay and signal quality.
* Clock shielding reduces coupling around sensitive clock routes.
* Ideal clocks are useful during preliminary timing analysis.
* Real clocks include the delays introduced by the physical clock network.
* OpenSTA is used for Static Timing Analysis.
* WNS represents the worst slack value.
* TNS represents the total negative slack.
* Both setup and hold requirements need to be satisfied.

---

# 🔄 Complete Module 4 Flow

```text
                 Timing Modelling
                       │
                       ▼
                  Delay Tables
                       │
                       ▼
               Input Slew + Load
                       │
                       ▼
                 Ideal Clock STA
                       │
                       ▼
                    Placement
                       │
                       ▼
              Clock Tree Synthesis
                   (TritonCTS)
                       │
                       ▼
               Clock Skew Analysis
                       │
                       ▼
             Crosstalk / Signal Integrity
                       │
                       ▼
                  Real Clock STA
                       │
                 ┌─────┴─────┐
                 ▼           ▼
               Setup        Hold
                 │           │
                 └─────┬─────┘
                       ▼
                Timing Reports
                       │
                       ▼
                   WNS / TNS
                       │
                       ▼
                 Timing Closure
```

---

# 📊 Setup vs Hold — Quick Reference

| Feature           |         Setup        |         Hold        |
| :---------------- | :------------------: | :-----------------: |
| Main Check        |   Data arrives late  |  Data arrives early |
| Timing Type       |     Maximum delay    |    Minimum delay    |
| Related Parameter |      Setup Time      |      Hold Time      |
| Uncertainty       |   Setup Uncertainty  |   Hold Uncertainty  |
| Violation         | Negative Setup Slack | Negative Hold Slack |
| Target            |       Slack ≥ 0      |      Slack ≥ 0      |

---

# 📊 Ideal Clock vs Real Clock

| Feature         | Ideal Clock |   Real Clock   |
| :-------------- | :---------: | :------------: |
| CTS             |  Before CTS |    After CTS   |
| Clock Network   |  Idealized  |    Physical    |
| Buffers         |   Ignored   |    Included    |
| Wire Delay      |   Ignored   |    Included    |
| Skew            |  Idealized  |    Measured    |
| Latency         |  Idealized  |    Included    |
| RC Effects      |   Ignored   |    Included    |
| Timing Accuracy | Preliminary | More realistic |

---

# 🛠️ Tools Used

| Tool           | Purpose                                    |
| :------------- | :----------------------------------------- |
| **OpenLane**   | Automated RTL-to-GDSII implementation flow |
| **Yosys**      | RTL synthesis                              |
| **OpenROAD**   | Physical design implementation             |
| **OpenSTA**    | Static Timing Analysis                     |
| **TritonCTS**  | Clock Tree Synthesis                       |
| **SKY130 PDK** | Process and technology information         |
| **Magic**      | Layout and physical verification           |
| **Linux**      | VLSI design environment                    |

---

# 📌 Final Summary

The major progression of Module 3 can be summarized as:

```text
Timing Modelling
       ↓
Delay Tables
       ↓
Input Slew & Output Load
       ↓
Setup & Hold Analysis
       ↓
Clock Jitter & Uncertainty
       ↓
Clock Tree Synthesis
       ↓
Clock Skew & Latency
       ↓
Crosstalk
       ↓
Clock Shielding
       ↓
Ideal Clock Analysis
       ↓
Real Clock Analysis
       ↓
OpenSTA
       ↓
WNS & TNS
       ↓
Timing Closure
```

The module demonstrates how timing analysis progresses from basic standard-cell characterization to a physical clock network and finally to realistic timing verification.

Physical implementation introduces several additional effects:

```text
Cell Delay
    +
Wire Delay
    +
Clock Delay
    +
Clock Skew
    +
RC Effects
    +
Crosstalk
    ↓
Realistic Timing Analysis
```

---

# 🏁 Conclusion

Module 4 focuses on timing analysis, clock distribution and timing closure within the ASIC physical-design flow.

The study begins with standard-cell timing models and delay tables, followed by the analysis of input slew and output loading. Setup and hold checks are then used to determine whether data reaches sequential elements within the required timing windows.

The analysis is extended to real clock behavior by introducing jitter, uncertainty, clock latency and clock skew. Clock Tree Synthesis creates the physical clock network, while crosstalk and shielding considerations help maintain signal integrity.

Finally, OpenSTA is used to analyze timing paths and obtain important metrics such as slack, WNS and TNS.

The overall concept can be represented as:

```text
                     TIMING
                       │
              ┌────────┴────────┐
              ▼                 ▼
           DATA PATH         CLOCK PATH
              │                 │
              ▼                 ▼
        Cell + Wire          CTS + Buffers
           Delay              + Wire Delay
              │                 │
              └────────┬────────┘
                       ▼
                    OpenSTA
                       │
                 ┌─────┴─────┐
                 ▼           ▼
               Setup        Hold
                 │           │
                 └─────┬─────┘
                       ▼
                 Timing Closure
```

This module provides a foundation for understanding timing-driven physical design, clock distribution and timing verification in modern VLSI implementation flows.
