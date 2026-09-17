**# Pipeline Oil Flow Measurement**

**# Digital Rotameter Scaling & Flow Rate Calculation**

## 1.0 Project Overview
This repository contains the software assets and Functional Design Specification (FDS) for processing discrete inputs from a digital rotameter to calculate real-time fluid flow. The system utilizes timers and mathematical instructions to convert volumetric pulse data (k-factor) into a continuous Gallons Per Minute (GPM) process variable.

**Target Hardware/Environment:** RSLogix 500 Micro Starter Lite - free license                                                                                                                                                                                                                                                                                                        
**Programming Language:** Ladder Diagram (LD)

## 2.0 System Architecture & Process Flow
The physical architecture features a pipeline equipped with a paddle-wheel digital rotameter. For simulation purposes, the rotameter's hardware pulse is substituted with an internal bit triggered by a programmable timer.

*Please refer to the process diagram:* `Process_Diagram_Rotameter.png`

## 3.0 Control Philosophy
The control system calculates the flow rate based on the following logic and mathematical calculation:

1. **Pulse Generation (Simulation):** A self-resetting timer pulses the internal bit (`B3:0/0`) at a predetermined interval (initially set to 12 seconds) to simulate the physical rotameter input.
2. **K-Factor Scaling:** The rotameter is calibrated with a k-factor of 6.3 gallons per pulse.
3. **Flow Calculation:** Upon receiving a pulse, the logic updates the flow rate every 12 seconds.

## 4.0 I/O Allocation Schedule

| Tag / Address | I/O Type | Device Description | Field State Definition |
| :--- | :--- | :--- | :--- |
| `B3:0/0` | Internal Bit | Simulated Rotameter Pulse | 1 = 6.3 Gallons Accumulated |
| `F8:0` | Float File | Real-Time Flow Rate | Output scaled in GPM |

## 5.0 Simulation & Verification Protocol (Dry Run Test)

The logic was validated via RSLogix Emulate software. The system successfully passed the following Dry Run Test simulation states, verifying the math logic under varying process conditions:

| Test State | Condition / Input Action | Expected Output | Status |
| :--- | :--- | :--- | :---: |
| **1. Startup (12s Pulse)** | Run emulator with default 12-second pulse timer. | Flow `F8:0` initializes at **0.0** | ✅ PASS |
| **2. Steady State (12s)** | Allow system to run for ~60 seconds. | Flow `F8:0` settles and holds at **31.5 GPM** | ✅ PASS |
| **3. Dynamic Change (2s)** | Adjust pulse timer preset from 12 seconds to 2 seconds. | Flow `F8:0` recalculates based on new frequency. | ✅ PASS |
| **4. Steady State (2s)** | Allow system to run for ~60 seconds under new preset. | Flow `F8:0` settles and holds at **189.0 GPM** | ✅ PASS |

## 6.0 Software Assets
* The raw ladder logic project file (`.RSS`) can be found in the `/src/` directory.
* A complete PDF export of the ladder logic program is available in the `/docs/` directory.

## 7.0 Acknowledgements & My Learning Journey
This project is a reflection of my ongoing, highly structured learning journey in PLC programming. The online courses I undertook made it incredibly easy to grasp the fundamentals and challenged me to apply critical thinking—specifically the 80/20 rule. By truly mastering just 20% of the core instruction sets, we can effectively execute 80% of real-world automation tasks. 

Special thanks to the course instructor for the exercise materials:
* **Course Detail:** Applied Logic (via Udemy) by Paul Lynn
* **Project Concept:** The base process flow, diagrams, and core test criteria were provided as part of his excellent course materials.