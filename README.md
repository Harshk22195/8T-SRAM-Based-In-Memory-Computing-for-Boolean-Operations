# Boolean Operation Implementation in 8T SRAM for In-Memory Computing

## Project Overview

This project, completed as part of the **ECE-611 (Memory Design and Test)** course by **Group Number 18**, presents the design and implementation of an **8T SRAM-based In-Memory Computing (IMC)** circuit. The primary goal is to seamlessly integrate **NAND** and **NOR** logic functions directly within the memory array to overcome the data transfer bottleneck (von Neumann bottleneck) while maintaining stability and energy efficiency.

## Team Members

- Mrinank (2022305)
- Mohit Mehra (2022301)
- Harshit Sagar (2022210)
- Harsh (2022195)

## Motivation & Background

### The Von Neumann Bottleneck
Traditional computing architectures require constant data movement between the processor and memory, leading to significant delays and energy consumption.

### In-Memory Computing (IMC)
IMC performs calculations directly within the memory unit, eliminating the need to transfer data back and forth. This reduces latency and saves power.

### Why 8T SRAM?
Initial designs using standard **6T SRAM cells** suffered from **read disturb** issues. Activating multiple rows simultaneously (common in IMC) causes excessive bitline voltage swing, which can accidentally flip stored bits.

The **8T SRAM** cell solves this by providing separate read and write paths, isolating the storage nodes from read operations and eliminating read disturb.

## Design Details

### 8T SRAM Bitcell Sizing (W/L in μm)

| Transistor Type | Width/Length (μm) |
| :-------------- | :---------------- |
| Pull-Up (PU)    | 0.135             |
| Pass Gate (PG)  | 0.205             |
| Pull-Down (PD)  | 0.270             |

### Implemented Logic Operations

#### NOR Function
- **Precharge:** Bitline is precharged to VDD.
- **Operation:** When the wordline is activated, NMOS transistors respond to input values.
- **Truth Table:** Output is HIGH (1) only when both inputs are LOW (0).
  - Inputs `00` → Bitline stays high
  - Inputs `01`, `10`, `11` → At least one NMOS turns on, discharging bitline low.
- **Result:** Implements NOR logic.

#### NAND Function
- **Technique:** Bitline capacitance is increased, and wordline pulse width is shortened. This slows down the discharge rate.
- **Operation:**
  - Inputs `01` or `10` → Only one NMOS conducts; discharge is too slow within the short pulse → bitline remains high.
  - Inputs `11` → Both NMOS transistors conduct, enabling fast discharge → bitline pulls low.
- **Result:** Bitline discharges only for input `11`, implementing NAND logic.

#### Truth Table Summary

| Input A | Input B | NAND Output | NOR Output |
| :-----: | :-----: | :---------: | :---------: |
|    0    |    0    |      1      |      1      |
|    0    |    1    |      1      |      0      |
|    1    |    0    |      1      |      0      |
|    1    |    1    |      0      |      0      |

## Layout & Area

- **Total Area:** 26.25 μm²

## Simulation Results

### Pre-Layout Performance Data

The following table summarizes leakage, dynamic, static, and total power across different process corners and temperatures (Voltage: 1.08V - 1.32V):

| Corner Condition      | Leakage Current | Dynamic Power | Static Power | Total Power |
| :-------------------- | :-------------- | :------------ | :----------- | :---------- |
| SS, 1.08V, 25°C       | -0.178 m        | 0.192 m       | 0.146 m      | 0.338 m     |
| SS, 1.08V, 125°C      | -0.170 m        | 0.183 m       | 0.140 m      | 0.324 m     |
| SS, 1.08V, -40°C      | -0.187 m        | 0.203 m       | 0.153 m      | 0.356 m     |
| TT, 1.32V, 125°C      | -0.321 m        | 0.424 m       | 0.323 m      | 0.748 m     |
| FF, 1.32V, 125°C      | -0.379 m        | 0.501 m       | 0.382 m      | 0.883 m     |
| FF, 1.32V, -40°C      | -0.428 m        | 0.565 m       | 0.424 m      | 0.990 m     |

> **Note:** Pre-layout and post-layout simulations were performed. Waveform images (refer to original report on pages 6-7, 9) confirm correct logical operation.

## Challenges Faced

1.  **Read Disturb in 6T SRAM:** Simultaneous activation of multiple wordlines caused bit flips in conventional 6T cells.
2.  **Figure of Merit (FOM) Analysis:** Accurately analyzing different performance metrics was challenging.
3.  **Layout Optimization:** Achieving an optimal layout with no parasitic issues required multiple iterations.

## Future Plans

- **Expand Logic Capabilities:** Implement additional Boolean operations such as **XOR**.
- **Layout Refinement:** Optimize the existing layout to resolve minor issues and improve performance.

## References

1.  A. Agrawal, A. Jaiswal, C. Lee, and K. Roy, "X-SRAM: Enabling In-Memory Boolean Computations in CMOS Static Random Access Memories," in *IEEE Transactions on Circuits and Systems I: Regular Papers*, vol. 65, no. 12, pp. 4219–4232, Dec. 2018. doi: 10.1109/TCSI.2018.2848999.
2.  C.-J. Jhang, C.-X. Xue, J.-M. Hung, F.-C. Chang, and M.-F. Chang, "Challenges and Trends of SRAM-Based Computing-In-Memory for AI Edge Devices," in *IEEE Transactions on Circuits and Systems I: Regular Papers*, vol. 68, no. 5, pp. 1773–1786, May 2021. doi: 10.1109/TCSI.2021.3064189.

## Contribution Statement

All work (design, simulation, layout, and documentation) was completed mutually by all team members.

## Acknowledgments

Thank you to the course instructors and teaching assistants of ECE-611 for their guidance and support.
