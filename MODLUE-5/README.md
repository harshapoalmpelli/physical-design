# 🚀 Module 5 – RTL to GDSII Physical Design Flow

<p align="center">
  <b>PDN • Routing • DRC • Parasitic Extraction • STA • GDSII</b>
</p>

<p align="center">
  Implementation of the final physical-design stages of an inverter
  using OpenLane, OpenROAD, TritonRoute, Magic, KLayout and OpenSTA
  with the SKY130A technology.
</p>

---

## 📚 Table of Contents

1. [🎯 Objective](#1--objective)
2. [🛠️ Tools and Technologies](#2--tools-and-technologies)
3. [📁 Design and Run Details](#3--design-and-run-details)
4. [🐳 OpenLane Docker Setup](#4--openlane-docker-setup)
5. [🔍 Design and Run Verification](#5--design-and-run-verification)
6. [⚡ Physical Design Stages](#6--physical-design-stages)
7. [🔋 Power Distribution Network](#7--power-distribution-network)
8. [🛣️ Detailed Routing](#8--detailed-routing)
9. [🔎 DRC Verification](#9--drc-verification)
10. [📡 SPEF Generation](#10--spef-generation)
11. [📊 Post-Route STA](#11--post-route-sta)
12. [📈 WNS and TNS Analysis](#12--wns-and-tns-analysis)
13. [🎨 Final GDSII Generation](#13--final-gdsii-generation)
14. [🔬 KLayout Inspection](#14--klayout-inspection)
15. [📂 Generated Files](#15--generated-files)
16. [🔄 Complete Module 5 Flow](#16--complete-module-5-flow)
17. [📊 Final Verification Summary](#17--final-verification-summary)
18. [🎯 Important Learning Points](#18--important-learning-points)
19. [🏁 Conclusion](#19--conclusion)

---

# 1. 🎯 Objective

The objective of **Module 5** is to perform the final physical-design steps
for the **inverter** circuit and obtain its final GDSII layout.

The major activities performed in this module include:

* Power Distribution Network generation
* Detailed routing of the design
* Design Rule Check
* Parasitic extraction
* SPEF generation
* Post-route Static Timing Analysis
* WNS and TNS analysis
* Final GDSII generation
* Physical-layout inspection using KLayout

The overall physical-design sequence is:

```text
RTL
 │
 ▼
Synthesis
 │
 ▼
Floorplanning
 │
 ▼
Placement
 │
 ▼
Clock Tree Synthesis
 │
 ▼
PDN Generation
 │
 ▼
Detailed Routing
 │
 ▼
DRC Verification
 │
 ▼
Parasitic Extraction
 │
 ▼
SPEF
 │
 ▼
Post-Route STA
 │
 ▼
GDSII Generation
 │
 ▼
KLayout Inspection


---

2. 🛠️ Tools and Technologies

Tool / Technology	Main Function

OpenLane	Complete RTL-to-GDSII implementation flow
Docker	Provides the OpenLane execution environment
OpenROAD	Physical-design implementation
TritonRoute	Detailed routing
OpenSTA	Static Timing Analysis
Magic	Layout processing and GDSII generation
KLayout	GDSII viewing and layout verification
SKY130A PDK	Technology and manufacturing design rules



---

3. 📁 Design and Run Details

🔹 Design Name

inverter

🔹 Technology Used

SKY130A

🔹 OpenLane Docker Version

efabless/openlane:v0.21

🔹 Selected Run


🔹 OpenLane Working Directory

/home/vsduser/Desktop/work/tools/openlane_working_dir/openlane

🔹 Design Location

designs/inverter

🔹 Module 5 Run Directory

designs/inverter/runs/26-09_15-08


---

4. 🐳 OpenLane Docker Setup

The OpenLane environment was executed inside a Docker container.

The following command was used to launch the OpenLane image:

sudo docker run -it \
-v $HOME/Desktop/work/tools/openlane_working_dir:/home/vsduser/share \
efabless/openlane:v0.21

After entering the container, the OpenLane directory was selected:

cd /home/vsduser/share/openlane

The current directory can be verified using:

pwd

Expected output:

/home/vsduser/share/openlane

This environment provides all the required tools for carrying out the physical-design flow.


---

5. 🔍 Design and Run Verification

The inverter design directory was examined using:

ls designs/inverter

Previously generated physical-design runs were listed using:

ls -lt designs/inverter/runs


For Module 5, the following run was selected:

This run contains the required physical-design results and reports.


---

6. ⚡ Physical Design Stages

The major physical-design stages completed in Module 5 are shown below:

Existing Design
                    │
                    ▼
              Clock Tree
              Synthesis
                    │
                    ▼
             PDN Generation
                    │
                    ▼
                Routing
                    │
                    ▼
              DRC Checking
                    │
                    ▼
          Parasitic Extraction
                    │
                    ▼
                  SPEF
                    │
                    ▼
                 OpenSTA
                    │
                    ▼
              WNS / TNS
                    │
                    ▼
               Final GDS
                    │
                    ▼
                KLayout

The required outputs were obtained from the selected run directory.


---

7. 🔋 Power Distribution Network

The Power Distribution Network (PDN) is responsible for delivering power and ground connections throughout the physical design.

The PDN consists of components such as:

Power rings

Power straps

Standard-cell power rails

VDD connections

VSS connections


The OpenLane command associated with PDN generation is:

gen_pdn

The PDN stage occurs after placement/CTS and before routing.

Placement / CTS
       │
       ▼
PDN Generation
       │
       ▼
Detailed Routing

💡 Important Point

> The PDN establishes the physical VDD and VSS network required to supply power and ground to the cells in the layout.




---

8. 🛣️ Detailed Routing

Routing establishes the physical connections between the placed standard cells according to the design netlist.

The OpenLane routing command is:

run_routing

The routed DEF file generated for the inverter is:

results/routing/inverter.def

A corresponding routing image is:

results/routing/inverter.def.png

The routing process can be represented as:

Placed Standard Cells
          │
          ▼
    Routing Engine
          │
          ▼
 Metal Interconnections
          │
          ▼
       Routed DEF

💡 Important Point

> Routing converts the logical connections of the design into physical metal interconnections in the chip layout.




---

9. 🔎 DRC Verification

Design Rule Check (DRC) is performed to determine whether the physical layout follows the manufacturing rules defined by the selected technology.

The TritonRoute DRC report is located at:

reports/routing/22-tritonRoute.drc

The report can be viewed using:

cat reports/routing/22-tritonRoute.drc

The command produced no output.

Therefore:

No DRC violations were reported.


---

🔹 KLayout DRC Result

The KLayout DRC report is available at:

reports/routing/22-tritonRoute.klayout.xml

It was inspected using:

head -50 reports/routing/22-tritonRoute.klayout.xml

The report included:

<categories/>
<items/>

This indicates that no violation items were reported in the generated KLayout DRC result.

💡 Important Point

> DRC verification ensures that the physical layout follows the applicable technology design rules.




---

10. 📡 SPEF Generation

Once routing is completed, parasitic effects from the physical interconnections are extracted.

The resulting file is:

results/routing/inverter.spef

SPEF means:

Standard Parasitic Exchange Format

The SPEF file contains extracted parasitic information from the routed design and is used for post-route timing analysis.

🔹 Parasitic Extraction Flow

Routed DEF
    │
    ▼
Parasitic Extraction
    │
    ▼
   SPEF
    │
    ▼
Post-Route STA

💡 Important Point

> SPEF represents the extracted parasitic information required for more realistic post-route timing analysis.




---

11. 📊 Post-Route STA

After generating the SPEF file, Static Timing Analysis was performed using the post-route parasitic information.

The main STA report is:

reports/synthesis/25-opensta_spef.rpt

It can be examined using:

cat reports/synthesis/25-opensta_spef.rpt

The generated report showed:

No paths found.

The WNS report was checked using:

cat reports/synthesis/25-opensta_spef_wns.rpt

The reported value was:

wns 0.00

The TNS report was checked using:

cat reports/synthesis/25-opensta_spef_tns.rpt

The reported value was:

tns 0.00

🔹 STA Results

Timing Parameter	Result

WNS	0.00
TNS	0.00
STA Report	No paths found.


The No paths found message is retained as reported by the generated STA output.

💡 Important Point

> Post-route STA evaluates timing using the physical design and extracted interconnect parasitics.




---

12. 📈 WNS and TNS Analysis

Two commonly used timing indicators are WNS and TNS.


---

🔹 Worst Negative Slack – WNS

WNS represents the minimum slack value among the timing paths considered by the timing analysis.

WNS = Minimum Reported Slack

The generated report gives:

WNS = 0.00


---

🔹 Total Negative Slack – TNS

TNS represents the accumulated negative slack of the analyzed timing paths.

TNS = Sum of Negative Slack

The generated report gives:

TNS = 0.00

🔹 Reported Values

WNS = 0.00
TNS = 0.00

💡 Important Point

> WNS indicates the minimum reported slack, while TNS represents the total negative slack reported by the timing analysis.




---

13. 🎨 Final GDSII Generation

The final physical layout was converted into GDSII format.

The generated GDS file is:

results/magic/inverter.gds

The corresponding GDS image is:

results/magic/inverter.gds.png

The generated image size was approximately:

293 KB

The image size can be checked with:

ls -lh \
~/Desktop/work/tools/openlane_working_dir/openlane/designs/inverter/runs/26-09_15-08/results/magic/inverter.gds.png

🔹 GDSII Generation Flow

Routed Layout
      │
      ▼
    Magic
      │
      ▼
 inverter.gds
      │
      ▼
   KLayout

💡 Important Point

> GDSII is the final physical-layout database representing the implemented design geometry.




---

14. 🔬 KLayout Inspection

The final GDSII file was opened using KLayout to inspect the completed physical layout.

First, move to the OpenLane working directory:

cd ~/Desktop/work/tools/openlane_working_dir/openlane

Then open the final GDS file:

klayout \
designs/inverter/runs/26-09_15-08/results/magic/inverter.gds

KLayout allows the final inverter layout to be visually inspected and verified.


---

15. 📂 Generated Files

The major output files generated during Module 5 are:

results/
│
├── routing/
│   ├── inverter.def
│   ├── inverter.def.png
│   └── inverter.spef
│
└── magic/
    ├── inverter.gds
    ├── inverter.gds.png
    ├── inverter.lef
    ├── inverter.lef.mag
    ├── inverter.mag
    └── .magicrc

The important verification reports are:

reports/
│
├── routing/
│   ├── 22-tritonRoute.drc
│   └── 22-tritonRoute.klayout.xml
│
└── synthesis/
    ├── 25-opensta_spef.rpt
    ├── 25-opensta_spef_tns.rpt
    ├── 25-opensta_spef_wns.rpt
    ├── 25-opensta_spef.min_max.rpt
    ├── 25-opensta_spef.slew.rpt
    └── 25-opensta_spef.timing.rpt


---

16. 🔄 Complete Module 5 Flow

The complete physical-design process followed in Module 5 is:

Inverter
                     │
                     ▼
               Existing Run
                     │
                     ▼
              PDN Generation
                     │
                     ▼
                  Routing
                     │
                     ▼
               TritonRoute
                     │
                     ▼
              DRC Verification
                     │
                     ▼
                Routed DEF
                     │
                     ▼
           Parasitic Extraction
                     │
                     ▼
                   SPEF
                     │
                     ▼
                 OpenSTA
                     │
                     ▼
                WNS / TNS
                     │
                     ▼
                Final GDSII
                     │
                     ▼
                 KLayout
                     │
                     ▼
             Final Layout


---

17. 📊 Final Verification Summary

Parameter	Result

Design	Inverter
Technology	SKY130A
OpenLane Version	v0.21
Run	
PDN	✅ Completed
Routing	✅ Completed
Routed DEF	✅ Generated
SPEF	✅ Generated
TritonRoute DRC	✅ No violations reported
KLayout DRC	✅ No violation items reported
Post-SPEF STA	✅ Report generated
WNS	0.00
TNS	0.00
Final GDSII	✅ Generated
GDS Image	✅ Generated
KLayout Inspection	✅ Completed



---

18. 🎯 Important Learning Points

The important concepts covered in Module 5 are:

PDN creates the physical power and ground distribution network.

Routing establishes physical connections between standard cells.

TritonRoute performs detailed routing.

DRC checks the layout against technology-specific design rules.

SPEF stores extracted parasitic information.

Post-route STA analyzes timing using physical parasitic effects.

WNS indicates the minimum reported slack.

TNS represents the accumulated negative slack.

Magic is used for physical-layout processing and GDSII generation.

KLayout is useful for viewing and inspecting GDSII layouts.

GDSII represents the final physical implementation of the design.



---

19. 🏁 Conclusion

Module 5 successfully covers the final stages of the physical-design flow for the inverter using the SKY130A PDK and OpenLane.

The main stages completed were:

PDN Generation
      ↓
Routing
      ↓
DRC Verification
      ↓
Parasitic Extraction
      ↓
SPEF Generation
      ↓
Post-Route STA
      ↓
WNS / TNS
      ↓
GDSII Generation
      ↓
KLayout Inspection

The routed DEF, SPEF, DRC reports, OpenSTA reports and final GDSII files were generated as part of the implementation.

The TritonRoute DRC report did not report any violations, while the KLayout DRC output contained no violation items.

The final GDSII database was generated successfully and opened in KLayout for inspection of the completed inverter layout.


---

📌 Final Module 5 Result

MODULE 5
                       │
             ┌─────────┴─────────┐
             ▼                   ▼
           PDN                 Routing
             │                   │
             └─────────┬─────────┘
                       ▼
                      DRC
                       │
                       ▼
                     SPEF
                       │
                       ▼
                    OpenSTA
                       │
                 ┌─────┴─────┐
                 ▼           ▼
                WNS          TNS
               0.00         0.00
                 │
                 ▼
             Final GDSII
                 │
                 ▼
              KLayout
                 │
                 ▼
          Final Inverter Layout
