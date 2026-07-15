2×2 Matrix Multiplication using Systolic Array (SystemVerilog)

A hardware implementation of 2×2 signed matrix multiplication using a systolic array architecture, verified with a self-checking SystemVerilog testbench (generator–driver–monitor–scoreboard structure, inspired by UVM methodology).

📌 Overview

A systolic array is a grid of small identical Processing Elements (PEs) that pass data to their neighbors on every clock cycle, instead of fetching operands from a central memory repeatedly. This architecture is the same core idea used in real-world ML accelerators (e.g. Google's TPU) for efficient, parallel matrix multiplication.

This project implements the smallest possible instance of that idea — a 2×2 PE array — that computes C = A × B for two 2×2 signed 8-bit matrices, using diagonally-staggered ("skewed") data flow between PEs.

🏗️ Architecture


pe module — a single Processing Element. On every clock edge it:

Multiplies its two inputs (in_a from the left, in_b from the top)
Accumulates the result into a local register
Forwards in_a to the PE on its right, and in_b to the PE below it (one cycle later)
Clears its accumulator when clr is asserted



matrix_mult_top module — instantiates 4 PEs in a 2×2 grid and wires them together with the correct diagonal skew, so each PE receives its operands at the right cycle to compute one element of the output matrix C.

Systolic Array Block Diagram :

a00        a10
         │          │
   ┌─────▼─────┐┌───▼───────┐
b00┤  PE(0,0)  ││  PE(1,0)  │
   └─────┬─────┘└───┬───────┘
         │ a00→a01   │ a10→a11
   ┌─────▼─────┐┌───▼───────┐
b01┤  PE(0,1)  ││  PE(1,1)  │
   └───────────┘└───────────┘


▶️ How to Run

This project is set up to run on EDA Playground.

Create a new project on EDA Playground (or open this repo's code there)
Paste design.sv into the design.sv file panel, and testbench.sv into the testbench.sv file panel
Select a SystemVerilog-capable simulator (e.g. Cadence Xcelium, or Aldec Riviera-PRO if you don't have simulator access validated)
Set the top-level modules if prompted: design.sv and testbench.sv (as flagged in the run command)
Click Run


   
