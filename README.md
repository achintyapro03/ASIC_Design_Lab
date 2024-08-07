# ASIC Design Lab

## Overview
This project comprises a series of tasks aimed at designing and implementing various components and programs for ASIC design.

## Table of Contents
| Task Number | Task Description                                |
|-------------|-------------------------------------------------|
| [Task 1](#task-1) | C program to calculate the sum of numbers from 1 to n |
| [Task 2](#task-2) | Description of Task 2                      |
| [Task 3](#task-3) | Description of Task 3                      |
| [Task 4](#task-4) | Description of Task 4                      |
| [Task 5](#task-5) | Description of Task 5                      |

## Tasks

### Task 1

The task involves creating a simple C program that calculates the sum of numbers from 1 to n. This was achieved using the following steps:

1. **Creating and Opening the File**:
    - The file `sum1ton.c` is created and opened using the vim editor.
     
    ```
        vim sum1ton.c
    ```

2. **Writing the Code**:
    - The code is written in the `sum1ton.c` file to calculate the sum of numbers from 1 to n.
    - ![Writing the Code](https://github.com/user-attachments/assets/0b90a649-b3e6-42d5-ade6-600329ae8a85)


3. **Compiling and Running the Code**:
    - The code is compiled and run using the following commands:
    ```bash
    gcc sum1ton.c
    ./a.out
    ```
4. **Output**:
   - ![cat](https://github.com/user-attachments/assets/f1bcb790-f27e-4a80-b626-41b78a1413ff)


### Task 2
1. The parameter 'n' was changed from 5 to 100 in this task.  
    ![cat](https://github.com/user-attachments/assets/da3d24f3-f028-4032-b190-96683d5642f7)

2. ```bash
   riscv64-unknown-elf-gcc -O1 -mabi=lp64 -march=rv64i -o sum1ton.o sum1ton.c
   ls -ltr sum1ton.c
   ```
   This command is used to compile a C source file (sum1ton.c) into an object file (sum1ton.o) for the RISC-V 64-bit architecture. Here’s a brief explanation of each part:
    - riscv64-unknown-elf-gcc: The compiler for the RISC-V 64-bit architecture.
    - O1: Optimization level 1, which provides a balance between compilation time and performance.
    - mabi=lp64: Specifies the ABI (Application Binary Interface) as lp64, meaning long and pointer data types are 64 bits.
    - march=rv64i: Specifies the target architecture as RISC-V 64-bit instruction set.
  
   ![Output](https://github.com/user-attachments/assets/bda4cd2a-ef57-43b8-80ba-6283a7a58b76)

![O1](https://github.com/user-attachments/assets/ac421b54-e705-401e-98b5-2d82f28ef6f9)

    ```bash
    riscv64-unknown-elf-objdump -d sum1ton.o | less
    ```



