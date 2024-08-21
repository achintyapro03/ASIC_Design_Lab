# ASIC Design Lab

## Overview
This project comprises a series of tasks aimed at designing and implementing various components and programs for ASIC design.

## Tasks

<details>
  <summary>Task 1 : C program to calculate the sum of numbers from 1 to n </summary>

The task involves creating a simple C program that calculates the sum of numbers from 1 to n. This was achieved using the following steps:

1. **Creating and Opening the File**:
    - The file `sum1ton.c` is created and opened using the vim editor.
    ```bash
    touch sum1ton.c
    vim sum1ton.c
    ```

2. **Writing the Code**:
    - The code is written in the `sum1ton.c` file to calculate the sum of numbers from 1 to n.
    ![Writing the Code](https://github.com/user-attachments/assets/0b90a649-b3e6-42d5-ade6-600329ae8a85)

3. **Compiling and Running the Code**:
    - The code is compiled and run using the following commands:
    ```bash
    gcc sum1ton.c
    ./a.out
    ```
    ![Output](https://github.com/user-attachments/assets/f1bcb790-f27e-4a80-b626-41b78a1413ff)

4. **Parameter Change**:
    - The parameter 'n' was changed from 5 to 100 in this task.  
    ![Output](https://github.com/user-attachments/assets/da3d24f3-f028-4032-b190-96683d5642f7)

5. **RISC-V Compilation**:
    - The following command is used to compile a C source file (sum1ton.c) into an object file (sum1ton.o) for the RISC-V 64-bit architecture.
    ```bash
    riscv64-unknown-elf-gcc -O1 -mabi=lp64 -march=rv64i -o sum1ton.o sum1ton.c
    ls -ltr sum1ton.c
    ```
    ![RISC-V Compilation](https://github.com/user-attachments/assets/bda4cd2a-ef57-43b8-80ba-6283a7a58b76)

    ```bash
    riscv64-unknown-elf-objdump -d sum1ton.o | less
    ```
    ![Output](https://github.com/user-attachments/assets/7a05c12a-7e05-418f-aa5b-65f685dac8e5)
    - From this, we observe that each instruction is 4 bytes. By subtracting the address of the last instruction from the first and dividing by 4, we get 15 instructions for the main section.

6. **OFast Optimization**:
    - Similarly, the same code was compiled using OFast optimization.
    ```bash
    riscv64-unknown-elf-gcc -Ofast -mabi=lp64 -march=rv64i -o sum1ton.o sum1ton.c
    ls -ltr sum1ton.c
    ```
    ![image](https://github.com/user-attachments/assets/97023caa-ba50-4252-99a0-e21d35e0b6bf)
   
    ```bash
    riscv64-unknown-elf-objdump -d sum1ton.o | less
    ```
    ![OFast Output](https://github.com/user-attachments/assets/8cd49589-baca-47ec-88ea-0a78dda6e9f8)
    - Here, we get 12 instructions, 3 less than O1 optimization.

</details>

<details>
  <summary>Task 2 : Debugging the code using Spike simulator</summary>

The code written in [Task 1](#task-1) is being debugged using the Spike simulator.

1. **Running the Code in Spike**:
    - The compiled object file is executed in the Spike simulator using the following command:
    ```bash
    spike pk sum1ton.o
    ```
    - This command loads the RISC-V program (`sum1ton.o`) and runs it in the Spike simulator environment.
    ![Spike Output](https://github.com/user-attachments/assets/82ce5a0b-112a-4974-bb1f-6a8f38ebf090)

2. **Analyzing the Output**:
    - The output from the Spike simulator helps in verifying the correctness of the program execution on the RISC-V architecture.
    - In this case, it confirms that the sum of numbers from 1 to 100 is calculated correctly.
    ![Spike Output 2](https://github.com/user-attachments/assets/25f03f79-d2f7-401a-8a9c-6c211321f70f)

</details>

<details>
  <summary>Task 3 : RISC-V Instructions simulations</summary>

This task involves understanding the different RISC-V instruction formats and their usage in the context of assembly language programming. Below are the details of each instruction format:

1. **R-type (Register Type)**
    - **Structure**: <br> <img width="772" alt="R" src="https://github.com/user-attachments/assets/ce5d99f4-7463-48f9-aa4f-f48b41afe84e">
    - **Fields**:
        - `funct7 (31:25)`: Function code, used to specify the exact operation (e.g., add, subtract) along with `funct3`.
        - `rs2 (24:20)`: Source register 2, the second source operand.
        - `rs1 (19:15)`: Source register 1, the first source operand.
        - `funct3 (14:12)`: Function code, used together with `funct7` to specify the exact operation.
        - `rd (11:7)`: Destination register, where the result of the operation will be stored.
        - `opcode (6:0)`: Operation code, used to determine the instruction format and basic operation.
    - **Usage**: Used for arithmetic, logical, and other operations that require two register operands.

2. **I-type (Immediate Type)**
    - **Structure**: <br> <img width="772" alt="I" src="https://github.com/user-attachments/assets/8fb86523-a443-4c29-9365-e64d9b1288a7">
    - **Fields**:
        - `imm[11:0] (31:20)`: Immediate value, a constant used in operations (e.g., load, `addi`).
        - `rs1 (19:15)`: Source register, the first operand.
        - `funct3 (14:12)`: Function code, used to specify the operation.
        - `rd (11:7)`: Destination register, where the result will be stored.
        - `opcode (6:0)`: Operation code.
    - **Usage**: Used for operations involving an immediate value, such as loading data or adding a constant to a register.

3. **S-type (Store Type)**
    - **Structure**: <br> <img width="772" alt="S" src="https://github.com/user-attachments/assets/1fde53ef-bca6-4338-be46-49c249ce2d33">
    - **Fields**:
        - `imm[11:5] (31:25)`: Upper 7 bits of the immediate value.
        - `rs2 (24:20)`: Source register 2, which holds the data to be stored.
        - `rs1 (19:15)`: Source register 1, which contains the base address.
        - `funct3 (14:12)`: Function code.
        - `imm[4:0] (11:7)`: Lower 5 bits of the immediate value.
        - `opcode (6:0)`: Operation code.
    - **Usage**: Used for store instructions, where data from a register is stored to memory.

4. **B-type (Branch Type)**
    - **Structure**: <br> <img width="772" alt="B" src="https://github.com/user-attachments/assets/0b7704e6-bfbc-4215-8919-6af69dc4b0b9">
    - **Fields**:
        - `imm[12] (31)`: Most significant bit of the immediate value.
        - `imm[10:5] (30:25)`: Bits 10 to 5 of the immediate value.
        - `rs2 (24:20)`: Source register 2, the second operand for comparison.
        - `rs1 (19:15)`: Source register 1, the first operand for comparison.
        - `funct3 (14:12)`: Function code, used to specify the comparison operation.
        - `imm[4:1] (11:8)`: Bits 4 to 1 of the immediate value.
        - `imm[11] (7)`: Bit 11 of the immediate value.
        - `opcode (6:0)`: Operation code.
    - **Usage**: Used for branch instructions, where the program may jump to a different location based on a comparison.

5. **U-type (Upper Immediate Type)**
    - **Structure**: <br> <img width="772" alt="U" src="https://github.com/user-attachments/assets/59a69fe4-de5a-4024-9a01-5843ad422c21">
    - **Fields**:
        - `imm[31:12] (31:12)`: Immediate value, shifted to the upper 20 bits.
        - `rd (11:7)`: Destination register.
        - `opcode (6:0)`: Operation code.
    - **Usage**: Used for instructions that involve large immediate values, typically for loading addresses or constants.

6. **J-type (Jump Type)**
    - **Structure**: <br> <img width="772" alt="J" src="https://github.com/user-attachments/assets/77987843-4c4c-462d-a0fc-5b5d7658b5f3">
    - **Fields**:
        - `imm[20] (31)`: Bit 20 of the immediate value.
        - `imm[10:1] (30:21)`: Bits 10 to 1 of the immediate value.
        - `imm[11] (20)`: Bit 11 of the immediate value.
        - `imm[19:12] (19:12)`: Bits 19 to 12 of the immediate value.
        - `rd (11:7)`: Destination register, often used to store the return address.
        - `opcode (6:0)`: Operation code.
    - **Usage**: Used for jump instructions, where the program counter (PC) is modified to jump to a different location in the code.

The following instructions were provided as part of an exercise. 
```
ADD r4, r5, r6
SUB r6, r4, r5
AND r5, r4, r6
OR r8, r5, r5
XOR r8, r4, r4
SLT r10, r2, r4
ADDI r12, r3, 5
SW r3, r1, 4
SRL r16, r11, r2
BNE r0, r1, 20
BEQ r0, r0, 15
LW r13, r11, 2
SLL r15, r11, r2
```


| Sl.No | Instruction     | Instruction Type | Opcode   | funct3 | funct7 | rd   | rs1  | rs2  | Immediate | 32-bit Instruction Code       |
|-------|-----------------|------------------|----------|--------|--------|------|------|------|-----------|-------------------------------|
| 1     | ADD r4, r5, r6  | R-type            | 0110011  | 000    | 0000000 | r4   | r5   | r6   | -         | 0000000 00110 00101 000 00100 0110011 |
| 2     | SUB r6, r4, r5  | R-type            | 0110011  | 000    | 0100000 | r6   | r4   | r5   | -         | 0100000 00101 00100 000 00110 0110011 |
| 3     | AND r5, r4, r6  | R-type            | 0110011  | 111    | 0000000 | r5   | r4   | r6   | -         | 0000000 00110 00100 111 00101 0110011 |
| 4     | OR r8, r5, r5   | R-type            | 0110011  | 110    | 0000000 | r8   | r5   | r5   | -         | 0000000 00101 00101 110 01000 0110011 |
| 5     | XOR r8, r4, r4  | R-type            | 0110011  | 100    | 0000000 | r8   | r4   | r4   | -         | 0000000 00100 00100 100 01000 0110011 |
| 6     | SLT r10, r2, r4 | R-type            | 0110011  | 010    | 0000000 | r10  | r2   | r4   | -         | 0000000 00100 00010 010 01010 0110011 |
| 7     | ADDI r12, r3, 5 | I-type            | 0010011  | 000    | -      | r12  | r3   | -    | 5         | 000000000101 00011 000 01100 0010011  |
| 8     | SW r3, r1, 4    | S-type            | 0100011  | 010    | -      | -    | r1   | r3   | 4         | 0000000 00011 00001 010 00010 0100011 |
| 9     | SRL r16, r11, r2| R-type            | 0110011  | 101    | 0000000 | r16  | r11  | r2   | -         | 0000000 00010 01011 101 10000 0110011 |
| 10    | BNE r0, r1, 20  | B-type            | 1100011  | 001    | -      | -    | r0   | r1   | 20        | 000000 00001 00000 001 00100 1100011  |
| 11    | BEQ r0, r0, 15  | B-type            | 1100011  | 000    | -      | -    | r0   | r0   | 15        | 000000 00000 00000 000 01111 1100011  |
| 12    | LW r13, r11, 2  | I-type            | 0000011  | 010    | -      | r13  | r11  | -    | 2         | 000000000010 01011 010 01101 0000011  |
| 13    | SLL r15, r11, r2| R-type            | 0110011  | 001    | 0000000 | r15  | r11  | r2   | -         | 0000000 00010 01011 001 01111 0110011 |


| Sl.No | Instruction     | 32-bit Instruction Code           | Hexadecimal Representation | Hardcoded Instructions    |
|-------|-----------------|-----------------------------------|----------------------------|-------------|
| 1     | ADD r4, r5, r6  | 0000000 00110 00101 000 00100 0110011 | 32'h00628233                 | 32'h02208300 |
| 2     | SUB r6, r4, r5  | 0100000 00101 00100 000 00110 0110011 | 32'h40A28233                 | 32'h02209380 |
| 3     | AND r5, r4, r6  | 0000000 00110 00100 111 00101 0110011 | 32'h0062F233                 | 32'h0230a400 |
| 4     | OR r8, r5, r5   | 0000000 00101 00101 110 01000 0110011 | 32'h0052E233                 | 32'h02513480 |
| 5     | XOR r8, r4, r4  | 0000000 00100 00100 100 01000 0110011 | 32'h00424233                 | 32'h0240c500 |
| 6     | SLT r10, r2, r4 | 0000000 00100 00010 010 01010 0110011 | 32'h00414233                 | 32'h02415580 |
| 7     | ADDI r12, r3, 5 | 000000000101 00011 000 01100 0010011  | 32'h00518313                 | 32'h00520600 |
| 8     | SW r3, r1, 4    | 0000000 00011 00001 010 00010 0100011 | 32'h00312223                 | 32'h00209181 |
| 9     | SRL r16, r11, r2| 0000000 00010 01011 101 10000 0110011 | 32'h0025A133                 | 32'h00208681 |
| 10    | BNE r0, r1, 20  | 000000 00001 00000 001 00100 1100011  | 32'h00102863                 | 32'h00f00002 |
| 11    | BEQ r0, r0, 15  | 000000 00000 00000 000 01111 1100011  | 32'h00007863                 | 32'h00210700 |
| 12    | LW r13, r11, 2  | 000000000010 01011 010 01101 0000011  | 32'h0025A503                 | 32'h01409002 |
| 13    | SLL r15, r11, r2| 0000000 00010 01011 001 01111 0110011 | 32'h0025B333                 | 32'h00520601 |


## RISC-V Instruction Simulation

Install verilog and gtkwave using the following the commands
```
sudo apt get update
sudo apt get upgrade
sudo apt get install iverilog gtkwave
```
Clone the repository and download the netlist files for simulation.
```
git clone https://github.com/vinayrayapati/rv32i/
```
![image](https://github.com/user-attachments/assets/f15d0f76-6b3c-449e-88d6-c188802cb984)



This document provides the hardcoded and actual instruction simulation results for a set of RISC-V instructions. Each instruction is paired with its corresponding hardcoded and actual instruction images.

<details>
  <summary><h2>Outputs</h2></summary>

### 1. ADD r4, r5, r6
- **Instruction Code**: `32'h00628233`
- **Hardcoded Instruction**: `32'h02208300`
- **Simulation Images**:
  - **Hardcoded Instruction**: <br>
    ![h_1](https://github.com/user-attachments/assets/110b2a94-ac3b-4677-9c9d-38c938b8bf11)
  - **Actual Instruction**: <br>
    ![1](https://github.com/user-attachments/assets/e564f75e-5b6b-4f0b-8c9a-2300e2d0d1ba)

### 2. SUB r6, r4, r5
- **Instruction Code**: `32'h40A28233`
- **Hardcoded Instruction**: `32'h02209380`
- **Simulation Images**:
  - **Hardcoded Instruction**: <br>
    ![h_2](https://github.com/user-attachments/assets/81a83c20-ba85-4fe1-8877-0d78809c47fa)
  - **Actual Instruction**: <br>
    ![2](https://github.com/user-attachments/assets/80a4f82c-9895-4201-8320-f0234eb238c6)

### 3. AND r5, r4, r6
- **Instruction Code**: `32'h0062F233`
- **Hardcoded Instruction**: `32'h0230a400`
- **Simulation Images**:
  - **Hardcoded Instruction**: <br>
    ![h_3](https://github.com/user-attachments/assets/2380e240-add7-4275-9b93-9c51f20139d9)
  - **Actual Instruction**: <br>
    ![3](https://github.com/user-attachments/assets/5db41025-a0cc-40d8-be52-e6e916bd9d3d)

### 4. OR r8, r5, r5
- **Instruction Code**: `32'h0052E233`
- **Hardcoded Instruction**: `32'h02513480`
- **Simulation Images**:
  - **Hardcoded Instruction**: <br>
    ![h_4](https://github.com/user-attachments/assets/7085d86d-c4c1-43e9-9fc3-5b14c001f91f)
  - **Actual Instruction**: <br>
    ![4](https://github.com/user-attachments/assets/57d3f509-ba21-40ed-ac0f-934c4cf93101)

### 5. XOR r8, r4, r4
- **Instruction Code**: `32'h00424233`
- **Hardcoded Instruction**: `32'h0240c500`
- **Simulation Images**:
  - **Hardcoded Instruction**: <br>
    ![h_5](https://github.com/user-attachments/assets/fcffea5f-f29e-4fae-8cc0-b01c2851c845)
  - **Actual Instruction**: <br>
    ![5](https://github.com/user-attachments/assets/de8dc87c-37b9-4fdd-8562-0221301cfe65)

### 6. SLT r10, r2, r4
- **Instruction Code**: `32'h00414233`
- **Hardcoded Instruction**: `32'h02415580`
- **Simulation Images**:
  - **Hardcoded Instruction**: <br>
    ![h_6](https://github.com/user-attachments/assets/fc95c999-80fa-48b6-80ab-a053bb3b146f)
  - **Actual Instruction**: <br>
    ![6](https://github.com/user-attachments/assets/a95158d7-c58a-45cc-8385-ca79758e19bb)

### 7. ADDI r12, r3, 5
- **Instruction Code**: `32'h00518313`
- **Hardcoded Instruction**: `32'h00520600`
- **Simulation Images**:
  - **Hardcoded Instruction**: <br>
    ![h_7](https://github.com/user-attachments/assets/3bf1e827-5515-4fd4-bdd8-76be24dc0aa3)
  - **Actual Instruction**: <br>
    ![7](https://github.com/user-attachments/assets/f13fde91-372f-4627-abcb-1c5c596e4a92)

### 8. SW r3, r1, 4
- **Instruction Code**: `32'h00312223`
- **Hardcoded Instruction**: `32'h00209181`
- **Simulation Images**:
  - **Hardcoded Instruction**: <br>
    ![h_8](https://github.com/user-attachments/assets/be02fdf7-ac66-46b7-8bd1-ea1ac193dd08)
  - **Actual Instruction**: <br>
    ![8](https://github.com/user-attachments/assets/32a648f2-bc23-4a65-8f8d-c67598181745)

### 9. SRL r16, r11, r2
- **Instruction Code**: `32'h0025A133`
- **Hardcoded Instruction**: `32'h00208681`
- **Simulation Images**:
  - **Hardcoded Instruction**: <br>
    ![h_9](https://github.com/user-attachments/assets/eda483c2-a4b2-473c-8682-ebe6fe895449)
  - **Actual Instruction**: <br>
    ![9](https://github.com/user-attachments/assets/57260bb9-8aa6-4f65-b423-53e21a1bcc15)

### 10. BNE r0, r1, 20
- **Instruction Code**: `32'h00102863`
- **Hardcoded Instruction**: `32'h00f00002`
- **Simulation Images**:
  - **Hardcoded Instruction**: <br>
    ![h_10](https://github.com/user-attachments/assets/f971b08b-64b3-477f-8d1f-aee3d077a190)
  - **Actual Instruction**: <br>
    ![10](https://github.com/user-attachments/assets/bf0f50c2-69a0-4d4a-9da6-aea268d87ec8)

### 11. BEQ r0, r0, 15
- **Instruction Code**: `32'h00007863`
- **Hardcoded Instruction**: `32'h00210700`
- **Simulation Images**:
  - **Hardcoded Instruction**: <br>
    ![h_11](https://github.com/user-attachments/assets/d720d527-9f47-465e-88bf-ba3f10fd03a5)
  - **Actual Instruction**: <br>
    ![11](https://github.com/user-attachments/assets/cf3e223f-dcf4-48b6-b2ff-a8354a4c47a4)

### 12. LW r13, r11, 2
- **Instruction Code**: `32'h0025A503`
**Simulation Images**: <br>
    ![Actual Instruction](https://github.com/user-attachments/assets/3b72e9ac-615f-4413-b3ad-dece32dacd9f)

### 13. SLL r15, r11, r2
- The above command was not able to be executed as there was not enough memory spaces and the program was compiled with a warning:
- ![image](https://github.com/user-attachments/assets/cf5ed2e3-c4a7-4808-be96-91d0dda94471)
- **Instruction Code**: `32'h0025B333`
- **Simulation Images**: <br>
    ![Actual Instruction](https://github.com/user-attachments/assets/323b9b69-8116-4370-9309-1177e62adac2)


</details>

### Conclusion:
We noticed a change in the output between hardcoded instructions and the and the derived instructions waveforms which was what was expected.
</details>

<details>
  <summary>Task 4 : To write an application in 'C' that can be compiled using GCC and RISC-V GCC</summary>

In this task the bubble sort algorithm has been used and the code is given below.
```c
#include <stdio.h>
#include <stdint.h>

int main() {
    int32_t arr[] = {64, 34, 25, 12, 22, 11, 90}; // Hardcoded input array
    int32_t n = sizeof(arr) / sizeof(arr[0]);
    int32_t i, j;
    int32_t temp;
    int32_t swapped;

    // Print the original array
    printf("Original array:\n");
    for (i = 0; i < n; i++) {
        printf("%d ", arr[i]);
    }
    printf("\n");

    // Bubble sort algorithm
    for (i = 0; i < n - 1; i++) {
        swapped = 0;
        for (j = 0; j < n - i - 1; j++) {
            if (arr[j] > arr[j + 1]) {
                // Swap arr[j] and arr[j + 1]
                temp = arr[j];
                arr[j] = arr[j + 1];
                arr[j + 1] = temp;
                swapped = 1;
            }
        }
        // If no two elements were swapped in the inner loop, then the array is sorted
        if (swapped == 0) {
            break;
        }
    }

    // Print the sorted array
    printf("Sorted array:\n");
    for (i = 0; i < n; i++) {
        printf("%d ", arr[i]);
    }
    printf("\n");

    return 0;
}
```

![image](https://github.com/user-attachments/assets/2defda40-386b-4eaf-a580-ddd1f5e0ece2)

Now run the following codes for compiling. 

```
gcc bubbleSort.c
./a.out
riscv64-unknown-elf-gcc -O1 -mabi=lp64 -march=rv64i -o bubbleSort.o bubbleSort.c
riscv64-unknown-elf-objdump -d bubbleSort.o | less
spike pk bubbleSort.o
```

![image](https://github.com/user-attachments/assets/3b998ae2-de12-4343-a5ca-6a81f23610e7)

From this picture we can see that O0 = 01.
</details>

<details>
  <summary>Task 5 : RISC-V Architecture using MakerChip</summary>

# TL-Verilog Implementation of Circuits using Makerchip IDE

## Day 3 - Digital Logic with TL-Verilog and Makerchip

## Combinatorial Circuits

1. **Inverter**
   - A basic logic gate that outputs the inverted value of the input signal.
   - ![Inverter](https://github.com/user-attachments/assets/d3f236c7-a1bb-4067-a775-87faeda2e124)

2. **2-input AND**
   - Outputs a high signal only when both inputs are high.
   - ![2-input AND](https://github.com/user-attachments/assets/b6e076de-b9df-4398-ae2c-1271721d809a)

3. **2-input OR**
   - Outputs a high signal if at least one input is high.
   - ![2-input OR](https://github.com/user-attachments/assets/1b3e290c-ce62-4c65-aa65-45e5a0d29f79)

4. **2-input XOR**
   - Outputs a high signal only when the inputs differ.
   - ![2-input XOR](https://github.com/user-attachments/assets/1257d8be-d847-4c61-a249-5eff99c38ea8)

5. **Vector Addition**
   - Adds corresponding elements of two input vectors.
   - ![Vector Addition](https://github.com/user-attachments/assets/2ffb268c-78b3-4163-8122-b71f325a3922)

6. **2:1 Mux**
   - Selects one of two inputs based on a control signal.
   - ![2:1 Mux](https://github.com/user-attachments/assets/1dc9e902-7145-40c7-b9fc-f9f2537c587f)

7. **2:1 Mux with Vectors**
   - A multiplexer that selects between vector inputs based on a control signal.
   - ![2:1 Mux with Vectors](https://github.com/user-attachments/assets/45a4cccc-7278-4d6d-83f6-a86a2721762a)

8. **Combinatorial Calculator**
   - Performs basic arithmetic operations using combinatorial logic.
   - ![Combinatorial Calculator](https://github.com/user-attachments/assets/bd3046ea-9b00-4df3-8fc3-329ffcde3fe2)

## Sequential Circuits

1. **Fibonacci Series**
   - Generates a Fibonacci sequence using sequential logic.
   - ![Fibonacci Series](https://github.com/user-attachments/assets/6be0adb4-59a0-4c13-978e-5dfe9b1f74e9)

2. **Free Running Counter**
   - A counter that increments its value continuously with each clock cycle.
   - ![Free Running Counter](https://github.com/user-attachments/assets/dd450316-6605-4b85-8975-d19b05cd6b00)

3. **Sequential Calculator**
   - A calculator that performs operations over multiple clock cycles, utilizing sequential logic.
   - ![Sequential Calculator](https://github.com/user-attachments/assets/2c5c60cd-dcd4-445f-8e6f-78647c266422)

## Pipeline Logic

1. **Pipeline Design**
   - A design that breaks down operations into multiple stages to improve processing throughput.
   - ![Pipeline Design](https://github.com/user-attachments/assets/4cd14b8c-d314-499b-95c5-270a0016f27d)
   - ![Pipeline Design Example](https://github.com/user-attachments/assets/2ea28c83-1336-4c9b-8c06-3719f1d1465a)

2. **2 Cycle Calculator**
   - A calculator that executes operations over two stages, leveraging pipelining for enhanced performance.
   - ![2 Cycle Calculator](https://github.com/user-attachments/assets/b08efe91-f86d-4c5c-84fb-8f245337c826)
   - ![2 Cycle Pipeline Example](https://github.com/user-attachments/assets/f08559dc-1706-4217-8a0e-b40a2d20c83f)
  
## Illustration of Validity

1. Distance Calculator
The block diagram of the distance accumulator is shown below :
![image](https://github.com/user-attachments/assets/0253bf98-8772-4029-9fc5-629370051853)

![image](https://github.com/user-attachments/assets/f39b922b-aea6-41c7-850d-ddd8308d8b98)


Once the valid signal is asserted the previous value of result will be added with the current value and it result will get updated otherwise the previous value is retained.

2. 2 Cycle Calculator

![image](https://github.com/user-attachments/assets/21c373ca-12dc-4f40-986b-19582e8b1c2e)

![image](https://github.com/user-attachments/assets/3ac54469-28cc-49a2-bcdf-3ada47bbd989)



## Day 4 - Basic RISC-V CPU micro-architecture
![image](https://github.com/user-attachments/assets/119800b0-40a6-4065-98a7-7aae36b2d7c1)

1. **Program Counter Increment and Instruction Fetch**
   - The Program Counter (PC) is a crucial register in the CPU that tracks the address of the next instruction to be retrieved and executed. As each instruction is fetched, the PC is incremented, pointing to the next             memory address for the upcoming instruction.
   - ![Program Counter](https://github.com/user-attachments/assets/f1a43cb4-2be6-4c25-ba3d-560ddd97e261)
   - ![image](https://github.com/user-attachments/assets/00a7ee89-e0f8-41b7-b033-75e018a2703f)
   - ```
       \m4_TLV_version 1d: tl-x.org
      \SV
         // This code can be found in: https://github.com/stevehoover/RISC-V_MYTH_Workshop
         
         m4_include_lib(['https://raw.githubusercontent.com/BalaDhinesh/RISC-V_MYTH_Workshop/master/tlv_lib/risc-v_shell_lib.tlv'])
      
      \SV
         m4_makerchip_module   // (Expanded in Nav-TLV pane.)
      \TLV
      
         // /====================\
         // | Sum 1 to 9 Program |
         // \====================/
         //
         // Program for MYTH Workshop to test RV32I
         // Add 1,2,3,...,9 (in that order).
         //
         // Regs:
         //  r10 (a0): In: 0, Out: final sum
         //  r12 (a2): 10
         //  r13 (a3): 1..10
         //  r14 (a4): Sum
         // 
         // External to function:
         m4_asm(ADD, r10, r0, r0)             // Initialize r10 (a0) to 0.
         // Function:
         m4_asm(ADD, r14, r10, r0)            // Initialize sum register a4 with 0x0
         m4_asm(ADDI, r12, r10, 1010)         // Store count of 10 in register a2.
         m4_asm(ADD, r13, r10, r0)            // Initialize intermediate sum register a3 with 0
         // Loop:
         m4_asm(ADD, r14, r13, r14)           // Incremental addition
         m4_asm(ADDI, r13, r13, 1)            // Increment intermediate register by 1
         m4_asm(BLT, r13, r12, 1111111111000) // If a3 is less than a2, branch to label named <loop>
         m4_asm(ADD, r10, r14, r0)            // Store final result to register a0 so that it can be read by main program
         
         // Optional:
         // m4_asm(JAL, r7, 00000000000000000000) // Done. Jump to itself (infinite loop). (Up to 20-bit signed immediate plus implicit 0 bit (unlike JALR) provides byte address; last immediate bit should also be 0)
         m4_define_hier(['M4_IMEM'], M4_NUM_INSTRS)
      
         |cpu
            // YOUR CODE HERE
            // ...
            @0
               $reset = *reset;
               $clk_ach = *clk;
               $pc[31:0] = >>1$reset ? 32'd0 : (>>1$pc+32'd4);
            @1
               $imem_rd_en = !$reset;
               $imem_rd_addr[M4_IMEM_INDEX_CNT-1:0] = $pc[M4_IMEM_INDEX_CNT+1:2];
               $instr[31:0] = $imem_rd_data[31:0];
            ?$imem_rd_en
               @1
                  $imem_rd_data[31:0] = /imem[$imem_rd_addr]$instr;
            // Note: Because of the magic we are using for visualisation, if visualisation is enabled below,
            //       be sure to avoid having unassigned signals (which you might be using for random inputs)
            //       other than those specifically expected in the labs. You'll get strange errors for these.
      
         
         // Assert these to end simulation (before Makerchip cycle limit).
         *passed = *cyc_cnt > 40;
         *failed = 1'b0;
         
         // Macro instantiations for:
         //  o instruction memory
         //  o register file
         //  o data memory
         //  o CPU visualization
         |cpu
            m4+imem(@1)    // Args: (read stage)
            //m4+rf(@1, @1)  // Args: (read stage, write stage) - if equal, no register bypass is required
            //m4+dmem(@4)    // Args: (read/write stage)
            //m4+myth_fpga(@0)  // Uncomment to run on fpga
      
         m4+cpu_viz(@4)    // For visualisation, argument should be at least equal to the last stage of CPU logic. @4 would work for all labs.
      \SV
         endmodule
     ```
  
### 2. Instruction Decoder
  - The Instruction Decoder is a vital circuit within the CPU that decodes machine instructions fetched from memory. It interprets the binary instruction format and produces control signals to direct other CPU components, ensuring the correct execution of each instruction.
  - Instruction Memory is a storage unit where the program's machine instructions are stored. It is typically read-only and contains the binary instructions that the CPU fetches for execution. The address provided by the       PC is used to retrieve the next instruction from this memory.
  - ![image](https://github.com/user-attachments/assets/2ad20cb6-c6eb-40dc-bc91-d6bf8c72416c)
  - ```
    \m4_TLV_version 1d: tl-x.org
    \SV
       // This code can be found in: https://github.com/stevehoover/RISC-V_MYTH_Workshop
       
       m4_include_lib(['https://raw.githubusercontent.com/BalaDhinesh/RISC-V_MYTH_Workshop/master/tlv_lib/risc-v_shell_lib.tlv'])
    
    \SV
       m4_makerchip_module   // (Expanded in Nav-TLV pane.)
    \TLV
    
       // /====================\
       // | Sum 1 to 9 Program |
       // \====================/
       //
       // Program for MYTH Workshop to test RV32I
       // Add 1,2,3,...,9 (in that order).
       //
       // Regs:
       //  r10 (a0): In: 0, Out: final sum
       //  r12 (a2): 10
       //  r13 (a3): 1..10
       //  r14 (a4): Sum
       // 
       // External to function:
       m4_asm(ADD, r10, r0, r0)             // Initialize r10 (a0) to 0.
       // Function:
       m4_asm(ADD, r14, r10, r0)            // Initialize sum register a4 with 0x0
       m4_asm(ADDI, r12, r10, 1010)         // Store count of 10 in register a2.
       m4_asm(ADD, r13, r10, r0)            // Initialize intermediate sum register a3 with 0
       // Loop:
       m4_asm(ADD, r14, r13, r14)           // Incremental addition
       m4_asm(ADDI, r13, r13, 1)            // Increment intermediate register by 1
       m4_asm(BLT, r13, r12, 1111111111000) // If a3 is less than a2, branch to label named <loop>
       m4_asm(ADD, r10, r14, r0)            // Store final result to register a0 so that it can be read by main program
       
       // Optional:
       // m4_asm(JAL, r7, 00000000000000000000) // Done. Jump to itself (infinite loop). (Up to 20-bit signed immediate plus implicit 0 bit (unlike JALR) provides byte address; last immediate bit should also be 0)
       m4_define_hier(['M4_IMEM'], M4_NUM_INSTRS)
    
       |cpu
          // YOUR CODE HERE
          // ...
          @0
             $reset = *reset;
             $clk_ach = *clk;
             $pc[31:0] = >>1$reset ? 32'd0 : (>>1$pc+32'd4);
          @1
             //Instruction Fetch
             $imem_rd_en = !$reset;
             $imem_rd_addr[M4_IMEM_INDEX_CNT-1:0] = $pc[M4_IMEM_INDEX_CNT+1:2];
             $instr[31:0] = $imem_rd_data[31:0];
          ?$imem_rd_en
             @1
                $imem_rd_data[31:0] = /imem[$imem_rd_addr]$instr;
          @1
             //Instruction Decode
             $is_i_instr = $instr[6:2] ==? 5'b0000x ||
                           $instr[6:2] ==? 5'b001x0 ||
                           $instr[6:2] ==? 5'b11001 ||
                           $instr[6:2] ==? 5'b11100;
             
             $is_u_instr = $instr[6:2] ==? 5'b0x101;
             
             $is_r_instr = $instr[6:2] ==? 5'b01011 ||
                           $instr[6:2] ==? 5'b011x0 ||
                           $instr[6:2] ==? 5'b10100;
             
             $is_b_instr = $instr[6:2] ==? 5'b11000;
             
             $is_j_instr = $instr[6:2] ==? 5'b11011;
             
             $is_s_instr = $instr[6:2] ==? 5'b0100x;
             
             $imm[31:0] = $is_i_instr ? {{21{$instr[31]}}, $instr[30:20]} :
                          $is_s_instr ? {{21{$instr[31]}}, $instr[30:25], $instr[11:7]} :
                          $is_b_instr ? {{20{$instr[31]}}, $instr[7], $instr[30:25], $instr[11:8], 1'b0} :
                          $is_u_instr ? {$instr[31:12], 12'b0} :
                          $is_j_instr ? {{12{$instr[31]}}, $instr[19:12], $instr[20], $instr[30:21], 1'b0} :
                                        32'b0;
             $opcode[6:0] = $instr[6:0];
             
             $rs2_valid = $is_r_instr || $is_s_instr || $is_b_instr;
             ?$rs2_valid
                $rs2[4:0] = $instr[24:20];
                
             $rs1_valid = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
             ?$rs1_valid
                $rs1[4:0] = $instr[19:15];
             
             $funct3_valid = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
             ?$funct3_valid
                $funct3[2:0] = $instr[14:12];
                
             $funct7_valid = $is_r_instr ;
             ?$funct7_valid
                $funct7[6:0] = $instr[31:25];
                
             $rd_valid = $is_r_instr || $is_i_instr || $is_u_instr || $is_j_instr;
             ?$rd_valid
                $rd[4:0] = $instr[11:7];
                
             $dec_bits [10:0] = {$funct7[5], $funct3, $opcode};
             $is_beq = $dec_bits ==? 11'bx_000_1100011;
             $is_bne = $dec_bits ==? 11'bx_001_1100011;
             $is_blt = $dec_bits ==? 11'bx_100_1100011;
             $is_bge = $dec_bits ==? 11'bx_101_1100011;
             $is_bltu = $dec_bits ==? 11'bx_110_1100011;
             $is_bgeu = $dec_bits ==? 11'bx_111_1100011;
             $is_addi = $dec_bits ==? 11'bx_000_0010011;
             $is_add = $dec_bits ==? 11'b0_000_0110011;
          
          // Note: Because of the magic we are using for visualisation, if visualisation is enabled below,
          //       be sure to avoid having unassigned signals (which you might be using for random inputs)
          //       other than those specifically expected in the labs. You'll get strange errors for these.
    
       
       // Assert these to end simulation (before Makerchip cycle limit).
       *passed = *cyc_cnt > 40;
       *failed = 1'b0;
       
       // Macro instantiations for:
       //  o instruction memory
       //  o register file
       //  o data memory
       //  o CPU visualization
       |cpu
          m4+imem(@1)    // Args: (read stage)
          //m4+rf(@1, @1)  // Args: (read stage, write stage) - if equal, no register bypass is required
          //m4+dmem(@4)    // Args: (read/write stage)
          //m4+myth_fpga(@0)  // Uncomment to run on fpga
    
       m4+cpu_viz(@4)    // For visualisation, argument should be at least equal to the last stage of CPU logic. @4 would work for all labs.
    \SV
       endmodule
    ```

### 3. Data Memory
- Data Memory serves as storage for data that is accessed or modified during program execution. Unlike Instruction Memory, Data Memory supports both read and write operations, storing variables, arrays, and other data needed by the program during its run.
- ![image](https://github.com/user-attachments/assets/e36acb8a-91b1-428d-8a72-0a51193a62aa)
- ```
  \m4_TLV_version 1d: tl-x.org
  \SV
     // This code can be found in: https://github.com/stevehoover/RISC-V_MYTH_Workshop
     
     m4_include_lib(['https://raw.githubusercontent.com/BalaDhinesh/RISC-V_MYTH_Workshop/master/tlv_lib/risc-v_shell_lib.tlv'])
  
  \SV
     m4_makerchip_module   // (Expanded in Nav-TLV pane.)
  \TLV
  
     // /====================\
     // | Sum 1 to 9 Program |
     // \====================/
     //
     // Program for MYTH Workshop to test RV32I
     // Add 1,2,3,...,9 (in that order).
     //
     // Regs:
     //  r10 (a0): In: 0, Out: final sum
     //  r12 (a2): 10
     //  r13 (a3): 1..10
     //  r14 (a4): Sum
     // 
     // External to function:
     m4_asm(ADD, r10, r0, r0)             // Initialize r10 (a0) to 0.
     // Function:
     m4_asm(ADD, r14, r10, r0)            // Initialize sum register a4 with 0x0
     m4_asm(ADDI, r12, r10, 1010)         // Store count of 10 in register a2.
     m4_asm(ADD, r13, r10, r0)            // Initialize intermediate sum register a3 with 0
     // Loop:
     m4_asm(ADD, r14, r13, r14)           // Incremental addition
     m4_asm(ADDI, r13, r13, 1)            // Increment intermediate register by 1
     m4_asm(BLT, r13, r12, 1111111111000) // If a3 is less than a2, branch to label named <loop>
     m4_asm(ADD, r10, r14, r0)            // Store final result to register a0 so that it can be read by main program
     
     // Optional:
     // m4_asm(JAL, r7, 00000000000000000000) // Done. Jump to itself (infinite loop). (Up to 20-bit signed immediate plus implicit 0 bit (unlike JALR) provides byte address; last immediate bit should also be 0)
     m4_define_hier(['M4_IMEM'], M4_NUM_INSTRS)
  
     |cpu
        @0
           $reset = *reset;
           $clk_ach = *clk;
           $pc[31:0] = >>1$reset ? 32'd0 : (>>1$pc+32'd4);
        @1
           //Instruction Fetch
           $imem_rd_en = !$reset;
           $imem_rd_addr[M4_IMEM_INDEX_CNT-1:0] = $pc[M4_IMEM_INDEX_CNT+1:2];
           $instr[31:0] = $imem_rd_data[31:0];
        ?$imem_rd_en
           @1
              $imem_rd_data[31:0] = /imem[$imem_rd_addr]$instr;
        @1
           //Instruction Decode
           $is_i_instr = $instr[6:2] ==? 5'b0000x ||
                         $instr[6:2] ==? 5'b001x0 ||
                         $instr[6:2] ==? 5'b11001 ||
                         $instr[6:2] ==? 5'b11100;
           
           $is_u_instr = $instr[6:2] ==? 5'b0x101;
           
           $is_r_instr = $instr[6:2] ==? 5'b01011 ||
                         $instr[6:2] ==? 5'b011x0 ||
                         $instr[6:2] ==? 5'b10100;
           
           $is_b_instr = $instr[6:2] ==? 5'b11000;
           
           $is_j_instr = $instr[6:2] ==? 5'b11011;
           
           $is_s_instr = $instr[6:2] ==? 5'b0100x;
           
           $imm[31:0] = $is_i_instr ? {{21{$instr[31]}}, $instr[30:20]} :
                        $is_s_instr ? {{21{$instr[31]}}, $instr[30:25], $instr[11:7]} :
                        $is_b_instr ? {{20{$instr[31]}}, $instr[7], $instr[30:25], $instr[11:8], 1'b0} :
                        $is_u_instr ? {$instr[31:12], 12'b0} :
                        $is_j_instr ? {{12{$instr[31]}}, $instr[19:12], $instr[20], $instr[30:21], 1'b0} :
                                      32'b0;
           $opcode[6:0] = $instr[6:0];
           
           $rs2_valid = $is_r_instr || $is_s_instr || $is_b_instr;
           ?$rs2_valid
              $rs2[4:0] = $instr[24:20];
              
           $rs1_valid = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
           ?$rs1_valid
              $rs1[4:0] = $instr[19:15];
           
           $funct3_valid = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
           ?$funct3_valid
              $funct3[2:0] = $instr[14:12];
              
           $funct7_valid = $is_r_instr ;
           ?$funct7_valid
              $funct7[6:0] = $instr[31:25];
              
           $rd_valid = $is_r_instr || $is_i_instr || $is_u_instr || $is_j_instr;
           ?$rd_valid
              $rd[4:0] = $instr[11:7];
              
           $dec_bits [10:0] = {$funct7[5], $funct3, $opcode};
           $is_beq = $dec_bits ==? 11'bx_000_1100011;
           $is_bne = $dec_bits ==? 11'bx_001_1100011;
           $is_blt = $dec_bits ==? 11'bx_100_1100011;
           $is_bge = $dec_bits ==? 11'bx_101_1100011;
           $is_bltu = $dec_bits ==? 11'bx_110_1100011;
           $is_bgeu = $dec_bits ==? 11'bx_111_1100011;
           $is_addi = $dec_bits ==? 11'bx_000_0010011;
           $is_add = $dec_bits ==? 11'b0_000_0110011;
           
        @1
           //Register File Read
           $rf_wr_en = 1'b0;
           $rf_wr_index[4:0] = 5'b0;
           $rf_wr_data[31:0] = 32'b0;
           
           $rf_rd_en1 = $rs1_valid;
           $rf_rd_index1[4:0] = $rs1;
           
           $rf_rd_en2 = $rs2_valid;
           $rf_rd_index2[4:0] = $rs2;
           
           $src1_value[31:0] = $rf_rd_data1;
           $src2_value[31:0] = $rf_rd_data2;
           
        // Note: Because of the magic we are using for visualisation, if visualisation is enabled below,
        //       be sure to avoid having unassigned signals (which you might be using for random inputs)
        //       other than those specifically expected in the labs. You'll get strange errors for these.
  
     
     // Assert these to end simulation (before Makerchip cycle limit).
     *passed = *cyc_cnt > 40;
     *failed = 1'b0;
     
     // Macro instantiations for:
     //  o instruction memory
     //  o register file
     //  o data memory
     //  o CPU visualization
     |cpu
        m4+imem(@1)    // Args: (read stage)
        m4+rf(@1, @1)  // Args: (read stage, write stage) - if equal, no register bypass is required
        //m4+dmem(@4)    // Args: (read/write stage)
        //m4+myth_fpga(@0)  // Uncomment to run on fpga
  
     m4+cpu_viz(@4)    // For visualisation, argument should be at least equal to the last stage of CPU logic. @4 would work for all labs.
  \SV
     endmodule
  ```

### 5. Arithmetic Logic Unit (ALU)
- The ALU is a central component of the CPU responsible for performing arithmetic and logical operations on data. It handles operations such as addition, subtraction, multiplication, division, and bitwise logic (e.g., AND, OR, XOR). The results produced by the ALU are essential for executing various instructions.
- ![image](https://github.com/user-attachments/assets/4631b210-fba3-4c4a-a3eb-8166118ae3b4)
- ```
  \m4_TLV_version 1d: tl-x.org
  \SV
     // This code can be found in: https://github.com/stevehoover/RISC-V_MYTH_Workshop
     
     m4_include_lib(['https://raw.githubusercontent.com/BalaDhinesh/RISC-V_MYTH_Workshop/master/tlv_lib/risc-v_shell_lib.tlv'])
  
  \SV
     m4_makerchip_module   // (Expanded in Nav-TLV pane.)
  \TLV
  
     // /====================\
     // | Sum 1 to 9 Program |
     // \====================/
     //
     // Program for MYTH Workshop to test RV32I
     // Add 1,2,3,...,9 (in that order).
     //
     // Regs:
     //  r10 (a0): In: 0, Out: final sum
     //  r12 (a2): 10
     //  r13 (a3): 1..10
     //  r14 (a4): Sum
     // 
     // External to function:
     m4_asm(ADD, r10, r0, r0)             // Initialize r10 (a0) to 0.
     // Function:
     m4_asm(ADD, r14, r10, r0)            // Initialize sum register a4 with 0x0
     m4_asm(ADDI, r12, r10, 1010)         // Store count of 10 in register a2.
     m4_asm(ADD, r13, r10, r0)            // Initialize intermediate sum register a3 with 0
     // Loop:
     m4_asm(ADD, r14, r13, r14)           // Incremental addition
     m4_asm(ADDI, r13, r13, 1)            // Increment intermediate register by 1
     m4_asm(BLT, r13, r12, 1111111111000) // If a3 is less than a2, branch to label named <loop>
     m4_asm(ADD, r10, r14, r0)            // Store final result to register a0 so that it can be read by main program
     
     // Optional:
     // m4_asm(JAL, r7, 00000000000000000000) // Done. Jump to itself (infinite loop). (Up to 20-bit signed immediate plus implicit 0 bit (unlike JALR) provides byte address; last immediate bit should also be 0)
     m4_define_hier(['M4_IMEM'], M4_NUM_INSTRS)
  
     |cpu
        // YOUR CODE HERE
        // ...
        @0
           $reset = *reset;
           $clk_ach = *clk;
           $pc[31:0] = >>1$reset ? 32'd0 : (>>1$pc+32'd4);
        @1
           //Instruction Fetch
           $imem_rd_en = !$reset;
           $imem_rd_addr[M4_IMEM_INDEX_CNT-1:0] = $pc[M4_IMEM_INDEX_CNT+1:2];
           $instr[31:0] = $imem_rd_data[31:0];
        ?$imem_rd_en
           @1
              $imem_rd_data[31:0] = /imem[$imem_rd_addr]$instr;
        @1
           //Instruction Decode
           $is_i_instr = $instr[6:2] ==? 5'b0000x ||
                         $instr[6:2] ==? 5'b001x0 ||
                         $instr[6:2] ==? 5'b11001 ||
                         $instr[6:2] ==? 5'b11100;
           
           $is_u_instr = $instr[6:2] ==? 5'b0x101;
           
           $is_r_instr = $instr[6:2] ==? 5'b01011 ||
                         $instr[6:2] ==? 5'b011x0 ||
                         $instr[6:2] ==? 5'b10100;
           
           $is_b_instr = $instr[6:2] ==? 5'b11000;
           
           $is_j_instr = $instr[6:2] ==? 5'b11011;
           
           $is_s_instr = $instr[6:2] ==? 5'b0100x;
           
           $imm[31:0] = $is_i_instr ? {{21{$instr[31]}}, $instr[30:20]} :
                        $is_s_instr ? {{21{$instr[31]}}, $instr[30:25], $instr[11:7]} :
                        $is_b_instr ? {{20{$instr[31]}}, $instr[7], $instr[30:25], $instr[11:8], 1'b0} :
                        $is_u_instr ? {$instr[31:12], 12'b0} :
                        $is_j_instr ? {{12{$instr[31]}}, $instr[19:12], $instr[20], $instr[30:21], 1'b0} :
                                      32'b0;
           $opcode[6:0] = $instr[6:0];
           
           $rs2_valid = $is_r_instr || $is_s_instr || $is_b_instr;
           ?$rs2_valid
              $rs2[4:0] = $instr[24:20];
              
           $rs1_valid = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
           ?$rs1_valid
              $rs1[4:0] = $instr[19:15];
           
           $funct3_valid = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
           ?$funct3_valid
              $funct3[2:0] = $instr[14:12];
              
           $funct7_valid = $is_r_instr ;
           ?$funct7_valid
              $funct7[6:0] = $instr[31:25];
              
           $rd_valid = $is_r_instr || $is_i_instr || $is_u_instr || $is_j_instr;
           ?$rd_valid
              $rd[4:0] = $instr[11:7];
              
           $dec_bits [10:0] = {$funct7[5], $funct3, $opcode};
           $is_beq = $dec_bits ==? 11'bx_000_1100011;
           $is_bne = $dec_bits ==? 11'bx_001_1100011;
           $is_blt = $dec_bits ==? 11'bx_100_1100011;
           $is_bge = $dec_bits ==? 11'bx_101_1100011;
           $is_bltu = $dec_bits ==? 11'bx_110_1100011;
           $is_bgeu = $dec_bits ==? 11'bx_111_1100011;
           $is_addi = $dec_bits ==? 11'bx_000_0010011;
           $is_add = $dec_bits ==? 11'b0_000_0110011;
           
        @1
           //Register File Read
           $rf_wr_en = 1'b0;
           $rf_wr_index[4:0] = 5'b0;
           $rf_wr_data[31:0] = 32'b0;
           
           $rf_rd_en1 = $rs1_valid;
           $rf_rd_index1[4:0] = $rs1;
           
           $rf_rd_en2 = $rs2_valid;
           $rf_rd_index2[4:0] = $rs2;
           
           $src1_value[31:0] = $rf_rd_data1;
           $src2_value[31:0] = $rf_rd_data2;
           
        @1
           //ALU
           $result[31:0] = $is_addi ? $src1_value + $imm :
                           $is_add ? $src1_value + $src2_value :
                           32'bx ;
           
        // Note: Because of the magic we are using for visualisation, if visualisation is enabled below,
        //       be sure to avoid having unassigned signals (which you might be using for random inputs)
        //       other than those specifically expected in the labs. You'll get strange errors for these.
  
     
     // Assert these to end simulation (before Makerchip cycle limit).
     *passed = *cyc_cnt > 40;
     *failed = 1'b0;
     
     // Macro instantiations for:
     //  o instruction memory
     //  o register file
     //  o data memory
     //  o CPU visualization
     |cpu
        m4+imem(@1)    // Args: (read stage)
        m4+rf(@1, @1)  // Args: (read stage, write stage) - if equal, no register bypass is required
        //m4+dmem(@4)    // Args: (read/write stage)
        //m4+myth_fpga(@0)  // Uncomment to run on fpga
  
     m4+cpu_viz(@4)    // For visualisation, argument should be at least equal to the last stage of CPU logic. @4 would work for all labs.
  \SV
     endmodule
  ```


### 6. Read Register File
The Read Register File consists of a set of registers used to store data during instruction execution. Instructions often require data from these registers, which are specified by the instruction. This data is then used as input operands for the ALU or other processing units.

### 7. Write Register File
 - The Write Register File is used to store the results of operations back into the CPU's registers. After an instruction is executed, the resulting data is written back to this register file, ensuring it is available for subsequent instructions.
 - ![image](https://github.com/user-attachments/assets/05d75db0-023d-425e-bef5-12b5c8eeab8e)
 - ```
   \m4_TLV_version 1d: tl-x.org
    \SV
       // This code can be found in: https://github.com/stevehoover/RISC-V_MYTH_Workshop
       
       m4_include_lib(['https://raw.githubusercontent.com/BalaDhinesh/RISC-V_MYTH_Workshop/master/tlv_lib/risc-v_shell_lib.tlv'])
    
    \SV
       m4_makerchip_module   // (Expanded in Nav-TLV pane.)
    \TLV
    
       // /====================\
       // | Sum 1 to 9 Program |
       // \====================/
       //
       // Program for MYTH Workshop to test RV32I
       // Add 1,2,3,...,9 (in that order).
       //
       // Regs:
       //  r10 (a0): In: 0, Out: final sum
       //  r12 (a2): 10
       //  r13 (a3): 1..10
       //  r14 (a4): Sum
       // 
       // External to function:
       m4_asm(ADD, r10, r0, r0)             // Initialize r10 (a0) to 0.
       // Function:
       m4_asm(ADD, r14, r10, r0)            // Initialize sum register a4 with 0x0
       m4_asm(ADDI, r12, r10, 1010)         // Store count of 10 in register a2.
       m4_asm(ADD, r13, r10, r0)            // Initialize intermediate sum register a3 with 0
       // Loop:
       m4_asm(ADD, r14, r13, r14)           // Incremental addition
       m4_asm(ADDI, r13, r13, 1)            // Increment intermediate register by 1
       m4_asm(BLT, r13, r12, 1111111111000) // If a3 is less than a2, branch to label named <loop>
       m4_asm(ADD, r10, r14, r0)            // Store final result to register a0 so that it can be read by main program
       
       // Optional:
       // m4_asm(JAL, r7, 00000000000000000000) // Done. Jump to itself (infinite loop). (Up to 20-bit signed immediate plus implicit 0 bit (unlike JALR) provides byte address; last immediate bit should also be 0)
       m4_define_hier(['M4_IMEM'], M4_NUM_INSTRS)
    
       |cpu
          // YOUR CODE HERE
          // ...
          @0
             $reset = *reset;
             $clk_ach = *clk;
             $pc[31:0] = >>1$reset ? 32'd0 : (>>1$pc+32'd4);
          @1
             //Instruction Fetch
             $imem_rd_en = !$reset;
             $imem_rd_addr[M4_IMEM_INDEX_CNT-1:0] = $pc[M4_IMEM_INDEX_CNT+1:2];
             $instr[31:0] = $imem_rd_data[31:0];
          ?$imem_rd_en
             @1
                $imem_rd_data[31:0] = /imem[$imem_rd_addr]$instr;
          @1
             //Instruction Decode
             $is_i_instr = $instr[6:2] ==? 5'b0000x ||
                           $instr[6:2] ==? 5'b001x0 ||
                           $instr[6:2] ==? 5'b11001 ||
                           $instr[6:2] ==? 5'b11100;
             
             $is_u_instr = $instr[6:2] ==? 5'b0x101;
             
             $is_r_instr = $instr[6:2] ==? 5'b01011 ||
                           $instr[6:2] ==? 5'b011x0 ||
                           $instr[6:2] ==? 5'b10100;
             
             $is_b_instr = $instr[6:2] ==? 5'b11000;
             
             $is_j_instr = $instr[6:2] ==? 5'b11011;
             
             $is_s_instr = $instr[6:2] ==? 5'b0100x;
             
             $imm[31:0] = $is_i_instr ? {{21{$instr[31]}}, $instr[30:20]} :
                          $is_s_instr ? {{21{$instr[31]}}, $instr[30:25], $instr[11:7]} :
                          $is_b_instr ? {{20{$instr[31]}}, $instr[7], $instr[30:25], $instr[11:8], 1'b0} :
                          $is_u_instr ? {$instr[31:12], 12'b0} :
                          $is_j_instr ? {{12{$instr[31]}}, $instr[19:12], $instr[20], $instr[30:21], 1'b0} :
                                        32'b0;
             $opcode[6:0] = $instr[6:0];
             
             $rs2_valid = $is_r_instr || $is_s_instr || $is_b_instr;
             ?$rs2_valid
                $rs2[4:0] = $instr[24:20];
                
             $rs1_valid = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
             ?$rs1_valid
                $rs1[4:0] = $instr[19:15];
             
             $funct3_valid = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
             ?$funct3_valid
                $funct3[2:0] = $instr[14:12];
                
             $funct7_valid = $is_r_instr ;
             ?$funct7_valid
                $funct7[6:0] = $instr[31:25];
                
             $rd_valid = $is_r_instr || $is_i_instr || $is_u_instr || $is_j_instr;
             ?$rd_valid 
                $rd[4:0] = $instr[11:7]; //rd - Destination Register
                
             $dec_bits [10:0] = {$funct7[5], $funct3, $opcode};
             $is_beq = $dec_bits ==? 11'bx_000_1100011;
             $is_bne = $dec_bits ==? 11'bx_001_1100011;
             $is_blt = $dec_bits ==? 11'bx_100_1100011;
             $is_bge = $dec_bits ==? 11'bx_101_1100011;
             $is_bltu = $dec_bits ==? 11'bx_110_1100011;
             $is_bgeu = $dec_bits ==? 11'bx_111_1100011;
             $is_addi = $dec_bits ==? 11'bx_000_0010011;
             $is_add = $dec_bits ==? 11'b0_000_0110011;
             
          @1
             //Register File Read
             $rf_wr_en = 1'b0;
             $rf_wr_index[4:0] = 5'b0;
             $rf_wr_data[31:0] = 32'b0;
             
             $rf_rd_en1 = $rs1_valid;
             $rf_rd_index1[4:0] = $rs1;
             
             $rf_rd_en2 = $rs2_valid;
             $rf_rd_index2[4:0] = $rs2;
             
             $src1_value[31:0] = $rf_rd_data1;
             $src2_value[31:0] = $rf_rd_data2;
             
          @1
             //ALU
             $result[31:0] = $is_addi ? $src1_value + $imm :
                             $is_add ? $src1_value + $src2_value :
                             32'bx ;
          @1
             //Register File Write
             $rf_wr_en = $rd_valid && $rd != 5'b0;
             $rf_wr_index[4:0] = $rd;
             $rf_wr_data[31:0] = $result;
             
          // Note: Because of the magic we are using for visualisation, if visualisation is enabled below,
          //       be sure to avoid having unassigned signals (which you might be using for random inputs)
          //       other than those specifically expected in the labs. You'll get strange errors for these.
    
       
       // Assert these to end simulation (before Makerchip cycle limit).
       *passed = *cyc_cnt > 40;
       *failed = 1'b0;
       
       // Macro instantiations for:
       //  o instruction memory
       //  o register file
       //  o data memory
       //  o CPU visualization
       |cpu
          m4+imem(@1)    // Args: (read stage)
          m4+rf(@1, @1)  // Args: (read stage, write stage) - if equal, no register bypass is required
          //m4+dmem(@4)    // Args: (read/write stage)
          //m4+myth_fpga(@0)  // Uncomment to run on fpga
    
       m4+cpu_viz(@4)    // For visualisation, argument should be at least equal to the last stage of CPU logic. @4 would work for all labs.
    \SV
       endmodule
   ```
### 8. Branch Instructions
  - ![image](https://github.com/user-attachments/assets/67ee9af7-4db7-4e64-9584-c3f1ee66aca5)
  - ```
    \m4_TLV_version 1d: tl-x.org
    \SV
       // This code can be found in: https://github.com/stevehoover/RISC-V_MYTH_Workshop
       
       m4_include_lib(['https://raw.githubusercontent.com/BalaDhinesh/RISC-V_MYTH_Workshop/master/tlv_lib/risc-v_shell_lib.tlv'])
    
    \SV
       m4_makerchip_module   // (Expanded in Nav-TLV pane.)
    \TLV
    
       // /====================\
       // | Sum 1 to 9 Program |
       // \====================/
       //
       // Program for MYTH Workshop to test RV32I
       // Add 1,2,3,...,9 (in that order).
       //
       // Regs:
       //  r10 (a0): In: 0, Out: final sum
       //  r12 (a2): 10
       //  r13 (a3): 1..10
       //  r14 (a4): Sum
       // 
       // External to function:
       m4_asm(ADD, r10, r0, r0)             // Initialize r10 (a0) to 0.
       // Function:
       m4_asm(ADD, r14, r10, r0)            // Initialize sum register a4 with 0x0
       m4_asm(ADDI, r12, r10, 1010)         // Store count of 10 in register a2.
       m4_asm(ADD, r13, r10, r0)            // Initialize intermediate sum register a3 with 0
       // Loop:
       m4_asm(ADD, r14, r13, r14)           // Incremental addition
       m4_asm(ADDI, r13, r13, 1)            // Increment intermediate register by 1
       m4_asm(BLT, r13, r12, 1111111111000) // If a3 is less than a2, branch to label named <loop>
       m4_asm(ADD, r10, r14, r0)            // Store final result to register a0 so that it can be read by main program
       
       // Optional:
       // m4_asm(JAL, r7, 00000000000000000000) // Done. Jump to itself (infinite loop). (Up to 20-bit signed immediate plus implicit 0 bit (unlike JALR) provides byte address; last immediate bit should also be 0)
       m4_define_hier(['M4_IMEM'], M4_NUM_INSTRS)
    
       |cpu
          // YOUR CODE HERE
          // ...
          @0
             $reset = *reset;
             $clk_ach = *clk;
             $pc[31:0] = >>1$reset ? 32'd0 
                   : (>>1$taken_branch ? >>1$br_tgt_pc :  (>>1$pc+32'd4));
          @1
             //Instruction Fetch
             $imem_rd_en = !$reset;
             $imem_rd_addr[M4_IMEM_INDEX_CNT-1:0] = $pc[M4_IMEM_INDEX_CNT+1:2];
             $instr[31:0] = $imem_rd_data[31:0];
          ?$imem_rd_en
             @1
                $imem_rd_data[31:0] = /imem[$imem_rd_addr]$instr;
          @1
             //Instruction Decode
             $is_i_instr = $instr[6:2] ==? 5'b0000x ||
                           $instr[6:2] ==? 5'b001x0 ||
                           $instr[6:2] ==? 5'b11001 ||
                           $instr[6:2] ==? 5'b11100;
             
             $is_u_instr = $instr[6:2] ==? 5'b0x101;
             
             $is_r_instr = $instr[6:2] ==? 5'b01011 ||
                           $instr[6:2] ==? 5'b011x0 ||
                           $instr[6:2] ==? 5'b10100;
             
             $is_b_instr = $instr[6:2] ==? 5'b11000;
             
             $is_j_instr = $instr[6:2] ==? 5'b11011;
             
             $is_s_instr = $instr[6:2] ==? 5'b0100x;
             
             $imm[31:0] = $is_i_instr ? {{21{$instr[31]}}, $instr[30:20]} :
                          $is_s_instr ? {{21{$instr[31]}}, $instr[30:25], $instr[11:7]} :
                          $is_b_instr ? {{20{$instr[31]}}, $instr[7], $instr[30:25], $instr[11:8], 1'b0} :
                          $is_u_instr ? {$instr[31:12], 12'b0} :
                          $is_j_instr ? {{12{$instr[31]}}, $instr[19:12], $instr[20], $instr[30:21], 1'b0} :
                                        32'b0;
             $opcode[6:0] = $instr[6:0];
             
             $rs2_valid = $is_r_instr || $is_s_instr || $is_b_instr;
             ?$rs2_valid
                $rs2[4:0] = $instr[24:20];
                
             $rs1_valid = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
             ?$rs1_valid
                $rs1[4:0] = $instr[19:15];
             
             $funct3_valid = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
             ?$funct3_valid
                $funct3[2:0] = $instr[14:12];
                
             $funct7_valid = $is_r_instr ;
             ?$funct7_valid
                $funct7[6:0] = $instr[31:25];
                
             $rd_valid = $is_r_instr || $is_i_instr || $is_u_instr || $is_j_instr;
             ?$rd_valid 
                $rd[4:0] = $instr[11:7]; //rd - Destination Register
                
             $dec_bits [10:0] = {$funct7[5], $funct3, $opcode};
             $is_beq = $dec_bits ==? 11'bx_000_1100011;
             $is_bne = $dec_bits ==? 11'bx_001_1100011;
             $is_blt = $dec_bits ==? 11'bx_100_1100011;
             $is_bge = $dec_bits ==? 11'bx_101_1100011;
             $is_bltu = $dec_bits ==? 11'bx_110_1100011;
             $is_bgeu = $dec_bits ==? 11'bx_111_1100011;
             $is_addi = $dec_bits ==? 11'bx_000_0010011;
             $is_add = $dec_bits ==? 11'b0_000_0110011;
             
          @1
             //Register File Read
             $rf_wr_en = 1'b0;
             $rf_wr_index[4:0] = 5'b0;
             $rf_wr_data[31:0] = 32'b0;
             
             $rf_rd_en1 = $rs1_valid;
             $rf_rd_index1[4:0] = $rs1;
             
             $rf_rd_en2 = $rs2_valid;
             $rf_rd_index2[4:0] = $rs2;
             
             $src1_value[31:0] = $rf_rd_data1;
             $src2_value[31:0] = $rf_rd_data2;
             
          @1
             //ALU
             $result[31:0] = $is_addi ? $src1_value + $imm :
                             $is_add ? $src1_value + $src2_value :
                             32'bx ;
          @1
             //Register File Write
             $rf_wr_en = $rd_valid && $rd != 5'b0;
             $rf_wr_index[4:0] = $rd;
             $rf_wr_data[31:0] = $result;
             
          @1
             //Branch Instructions
             $taken_branch = $is_beq ? ($src1_value == $src2_value):
                             $is_bne ? ($src1_value != $src2_value):
                             $is_blt ? (($src1_value < $src2_value) ^ ($src1_value[31] != $src2_value[31])):
                             $is_bge ? (($src1_value >= $src2_value) ^ ($src1_value[31] != $src2_value[31])):
                             $is_bltu ? ($src1_value < $src2_value):
                             $is_bgeu ? ($src1_value >= $src2_value):
                                        1'b0;
             `BOGUS_USE($taken_branch)
             $br_tgt_pc[31:0] = $pc + $imm;
          // Note: Because of the magic we are using for visualisation, if visualisation is enabled below,
          //       be sure to avoid having unassigned signals (which you might be using for random inputs)
          //       other than those specifically expected in the labs. You'll get strange errors for these.
    
       
       // Assert these to end simulation (before Makerchip cycle limit).
       *passed = *cyc_cnt > 40;
       *failed = 1'b0;
       
       // Macro instantiations for:
       //  o instruction memory
       //  o register file
       //  o data memory
       //  o CPU visualization
       |cpu
          m4+imem(@1)    // Args: (read stage)
          m4+rf(@1, @1)  // Args: (read stage, write stage) - if equal, no register bypass is required
          //m4+dmem(@4)    // Args: (read/write stage)
          //m4+myth_fpga(@0)  // Uncomment to run on fpga
    
       m4+cpu_viz(@4)    // For visualisation, argument should be at least equal to the last stage of CPU logic. @4 would work for all labs.
    \SV
       endmodule
    ```


These components work together seamlessly to execute machine instructions. The Program Counter manages the instruction fetch process, the Instruction Decoder interprets the fetched instructions, the ALU handles data computations, and the Register Files and Memory components ensure data is stored and accessed as needed. This collaboration allows the CPU to efficiently execute the tasks defined by the program.

To test the code using the testbech include the line in @1 stage :
```
  *passed = |cpu/xreg[14]>>5$value == (1+2+3+4+5+6+7+8+9) ;
```

![image](https://github.com/user-attachments/assets/ecdd200b-8925-4cb7-b457-1244e383d66f)
![image](https://github.com/user-attachments/assets/42a4fd0f-d95d-4323-80d5-e0cdfa96ecf3)
![image](https://github.com/user-attachments/assets/97b04437-300b-4893-b973-21017c79644b)
![image](https://github.com/user-attachments/assets/6294f147-db88-4121-a42a-c40eee4157bf)

The sum of numbers from 1 to 9 is 45(i.e 2D in hex) which is verified in the waveform for |cpu/xreg[14] in the above figure.

## Day 5 - Pipelining the CPU

Pipelining of the CPU core has been completed, significantly simplifying retiming and greatly reducing functional bugs. Pipelining enables faster computation. As previously mentioned, to implement pipelining, we simply need to add @1, @2, and so on. The snapshot of the pipelining process is shown below. In TL-Verilog, another advantage is that the pipeline can be defined without needing to follow a strict order.

![image](https://github.com/user-attachments/assets/19867260-2437-433f-b6f4-9e8d8496b2f2)
![image](https://github.com/user-attachments/assets/3fd84626-5549-45fe-9bda-6bdbc290e05d)
![image](https://github.com/user-attachments/assets/c228e752-a8b4-45f4-8776-0a578b7a7bf1)
![image](https://github.com/user-attachments/assets/4187e1ec-16ca-4996-86b5-636452acff7b)
![image](https://github.com/user-attachments/assets/b9f6e0df-c21a-4024-9aa0-2decdd9c440c)


The sum of numbers from 1 to 9 is 45(i.e 2D in hex) which is verified in the waveform for |cpu/xreg[14] in the above figure.


</details>

