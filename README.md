# Asynchronous FIFO Design (Verilog)
## Overview
This repository contains the RTL implementation of an **Asynchronous FIFO (First-In First-Out)** using Verilog.  
An asynchronous FIFO is used when data must be transferred between two modules that operate on **different clock domains**. Since the write and read clocks are independent, special techniques such as **Gray code pointers and synchronizers** are used to safely transfer pointer information across clock domains.

The design was developed and simulated using **Xilinx Vivado**.
## Architecture
The block diagram of the implemented asynchronous FIFO has been provided in the repository.

The FIFO contains:
- FIFO memory (buffer)
- Write pointer logic
- Read pointer logic
- Pointer synchronization logic
- Full and Empty flag generation

Two different clock domains are used:

- **Write clock (wclk)** – controls write operations
- **Read clock (rclk)** – controls read operations

## Design Approach
### Independent Clock Domains
Unlike synchronous FIFOs, asynchronous FIFOs operate with **separate read and write clocks**.  
Because of this, signals from one clock domain cannot be directly used in another domain without proper synchronization.
### Gray Code Pointers
The read and write pointers are implemented using **Gray code counters**.  
Gray code ensures that **only one bit changes at a time**, which helps reduce errors when transferring pointer values between clock domains.
### Pointer Synchronization
To safely pass pointer information between the read and write clock domains, a **two flip-flop synchronizer** is used.  
This helps reduce the probability of metastability.
### Full and Empty Detection
The FIFO status flags are generated using the relationship between the read and write pointers.
- **FIFO Empty**  
  Occurs when the read pointer and write pointer are equal.
- **FIFO Full**  
  Occurs when the write pointer wraps around and reaches the read pointer position.
An additional pointer bit is used to differentiate between the full and empty conditions.

## Design Modules
The FIFO is divided into multiple modules to simplify the design and make the logic easier to understand.
### 1. FIFO (Top Module)
The top module connects all the submodules together. It includes the memory block, pointer logic and synchronization modules.
### 2. FIFO Memory
This module acts as the storage element for the FIFO.  
It behaves like a **dual-port memory**, allowing simultaneous read and write operations from different clock domains.
### 3. Two Flip-Flop Synchronizer
This module synchronizes pointer values between the write and read clock domains using two sequential flip-flops.
### 4. Read Pointer Logic
Handles the read pointer update and generates the **empty signal** when no valid data is available in the FIFO.
### 5. Write Pointer Logic
Handles the write pointer update and generates the **full signal** when the FIFO buffer is full.

## RTL Design
The RTL schematic of the FIFO generated from Vivado is also provided alongside the architecture.

## Testbench and Verification
A Verilog testbench was written to verify the behavior of the FIFO.  
The following scenarios were tested during simulation:

1. Writing multiple data values into the FIFO
2. Reading data from the FIFO and verifying the order
3. Checking FIFO **full condition**
4. Checking FIFO **empty condition**

The simulation confirmed that (output waveform provided):

- Data written into the FIFO is read in the same order
- Full and empty signals behave correctly
- Pointer synchronization works correctly between clock domains
  
## Key Concepts Demonstrated
This project demonstrates several important digital design concepts:

- Clock Domain Crossing (CDC)
- Gray code counters
- Pointer synchronization
- FIFO full and empty detection
- Modular RTL design
