![Screenshot from 2024-11-13 00-51-35](https://github.com/user-attachments/assets/8bf3e4d3-c94a-432d-a00d-5a81d201e418)# ASIC Design Lab

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

<details>
  <summary>Task 6 : Converting TL-Verilog to Verilog and Simulating with a Testbench</summary>

  ```bash
  sudo apt install make python python3 python3-pip git iverilog gtkwave
  cd ~
  sudo apt-get install python3-venv
  python3 -m venv .venv
  source ~/.venv/bin/activate
  pip3 install pyyaml click sandpiper-saas
  ```

  ![1](https://github.com/user-attachments/assets/508edfdd-7c0c-4cba-8a43-f920789c6c05)

  ```bash
  sudo apt install make python python3 python3-pip git iverilog gtkwave docker.io
  sudo chmod 666 /var/run/docker.sock
  cd ~
  pip3 install pyyaml click sandpiper-saas
  ```

  ![2](https://github.com/user-attachments/assets/8fa11e76-484a-4682-92de-68e31568ea8f)

  ```bash
  cd ~
  git clone https://github.com/manili/VSDBabySoC.git
  cd /home/vsduser/VSDBabySoC
  make pre_synth_sim
  ```
  ![3](https://github.com/user-attachments/assets/afb2ec44-3230-4c6c-b656-472a17f5c3c7)
  ![4](https://github.com/user-attachments/assets/3e9c491d-4799-4a00-8dd7-cb728a8613cc)
  
To replace the rvmyth.tlv file in the `VSDBabySoC/src/module` folder with our RISC-V design from the Makerchip .tlv file, we must first convert the .tlv code into Verilog. Additionally, we need to update the testbench to align with our Makerchip design. To translate the .tlv definition of the RISC-V design into Verilog (.v file), use the following code.
 ```tl-verilog
  
\m4_TLV_version 1d: tl-x.org
\SV
   m4_include_lib(['https://raw.githubusercontent.com/shivanishah269/risc-v-core/master/FPGA_Implementation/riscv_shell_lib.tlv'])
   
   // Module interface, either for Makerchip, or not.
   m4_ifelse_block(M4_MAKERCHIP, 1, ['
   // Makerchip module interface.
   m4_makerchip_module
   wire CLK = clk;
   logic [9:0] OUT;
   assign passed = cyc_cnt > 300;
   '], ['
   // Custom module interface for BabySoC.
   module rvmyth(
      output reg [9:0] OUT,
      input CLK,
      input reset
   );
   wire clk = CLK;
   '])
   
\TLV
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
   m4_asm(SW, r0, r10, 10000)           // Store the final result value to byte address 16
   m4_asm(LW, r17, r0, 10000)           // Load the final result value from adress 16 to x17
   //
   m4_define_hier(['M4_IMEM'], M4_NUM_INSTRS)
   //
   |cpu
      @0
         $reset = *reset;
         $clk_ach = *clk;
         `BOGUS_USE($clk_ach)
      //Fetch
         // Next PC
         $pc[31:0] = (>>1$reset) ? 32'd0 : 
                     (>>3$valid_taken_br) ? >>3$br_tgt_pc : 
                     (>>3$valid_load) ? >>3$inc_pc : 
                     (>>3$valid_jump && >>3$is_jal) ? >>3$br_tgt_pc :
                     (>>3$valid_jump && >>3$is_jalr) ? >>3$jalr_tgt_pc : >>1$inc_pc;
         
         $imem_rd_en = !$reset;
         $imem_rd_addr[31:0] = $pc[M4_IMEM_INDEX_CNT+1:2];
         
      @1         
         $instr[31:0] = $imem_rd_data[31:0];
         $inc_pc[31:0] = $pc + 32'd4;          
      // Decode   
         $is_i_instr = $instr[6:2] == 5'b00000 ||
                       $instr[6:2] == 5'b00001 ||
                       $instr[6:2] == 5'b00100 ||
                       $instr[6:2] == 5'b00110 ||
                       $instr[6:2] == 5'b11001;
         $is_r_instr = $instr[6:2] == 5'b01011 ||
                       $instr[6:2] == 5'b10100 ||
                       $instr[6:2] == 5'b01100 ||
                       $instr[6:2] == 5'b01101;                       
         $is_b_instr = $instr[6:2] == 5'b11000;
         $is_u_instr = $instr[6:2] == 5'b00101 ||
                       $instr[6:2] == 5'b01101;
         $is_s_instr = $instr[6:2] == 5'b01000 ||
                       $instr[6:2] == 5'b01001;
         $is_j_instr = $instr[6:2] == 5'b11011;
         
         $imm[31:0] = $is_i_instr ? { {21{$instr[31]}} , $instr[30:20] } :
                      $is_s_instr ? { {21{$instr[31]}} , $instr[30:25] , $instr[11:8] , $instr[7] } :
                      $is_b_instr ? { {20{$instr[31]}} , $instr[7] , $instr[30:25] , $instr[11:8] , 1'b0} :
                      $is_u_instr ? { $instr[31:12] , 12'b0} : 
                      $is_j_instr ? { {12{$instr[31]}} , $instr[19:12] , $instr[20] , $instr[30:21] , 1'b0} : 32'b0;
         
         $rs2_valid = $is_r_instr || $is_s_instr || $is_b_instr;
         $rs1_valid = $is_r_instr || $is_s_instr || $is_b_instr || $is_i_instr;
         $rd_valid = $is_r_instr || $is_i_instr || $is_u_instr || $is_j_instr;
         $funct3_valid = $is_r_instr || $is_s_instr || $is_b_instr || $is_i_instr;
         $funct7_valid = $is_r_instr;
         
         ?$rs2_valid
            $rs2[4:0] = $instr[24:20];
         ?$rs1_valid
            $rs1[4:0] = $instr[19:15];
         ?$rd_valid
            $rd[4:0] = $instr[11:7];
         ?$funct3_valid
            $funct3[2:0] = $instr[14:12];
         ?$funct7_valid
            $funct7[6:0] = $instr[31:25];
            
         $opcode[6:0] = $instr[6:0];
         
         $dec_bits[10:0] = {$funct7[5],$funct3,$opcode};
         
         // Branch Instruction
         $is_beq = $dec_bits[9:0] == 10'b000_1100011;
         $is_bne = $dec_bits[9:0] == 10'b001_1100011;
         $is_blt = $dec_bits[9:0] == 10'b100_1100011;
         $is_bge = $dec_bits[9:0] == 10'b101_1100011;
         $is_bltu = $dec_bits[9:0] == 10'b110_1100011;
         $is_bgeu = $dec_bits[9:0] == 10'b111_1100011;
         
         // Arithmetic Instruction
         $is_add = $dec_bits == 11'b0_000_0110011;
         $is_addi = $dec_bits[9:0] == 10'b000_0010011;
         $is_or = $dec_bits == 11'b0_110_0110011;
         $is_ori = $dec_bits[9:0] == 10'b110_0010011;
         $is_xor = $dec_bits == 11'b0_100_0110011;
         $is_xori = $dec_bits[9:0] == 10'b100_0010011;
         $is_and = $dec_bits == 11'b0_111_0110011;
         $is_andi = $dec_bits[9:0] == 10'b111_0010011;
         $is_sub = $dec_bits == 11'b1_000_0110011;
         $is_slti = $dec_bits[9:0] == 10'b010_0010011;
         $is_sltiu = $dec_bits[9:0] == 10'b011_0010011;
         $is_slli = $dec_bits == 11'b0_001_0010011;
         $is_srli = $dec_bits == 11'b0_101_0010011;
         $is_srai = $dec_bits == 11'b1_101_0010011;
         $is_sll = $dec_bits == 11'b0_001_0110011;
         $is_slt = $dec_bits == 11'b0_010_0110011;
         $is_sltu = $dec_bits == 11'b0_011_0110011;
         $is_srl = $dec_bits == 11'b0_101_0110011;
         $is_sra = $dec_bits == 11'b1_101_0110011;
         
         // Load Instruction
         $is_load = $dec_bits[6:0] == 7'b0000011;
         
         // Store Instruction
         $is_sb = $dec_bits[9:0] == 10'b000_0100011;
         $is_sh = $dec_bits[9:0] == 10'b001_0100011;
         $is_sw = $dec_bits[9:0] == 10'b010_0100011;
         
         // Jump Instruction
         $is_lui = $dec_bits[6:0] == 7'b0110111;
         $is_auipc = $dec_bits[6:0] == 7'b0010111;
         $is_jal = $dec_bits[6:0] == 7'b1101111;
         $is_jalr = $dec_bits[9:0] == 10'b000_1100111;
         
         $is_jump = $is_jal || $is_jalr;
         
      @2   
      // Register File Read
         $rf_rd_en1 = $rs1_valid;
         ?$rf_rd_en1
            $rf_rd_index1[4:0] = $rs1[4:0];
         
         $rf_rd_en2 = $rs2_valid;
         ?$rf_rd_en2
            $rf_rd_index2[4:0] = $rs2[4:0];
            
      // Branch Target PC       
         $br_tgt_pc[31:0] = $pc + $imm;
      
      // Jump Target PC
         $jalr_tgt_pc[31:0] = $src1_value + $imm;
         
      // Input signals to ALU
         $src1_value[31:0] = ((>>1$rd == $rs1) && >>1$rf_wr_en) ? >>1$result : $rf_rd_data1[31:0];
         $src2_value[31:0] = ((>>1$rd == $rs2) && >>1$rf_wr_en) ? >>1$result : $rf_rd_data2[31:0];
         
      @3   
         
      // ALU
         $sltu_result = $src1_value < $src2_value ;
         $sltiu_result = $src1_value < $imm ;
         
         $result[31:0] = $is_addi ? $src1_value + $imm :
                         $is_add ? $src1_value + $src2_value : 
                         $is_or ? $src1_value | $src2_value : 
                         $is_ori ? $src1_value | $imm :
                         $is_xor ? $src1_value ^ $src2_value :
                         $is_xori ? $src1_value ^ $imm :
                         $is_and ? $src1_value & $src2_value :
                         $is_andi ? $src1_value & $imm :
                         $is_sub ? $src1_value - $src2_value :
                         $is_slti ? (($src1_value[31] == $imm[31]) ? $sltiu_result : {31'b0,$src1_value[31]}) :
                         $is_sltiu ? $sltiu_result :
                         $is_slli ? $src1_value << $imm[5:0] :
                         $is_srli ? $src1_value >> $imm[5:0] :
                         $is_srai ? ({{32{$src1_value[31]}}, $src1_value} >> $imm[4:0]) :
                         $is_sll ? $src1_value << $src2_value[4:0] :
                         $is_slt ? (($src1_value[31] == $src2_value[31]) ? $sltu_result : {31'b0,$src1_value[31]}) :
                         $is_sltu ? $sltu_result :
                         $is_srl ? $src1_value >> $src2_value[5:0] :
                         $is_sra ? ({{32{$src1_value[31]}}, $src1_value} >> $src2_value[4:0]) :
                         $is_lui ? ({$imm[31:12], 12'b0}) :
                         $is_auipc ? $pc + $imm :
                         $is_jal ? $pc + 4 :
                         $is_jalr ? $pc + 4 : 
                         ($is_load || $is_s_instr) ? $src1_value + $imm : 32'bx;
                         
      // Register File Write
         $rf_wr_en = ($rd_valid && $valid && $rd != 5'b0) || >>2$valid_load;
         ?$rf_wr_en
            $rf_wr_index[4:0] = !$valid ? >>2$rd[4:0] : $rd[4:0];
      
         $rf_wr_data[31:0] = !$valid ? >>2$ld_data[31:0] : $result[31:0];
      
      // Branch
         $taken_br = $is_beq ? ($src1_value == $src2_value) :
                     $is_bne ? ($src1_value != $src2_value) :
                     $is_blt ? (($src1_value < $src2_value) ^ ($src1_value[31] != $src2_value[31])) :
                     $is_bge ? (($src1_value >= $src2_value) ^ ($src1_value[31] != $src2_value[31])) :
                     $is_bltu ? ($src1_value < $src2_value) :
                     $is_bgeu ? ($src1_value >= $src2_value) : 1'b0;
                     
         $valid_taken_br = $valid && $taken_br;
         
      // Load
         $valid_load = $valid && $is_load;
         $valid = !(>>1$valid_taken_br || >>2$valid_taken_br || >>1$valid_load || >>2$valid_load || >>1$valid_jump || >>2$valid_jump);
      
      // Jump
         $valid_jump = $valid && $is_jump;
                  
      @4
         $dmem_rd_en = $valid_load;
         $dmem_wr_en = $valid && $is_s_instr;
         $dmem_addr[3:0] = $result[5:2];
         $dmem_wr_data[31:0] = $src2_value[31:0];
         
      @5   
         $ld_data[31:0] = $dmem_rd_data[31:0];
         
      // Note: Because of the magic we are using for visualisation, if visualisation is enabled below,
      //       be sure to avoid having unassigned signals (which you might be using for random inputs)
      //       other than those specifically expected in the labs. You'll get strange errors for these.

         `BOGUS_USE($is_beq $is_bne $is_blt $is_bge $is_bltu $is_bgeu)
         `BOGUS_USE($is_sb $is_sh $is_sw)
   // Assert these to end simulation (before Makerchip cycle limit).
   \SV_plus
      always @ (posedge CLK) begin
         *OUT = |cpu/xreg[14]>>5$value;                
      end
   
   // Macro instantiations for:
   //  o instruction memory
   //  o register file
   //  o data memory
   //  o CPU visualization
   |cpu
      m4+imem(@1)    // Args: (read stage)
      m4+rf(@2, @3)  // Args: (read stage, write stage) - if equal, no register bypass is required
      m4+dmem(@4)    // Args: (read/write stage)

     
\SV
   
   endmodule

  ```


  ```bash
  sandpiper-saas -i ./src/module/rvmyth.tlv -o rvmyth.v --bestsv --noline -p verilog --outdir ./src/module/
  ```
  ![5](https://github.com/user-attachments/assets/c5406c82-0e84-4eba-8556-e36ba4df069a)
  Compile and simulate RISC-V design

  ```bash
  iverilog -o output/pre_synth_sim.out -DPRE_SYNTH_SIM src/module/testbench.v -I src/include -I src/module
  ```

  ![6](https://github.com/user-attachments/assets/10437399-d53c-4da3-82d2-0518e92b4c60)

  To view output, 
  ```bash
    gtkwave pre_synth_sim.vcd
  ```

 ## Gtkwave Waveforms
 The clk, output, instruction and the reset signal all in one waveform.
![Screenshot 2024-08-27 003255](https://github.com/user-attachments/assets/0cd63fbf-c326-4b50-ba0e-dcb21be254df)

## MakerChip Waveforms
1) Clock signal
![Screenshot 2024-08-27 005255](https://github.com/user-attachments/assets/33f1f3ce-aae9-4502-a994-ea211c84ecf1)

2) Reset Signal
![Screenshot 2024-08-27 005328](https://github.com/user-attachments/assets/833891b5-7c86-43c7-a66a-e281b04b5e1a)

3) Register Value
![image](https://github.com/user-attachments/assets/c4ae03b4-4bc6-43f1-bf03-0391978e312a)

Thus from he waveforms, we can concule both give same result od 2d. 
</details>

<details>
<summary>Task 7 : Generate PLL and DAC output waveforms</summary>
  
## Verilog and GTKWave Installation

![Verilog and GTKWave](https://github.com/user-attachments/assets/9736822e-656c-40f9-84ae-c404253b0d7d)

## Yosys Installation

![Yosys Installation](https://github.com/user-attachments/assets/a1b1f155-a195-4192-b649-55cd5f723cc5)

## Clone the BabySoC_Simulation Repository

To set up your simulation environment, clone the BabySoC_Simulation repository:

```bash
git clone https://github.com/Subhasis-Sahu/BabySoC_Simulation.git
```

# Simulation Setup

## 1. Edit the Top-Level Code

Edit the top-level code as shown in the image below:

![Top-Level Code](https://github.com/user-attachments/assets/79a40bf9-1604-4fe6-b276-00f50626a003)

## 2. Run Simulation Commands

Navigate to the `BabySoC_Simulation` directory and execute the following commands in your terminal:

```bash
cd BabySoC_Simulation
iverilog -o ./pre_synth_sim.out -DPRE_SYNTH_SIM src/module/testbench.v -I src/include -I src/module/
./pre_synth_sim.out
gtkwave pre_synth_sim.vcd
```

## Output Waveforms

The following images show the output waveforms obtained from the simulation:

### Sum Calculation

![Output Waveform 1](https://github.com/user-attachments/assets/c5252c58-ce26-47a7-8008-0f06cbcb0444)

*The sum from 1 to 9 is calculated gradually over each clock cycle.*

### PLL waveform

![Output Waveform 2](https://github.com/user-attachments/assets/b4bc7398-8028-4456-9d58-699ef48b52cd)

*The out signal which is set as analog exhibits a periodic movement.*


</details>

<details>
<summary>Task 8 : RTL Design and Implementation with Sky130 PDK using Verilog</summary>

<details>
<summary>Software Installation</summary>

## 1. Yosys

Yosys is a versatile synthesis tool that can be customized for various synthesis tasks by combining its existing algorithms, called "passes," through synthesis scripts. Additional passes can also be added by modifying the Yosys C++ code base. It serves as a backend for several formal verification tools, such as sby, which utilizes SMT solvers for formal property checking, and mcy, which assesses testbench quality using mutation coverage metrics. Yosys is open-source software licensed under the ISC license, which is compatible with GPL and is similar to the MIT or 2-clause BSD licenses.

```bash
git clone https://github.com/YosysHQ/yosys.git
cd yosys 
sudo apt install make
sudo apt-get install build-essential clang bison flex \
libreadline-dev gawk tcl-dev libffi-dev git \
graphviz xdot pkg-config python3 libboost-system-dev \
libboost-python-dev libboost-filesystem-dev zlib1g-dev
make config-gcc
make 
sudo make install
```

![Screenshot from 2024-10-20 19-26-22](https://github.com/user-attachments/assets/68da94c5-3b61-47a8-be2e-32b717cb87fc)
![image](https://github.com/user-attachments/assets/91381644-a88d-492c-b69c-899eea5d4edb)


## 2. Icarus Verilog

Icarus Verilog is a free, open-source Verilog compiler that generates netlists in formats like EDIF. It supports Verilog standards from 1995, 2001, and 2005, as well as parts of SystemVerilog and some extensions. Released under the GNU General Public License, it includes a Verilog compiler with a preprocessor, supports plug-in backends, and features a virtual machine for simulating designs.

```bash
sudo apt-get install iverilog
```
![Screenshot from 2024-10-20 19-29-30](https://github.com/user-attachments/assets/12d4715b-acc5-4e3d-a110-0b22922684d4)


## 3. GTKWave

GTKWave is a comprehensive waveform viewer built with GTK+ for Unix and Win32 systems. It supports various file formats such as LXT, LXT2, VZT, FST, GHW, as well as standard Verilog VCD/EVCD files, enabling users to view and analyze waveforms with ease.

```bash
sudo apt install gtkwave
```
![image](https://github.com/user-attachments/assets/ae0deca6-e58c-4dd2-94f8-8506082ad239)

## 4. NGspice

ngspice is an open-source SPICE simulator used for simulating electronic circuits, including analog, digital, and mixed-signal designs. It supports a variety of devices such as:

- JFETs
- MOSFETs
- Bipolar transistors
- Resistors
- Capacitors
- Inductors
- Diodes
- Transmission lines

These devices are interconnected through a **netlist**, allowing users to simulate their circuits and obtain outputs such as voltage, current, and other electrical parameters in the form of graphs or data files for further analysis.

### Installation

1. Visit [ngspice on SourceForge](https://sourceforge.net/projects/ngspice/).
2. Download the latest tarball (.tar.gz file) to your local directory.

### Dependencies

```bash
sudo apt-get install build-essential
sudo apt-get install libxaw7-dev
```

```bash
# Dependency for ngspice:
sudo apt-get install build-essential
sudo apt-get install libxaw7-dev

# ngspice installation:
tar -zxvf ngspice-40.tar.gz
cd ngspice-40
mkdir release
cd release
../configure  --with-x --with-readline=yes --disable-debug
make
sudo make install
```

## 5. MAGIC

MAGIC is a layout tool used for creating, editing, and verifying IC (Integrated Circuit) layouts. As an open-source tool, MAGIC is accessible to students, researchers, and professionals involved in VLSI circuit design and simulation. The software enables users to visually create geometric patterns representing various components like transistors and wires in a chip.

```bash
sudo apt-get install m4
sudo apt-get install tcsh
sudo apt-get install csh
sudo apt-get install libx11-dev
sudo apt-get install tcl-dev tk-dev
sudo apt-get install libcairo2-dev
sudo apt-get install mesa-common-dev libglu1-mesa-dev
sudo apt-get install libncurses-dev
git clone https://github.com/RTimothyEdwards/magic
cd magic
./configure
make
sudo make install
```

![Screenshot from 2024-10-20 19-43-43](https://github.com/user-attachments/assets/393c80f4-3652-4cdf-a15a-a9d21dc741cd)

## 6. OpenLane

OpenLane is an open-source, automated RTL-to-GDSII flow built on top of several open-source EDA tools. Developed as part of the Google SkyWater PDK (Process Design Kit) initiative, OpenLane offers a fully open-source process for chip design and fabrication. Its goal is to provide a flow that takes in RTL code (describing circuit functionality) and outputs final GDSII files required for chip fabrication.

```bash
sudo apt-get update
sudo apt-get upgrade
sudo apt install -y build-essential python3 python3-venv python3-pip make git
```

### Docker Installation :
```bash
sudo apt install apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io
sudo docker run hello-world

sudo groupadd docker
sudo usermod -aG docker $USER
sudo reboot 


# Check for installation
sudo docker run hello-world
```
![image](https://github.com/user-attachments/assets/a8ae95e5-79cf-4748-8db5-e884494385c4)


### OpenLane Installation :
```bash
cd $HOME
git clone https://github.com/The-OpenROAD-Project/OpenLane
cd OpenLane
make
make test
```
</details>

<details>
<summary>Day 1: Introduction to Verilog RTL Design and Synthesis</summary>

A simulator helps ensure that a hardware design meets its specifications by simulating circuit behavior. It monitors input signals and re-evaluates outputs when inputs change, identifying errors early on. In RTL design, Verilog code models a circuit's functionality, and a testbench is written to simulate the design. Using Icarus Verilog, the testbench is simulated, and a Value Change Dump (VCD) file is generated.

The VCD file captures signal transitions and is viewed using GTKWave, which allows designers to inspect waveforms, timing relationships, and signal interactions. This process helps verify and debug the design before moving to synthesis or physical implementation. The typical Icarus Verilog-based flow involves writing RTL code, running simulations, generating VCD files, and analyzing them with GTKWave to ensure correct circuit behavior.


![image](https://github.com/user-attachments/assets/0084288d-b8d9-4fa3-ba8c-8712738a4cd1)

Start by executing the following commands:

```bash
mkdir ASIC
cd ASIC
git clone https://github.com/kunalg123/vsdflow.git
git clone https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git
cd sky130RTLDesignAndSynthesisWorkshop
ls -R
```

![image](https://github.com/user-attachments/assets/8fde7f46-f5e9-4e84-bdc0-12e76220c8ff)

THe following verilog files will be used in the lab:

![image](https://github.com/user-attachments/assets/d6c3d7c6-ac86-491a-925b-0d0aef1f2a45)


Several Verilog design and testbench files are available for simulation. To simulate the Verilog code in 'good_mux.v', follow these steps. First, compile the design and testbench by running the given command. This checks for syntax errors, and if successful, generates an executable file named 'a.out'. Running 'a.out' will create a VCD file, which logs changes in input and output values during the simulation. Finally, use GTKWave to view and analyze the waveform data captured in the VCD file.

```bash
iverilog good_mux.v tb_good_mux.v
./a.out
gtkwave tb_good_mux.vcd
```

![image](https://github.com/user-attachments/assets/67530f41-a815-4d8d-ae99-aea5246d284b)

![image](https://github.com/user-attachments/assets/3bf58e6d-a0b5-4654-b167-a8eb52408563)


## good_mux.v Code

```verilog
module good_mux (input i0 , input i1 , input sel , output reg y);
  always @ (*)
    begin
      if(sel)
      y <= i1;
    else 
      y <= i0;
  end
endmodule
```

## tb_good_mux.v Code
```verilog
`timescale 1ns / 1ps
module tb_good_mux;
  // Inputs
  reg i0,i1,sel;
  // Outputs
  wire y;
  
    // Instantiate the Unit Under Test (UUT)
  good_mux uut (
    .sel(sel),
    .i0(i0),
    .i1(i1),
    .y(y)
  );
  
  initial begin
  $dumpfile("tb_good_mux.vcd");
  $dumpvars(0,tb_good_mux);
  // Initialize Inputs
  sel = 0;
  i0 = 0;
  i1 = 0;
  #300 $finish;
  end
  
  always #75 sel = ~sel;
  always #10 i0 = ~i0;
  always #55 i1 = ~i1;
endmodule
```

A synthesizer is a tool that converts RTL code into a netlist. One example of an open-source synthesizer is Yosys. Yosys not only optimizes the design but also maps it to specific technology libraries or FPGA architectures, producing an optimized netlist that can be analyzed and prepared for physical layout and fabrication.

### Yosys Flow
![image](https://github.com/user-attachments/assets/595cea2e-7620-48e6-aeff-f637e68b94b1)
![image](https://github.com/user-attachments/assets/01d1c8af-19c0-4a5d-8fb9-d73016ec3a9d)
![image](https://github.com/user-attachments/assets/f03056ed-1ba3-454f-ac83-bbd30475c4a1)
![image](https://github.com/user-attachments/assets/350088b2-293a-48c9-bcc3-edbba170faaa)
![image](https://github.com/user-attachments/assets/385a8f43-857c-43f9-ba3b-d6627b2e1e9b)

### Synthesis Overview

Synthesis is a crucial process in digital circuit design that consists of three main steps:

1. **RTL to Gate-Level Translation**: The design described in Register Transfer Level (RTL) is transformed into a gate-level representation. 

2. **Gate Implementation**: The design is converted into specific logic gates, with the necessary connections established between them.

3. **Netlist Generation**: The output of the synthesis process is a file known as the netlist, which details the interconnections of the gates.

### Liberty File (.lib)

A Liberty file is a collection of logical modules that includes various logic gates, such as AND, OR, and NOT. This file contains multiple variants of each gate, differentiated by characteristics like:

- **Number of Inputs**: e.g., 2-input, 3-input, 4-input gates.
- **Speed Variants**: Gates classified as slow, medium, or fast.

The choice of gate speed impacts design performance:

- **Fast Cells**: These are utilized when high performance is essential, but their selection may lead to increased area and power consumption, as well as potential hold time violations.
- **Slow Cells**: While they help mitigate hold time issues, relying too much on slower cells can result in suboptimal circuit performance.

### Optimal Cell Selection

The selection of cells during synthesis must strike a balance among area, power, and timing constraints. Effective synthesis requires careful consideration of these factors to ensure optimal circuit performance.


## Code for synthesis:

```bash
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog good_mux.v
synth -top good_mux
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog good_mux_netlist.v
write_verilog -noattr good_mux_netlist.v
```

![image](https://github.com/user-attachments/assets/f9a33419-d9b0-4a98-ab5a-c87f0dd2998c)
![image](https://github.com/user-attachments/assets/2aef21ea-65c1-4c9a-90fd-dfc42dc7d21d)
![image](https://github.com/user-attachments/assets/afdce8c9-b526-4af9-aa6a-414597d7160e)

</details>

<details>
<summary>Day 2: Timing libs, hierarchical vs flat synthesis and efficient flop coding styles</summary>

## Overview of the .lib file
Execute the following commands to check the contents of the .lib file:
```bash
cd ASIC/sky130RTLDesignAndSynthesisWorkshop/lib/
vim sky130_fd_sc_hd__tt_025C_1v80.lib
```

![image](https://github.com/user-attachments/assets/9acc2dd1-99ba-4b68-aca0-a8a8786e6c0e)

Liberty (.lib) files store crucial PVT (Process, Voltage, Temperature) parameters that impact circuit performance significantly. 

1. **Impact of PVT Variations**:
   - **Process Variations**: Manufacturing inconsistencies can cause changes in transistor characteristics, affecting timing and power.
   - **Voltage Fluctuations**: Variations in supply voltage influence transistor speed and power consumption, critical for high-performance designs.
   - **Temperature Changes**: Temperature shifts can alter charge carrier mobility, impacting performance and increasing leakage currents.

2. **Multiple Versions of Cells**: 
   - Liberty files often provide different versions of the same cell, like an AND gate, optimized for speed, area, or power. This allows designers to select the best configuration for specific circuit requirements, enhancing overall design efficiency.


When evaluating different versions of the same cell, such as an AND gate, we can observe distinct trade-offs in performance and resource utilization. 

1. **and2_0**: This version occupies the least area, resulting in more delay and lower power consumption. It is ideal for compact designs where minimizing space is critical.

2. **and2_1**: This configuration takes up more area but offers reduced delay and higher power consumption. It's suitable for applications where speed is prioritized over space.

3. **and2_2**: This variant has the largest area, resulting in increased delay and the highest power usage. It may be selected for applications that demand maximum performance, such as high-speed circuits or systems that can accommodate larger footprints.

These different versions highlight the importance of balancing area, speed, and power in circuit design. Selecting the appropriate version depends on the specific requirements of the application, including size constraints, performance needs, and power efficiency goals.

![image](https://github.com/user-attachments/assets/d0c1e0a3-1b19-473f-be6e-43bdd3067a77)


### Hierarchical vs. Flat Synthesis

**Hierarchical Synthesis** breaks down a complex design into smaller sub-modules, synthesizing each separately to generate gate-level netlists. This approach enhances organization, promotes module reuse, and allows for incremental design changes without affecting the entire system. Hierarchical synthesis also facilitates easier debugging and testing, as individual modules can be verified independently before integration.

**Flat Synthesis**, in contrast, treats the entire design as a single entity, synthesizing it into one monolithic netlist without regard for its hierarchical structure. While flat synthesis can optimize specific designs for performance or area, it poses significant challenges in maintenance and modification. The lack of modularity can make it difficult to analyze and debug, as changes in one part of the design may impact others unexpectedly.

For instance, consider the Verilog file `multiple_modules.v` located in the `verilog_files` directory. If this file contains several interconnected modules, using hierarchical synthesis would allow each module to be developed and tested independently, streamlining the overall design process. In contrast, flat synthesis would generate a single netlist encompassing all modules, potentially complicating future modifications and testing efforts. 

Ultimately, the choice between hierarchical and flat synthesis depends on the specific needs of the design project, including complexity, required optimization, and team workflow preferences.


```verilog
module sub_module2 (input a, input b, output y);
    assign y = a | b;
endmodule

module sub_module1 (input a, input b, output y);
    assign y = a&b;
endmodule


module multiple_modules (input a, input b, input c , output y);
    wire net1;
    sub_module1 u1(.a(a),.b(b),.y(net1));  //net1 = a&b
    sub_module2 u2(.a(net1),.b(c),.y(y));  //y = net1|c ,ie y = a&b + c;
endmodule
```


To conduct hierarchical synthesis on the `multiple_modules.v` file, execute the following commands:

```bash
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog multiple_modules.v
synth -top multiple_modules
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show multiple_modules
write_verilog -noattr multiple_modules_hier.v
```
### Statistics
![image](https://github.com/user-attachments/assets/3d4d56ff-248d-41e9-a625-d7e7a42321f6)

### Schematics
![image](https://github.com/user-attachments/assets/4a9f3868-07c3-47c2-8219-d29226532aef)

```verilog
/* Generated by Yosys 0.44+60 (git sha1 c25448f1d, g++ 11.4.0-1ubuntu1~22.04 -fPIC -O3) */

module multiple_modules(a, b, c, y);
  input a;
  wire a;
  input b;
  wire b;
  input c;
  wire c;
  wire net1;
  output y;
  wire y;
  sub_module1 u1 (
    .a(a),
    .b(b),
    .y(net1)
  );
  sub_module2 u2 (
    .a(net1),
    .b(c),
    .y(y)
  );
endmodule

module sub_module1(a, b, y);
  wire _0_;
  wire _1_;
  wire _2_;
  input a;
  wire a;
  input b;
  wire b;
  output y;
  wire y;
  sky130_fd_sc_hd__and2_0 _3_ (
    .A(_1_),
    .B(_0_),
    .X(_2_)
  );
  assign _1_ = b;
  assign _0_ = a;
  assign y = _2_;
endmodule

module sub_module2(a, b, y);
  wire _0_;
  wire _1_;
  wire _2_;
  input a;
  wire a;
  input b;
  wire b;
  output y;
  wire y;
  sky130_fd_sc_hd__or2_0 _3_ (
    .A(_1_),
    .B(_0_),
    .X(_2_)
  );
  assign _1_ = b;
  assign _0_ = a;
  assign y = _2_;
endmodule
```

To conduct flat synthesis on the `multiple_modules.v` file, execute the following commands:

```bash
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog multiple_modules.v
synth -top multiple_modules
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
flatten
show
write_verilog -noattr multiple_modules_flat.v
```

### Statistics
![image](https://github.com/user-attachments/assets/abfd7f57-2939-4677-885f-e811d8257ff2)

### Schematics
![image](https://github.com/user-attachments/assets/d7b54310-82d3-4590-8887-13d4df8f8246)

```verilog
  /* Generated by Yosys 0.44+60 (git sha1 c25448f1d, g++ 11.4.0-1ubuntu1~22.04 -fPIC -O3) */

module multiple_modules(a, b, c, y);
  wire _0_;
  wire _1_;
  wire _2_;
  wire _3_;
  wire _4_;
  wire _5_;
  input a;
  wire a;
  input b;
  wire b;
  input c;
  wire c;
  wire net1;
  wire \u1.a ;
  wire \u1.b ;
  wire \u1.y ;
  wire \u2.a ;
  wire \u2.b ;
  wire \u2.y ;
  output y;
  wire y;
  sky130_fd_sc_hd__and2_0 _6_ (
    .A(_1_),
    .B(_0_),
    .X(_2_)
  );
  sky130_fd_sc_hd__or2_0 _7_ (
    .A(_4_),
    .B(_3_),
    .X(_5_)
  );
  assign _4_ = \u2.b ;
  assign _3_ = \u2.a ;
  assign \u2.y  = _5_;
  assign \u2.a  = net1;
  assign \u2.b  = c;
  assign y = \u2.y ;
  assign _1_ = \u1.b ;
  assign _0_ = \u1.a ;
  assign \u1.y  = _2_;
  assign \u1.a  = a;
  assign \u1.b  = b;
  assign net1 = \u1.y ;
endmodule
```
Sub module synthesis can beperformed by typing the below commands:

```verilog
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
read_verilog multiple_modules.v 
synth -top sub_module
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
show
```

![image](https://github.com/user-attachments/assets/79972ea5-ec94-4728-bd7d-71509421549a)

```verilog
module sub_module1(a, b, y);
  input a;
  input b;
  output y;

  wire _0_;
  wire _1_;
  wire _2_;

  assign _1_ = b;
  assign _0_ = a;

  sky130_fd_sc_hd__and2_0 _3_ (
    .A(_1_),
    .B(_0_),
    .X(_2_)
  );

  assign y = _2_;

endmodule
```

# Flip-Flop Design and Optimization Techniques

Flip-flops are integral components in sequential logic circuits, enabling the storage of intermediate values and preventing glitches in digital systems. They play a crucial role in ensuring that inputs to combinational circuits remain stable until the clock edge, thereby facilitating reliable operation.

## Asynchronous Reset Flip-Flop

### Description
The asynchronous reset flip-flop allows immediate resetting of its output (`q`) when the reset signal is asserted, irrespective of the clock signal state. This characteristic is beneficial when quick initialization of the flip-flop state is necessary, providing a swift response to changes in system conditions.

### Verilog Code Implementation
The following Verilog code implements an asynchronous reset flip-flop:
```verilog
module dff_asyncres (
    input clk,
    input async_reset,
    input d,
    output reg q
);
always @(posedge clk or posedge async_reset) begin
    if (async_reset)
        q <= 1'b0;  // Reset output to 0 when async_reset is high
    else
        q <= d;     // On clock edge, capture the input data
end
endmodule
```

### Simulation Instructions
To simulate the functionality of the asynchronous reset flip-flop, use the following commands:
```bash
iverilog dff_asyncres.v tb_dff_asyncres.v
./a.out
gtkwave tb_dff_asyncres.vcd
```

### Simulation Results
The simulation generates a waveform that illustrates the behavior of the flip-flop:
![Screenshot from 2024-10-21 00-07-42](https://github.com/user-attachments/assets/54ab667a-abd0-47a0-a543-cc0d738b9c60)

### Netlist Generation
To create the netlist for synthesis, execute the following commands:
```bash
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_asyncres.v
synth -top dff_asyncres
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```

### Synthesized Netlist
The synthesized netlist can be visualized as follows:
![image](https://github.com/user-attachments/assets/2851a32f-e115-4c7a-8f05-eb89e54248ed)


---

## Synchronous Reset Flip-Flop

### Description
The synchronous reset flip-flop updates its output only on the rising edge of the clock. In this design, the reset action occurs in synchronization with the clock signal, ensuring predictable behavior during clock cycles.

### Verilog Code Implementation
The Verilog code for a synchronous reset flip-flop is as follows:
```verilog
module dff_syncres (
    input clk,
    input sync_reset,
    input d,
    output reg q
);
always @(posedge clk) begin
    if (sync_reset)
        q <= 1'b0;  // Reset output to 0 during the clock edge if sync_reset is high
    else
        q <= d;     // Capture the input data on the clock edge
end
endmodule
```

### Simulation Instructions
To simulate this synchronous reset flip-flop, run the following commands:
```bash
iverilog dff_syncres.v tb_dff_syncres.v
./a.out
gtkwave tb_dff_syncres.vcd
```

### Simulation Results
The resulting waveform from this simulation can be observed here:
![image](https://github.com/user-attachments/assets/49e9301e-ff2b-4bd1-858c-d13c289d8042)

### Netlist Generation
Generate the netlist using the following commands:
```bash
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_syncres.v
synth -top dff_syncres
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```

### Synthesized Netlist
The synthesized netlist is displayed as follows:
![image](https://github.com/user-attachments/assets/7a216b58-174d-4b71-9f44-cac3cba848d7)

---

## Asynchronous Set Flip-Flop

### Description
An asynchronous set flip-flop immediately asserts its output (`q`) to a high state when the set signal is activated. This design allows for an instantaneous response to changes in the set signal, independent of the clock.

### Verilog Code Implementation
The following Verilog code demonstrates how to create an asynchronous set flip-flop:
```verilog
module dff_async_set (
    input clk,
    input async_set,
    input d,
    output reg q
);
always @(posedge clk or posedge async_set) begin
    if (async_set)
        q <= 1'b1;  // Set output to 1 when async_set is high
    else
        q <= d;     // Capture the input data on the clock edge
end
endmodule
```

### Simulation Instructions
To view the simulation results for this flip-flop, run:
```bash
iverilog dff_async_set.v tb_dff_async_set.v
./a.out
gtkwave tb_dff_async_set.vcd
```

### Simulation Results
The resulting waveform from the simulation is as follows:
![image](https://github.com/user-attachments/assets/2af8e247-07f1-4694-95a1-7d80f4c27513)

### Netlist Generation
Generate the netlist by executing the following commands:
```bash
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_async_set.v
synth -top dff_async_set
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```

### Synthesized Netlist
The synthesized netlist can be visualized as follows:
![image](https://github.com/user-attachments/assets/4a8d6032-29d3-4f36-ba3f-d1dd8a6a2857)

---

## Design Optimizations

### Example: Multiplication by 2
Consider a simple example where we want to multiply a 3-bit input by 2. The following Verilog code demonstrates this:
```verilog
module mul2 (
    input [2:0] a,
    output [3:0] y
);
assign y = a * 2;  // Multiply input by 2
endmodule
```

### Truth Table
The truth table for the multiplication by 2 operation is as follows:

| a2 | a1 | a0 | y3 | y2 | y1 | y0 |
|----|----|----|----|----|----|----|
| 0  | 0  | 0  | 0  | 0  | 0  | 0  |
| 0  | 0  | 1  | 0  | 0  | 1  | 0  |
| 0  | 1  | 0  | 0  | 1  | 0  | 0  |
| 0  | 1  | 1  | 0  | 1  | 1  | 0  |
| 1  | 0  | 0  | 1  | 0  | 0  | 0  |
| 1  | 0  | 1  | 1  | 0  | 1  | 0  |
| 1  | 1  | 0  | 1  | 1  | 0  | 0  |
| 1  | 1  | 1  | 1  | 1  | 1  | 0  |

### Optimization Insight
Multiplying a number by 2 does not require additional hardware; instead, we can achieve this by shifting the input bits left and appending a zero in the least significant bit (LSB) position. This optimization simplifies the design and reduces resource usage.

### Netlist Generation
To generate the netlist for this multiplication operation, execute the following commands:
```bash
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog mult_2.v
synth -top mul2
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog -noattr mult_2_net.v
```

### Optimization Statistics
The synthesis statistics highlight the efficient use of resources in implementing the multiplication operation:

![image](https://github.com/user-attachments/assets/d95bd309-a12a-4ea4-8882-d9ab5e06a981)

### Synthesized Netlist
The synthesized netlist representing the optimized design is as follows:

![image](https://github.com/user-attachments/assets/89a8698f-1bb3-40df-af03-d07b050546c5)

### Netlist Code
The corresponding netlist code can be inspected to further understand the synthesized design:

```verilog
/* Generated by Yosys 0.44+60 (git sha1 c25448f1d, g++ 11.4.0-1ubuntu1~22.04 -fPIC -O3) */

module mul2(a, y);
  input [2:0] a;
  wire [2:0] a;
  output [3:0] y;
  wire [3:0] y;
  assign y = { a, 1'h0 };
endmodule
```



### Multiplication by 9 Example

Consider the Verilog code for multiplying a 3-bit input number `a` by 9, shown in the following module named `mult8`:

```verilog
module mult8 (input [2:0] a, output [5:0] y);
	assign y = a * 9;
endmodule
```

In this design, the 3-bit input number \( a \) undergoes multiplication by 9. This operation can be mathematically restructured as:

\[
(a * 9) = (a * 8) + a
\]

Here, the term \( (a * 8) \) represents a left shift of the number \( a \) by three bits. If we denote \( a \) as \( a_2 a_1 a_0 \), the operation \( (a * 8) \) results in the binary representation \( a_2 a_1 a_0 0 0 0 \). Thus, combining both terms gives us:

\[
(a * 9) = (a * 8) + a = a_2 a_1 a_0 a_2 a_1 a_0
\]

This implies that the result can be expressed as \( aa \) in 6-bit format, indicating that no additional hardware realization is necessary for this operation.

### Synthesized Netlist
To synthesize the design and generate the netlist, run the following commands:

```bash
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog mult_8.v
synth -top mult8
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog -noattr mult_8_net.v
```

### Optimization Statistics
The synthesis statistics for the multiplication by 9 operation highlight the efficient use of resources in this design:

![image](https://github.com/user-attachments/assets/9afca8e6-fa69-4236-abec-c0d93458eed0)

### Synthesized Netlist
The synthesized netlist for this design is represented as follows:

![image](https://github.com/user-attachments/assets/9cada6dc-cc37-4cc3-8243-699181a26618)

### Netlist Code
The corresponding netlist code, which provides insights into the synthesized design, is as follows:

```verilog
/* Generated by Yosys 0.44+60 (git sha1 c25448f1d, g++ 11.4.0-1ubuntu1~22.04 -fPIC -O3) */

module mult8(a, y);
  input [2:0] a;
  wire [2:0] a;
  output [5:0] y;
  wire [5:0] y;
  assign y = { a, a };
endmodule
```

</details>


<details>
	<summary>Day 3 - Optimizing Combinational and Sequential Logic</summary>

In the realm of digital design, optimizations are classified into two primary categories: combinational optimizations and sequential optimizations. These techniques aim to enhance designs for improved efficiency regarding area, power consumption, and overall performance. Effective optimization not only reduces the resource requirements but also increases the reliability of the design by minimizing potential points of failure.

### Combinational Optimization Techniques

Combinational optimizations can be achieved through various methodologies, including:

- **Constant Propagation** (Direct Optimization)
- **Boolean Logic Optimization** (utilizing methods like Karnaugh Maps or the Quine-McCluskey algorithm)

#### Constant Propagation

Constant propagation is a powerful optimization technique where constant values are substituted directly into the equations governing the logic circuit. This simplifies the logic, reduces the number of required gates, and consequently lowers power consumption.

To illustrate constant propagation, consider the following circuit diagram:

![Screenshot from 2024-10-21 00-59-54](https://github.com/user-attachments/assets/638670f6-e56f-4f4a-8f86-444b81dd2aef)

In the above representation, the top circuit is composed of 6 transistors (3 NMOS and 3 PMOS). However, the lower circuit efficiently reduces this to just 2 transistors (1 NMOS and 1 PMOS) when the input \( A \) is set to zero, effectively transforming the configuration into an inverter. This results in a significant reduction in the area occupied by the circuit and improvements in power efficiency.

#### Boolean Logic Optimization

Boolean logic optimization aims to simplify the logic expressions that define a circuit's functionality. This can be achieved using various methods, including Karnaugh Maps and the Quine-McCluskey algorithm. These methods help identify and eliminate redundant logic, resulting in smaller and faster circuits.

Examine the following Verilog code snippet:

```verilog
assign y = a ? (b ? c : (c ? a : 0)) : (!c);
```

This code employs a ternary operator (`?:`), which synthesizes into a multiplexer as depicted below:

![Screenshot from 2024-10-21 01-00-15](https://github.com/user-attachments/assets/19fef501-d541-4f79-ac4b-6f030b34f7b6)

The initial synthesis results in a complex multiplexer configuration. Upon further optimization, the circuit can be simplified as follows:

![Screenshot from 2024-10-21 01-00-29](https://github.com/user-attachments/assets/ecb23768-7aa1-43fc-b76b-ca0d2122370c)

This simplification process reduces the overall gate count and improves performance by minimizing propagation delays.

### Example 1: Multiplexer to AND Gate Optimization

Consider the following Verilog code:

```verilog
module opt_check (input a, input b, output y);
	assign y = a ? b : 0;
endmodule
```

In this scenario, the code generates a multiplexer. Because one of the multiplexer’s inputs is permanently grounded, optimization will yield an AND gate instead. This transformation is a prime example of how design intent can be translated into hardware that is more efficient and cost-effective.

#### Netlist Generation

To produce the netlist, execute the following commands:

```bash
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog opt_check.v
synth -top opt_check
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog -noattr opt_check_net.v
```

### Visualization of Optimization

![image](https://github.com/user-attachments/assets/4c344281-c923-47a8-a858-7705ab0e5618)

```verilog
/* Generated by Yosys 0.44+60 (git sha1 c25448f1d, g++ 11.4.0-1ubuntu1~22.04 -fPIC -O3) */

module opt_check(a, b, y);
  wire _0_;
  wire _1_;
  wire _2_;
  input a;
  wire a;
  input b;
  wire b;
  output y;
  wire y;
  sky130_fd_sc_hd__and2_0 _3_ (
    .A(_1_),
    .B(_0_),
    .X(_2_)
  );
  assign _1_ = b;
  assign _0_ = a;
  assign y = _2_;
endmodule
```

### Example 2: OR Gate Implementation from Multiplexer

Consider this Verilog example:

```verilog
module opt_check2 (input a, input b, output y);
	assign y = a ? 1 : b;
endmodule
```

In this case, the multiplexer’s output is always connected to logic 1, resulting in an optimization that infers an OR gate. Notably, the OR gate is implemented using a NAND configuration since the NAND design employs stacked NMOS transistors, while a NOR gate would require stacked PMOS transistors. This design choice is often made to enhance performance, as NMOS transistors typically have lower resistance than PMOS counterparts.

#### Netlist Generation

Run the following commands to create the netlist:

```bash
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog opt_check2.v
synth -top opt_check2
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog -noattr opt_check2_net.v
```

### Visualization of Optimization

![image](https://github.com/user-attachments/assets/dea1c341-25dc-4ce3-ae9c-9a8385377e55)

```verilog
/* Generated by Yosys 0.44+60 (git sha1 c25448f1d, g++ 11.4.0-1ubuntu1~22.04 -fPIC -O3) */

module opt_check2(a, b, y);
  wire _0_;
  wire _1_;
  wire _2_;
  input a;
  wire a;
  input b;
  wire b;
  output y;
  wire y;
  sky130_fd_sc_hd__or2_0 _3_ (
    .A(_0_),
    .B(_1_),
    .X(_2_)
  );
  assign _0_ = a;
  assign _1_ = b;
  assign y = _2_;
endmodule
```

### Example 3: Optimizing a 3-Input AND Gate

In this example, we aim to optimize a design that inherently performs the function of a 3-input AND gate:

**Verilog Code:**

```verilog
module opt_check3 (input a , input b, input c , output y);
	assign y = a ? (c ? b : 0) : 0;
endmodule
```

**Explanation:**  
Initially, this code infers a multiplexer with multiple conditional checks. However, after optimization, the design simplifies to a 3-input AND gate, effectively reducing gate count and circuit complexity.

**Steps to Generate Netlist:**

```bash
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog opt_check3.v
synth -top opt_check3
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog -noattr opt_check3_net.v
```

**Visualization After Optimization:**

![image](https://github.com/user-attachments/assets/0174f865-4d5e-45e2-8d23-3d8b552dd2b0)

```verilog
/* Generated by Yosys 0.44+60 (git sha1 c25448f1d, g++ 11.4.0-1ubuntu1~22.04 -fPIC -O3) */

module opt_check3(a, b, c, y);
  wire _0_;
  wire _1_;
  wire _2_;
  wire _3_;
  wire _4_;
  input a;
  wire a;
  input b;
  wire b;
  input c;
  wire c;
  output y;
  wire y;
  sky130_fd_sc_hd__and3_1 _5_ (
    .A(_2_),
    .B(_3_),
    .C(_1_),
    .X(_4_)
  );
  assign _2_ = b;
  assign _3_ = c;
  assign _1_ = a;
  assign y = _4_;
endmodule
```

---

### Example 4: Transforming to a 2-Input XNOR Gate

In this example, we optimize a design that can be simplified to a 2-input XNOR gate.

**Verilog Code:**

```verilog
module opt_check4 (input a , input b, input c , output y);
	assign y = a ? (c ? b : 0) : 0;
endmodule
```

**Explanation:**  
Although the original design appears complex, the optimization process reveals that the design functions equivalently to a 2-input XNOR gate. This transformation leads to a significant reduction in the transistor count and improves performance.

**Steps to Generate Netlist:**

```bash
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog opt_check4.v
synth -top opt_check4
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog -noattr opt_check4_net.v
```

**Visualization After Optimization:**

![image](https://github.com/user-attachments/assets/928260b9-2508-4714-b382-41d03d389ead)

```verilog
/* Generated by Yosys 0.44+60 (git sha1 c25448f1d, g++ 11.4.0-1ubuntu1~22.04 -fPIC -O3) */

module opt_check4(a, b, c, y);
  wire _0_;
  wire _1_;
  wire _2_;
  wire _3_;
  wire _4_;
  wire _5_;
  wire _6_;
  input a;
  wire a;
  input b;
  wire b;
  input c;
  wire c;
  output y;
  wire y;
  sky130_fd_sc_hd__xnor2_1 _7_ (
    .A(_5_),
    .B(_3_),
    .Y(_6_)
  );
  assign _5_ = c;
  assign _3_ = a;
  assign _4_ = b;
  assign y = _6_;
endmodule
```

---

### Example 5: Optimization of a Complex Multi-Module Design

This example illustrates the optimization of a design with multiple interconnected modules.

**Verilog Code:**

```verilog
module sub_module1(input a , input b , output y);
	assign y = a & b;
endmodule

module sub_module2(input a , input b , output y);
	assign y = a ^ b;
endmodule

module multiple_module_opt(input a , input b , input c , input d , output y);
	wire n1, n2, n3;

	sub_module1 U1 (.a(a), .b(1'b1), .y(n1));
	sub_module2 U2 (.a(n1), .b(1'b0), .y(n2));
	sub_module2 U3 (.a(b), .b(d), .y(n3));

	assign y = c | (b & n1);
endmodule
```

**Explanation:**  
Initially, this design uses two sub-modules that generate intermediate signals. Upon optimization, the logic is simplified to an AND-OR gate combination, which reduces the need for multiple sub-modules and unnecessary wire connections.

**Steps to Generate Netlist:**

```bash
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog multiple_module_opt.v
synth -top multiple_module_opt
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
flatten
show
write_verilog -noattr multiple_module_opt_net.v
```

**Visualization After Optimization:**

![image](https://github.com/user-attachments/assets/5d62351e-915a-45b8-921c-d0d2647e3943)

```verilog
/* Generated by Yosys 0.44+60 (git sha1 c25448f1d, g++ 11.4.0-1ubuntu1~22.04 -fPIC -O3) */

module multiple_module_opt(a, b, c, d, y);
  wire _0_;
  wire _1_;
  wire _2_;
  wire _3_;
  wire _4_;
  wire _5_;
  wire _6_;
  wire _7_;
  wire \U1.a ;
  wire \U1.b ;
  wire \U1.y ;
  input a;
  wire a;
  input b;
  wire b;
  input c;
  wire c;
  input d;
  wire d;
  wire n1;
  output y;
  wire y;
  sky130_fd_sc_hd__a21o_1 _8_ (
    .A1(_3_),
    .A2(_1_),
    .B1(_2_),
    .X(_4_)
  );
  sky130_fd_sc_hd__and2_0 _9_ (
    .A(_6_),
    .B(_5_),
    .X(_7_)
  );
  assign _3_ = n1;
  assign _1_ = b;
  assign _2_ = c;
  assign y = _4_;
  assign _6_ = \U1.b ;
  assign _5_ = \U1.a ;
  assign \U1.y  = _7_;
  assign \U1.a  = a;
  assign \U1.b  = 1'h1;
  assign n1 = \U1.y ;
endmodule
```

---

### Example 6: Design Simplified to Constant Output (Y=0)

In this example, we demonstrate a design that simplifies entirely to a constant value, \( Y = 0 \), after optimization.

**Verilog Code:**

```verilog
module sub_module(input a , input b , output y);
	assign y = a & b;
endmodule

module multiple_module_opt2(input a , input b , input c , input d , output y);
	wire n1, n2, n3;
	
	sub_module U1 (.a(a), .b(1'b0), .y(n1));
	sub_module U2 (.a(b), .b(c), .y(n2));
	sub_module U3 (.a(n2), .b(d), .y(n3));
	sub_module U4 (.a(n3), .b(n1), .y(y));
endmodule
```

**Explanation:**  
This design involves several sub-modules, but the optimization process reveals that due to the presence of certain constant values (like 0 in the input), the final output of the circuit is always zero, making it unnecessary to implement complex logic.

**Steps to Generate Netlist:**

```bash
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog multiple_module_opt2.v
synth -top multiple_module_opt2
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
flatten
show
write_verilog -noattr multiple_module_opt2_net.v
```

**Visualization After Optimization:**

![image](https://github.com/user-attachments/assets/5ef80c4c-0341-4eb0-af6e-3e11acf5ff77)

```verilog
	/* Generated by Yosys 0.44+60 (git sha1 c25448f1d, g++ 11.4.0-1ubuntu1~22.04 -fPIC -O3) */

module multiple_module_opt2(a, b, c, d, y);
  wire _00_;
  wire _01_;
  wire _02_;
  wire _03_;
  wire _04_;
  wire _05_;
  wire _06_;
  wire _07_;
  wire _08_;
  wire _09_;
  wire _10_;
  wire _11_;
  wire \U1.a ;
  wire \U1.b ;
  wire \U1.y ;
  wire \U2.a ;
  wire \U2.b ;
  wire \U2.y ;
  wire \U3.a ;
  wire \U3.b ;
  wire \U3.y ;
  wire \U4.a ;
  wire \U4.b ;
  wire \U4.y ;
  input a;
  wire a;
  input b;
  wire b;
  input c;
  wire c;
  input d;
  wire d;
  wire n1;
  wire n2;
  wire n3;
  output y;
  wire y;
  sky130_fd_sc_hd__and2_0 _12_ (
    .A(_01_),
    .B(_00_),
    .X(_02_)
  );
  sky130_fd_sc_hd__and2_0 _13_ (
    .A(_04_),
    .B(_03_),
    .X(_05_)
  );
  sky130_fd_sc_hd__and2_0 _14_ (
    .A(_07_),
    .B(_06_),
    .X(_08_)
  );
  sky130_fd_sc_hd__and2_0 _15_ (
    .A(_10_),
    .B(_09_),
    .X(_11_)
  );
  assign _10_ = \U4.b ;
  assign _09_ = \U4.a ;
  assign \U4.y  = _11_;
  assign \U4.a  = n3;
  assign \U4.b  = n1;
  assign y = \U4.y ;
  assign _07_ = \U3.b ;
  assign _06_ = \U3.a ;
  assign \U3.y  = _08_;
  assign \U3.a  = n2;
  assign \U3.b  = d;
  assign n3 = \U3.y ;
  assign _04_ = \U2.b ;
  assign _03_ = \U2.a ;
  assign \U2.y  = _05_;
  assign \U2.a  = b;
  assign \U2.b  = c;
  assign n2 = \U2.y ;
  assign _01_ = \U1.b ;
  assign _00_ = \U1.a ;
  assign \U1.y  = _02_;
  assign \U1.a  = a;
  assign \U1.b  = 1'h0;
  assign n1 = \U1.y ;
endmodule
```

---


### **Sequential Logic Optimizations**

### **Example 1: Constant Output Flip-Flop (Logic 1)**

**Verilog Code:**

```verilog
module dff_const1(input clk, input reset, output reg q);
always @(posedge clk, posedge reset)
begin
	if(reset)
		q <= 1'b0;
	else
		q <= 1'b1;
end
endmodule
```

In this circuit, `q` is reset to `0` when `reset` is high, and when the clock edge triggers without reset, `q` takes a constant logic `1`. Essentially, the `q` output is hardcoded to `1` in the absence of a reset. 

**Netlist Generation and Synthesis Commands:**

```bash
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_const1.v
synth -top dff_const1
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog -noattr dff_const1_net.v
```

- After synthesis, the netlist reveals that this design synthesizes to a very basic flip-flop, with `q` eventually stabilizing at `1` as soon as the reset is released.
- The constant logic assignment simplifies the internal state machine, optimizing the design.

![image](https://github.com/user-attachments/assets/9797d4f3-ed46-4b9e-9c77-2718efd68dbb)

```verilog
/* Generated by Yosys 0.44+60 (git sha1 c25448f1d, g++ 11.4.0-1ubuntu1~22.04 -fPIC -O3) */

module dff_const1(clk, reset, q);
  wire _0_;
  wire _1_;
  wire _2_;
  input clk;
  wire clk;
  output q;
  wire q;
  input reset;
  wire reset;
  sky130_fd_sc_hd__clkinv_1 _3_ (
    .A(_1_),
    .Y(_0_)
  );
  sky130_fd_sc_hd__dfrtp_1 _4_ (
    .CLK(clk),
    .D(1'h1),
    .Q(q),
    .RESET_B(_2_)
  );
  assign _1_ = reset;
  assign _2_ = _0_;
endmodule
```

**Waveform Output:**

```bash
iverilog dff_const1.v tb_dff_const1.v
./a.out
gtkwave tb_dff_const1.vcd
```

![image](https://github.com/user-attachments/assets/599c5fb0-d5d5-4542-9b83-56883015183e)

- As shown in the waveform, `q` changes from `0` (when reset is high) to `1` after the reset is de-asserted. The signal is stable at `1` afterward.

---

### **Example 2: Constant High Flip-Flop (Hardwired 1)**

**Verilog Code:**

```verilog
module dff_const2(input clk, input reset, output reg q);
always @(posedge clk, posedge reset)
begin
	if(reset)
		q <= 1'b1;
	else
		q <= 1'b1;
end
endmodule
```

This design hardwires the output `q` to always remain `1` regardless of the clock or reset signals. The logic is simplified since the state never changes.

**Synthesis and Netlist Generation:**

```bash
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_const2.v
synth -top dff_const2
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog -noattr dff_const2_net.v
```

- After synthesis, the netlist reveals the absence of flip-flop logic, as the constant high signal is optimized away. It is simply reduced to a wire connected to a `1` logic level.

![image](https://github.com/user-attachments/assets/ced2fa80-b67d-4b73-a352-dc93a82e0bdd)

```verilog
/* Generated by Yosys 0.44+60 (git sha1 c25448f1d, g++ 11.4.0-1ubuntu1~22.04 -fPIC -O3) */

module dff_const2(clk, reset, q);
  input clk;
  wire clk;
  output q;
  wire q;
  input reset;
  wire reset;
  assign q = 1'h1;
endmodule
```

**Waveform Output:**

```bash
iverilog dff_const2.v tb_dff_const2.v
./a.out
gtkwave tb_dff_const2.vcd
```
![image](https://github.com/user-attachments/assets/781da819-fe50-44bd-8c8e-119209144429)



- The output `q` is always `1`, no matter the clock or reset. The reset has no impact, reflecting the fixed nature of the logic.

---

#### **Example 3: DFF with Intermediate Flip-Flop**

**Verilog Code:**

```verilog
module dff_const3(input clk, input reset, output reg q);
reg q1;

always @(posedge clk, posedge reset)
begin
	if(reset)
	begin
		q <= 1'b1;
		q1 <= 1'b0;
	end
	else
	begin
		q1 <= 1'b1;
		q <= q1;
	end
end
endmodule
```

In this example, `q1` is introduced as an intermediate storage element. Upon reset, `q1` is set to `0` and `q` to `1`. Afterward, `q` takes the value of `q1`, which is set to `1` in the next cycle.

**Synthesis and Netlist Generation:**

```bash
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_const3.v
synth -top dff_const3
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog -noattr dff_const3_net.v
```

- The logic here results in a more complex circuit, as the synthesis includes two flip-flops (`q` and `q1`) interacting over the clock cycles.

![image](https://github.com/user-attachments/assets/34e91632-b54f-42cb-8dd9-de494708f342)

```verilog
/* Generated by Yosys 0.44+60 (git sha1 c25448f1d, g++ 11.4.0-1ubuntu1~22.04 -fPIC -O3) */

module dff_const3(clk, reset, q);
  wire _0_;
  wire _1_;
  wire _2_;
  wire _3_;
  wire _4_;
  input clk;
  wire clk;
  output q;
  wire q;
  wire q1;
  input reset;
  wire reset;
  sky130_fd_sc_hd__clkinv_1 _5_ (
    .A(_2_),
    .Y(_0_)
  );
  sky130_fd_sc_hd__clkinv_1 _6_ (
    .A(_2_),
    .Y(_1_)
  );
  sky130_fd_sc_hd__dfstp_2 _7_ (
    .CLK(clk),
    .D(q1),
    .Q(q),
    .SET_B(_3_)
  );
  sky130_fd_sc_hd__dfrtp_1 _8_ (
    .CLK(clk),
    .D(1'h1),
    .Q(q1),
    .RESET_B(_4_)
  );
  assign _2_ = reset;
  assign _3_ = _0_;
  assign _4_ = _1_;
endmodule

```

**Waveform Output:**

```bash
iverilog dff_const3.v tb_dff_const3.v
./a.out
gtkwave tb_dff_const3.vcd
```

![image](https://github.com/user-attachments/assets/0e0e7bbe-ae3e-47cd-ace9-ceda8b39e185)

- The waveform shows the interaction between `q1` and `q`. Initially, upon reset, `q` is set to `1` and `q1` to `0`. Afterward, `q` follows `q1`, and both stabilize at `1`.

---

### **Example 4: Redundant Constant Assignment Flip-Flop**

**Verilog Code:**

```verilog
module dff_const4(input clk, input reset, output reg q);
reg q1;

always @(posedge clk, posedge reset)
begin
	if(reset)
	begin
		q <= 1'b1;
		q1 <= 1'b1;
	end
else
	begin
		q1 <= 1'b1;
		q <= q1;
	end
end
endmodule
```

This design introduces redundancy. Both `q` and `q1` are set to `1` both during reset and clock cycles. Essentially, this is an overcomplicated version of a circuit that always assigns `1` to both outputs.

**Synthesis and Netlist Generation:**

```bash
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_const4.v
synth -top dff_const4
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog -noattr dff_const4_net.v
```

- This redundant assignment is optimized away during synthesis. The final netlist is reduced to a constant `1` logic signal.

![image](https://github.com/user-attachments/assets/775f59bf-2873-45bd-80f0-edb8ed368d8f)

```verilog
/* Generated by Yosys 0.44+60 (git sha1 c25448f1d, g++ 11.4.0-1ubuntu1~22.04 -fPIC -O3) */

module dff_const4(clk, reset, q);
  input clk;
  wire clk;
  output q;
  wire q;
  wire q1;
  input reset;
  wire reset;
  assign q = 1'h1;
  assign q1 = 1'h1;
endmodule
```

**Waveform Output:**

```bash
iverilog dff_const4.v tb_dff_const4.v
./a.out
gtkwave tb_dff_const4.vcd
```

![image](https://github.com/user-attachments/assets/9fabe203-4dd9-458c-8830-1365fc682c89)

- As expected, the signal remains high (`1`) throughout.

---

### Sequential Logic Optimization: Enhanced Understanding and Examples

In digital circuits, optimization of sequential logic is key to improving performance and minimizing resource usage. Optimizing flip-flop (FF) designs often involves identifying redundant or constant assignments and simplifying the logic accordingly. Below, we explore several examples of optimization strategies using Verilog and tools like Yosys, ABC, and GTKWave.

---

### **Example 5: Unused Flip-Flop**

Here a D flip-flop is fed a constant value after the reset signal.

Verilog Code:
```verilog
module dff_const5(input clk, input reset, output reg q);
    reg q1;
    always @(posedge clk, posedge reset) begin
        if (reset) begin
            q <= 1'b0;
            q1 <= 1'b0;
        end else begin
            q1 <= 1'b1;
            q <= q1;
        end
    end
endmodule
```

The goal here is to remove redundancy. Notice that after the initial reset, `q1` is always assigned a value of `1'b1`. This design could be simplified by eliminating unnecessary registers or merging operations.

**Steps for Netlist Generation:**
   ```sh
   yosys
   read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
   read_verilog dff_const5.v
   synth -top dff_const5
   dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
   abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
   show
   write_verilog -noattr dff_const5_net.v
   ```

![image](https://github.com/user-attachments/assets/14945d87-c46d-4d6f-a60f-56652c152c86)

```verilog
/* Generated by Yosys 0.44+60 (git sha1 c25448f1d, g++ 11.4.0-1ubuntu1~22.04 -fPIC -O3) */

module dff_const5(clk, reset, q);
  wire _0_;
  wire _1_;
  wire _2_;
  wire _3_;
  wire _4_;
  input clk;
  wire clk;
  output q;
  wire q;
  wire q1;
  input reset;
  wire reset;
  sky130_fd_sc_hd__clkinv_1 _5_ (
    .A(_2_),
    .Y(_0_)
  );
  sky130_fd_sc_hd__clkinv_1 _6_ (
    .A(_2_),
    .Y(_1_)
  );
  sky130_fd_sc_hd__dfrtp_1 _7_ (
    .CLK(clk),
    .D(q1),
    .Q(q),
    .RESET_B(_3_)
  );
  sky130_fd_sc_hd__dfrtp_1 _8_ (
    .CLK(clk),
    .D(1'h1),
    .Q(q1),
    .RESET_B(_4_)
  );
  assign _2_ = reset;
  assign _3_ = _0_;
  assign _4_ = _1_;
endmodule
```

**Waveform Output:**

```bash
iverilog dff_const5.v tb_dff_const5.v
./a.out
gtkwave tb_dff_const5.vcd
```

![image](https://github.com/user-attachments/assets/4f66e719-2dc9-4c14-8b05-38ff8949dd47)

---

### **Optimizing Unused Outputs in Counters**

#### **Example 1: Simple Counter with Unused Bits**

Verilog Code:
```verilog
module counter_opt(input clk, input reset, output q);
    reg [2:0] count;
    assign q = count[0];
    always @(posedge clk, posedge reset) begin
        if (reset)
            count <= 3'b000;
        else
            count <= count + 1;
    end
endmodule
```

**Observation:**
This design increments a 3-bit counter, but the output `q` only reflects the least significant bit (`count[0]`). The rest of the bits (`count[2:1]`) are unused, leading to redundant logic.

**Steps for Netlist Generation:**
   ```sh
   yosys
   read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
   read_verilog counter_opt.v
   synth -top counter_opt
   dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
   abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
   show
   write_verilog -noattr counter_opt_net.v
   ```

![image](https://github.com/user-attachments/assets/a96b3f43-c1f0-4ffd-91b7-647b10b27ff4)

```verilog
/* Generated by Yosys 0.44+60 (git sha1 c25448f1d, g++ 11.4.0-1ubuntu1~22.04 -fPIC -O3) */

module counter_opt(clk, reset, q);
  wire _0_;
  wire _1_;
  wire _2_;
  wire _3_;
  wire [2:0] _4_;
  wire _5_;
  input clk;
  wire clk;
  wire [2:0] count;
  output q;
  wire q;
  input reset;
  wire reset;
  sky130_fd_sc_hd__clkinv_1 _6_ (
    .A(_2_),
    .Y(_0_)
  );
  sky130_fd_sc_hd__clkinv_1 _7_ (
    .A(_3_),
    .Y(_1_)
  );
  sky130_fd_sc_hd__dfrtp_1 _8_ (
    .CLK(clk),
    .D(_4_[0]),
    .Q(count[0]),
    .RESET_B(_5_)
  );
  assign _4_[2:1] = count[2:1];
  assign q = count[0];
  assign _2_ = count[0];
  assign _4_[0] = _0_;
  assign _3_ = reset;
  assign _5_ = _1_;
endmodule
```

GTKWave Output:
- The simulation reveals that optimizing the counter would result in fewer registers, as the higher-order bits do not affect the output.

![image](https://github.com/user-attachments/assets/6989e283-ffe9-42ed-8a67-8eed63e78466)

  
#### **Modified Counter Logic: Reducing Unused Bits**

Optimized Verilog Code:
```verilog
module counter_opt(input clk, input reset, output q);
    reg [2:0] count;
    assign q = (count == 3'b100);
    always @(posedge clk, posedge reset) begin
        if (reset)
            count <= 3'b000;
        else
            count <= count + 1;
    end
endmodule
```

Code: 

```sh
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog counter_opt.v
synth -top counter_opt
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog -noattr counter_opt_net.v
```
![Screenshot from 2024-10-21 02-04-49](https://github.com/user-attachments/assets/1d1c24f1-6f07-425e-acb8-0adb291f7b1c)


```verilog
/* Generated by Yosys 0.44+60 (git sha1 c25448f1d, g++ 11.4.0-1ubuntu1~22.04 -fPIC -O3) */

module counter_opt(clk, reset, q);
  wire _00_;
  wire _01_;
  wire _02_;
  wire _03_;
  wire _04_;
  wire _05_;
  wire _06_;
  wire _07_;
  wire _08_;
  wire _09_;
  wire _10_;
  wire _11_;
  wire _12_;
  wire _13_;
  wire [2:0] _14_;
  wire [2:0] _15_;
  wire _16_;
  wire _17_;
  wire _18_;
  input clk;
  wire clk;
  wire [2:0] count;
  output q;
  wire q;
  input reset;
  wire reset;
  sky130_fd_sc_hd__clkinv_1 _19_ (
    .A(_08_),
    .Y(_02_)
  );
  sky130_fd_sc_hd__clkinv_1 _20_ (
    .A(_13_),
    .Y(_05_)
  );
  sky130_fd_sc_hd__nor3b_1 _21_ (
    .A(_08_),
    .B(_09_),
    .C_N(_10_),
    .Y(_12_)
  );
  sky130_fd_sc_hd__nand2_1 _22_ (
    .A(_08_),
    .B(_09_),
    .Y(_11_)
  );
  sky130_fd_sc_hd__xor2_1 _23_ (
    .A(_08_),
    .B(_09_),
    .X(_03_)
  );
  sky130_fd_sc_hd__xnor2_1 _24_ (
    .A(_10_),
    .B(_11_),
    .Y(_04_)
  );
  sky130_fd_sc_hd__clkinv_1 _25_ (
    .A(_13_),
    .Y(_06_)
  );
  sky130_fd_sc_hd__clkinv_1 _26_ (
    .A(_13_),
    .Y(_07_)
  );
  sky130_fd_sc_hd__dfrtp_1 _27_ (
    .CLK(clk),
    .D(_14_[0]),
    .Q(count[0]),
    .RESET_B(_16_)
  );
  sky130_fd_sc_hd__dfrtp_1 _28_ (
    .CLK(clk),
    .D(_15_[1]),
    .Q(count[1]),
    .RESET_B(_17_)
  );
  sky130_fd_sc_hd__dfrtp_1 _29_ (
    .CLK(clk),
    .D(_15_[2]),
    .Q(count[2]),
    .RESET_B(_18_)
  );
  assign _15_[0] = _14_[0];
  assign _14_[2:1] = count[2:1];
  assign _08_ = count[0];
  assign _14_[0] = _02_;
  assign _09_ = count[1];
  assign _10_ = count[2];
  assign q = _12_;
  assign _15_[1] = _03_;
  assign _15_[2] = _04_;
  assign _13_ = reset;
  assign _16_ = _05_;
  assign _17_ = _06_;
  assign _18_ = _07_;
endmodule
```

**Waveform Output:**

```bash
iverilog counter_opt.v tb_counter_opt.v
./a.out
gtkwave tb_counter_opt.vcd
```

![image](https://github.com/user-attachments/assets/259b9984-a762-4558-af9b-2f7a3b5a57ad)


**Explanation:**
- Instead of outputting the least significant bit, the optimized design directly checks for a condition (`count == 3'b100`) and reflects this in the output `q`.
- This minimizes the number of state bits tracked by the counter and eliminates the need for constant increments and tracking of unnecessary bits.

---

</details>


<details>
	<summary>Day 4 - GLS, Blocking vs Non-Blocking Assignments, and Synthesis-Simulation Discrepancies</summary>
	
Gate-Level Simulation (GLS) plays a pivotal role in the validation of digital circuits. This simulation is executed on a synthesized netlist, which is a low-level representation of the design, using a testbench to verify both logical correctness and timing behavior. By comparing the simulation results to expected outputs, GLS helps ensure that no errors have crept in during synthesis and that the design meets all timing requirements.

![GLS Overview](https://github.com/user-attachments/assets/fa59f230-4a0d-4c8e-9353-a01a9209a6d6)

One of the critical elements for proper circuit operation is a correctly defined sensitivity list in always blocks. An incomplete sensitivity list can lead to the unintended generation of latches. Additionally, the use of blocking and non-blocking assignments can affect how signals are updated, potentially leading to timing mismatches between simulation and synthesis. To avoid such issues, it’s essential to carefully structure always blocks, ensuring the proper use of sensitivity lists and assignments.

### Gate-Level Simulation Example 1: Basic Ternary Mux

#### Verilog Code:

```verilog
module ternary_operator_mux (input i0, input i1, input sel, output y);
assign y = sel ? i1 : i0;
endmodule
```

#### RTL Simulation:

```bash
iverilog ternary_operator_mux.v tb_ternary_operator_mux.v
./a.out
gtkwave tb_ternary_operator_mux.vcd
```

![image](https://github.com/user-attachments/assets/8832d2d0-2d17-4a5f-96c5-c10a9dd8759a)

#### Synthesizing the Design:

```bash
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib  
read_verilog ternary_operator_mux.v
synth -top ternary_operator_mux
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
show
write_verilog -noattr ternary_operator_mux_net.v
```

Synthesized netlist view:

![image](https://github.com/user-attachments/assets/8141816d-2208-47d4-8a7e-d3ad5c1f1dd1)

```verilog
/* Generated by Yosys 0.44+60 (git sha1 c25448f1d, g++ 11.4.0-1ubuntu1~22.04 -fPIC -O3) */

module ternary_operator_mux(i0, i1, sel, y);
  wire _0_;
  wire _1_;
  wire _2_;
  wire _3_;
  input i0;
  wire i0;
  input i1;
  wire i1;
  input sel;
  wire sel;
  output y;
  wire y;
  sky130_fd_sc_hd__mux2_1 _4_ (
    .A0(_0_),
    .A1(_1_),
    .S(_2_),
    .X(_3_)
  );
  assign _0_ = i0;
  assign _1_ = i1;
  assign _2_ = sel;
  assign y = _3_;
endmodule

```

#### Gate-Level Simulation:

```bash
iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v ternary_operator_mux_net.v tb_ternary_operator_mux.v
./a.out
gtkwave tb_ternary_operator_mux.vcd
```

GLS results show no mismatch between pre-synthesis and post-synthesis waveforms:

![image](https://github.com/user-attachments/assets/a268a77d-5d47-4928-bfe0-2a952d5a5864)

### Gate-Level Simulation Example 2: Bad Mux Example

#### Verilog Code:

```verilog
module bad_mux (input i0, input i1, input sel, output reg y);
always @(sel)
begin
    if (sel)
        y <= i1;
    else
        y <= i0;
end
endmodule
```

#### RTL Simulation:

```bash
iverilog bad_mux.v tb_bad_mux.v
./a.out
gtkwave tb_bad_mux.vcd
```

![image](https://github.com/user-attachments/assets/4f146067-3d1f-4530-9797-261971822cc6)


#### Synthesizing the Design:

```bash
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib  
read_verilog bad_mux.v
synth -top bad_mux
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
show
write_verilog -noattr bad_mux_net.v
```

Netlist view post-synthesis:

![image](https://github.com/user-attachments/assets/5d7031ad-2fa4-4879-b82c-f392ff7a683a)

```verilog
/* Generated by Yosys 0.44+60 (git sha1 c25448f1d, g++ 11.4.0-1ubuntu1~22.04 -fPIC -O3) */

module bad_mux(i0, i1, sel, y);
  wire _0_;
  wire _1_;
  wire _2_;
  wire _3_;
  input i0;
  wire i0;
  input i1;
  wire i1;
  input sel;
  wire sel;
  output y;
  wire y;
  sky130_fd_sc_hd__mux2_1 _4_ (
    .A0(_0_),
    .A1(_1_),
    .S(_2_),
    .X(_3_)
  );
  assign _0_ = i0;
  assign _1_ = i1;
  assign _2_ = sel;
  assign y = _3_;
endmodule
```


#### Gate-Level Simulation:

```bash
iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v bad_mux_net.v tb_bad_mux.v
./a.out
gtkwave tb_bad_mux.vcd
```

GLS results show a mismatch between pre- and post-synthesis due to the incorrect sensitivity list, which Yosys corrected during synthesis:

![image](https://github.com/user-attachments/assets/fea71116-08ec-462c-9bf5-51afaa5f35db)

### Investigation of Synthesis-Simulation Mismatches for Blocking Assignments

#### Verilog Code:

```verilog
module blocking_caveat (input a, input b, input c, output reg d); 
reg x;
always @ (*)
begin
    d = x & c;
    x = a | b;
end
endmodule
```

#### RTL Simulation:

```bash
iverilog blocking_caveat.v tb_blocking_caveat.v
./a.out
gtkwave tb_blocking_caveat.vcd
```

![image](https://github.com/user-attachments/assets/35c82c71-2dc5-4c6e-86ec-7a753b29972d)

#### Synthesizing the Design:

```bash
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib  
read_verilog blocking_caveat.v
synth -top blocking_caveat
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
show
write_verilog -noattr blocking_caveat_net.v
```

Netlist view post-synthesis:

![image](https://github.com/user-attachments/assets/bc48b580-bea1-420e-9a1d-1bfbf3580fc1)

```verilog
/* Generated by Yosys 0.44+60 (git sha1 c25448f1d, g++ 11.4.0-1ubuntu1~22.04 -fPIC -O3) */

module blocking_caveat(a, b, c, d);
  wire _0_;
  wire _1_;
  wire _2_;
  wire _3_;
  wire _4_;
  input a;
  wire a;
  input b;
  wire b;
  input c;
  wire c;
  output d;
  wire d;
  sky130_fd_sc_hd__o21a_1 _5_ (
    .A1(_2_),
    .A2(_1_),
    .B1(_3_),
    .X(_4_)
  );
  assign _2_ = b;
  assign _1_ = a;
  assign _3_ = c;
  assign d = _4_;
endmodule
```

#### Gate-Level Simulation:

```bash
iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v blocking_caveat_net.v tb_blocking_caveat.v
./a.out
gtkwave tb_blocking_caveat.vcd
```

GLS results show a mismatch between the RTL and post-synthesis waveforms due to the latch created by incorrect blocking assignments, which was fixed during synthesis by Yosys:

![image](https://github.com/user-attachments/assets/a7ab53e0-ad18-4a2c-9437-09b15b0bd84c)

This illustrates how synthesis tools can correct certain errors, but it's still crucial to write clean Verilog code to avoid mismatches and unintended behaviors.
</details>

</details>

<details>
	<summary>Task 9 : Simulating Pre-Synthesis and Post-Synthesis Output for RVMyth</summary> 

### 1. **Pre-Synthesis Simulation:**
The pre-synthesis output of the `rvmyth.v` file is generated using Sandpiper and a TL-Verilog (TLV) file. Follow the steps below to perform the pre-synthesis simulation:

```sh
cd VSDBabySoC
make pre_synth_sim
gtkwave output/pre_synth_sim/pre_synth_sim.vcd
```

This will generate a waveform file (`.vcd`) that can be opened and visualized using GTKWave. Below is an example of the pre-synthesis waveform:

![Pre-synthesis simulation waveform](https://github.com/user-attachments/assets/78f4ad1b-8eca-43ae-905f-b8fcaa1e24df)

The output waveform:

![Pre-synthesis simulation result](https://github.com/user-attachments/assets/407ad14f-941c-4a6c-bde6-9a6799a4bd12)

Here is a zoomed-out view showing multiple PLL cycles:

![Zoomed pre-synthesis waveform](https://github.com/user-attachments/assets/c8105bee-f2b2-4b54-abfa-f61ba0c0bb46)

---

### 2. **Post-Synthesis Simulation:**
Next, we proceed with the post-synthesis simulation using Yosys, an open-source synthesis tool. The `rvmyth.v` design is synthesized with the Sky130 PDK standard cell library.

#### Steps for Post-Synthesis Simulation:

1. **Open Yosys**:
    ```sh
    yosys
    ```

2. **Read the standard cell library and Verilog design files**:
    ```sh
    read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
    read_verilog clk_gate.v
    read_verilog rvmyth.v
    ```

3. **Synthesize the design with the top module `rvmyth`**:
    ```sh
    synth -top rvmyth
    ```

4. **Map the design to standard cells and generate the gate-level netlist**:
    ```sh
    abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
    show
    write_verilog -noattr rvmyth_net.v
    ```

5. **View the generated gate-level netlist** (`rvmyth_net.v`):
    ```sh
    !gedit rvmyth_net.v
    exit
    ```

The post-synthesis netlist and gate-level design can be visualized as shown below:

![Yosys post-synthesis result](https://github.com/user-attachments/assets/a351dd82-9da0-4b14-87fc-f04e2b0825db)

Synthesis statistics:

![Yosys gate-level design](https://github.com/user-attachments/assets/0f8e26eb-5a2b-4430-8746-e63568479852)

Post-synthesis visualization:

![Post-synthesis visualization](https://github.com/user-attachments/assets/ca87d612-7c04-4b86-adca-f652a1e8aeeb)

Netlist Generated:

![image](https://github.com/user-attachments/assets/eab56876-75b8-4f32-a7d5-8aa58d27e708)


---

After synthesis, the gate-level `.v` code is generated, as shown below:

![Gate-level Verilog code](https://github.com/user-attachments/assets/c72e9283-51e7-44e1-aeec-4be512dd6e8c)

![Another view of gate-level Verilog code](https://github.com/user-attachments/assets/33a6b3af-03c8-4788-91ab-e8dc0b7adbf3)

---

#### Viewing Post-Synthesis Simulation Waveforms:
To view the post-synthesis waveforms, use the following commands:

```sh
cd ~/VSDBabySoC
make post_synth_sim
gtkwave output/post_synth_sim/post_synth_sim.vcd
```

Here is an example of the post-synthesis waveform:

![Post-synthesis waveform](https://github.com/user-attachments/assets/09dd63b5-8f5f-4f41-b56b-ad98c1e1a89f)

---

#### Conclusion:
Both pre-synthesis and post-synthesis waveforms are the same, confirming that the synthesis process preserves the correct functionality of the design.
</details>



<details>
	<summary>Task 10 : Post-Synthesis Static Timing Analysis of VSDBabySoC Using OpenSTA </summary>
<br>
In this task, we utilize `OpenSTA` (Open Source Static Timing Analyzer) to conduct a post-synthesis static timing analysis on the `VSDBabySoC` design. The primary objective of static timing analysis (STA) is to verify that the synthesized design adheres to timing constraints for setup and hold times, ensuring reliable performance across all paths.

### SDC File Configuration

The timing constraints are specified in the `vsdbabysoc_synthesis.sdc` file, which outlines the clock period, clock uncertainty, and input transitions. Below is a detailed breakdown of each line in the SDC file:

```tcl
# Define the clock period
set PERIOD 11.35

# Set timing units to nanoseconds (ns)
set_units -time ns

# Create a clock named 'clk' with a period of 11.35 ns, based on the pll/CLK pin
create_clock [get_pins {pll/CLK}] -name clk -period $PERIOD

# Define setup uncertainty as 5% of the clock period
set_clock_uncertainty -setup [expr $PERIOD * 0.05] [get_clocks clk]

# Define clock transition time as 5% of the clock period
set_clock_transition [expr $PERIOD * 0.05] [get_clocks clk]

# Define hold uncertainty as 8% of the clock period
set_clock_uncertainty -hold [expr $PERIOD * 0.08] [get_clocks clk]

# Define input transition times for the specified ports
set_input_transition [expr $PERIOD * 0.08] [get_ports ENb_CP]
set_input_transition [expr $PERIOD * 0.08] [get_ports ENb_VCO]
set_input_transition [expr $PERIOD * 0.08] [get_ports REF]
set_input_transition [expr $PERIOD * 0.08] [get_ports VCO_IN]
set_input_transition [expr $PERIOD * 0.08] [get_ports VREFH]
```

### Execution Commands for Timing Analysis

To perform the static timing analysis using OpenSTA, execute the following commands sequentially:

```bash
cd VSDBabySoc/src
sta

# Load Liberty files for cell delays and constraints
read_liberty -min ./lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_liberty -min ./lib/avsdpll.lib
read_liberty -min ./lib/avsddac.lib
read_liberty -max ./lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_liberty -max ./lib/avsdpll.lib
read_liberty -max ./lib/avsddac.lib

# Load the synthesized Verilog file
read_verilog ../output/synth/vsdbabysoc.synth.v

# Link the design for analysis
link_design vsdbabysoc

# Apply timing constraints from the SDC file
read_sdc ./sdc/vsdbabysoc_synthesis.sdc

# Generate a detailed timing report including min/max delays
report_checks -path_delay min_max -format full_clock_expanded -digits 4
```

### Timing Report: Key Metrics

#### Overall Timing Analysis
![Overall Timing Analysis](https://github.com/user-attachments/assets/52a3e12b-3c1a-46ef-88e5-382c85d9a297)

#### Setup Time Analysis

The setup time analysis verifies that signals have stabilized before the clock edge, ensuring data integrity. The following image presents a snapshot of the setup time analysis for `VSDBabySoC`:

*Setup Time Report:*
![Setup Time](https://github.com/user-attachments/assets/7302bcd5-2aae-4edd-8bb2-ba206e6e8fa4)

#### Hold Time Analysis

The hold time analysis ensures that data remains stable for a brief period after the clock edge, allowing proper data capture. Below is the hold time analysis for `VSDBabySoC`:

*Hold Time Report:*
![Hold Time](https://github.com/user-attachments/assets/fa490618-e7e9-4831-b0c0-a596781563f8)

---

### Conclusion

Upon analyzing the timing reports, it is evident that the setup time was violated due to an insufficient clock period (Tc). This indicates that the current timing constraints do not allow for reliable data capture and may require adjustments to the design or constraints to ensure proper functionality and performance.

</details>

<details>
	<summary>Task 11 : Post-Synthesis Static Timing Analysis Using OpenSTA for Sky130 Libraries</summary>
<br>
This task involved executing a post-synthesis static timing analysis (STA) on a synthesized Verilog netlist using OpenSTA with Sky130 library files. The objective was to set up a timing analysis environment, read in the necessary library and netlist files, and generate detailed timing reports for various process-voltage-temperature (PVT) corners. Below is an organized procedure and explanation of each step.

#### Procedure

1. **Environment and Library File Preparation**
   - To streamline the analysis process, all required `.lib` files were stored in a directory named `lib_dir` within the main project directory (`VSDBabySoC`). This structure allowed for easy access to library files during analysis.

   **Directory Structure:**

   ```
   VSDBabySoC/
   ├── output/
   │   └── synth/
   │       └── vsdbabysoc.synth.v
   ├── src/
   │   └── script.tcl.tcl
   ├── lib_dir/
   │   ├── sky130_fd_sc_hd__ff_100C_1v65.lib
   │   ├── sky130_fd_sc_hd__ff_100C_1v95.lib
   │   ├── ... (additional .lib files)
   └── sdc/
       └── vsdbabysoc_synthesis.sdc
   ```

2. **TCL Script Creation (`script.tcl.tcl`)**
   - A TCL script, `script.tcl`, was created in the `VSDBabySoC/src` directory to automate the process. This script reads library files, loads the synthesized Verilog code, and generates timing reports across different PVT corners.

   **Script Content:**

   ```tcl
	set lib_li(1) "sky130_fd_sc_hd__ff_100C_1v65.lib"
	set lib_li(2) "sky130_fd_sc_hd__ff_100C_1v95.lib"
	set lib_li(3) "sky130_fd_sc_hd__ff_n40C_1v56.lib"
	set lib_li(4) "sky130_fd_sc_hd__ff_n40C_1v65.lib"
	set lib_li(5) "sky130_fd_sc_hd__ff_n40C_1v76.lib"
	set lib_li(6) "sky130_fd_sc_hd__ff_n40C_1v95.lib"
	set lib_li(7) "sky130_fd_sc_hd__ss_100C_1v40.lib"
	set lib_li(8) "sky130_fd_sc_hd__ss_100C_1v60.lib"
	set lib_li(9) "sky130_fd_sc_hd__ss_n40C_1v28.lib"
	set lib_li(10) "sky130_fd_sc_hd__ss_n40C_1v35.lib"
	set lib_li(11) "sky130_fd_sc_hd__ss_n40C_1v40.lib"
	set lib_li(12) "sky130_fd_sc_hd__ss_n40C_1v44.lib"
	set lib_li(13) "sky130_fd_sc_hd__ss_n40C_1v60.lib"
	set lib_li(14) "sky130_fd_sc_hd__ss_n40C_1v76.lib"
	set lib_li(15) "sky130_fd_sc_hd__tt_025C_1v80.lib"
	set lib_li(16) "sky130_fd_sc_hd__tt_100C_1v80.lib"

   
   read_liberty -min ./lib/avsdpll.lib
   read_liberty -min ./lib/avsddac.lib
   read_liberty -max ./lib/avsdpll.lib
   read_liberty -max ./lib/avsddac.lib
   
   for {set i 1} {$i <= [array size lib_li]} {incr i} {
       read_liberty ./lib_dir/$lib_li($i)
       read_verilog ../output/synth/vsdbabysoc.synth.v
       link_design vsdbabysoc
       read_sdc ./sdc/vsdbabysoc_synthesis.sdc
       check_setup -verbose
       report_checks -path_delay min_max -fields {nets cap slew input_pins fanout} -digits 4 > ./out_sta/min_max_$lib_li($i).txt
       
       exec echo "$lib_li($i)" >> ./out_sta/sta_worst_max_slack.txt
       report_worst_slack -max -digits 4 >> ./out_sta/sta_worst_max_slack.txt
       
       exec echo "$lib_li($i)" >> ./out_sta/sta_worst_min_slack.txt
       report_worst_slack -min -digits 4 >> ./out_sta/sta_worst_min_slack.txt
       
       exec echo "$lib_li($i)" >> ./out_sta/sta_tns.txt
       report_tns -digits 4 >> ./out_sta/sta_tns.txt
       
       exec echo "$lib_li($i)" >> ./out_sta/sta_wns.txt
       report_wns -digits 4 >> ./out_sta/sta_wns.txt
   }
   ```

   **Explanation of the Commands in the Script:**
   - `read_liberty`: Reads a specified timing library file.
   - `read_verilog`: Loads the synthesized Verilog netlist for analysis.
   - `link_design`: Ensures all cells and nets are correctly connected in the design.
   - `read_sdc`: Reads the SDC (design constraint) file.
   - `check_setup`: Checks setup timing violations and provides a detailed output.
   - `report_checks`: Generates a report containing information such as path delays, capacitance, slew, and fanout, with results formatted to four decimal places.
   - `report_worst_slack`, `report_tns`, and `report_wns`: Generate reports on worst-case slack, total negative slack, and worst negative slack across timing paths, with results appended to dedicated output files.

3. **Running OpenSTA with the TCL Script**
   - After creating the TCL script, OpenSTA was run from the `VSDBabySoC/src` directory with the following commands:

   ```bash
   cd VSDBabySoC/src
   sta
   source script.tcl
   ```

4. **Output and Report Generation**
   - Detailed timing reports were generated for each library file, saved in the `out_sta` directory, and named according to the library file used (e.g., `min_max_sky130_fd_sc_hd__ff_100C_1v65.lib.txt`).

5. **Report Analysis and Visualization**
   - The output reports were analyzed to detect setup or hold violations and to evaluate timing characteristics such as worst-case paths, slew rates, and fanout. Visualization tools were used to plot data from the reports, assisting in identifying potential bottlenecks and areas for optimization.

   **Example Report Snapshots and Graphs:**

   - `sta_worst_max_slack`: Visual representation of the worst-case slack for maximum delay paths.
   - `sta_worst_min_slack`: Snapshot of worst-case slack for minimum delay paths.
   - `sta_wns`: Chart of the worst negative slack.
   - `sta_tns`: Graph depicting the total negative slack across timing paths.

   ![image](https://github.com/user-attachments/assets/3df44dcb-1cb2-4214-9eb9-233ff7ed2928)
   
![sta_worst_max_slack](https://github.com/user-attachments/assets/819a273b-ffdb-4ba3-8a5b-fc9c6c1983e5)
![sta_worst_min_slack](https://github.com/user-attachments/assets/2be8727a-cfa7-47e8-8f86-ad9ab7178230)
![sta_wns](https://github.com/user-attachments/assets/ab612b36-61d6-45a3-b24f-6ede7ba1df56)
![sta_tns](https://github.com/user-attachments/assets/a4c4530e-2334-4cd5-8816-dbd29db528f5)

</details>

<details>
	<summary>Task 12 : Advanced Physical Design using OpenLANE - Sky130 </summary>


<br>
 <details>
	 <summary>Day 1 :  Inception of open-source EDA, OpenLANE and Sky130 PDK</summary>
<br>
  
A **QFN-48 Package** (Quad Flat No-leads with 48 connections) is a type of IC packaging that places 48 metal connection pads around the perimeter of the chip without any protruding leads, creating a compact, low-profile package. This design offers high thermal efficiency and superior electrical performance, making it well-suited for densely packed applications where space is limited and effective heat dissipation is needed.

![image](https://github.com/user-attachments/assets/cfd53c2c-cfc9-488f-bf9c-18391bad1ef2)
![image](https://github.com/user-attachments/assets/80ee2ed1-7503-43b9-85e0-eada001cfb8a)

A **Chip**, or integrated circuit (IC), consists of various functional units—like memory, processing units, and input/output (I/O) interfaces—that are embedded in a silicon substrate. Chips are tailored to handle specific applications in electronics, from processing and data storage to communication and control functions.

**Pads** are the small, metallic regions on an IC or package that connect internal circuits to external connections. These areas enable electrical signals to flow in and out of the chip, bridging internal processing to external components.

The **Core** of a chip is where the primary processing units and logic are located. Optimized for both performance and power, it handles the main computing tasks, often integrating components such as arithmetic logic units (ALUs), memory controllers, and cache.

A **Die** is a single, square section of a silicon wafer that holds all the active circuitry for an IC. Each die contains transistors, capacitors, and other components. Once the die is cut from the wafer and packaged, it becomes a functional chip ready for integration into electronic devices.

![image](https://github.com/user-attachments/assets/00d85e01-9693-4a2b-ae48-881dc1c2bba6)

**IPs (Intellectual Properties)** are pre-designed functional modules, such as USB controllers, memory blocks, and processor cores, that are used as building blocks within a chip. Licensing IPs saves time and costs in chip design, as companies can reuse these verified modules across different designs rather than creating each component from scratch.

![image](https://github.com/user-attachments/assets/6232d723-4ebf-4f1e-8407-6c1d82acc064)

---

### From Software Applications to Hardware Execution

To run an application on hardware, software must be translated from a high-level programming language into binary code that hardware can interpret:

![image](https://github.com/user-attachments/assets/a8871bcf-e13e-4233-9e1f-42362faf5b40)


1. **System Software Translation**: The system software layer processes high-level code into machine language. The Operating System (OS), compiler, and assembler play key roles in this transformation.

2. **Execution Process**:
   - The **OS** interprets and organizes high-level application functions written in languages like C, C++, or Java.
   - The **Compiler** translates these into low-level, architecture-specific instructions tailored to the specific hardware in use (e.g., x86, ARM, RISC-V).
   - The **Assembler** then converts these instructions into binary (machine code), which the hardware can read and execute.
   - The **Hardware** finally executes these instructions, performing tasks as dictated by the program.

	![image](https://github.com/user-attachments/assets/73d3ef7f-f149-42c1-97d4-9a87405fed51)


   **Example**: Consider a stopwatch application running on a RISC-V core. The OS takes a function written in C, which is compiled into RISC-V-specific assembly instructions. The assembler converts this into binary code, and the hardware executes it, providing stopwatch functionality.

![image](https://github.com/user-attachments/assets/ed63bcb4-e349-4a74-996b-4d1161d08ae8)

3. **Hardware Translation to Layout**: In hardware design, the RTL (Register Transfer Level) describes functionality in an HDL (Hardware Description Language) like Verilog. This is synthesized into a netlist of gates that are then mapped onto a physical layout, eventually forming the chip.

---

### ASIC Design Flow Overview

The process of creating an ASIC (Application-Specific Integrated Circuit) involves several stages, moving from a high-level description to a final, physical layout for fabrication:

![image](https://github.com/user-attachments/assets/8b723882-fd73-4da3-8af6-a0c90567424b)


1. **RTL Design**: Describes the intended logic and function of the circuit in Verilog or VHDL. This high-level code specifies data paths, logic operations, and interactions between different parts of the chip.

2. **RTL Synthesis**: Transforms the RTL code into a gate-level netlist, which includes standard cells (AND gates, multiplexers, flip-flops). Optimization steps target area, power, and timing constraints to ensure efficient performance.

3. **Floor and Power Planning**: This stage involves arranging the chip’s major functional blocks on the silicon die and defining the power distribution network. Proper floorplanning reduces signal delay and improves power efficiency by placing components in optimal positions.

4. **Placement**: Physical locations are assigned to all cells and modules, minimizing wiring length and ensuring compliance with design constraints. An optimal placement balances performance, area, and power.

5. **Clock Tree Synthesis (CTS)**: CTS creates a clock distribution network to evenly distribute the clock signal to all registers and flip-flops, reducing clock skew to ensure accurate timing across the chip.

6. **Routing**: Connects all components with metal layers, optimizing for signal integrity and avoiding congestion. Routing tools prioritize paths to ensure minimal resistance and signal delay, while following design rules.

7. **Sign-off**: This stage verifies the design for functionality, performance, power consumption, and manufacturability. Checks include timing analysis (meeting setup and hold times), power analysis, and physical verification to ensure compliance with manufacturing constraints.

8. **GDSII File Generation**: The final layout is exported in the GDSII format, which serves as the blueprint for manufacturing the chip. The GDSII file contains all information needed to create the photomasks for the chip fabrication process.

---

### OpenLane ASIC Flow

OpenLane is an open-source ASIC design flow that integrates multiple open-source tools for each stage of the ASIC development process:

![image](https://github.com/user-attachments/assets/063d9773-caeb-488c-9710-e920a014d93f)


1. **RTL Synthesis, Technology Mapping, and Formal Verification**: Yosys (RTL synthesis), ABC (technology mapping, formal verification).
2. **Static Timing Analysis**: OpenSTA is used for timing verification.
3. **Floor Planning**: Tools include init_fp (initial floor planning), ioPlacer (I/O placement), pdn (power grid planning), tapcell (inserts tap cells to manage well contacts).
4. **Placement**: Global placement (RePLace), optional cell resizing (Resizer), OpenDP (detailed placement).
5. **Clock Tree Synthesis**: TritonCTS is used to create a balanced clock tree.
6. **Fill Insertion**: OpenDP manages filler cell placement for uniform density.
7. **Routing**: FastRoute for global routing, TritonRoute for detailed routing.
8. **SPEF Extraction**: OpenRCX extracts Standard Parasitic Exchange Format data for timing analysis.
9. **GDSII Streaming Out**: Magic and KLayout handle layout generation and visualization.
10. **Design Rule Checking (DRC)**: Magic and KLayout perform design rule verification.
11. **Layout vs. Schematic (LVS) Check**: Netgen ensures layout matches schematic.
12. **Antenna Checks**: Magic verifies for antenna effect, crucial for reliability.

---

### OpenLane Directory Structure

The OpenLane directory structure is organized to support open-source ASIC design compatibility and PDK (Process Design Kit) integration:

```
├── OpenLane             -> Main directory to invoke tools (run docker first)
│   ├── designs          -> Holds all design files for the flow
│   │   ├── picorv32a    -> Example design used in workshops or tutorials
│   │   ├── ...          -> Additional designs
├── pdks                 -> Stores PDK-related files, supporting open-source tools
│   ├── skywater-pdk     -> Contains the Skywater 130nm PDK
│   ├── open-pdks        -> Scripts to adapt commercial PDKs for open-source tools
│   ├── sky130A          -> Variant of PDK with open-source compatibility
│   │   ├── libs.ref     -> Node-specific files (e.g., timing libraries, cell LEF files)
│   │   ├── libs.tech    -> Tool-specific files for KLayout, Netgen, Magic, etc.
```

This directory structure ensures compatibility with open-source EDA tools by separating files specific to each design, technology node, and tool.


### OpenLane Synthesis Process for `picorv32a` Design

### **1. Open the OpenLane Directory**
   - Navigate to the OpenLane working directory within the VSD Virtual Box:
     ```bash
     cd Desktop/work/tools/openlane_working_dir/openlane
     docker
     ./flow.tcl -interactive
     ```

### **2. Launch OpenLane in Interactive Mode**
   - Start the Docker container and enter OpenLane's interactive mode:
     ```bash
     package require openlane 0.9
     prep -design picorv32a
     run_synthesis
     ```
![image](https://github.com/user-attachments/assets/bebc9c98-bbcc-4939-a2fd-04ae7ad36c60)

---

### Viewing the Netlist
   - After synthesis completes, go to the folder containing the netlist:
     ```bash
     cd designs/picorv32a/runs/10-11_22-19/results/synthesis/
     gedit picorv32a.synthesis.v
     ```
---
![image](https://github.com/user-attachments/assets/2e92ed31-c58f-416d-8e16-05944f404aeb)
![image](https://github.com/user-attachments/assets/2928bfc1-429d-46b6-a138-1fdc5cdfa9df)


### Viewing the Yosys Report
```bash
cd ../..
cd reports/synthesis
gedit 1-yosys_4.stat.rpt
```

![image](https://github.com/user-attachments/assets/09440b89-005b-4f7a-b4f7-f4d19a51fc67)

```

28. Printing statistics.

=== picorv32a ===

   Number of wires:              14596
   Number of wire bits:          14978
   Number of public wires:        1565
   Number of public wire bits:    1947
   Number of memories:               0
   Number of memory bits:            0
   Number of processes:              0
   Number of cells:              14876
     sky130_fd_sc_hd__a2111o_2       1
     sky130_fd_sc_hd__a211o_2       35
     sky130_fd_sc_hd__a211oi_2      60
     sky130_fd_sc_hd__a21bo_2      149
     sky130_fd_sc_hd__a21boi_2       8
     sky130_fd_sc_hd__a21o_2        57
     sky130_fd_sc_hd__a21oi_2      244
     sky130_fd_sc_hd__a221o_2       86
     sky130_fd_sc_hd__a22o_2      1013
     sky130_fd_sc_hd__a2bb2o_2    1748
     sky130_fd_sc_hd__a2bb2oi_2     81
     sky130_fd_sc_hd__a311o_2        2
     sky130_fd_sc_hd__a31o_2        49
     sky130_fd_sc_hd__a31oi_2        7
     sky130_fd_sc_hd__a32o_2        46
     sky130_fd_sc_hd__a41o_2         1
     sky130_fd_sc_hd__and2_2       157
     sky130_fd_sc_hd__and3_2        58
     sky130_fd_sc_hd__and4_2       345
     sky130_fd_sc_hd__and4b_2        1
     sky130_fd_sc_hd__buf_1       1656
     sky130_fd_sc_hd__buf_2          8
     sky130_fd_sc_hd__conb_1        42
     sky130_fd_sc_hd__dfxtp_2     1613
     sky130_fd_sc_hd__inv_2       1615
     sky130_fd_sc_hd__mux2_1      1224
     sky130_fd_sc_hd__mux2_2         2
     sky130_fd_sc_hd__mux4_1       221
     sky130_fd_sc_hd__nand2_2       78
     sky130_fd_sc_hd__nor2_2       524
     sky130_fd_sc_hd__nor2b_2        1
     sky130_fd_sc_hd__nor3_2        42
     sky130_fd_sc_hd__nor4_2         1
     sky130_fd_sc_hd__o2111a_2       2
     sky130_fd_sc_hd__o211a_2       69
     sky130_fd_sc_hd__o211ai_2       6
     sky130_fd_sc_hd__o21a_2        54
     sky130_fd_sc_hd__o21ai_2      141
     sky130_fd_sc_hd__o21ba_2      209
     sky130_fd_sc_hd__o21bai_2       1
     sky130_fd_sc_hd__o221a_2      204
     sky130_fd_sc_hd__o221ai_2       7
     sky130_fd_sc_hd__o22a_2      1312
     sky130_fd_sc_hd__o22ai_2       59
     sky130_fd_sc_hd__o2bb2a_2     119
     sky130_fd_sc_hd__o2bb2ai_2     92
     sky130_fd_sc_hd__o311a_2        8
     sky130_fd_sc_hd__o31a_2        19
     sky130_fd_sc_hd__o31ai_2        1
     sky130_fd_sc_hd__o32a_2       109
     sky130_fd_sc_hd__o41a_2         2
     sky130_fd_sc_hd__or2_2       1088
     sky130_fd_sc_hd__or2b_2        25
     sky130_fd_sc_hd__or3_2         68
     sky130_fd_sc_hd__or3b_2         5
     sky130_fd_sc_hd__or4_2         93
     sky130_fd_sc_hd__or4b_2         6
     sky130_fd_sc_hd__or4bb_2        2

   Chip area for module '\picorv32a': 147712.918400

```

---
 </details>

<details>
	<summary>Day-2: Good Floorplan vs Bad Floorplan and Introduction to Library Cells</summary>

 
## **Core Concepts in IC Floorplanning**

### 1. **Utilization Factor**
The utilization factor is an essential metric that compares the area occupied by the circuit (netlist) to the total core area of the chip. A higher utilization means more of the chip's area is being used effectively, but over-utilization can cause issues with routing and space for other necessary components.

In ideal scenarios, we'd aim for a utilization factor of 1 (100%), but in reality, a range of **0.5 to 0.6** is preferred to account for buffer zones, routing channels, and the flexibility for adjustments.

#### **Utilization Factor Formula**:
$$
\text{Utilization Factor} = \frac{\text{Area Occupied by Netlist}}{\text{Total Core Area}}
$$

### 2. **Aspect Ratio**
The aspect ratio describes the shape of the chip, calculated as the ratio of height to width. An **aspect ratio of 1** gives a square shape, while other values result in a rectangular layout. The ideal aspect ratio is determined by factors like functionality, packaging, and manufacturing constraints.

#### **Aspect Ratio Formula**:
$$
\text{Aspect Ratio} = \frac{\text{Height}}{\text{Width}}
$$

### **Pre-Placed Cells**
Pre-placed cells are essential functional blocks like memory units, custom processors, and analog circuits, which are manually positioned in fixed locations during the floorplanning stage. These blocks are **critical to the chip's operation** and cannot be moved during the placement and routing phases to ensure their functionality remains intact.

### **Decoupling Capacitors**
- **Purpose**: These capacitors are placed near logic circuits to **smooth out power supply fluctuations** during high-speed switching.
- **Benefits**:
  - Minimize **voltage fluctuations**
  - Reduce **electromagnetic interference (EMI)**
  - Ensure **reliable power delivery**, especially to sensitive circuits

### **Power Planning**
In a well-designed IC, power planning ensures that **VDD and VSS** are distributed evenly across the chip using a **power mesh**. The goal is to provide a stable power supply, minimize **voltage drops**, and optimize the overall **power efficiency** of the design. More power and ground points help reduce the likelihood of instability.

### **Pin Placement**
The placement of **I/O pins** is crucial for the chip’s overall performance. A careful pin distribution minimizes signal integrity issues and heat buildup, which contributes to the chip’s stability and manufacturability.

---

## **Floorplanning with OpenLANE**

### 1. **Set Up OpenLANE**
First, navigate to the OpenLANE directory and initiate the interactive session:
```bash
cd Desktop/work/tools/openlane_working_dir/openlane
docker
```

### 2. **Run OpenLANE Flow**
To prepare the design (`picorv32a`) and begin the floorplanning process, execute the following commands:
```bash
./flow.tcl -interactive
package require openlane 0.9
prep -design picorv32a
run_synthesis
run_floorplan
```
![image](https://github.com/user-attachments/assets/7226b330-ca85-4442-a69c-d79a95cde99f)
![image](https://github.com/user-attachments/assets/226730e7-af1e-453a-bafb-a367dac53d9e)

#### **Floorplan Results**:
After executing the commands, you’ll get the floorplan results, which you can visualize in the OpenLANE output directory.

### 3. **Floorplan Definition**
Once the floorplan is generated, you can inspect the `.def` file:
```bash
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/10-11_22-57/results/floorplan/
gedit picorv32a.floorplan.def
```



![image](https://github.com/user-attachments/assets/6945f225-abb9-4783-b2d8-f1117f1b6ee2)

#### **Floorplan Calculation**:
- **Unit Distance**: 1 micron = 1000 unit distance
- **Die Dimensions**:
  - **Width**: 660,685 unit distance → 660.685 microns
  - **Height**: 671,405 unit distance → 671.405 microns
- **Area of the Die**: 660.685 × 671.405 = 443,587.212 µm²

### 4. **View Floorplan in Magic**
Use Magic to visualize the floorplan:
```bash
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/10-11_22-57/results/floorplan/
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &
```

#### **Floorplan Visualizations**:
Once the command runs, you’ll see a graphical representation of the chip layout in Magic, which shows how cells and components are placed.

![image](https://github.com/user-attachments/assets/de7254ba-a0f5-4175-aa0d-6fa08c9ee907)

---

## **Decap and Tap Cells**

- **Decap Cells**: These are placed near logic cells to manage transient power supply fluctuations and stabilize the power delivery network.
- **Tap Cells**: Used for **connection to the power grid** and to reduce **substrate noise** that could affect the chip's performance.

![Decap and Tap Cells](https://github.com/user-attachments/assets/87f3202e-504c-4a9d-ac3e-4f4512348823)

---

## **Placement Process**

### 1. **Unplaced Standard Cells**
Initially, cells are unplaced, appearing at the origin. These will be moved into place during the placement phase.

![image](https://github.com/user-attachments/assets/ae5804c9-6a0a-40ee-9e20-f007a94792a6)

### 2. **Run Placement**
Execute placement after floorplanning:
```bash
run_placement
```

### 3. **View Placement in Magic**
Once placement is complete, you can view the result using Magic:
```bash
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/17-03_12-06/results/placement/
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &
```

![image](https://github.com/user-attachments/assets/118cce61-6826-4b07-afd0-f5d0f4c43c43)


#### **Placement Visualizations**:
After running the command, you will see how the standard cells are placed on the chip layout.

![image](https://github.com/user-attachments/assets/bdc84efb-2cbf-4585-bade-e9570c356783)

---

## **Cell Design and Characterization Flow**

### 1. **Library Cells** 
A library contains cells that define different functionalities, sizes, and thresholds. It is crucial for creating the building blocks of an IC.

### 2. **Design Flow**:
- **Inputs**: PDKs, SPICE models, DRC, LVS, and user-defined specifications.
- **Steps**:
  - **Circuit design**
  - **Layout design**
  - **Parasitic extraction**
  - **Characterization (timing, noise, and power)**
- **Outputs**:
  - CDL (Circuit Description Language), LEF, GDSII, SPICE netlist, and various characterization files like `.lib` for timing, noise, and power.

---

## **Standard Cell Characterization Flow**

### 1. **Steps in Characterization**:
- **Load Models and Tech Files**: These provide process-specific details.
- **Extract Spice Netlist**: Extract the circuit netlist for analysis.
- **Characterization with GUNA**: Using characterization software like GUNA, the following models are generated:
  - **Timing Models**
  - **Power Models**
  - **Noise Models**

---

## **Timing Parameters**

| **Timing Parameter**         | **Value**          |
|------------------------------|--------------------|
| **Slew Low Rise Threshold**   | 20%                |
| **Slew High Rise Threshold**  | 80%                |
| **Slew Low Fall Threshold**   | 20%                |
| **Slew High Fall Threshold**  | 80%                |
| **Input Rise Threshold**      | 50%                |
| **Input Fall Threshold**      | 50%                |
| **Output Rise Threshold**     | 50%                |
| **Output Fall Threshold**     | 50%                |

### **Propagation Delay**:
Propagation delay is the time it takes for an input signal to propagate and affect the output signal.

$$
\text{Rise Delay} = \text{time(out-fall-thr)} - \text{time(in-rise-thr)}
$$

### **Transition Time**:
Transition time is the time taken for a signal to change between logic levels. This is typically measured between **10% and 90%** or **20% and 80%** of the signal’s swing.

$$
\text{Fall Transition Time} = \text{time(slew-high-fall-thr)} - \text{time(slew-low-fall-thr)}
$$

$$
\text{Rise Transition Time} = \text{time(slew-high-rise-thr)} - \text{time(slew-low-rise-thr)}
$$

</details>


<details>
	<summary>Day 3 : Design library cell using Magic Layout and ngspice characterization</summary>
<br>
A CMOS inverter, which consists of a PMOS and NMOS transistor, is one of the fundamental components in digital circuits. The circuit switches between high and low output states based on the input voltage level. This guide includes details on how to model and simulate this behavior in SPICE.

---

## SPICE Deck Structure

### 1. **Netlist Creation**

The netlist defines the interconnections between the components (PMOS and NMOS transistors) in the inverter circuit. Here’s a sample netlist structure for a CMOS inverter with labeled nodes for easy identification:

- **Input Node:** Input signal (IN)
- **Output Node:** Output of the inverter (OUT)
- **Power Supply Node:** Connected to VDD
- **Ground Node:** Connected to GND

Example of transistor description syntax:
```spice
[component name] [drain] [gate] [source] [substrate] [transistor type] W=[width] L=[length]
```

### 2. **Device Sizing**

Define the Width-to-Length (W/L) ratios of the PMOS and NMOS transistors. Proper sizing ensures balanced drive strength, typically achieved with a PMOS width 2x–3x that of the NMOS.

Example sizing:
```spice
M1 OUT IN VDD VDD PMOS W=2u L=0.25u
M2 OUT IN GND GND NMOS W=1u L=0.25u
```

### 3. **Voltage Levels**

Set the input (gate) voltage and the supply voltage (VDD). Adjust the levels as per design requirements; in this case, we use 2.5V as a supply voltage reference.

Example:
```spice
Vin IN 0 DC 0V
VDD VDD 0 DC 2.5V
```

### 4. **Node Naming**

Assign names to nodes for clarity in the netlist, such as `VDD`, `GND`, `IN`, and `OUT`, which SPICE uses to reference and simulate each component.

![image](https://github.com/user-attachments/assets/79463427-6c48-45f7-b40e-73b4ec978baf)

---

## Model File

Include the MOSFET model file specific to the fabrication technology. For instance, `tsmc_025um_model.mod` contains parameters for 0.25µm technology.

Example:
```spice
.include tsmc_025um_model.mod
```

---

## Simulation Commands

### 1. **DC Analysis: Switching Threshold**

The switching threshold (Vm) is a critical voltage level at which the inverter transitions between logic high and low states. Vm is observed when both transistors are in the saturation region, resulting in high leakage current due to simultaneous conduction.


#### Simulation Steps
- **Load the Circuit**: Load the SPICE deck with `.cir` extension.
- **Run DC Analysis**: Execute a sweep from 0V to 2.5V.
- **Plot Results**: Plot `OUT` vs `IN` to identify the switching threshold.

```spice
source filename.cir
run
setplot dc1
plot OUT vs IN
```

![aa(1)](https://github.com/user-attachments/assets/1a050859-3284-438d-80b5-9f1f125655d8)

Example DC sweep to find the switching threshold:
```spice
Vin IN 0 DC 0V
.dc Vin 0 2.5 0.05
```

![image](https://github.com/user-attachments/assets/d4ae7894-4f4c-4e9a-b22c-dcaa160faf49)


### 2. **Transient Analysis: Propagation Delay**

Transient analysis helps calculate the propagation delay of the inverter by applying a pulse input signal.

Example pulse signal for transient analysis:
```spice
Vin IN 0 pulse 0 2.5 0 10p 10p 1n 2n
.tran 10p 4n
```

![image](https://github.com/user-attachments/assets/ef6c879d-181c-4d2d-a738-c709d8f73e94)


#### Simulation Steps
- **Setup Input Pulse**: Define the pulse with specified rise/fall times and pulse width.
- **Run Transient Analysis**: Execute the transient simulation to observe the inverter’s response.
- **Plot Output**: Analyze `OUT` vs `time` to determine delay characteristics.


![image](https://github.com/user-attachments/assets/0180c876-36a1-4e6e-b796-6d3c36702b34)

---

## Sample SPICE Deck

Combining the above components, here’s a complete SPICE deck for simulating a CMOS inverter:

```spice
*** CMOS Inverter SPICE Simulation ***

* Device Netlist
M1 OUT IN VDD VDD PMOS W=2u L=0.25u
M2 OUT IN GND GND NMOS W=1u L=0.25u

* Voltage Sources
VDD VDD 0 DC 2.5V
Vin IN 0 DC 0V

* Model File
.include tsmc_025um_model.mod

* DC Analysis
.dc Vin 0 2.5 0.05

* Transient Analysis
Vin IN 0 pulse 0 2.5 0 10p 10p 1n 2n
.tran 10p 4n

.end
```

### Interpreting Results
- **Switching Threshold**: Identified at the Vm point, where leakage current is high.
- **Propagation Delay**: Measured from the transient plot, showing the time taken for output transitions.

---

## CMOS Custom Inverter Simulation

### 1. Cloning and Setting Up Custom Inverter

Clone the custom inverter library for use with the OpenLane toolchain:
```bash
cd Desktop/work/tools/openlane_working_dir/openlane
git clone https://github.com/nickson-jose/vsdstdcelldesign
cd vsdstdcelldesign
cp /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech .
magic -T sky130A.tech sky130_inv.mag &
```
![image](https://github.com/user-attachments/assets/51c7f3de-9e1b-4628-a8e8-5512e3b8322d)

![image](https://github.com/user-attachments/assets/2bc8fc3b-0b55-443c-ac11-9eb341a0c139)


**Layout in Magic**:
- Launch Magic with the custom `sky130_inv.mag` layout file.
- Identify the components: `NMOS`, `PMOS`, output `Y`, and connections to `VDD` and `GND`.

### 2. Inverter Layout and CMOS Fabrication Process

#### CMOS Fabrication Process (16-Mask Method)

The CMOS fabrication process is a series of precise, photolithographic and chemical steps to create integrated circuits on a silicon wafer. This process includes multiple layers and utilizes 16 photomasks to define and protect specific areas of the wafer at various stages. Each step in this process is carefully controlled to ensure that the resulting chips are reliable and functionally correct.

1. **Substrate Preparation**: Start with a cleaned, polished silicon wafer as the base.
2. **N-Well Formation**: Form N-well regions in the p-type substrate using ion implantation with phosphorus.
3. **P-Well Formation**: Create P-well regions in n-type areas by implanting boron.
4. **Gate Oxide Deposition**: Deposit a thin silicon dioxide layer as insulation between the gate and channel.
5. **Polysilicon Deposition**: Apply a polysilicon layer to serve as the gate electrode.
6. **Polysilicon Masking and Etching**: Mask and etch to shape polysilicon into gate structures.
7. **N-Well Masking and Implantation**: Define and implant N-well areas with phosphorus.
8. **P-Well Masking and Implantation**: Define and implant P-well areas with boron.
9. **Source/Drain Implantation**: Form source/drain regions by implanting dopants (e.g., arsenic for NMOS, boron for PMOS).
10. **Gate Formation**: Use a mask to define and etch the gate structure.
11. **Source/Drain Masking and Etching**: Etch to expose source/drain areas for further processing.
12. **Contact/Via Formation**: Etch holes to access source, drain, and gate regions for connections.
13. **Metal Deposition**: Deposit metal (e.g., aluminum or copper) to form interconnections.
14. **Metal Masking and Etching**: Mask and etch metal to create interconnect patterns.
15. **Passivation Layer Deposition**: Apply a protective silicon dioxide or nitride layer over the wafer.
16. **Final Testing and Packaging**: Test the wafer, separate functioning chips, and package for deployment. 

![image](https://github.com/user-attachments/assets/6b392b55-1b91-4aef-9926-c8835b543286)

#### Identification of Transistors in the Inverter

In the Magic layout viewer, hover over the required transistor and press `S` to select it. In the Magic terminal, type `what` to get information about the component type.

**NMOS Transistor:**
![NMOS Transistor](https://github.com/user-attachments/assets/0692d81f-63c2-4945-b3d1-699994d46b78)

**PMOS Transistor:**
![PMOS Transistor](https://github.com/user-attachments/assets/73c13e97-c938-4117-811b-3bf87a15cd03)

**Output Node - Y:**
ss![image](https://github.com/user-attachments/assets/7dd9b871-1bb0-48ce-b0b8-edb99a5c543f)


### 3. SPICE Extraction in Magic

1. **Run SPICE Extraction Commands**:
    In Magic’s tkcon window:
    ```tcl
    extract all
    ext2spice cthresh 0 rthresh 0
    ext2spice
    ```

    ![image](https://github.com/user-attachments/assets/7c0726a6-a3e7-49c5-88f6-bb8adce7d8a2)


2. **View SPICE File**:
    The extracted SPICE file (`sky130_inv.spice`) contains transistor models and capacitances.


    ```spice
	* SPICE3 file created from sky130_inv.ext - technology: sky130A
	
	.option scale=10m
	
	.subckt sky130_inv A Y VPWR VGND
	X0 Y A VGND VGND sky130_fd_pr__nfet_01v8 ad=1.44n pd=0.152m as=1.37n ps=0.148m w=35 l=23
	X1 Y A VPWR VPWR sky130_fd_pr__pfet_01v8 ad=1.44n pd=0.152m as=1.52n ps=0.156m w=37 l=23
	C0 VPWR Y 0.117f
	C1 A Y 0.0754f
	C2 A VPWR 0.0774f
	C3 Y VGND 0.279f
	C4 A VGND 0.45f
	C5 VPWR VGND 0.781f
	.ends
    ```

    ![image](https://github.com/user-attachments/assets/26341293-fd5d-4f43-b243-5557d02dd0b3)


### 4. Transient Response Analysis

Modify the SPICE file to perform transient response analysis and determine propagation delay.

Check the dimensions of the box.

![image](https://github.com/user-attachments/assets/39150b1c-3bdf-446f-b580-c22b050297c2)

It is 0.01 microns. This has to accordingly updated in the spice file.

```spice
* SPICE3 file created from sky130_inv.ext - technology: sky130A

.option scale=0.01u

.include ./libs/pshort.lib
.include ./libs/nshort.lib

// .subckt sky130_inv A Y VPWR VGND

M1000 Y A VGND VGND nshort_model.0 w=35 l=23
+  ad=1.44n pd=0.152m as=1.37n ps=0.148m
M1001 Y A VPWR VPWR pshort_model.0 w=37 l=23
+  ad=1.44n pd=0.152m as=1.52n ps=0.156m

VDD VPWR 0 3.3V
VSS VGND 0 0V
Va A VGND PULSE(0V 3.3V 0 0.1ns 0.1ns 2ns 4ns)

C0 VPWR Y 0.117f
C1 A Y 0.0754f
C2 A VPWR 0.0774f
C3 Y VGND 2f
C4 A VGND 0.45f
C5 VPWR VGND 0.781f
// .ends


.tran 1n 20n
.control
run
.endc
.end
```

Run the modified file in ngspice:
```bash
ngspice sky130_inv.spice
```
![image](https://github.com/user-attachments/assets/49f4b402-a478-4b6e-9dad-f2ee78169b26)


**Plot the Waveform**:
```spice
plot y vs time a
```

![image](https://github.com/user-attachments/assets/64ea923b-8f9b-4738-ab0f-6bb43ffcafcd)

| Parameter           | Output                                                                                                                                                | Calculation                                       |
|---------------------|------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------|
| **Rise Time**       | ![Image](https://github.com/user-attachments/assets/335354fa-6ada-4616-9b34-942fcc2fe772)                                                           | 2.23965 ns - 2.18012 ns = 0.05953 ns              |
| **Fall Time**       | ![Image](https://github.com/user-attachments/assets/803fd422-cfa6-4de8-9a65-864952b01522)                                                           | 4.09328 ns - 4.05058 ns = 0.04270 ns              |
| **Cell Rise Delay** | ![Image](https://github.com/user-attachments/assets/b2fa2c94-2b3d-48fe-892a-415a2163d971)                                                           | 2.20721 ns - 2.15001 ns = 0.05720 ns              |
| **Cell Fall Delay** | ![Image](https://github.com/user-attachments/assets/e6c43943-1bb2-45b4-ab1a-0dbd859118b5)                                                           | 4.07532 ns - 4.05 ns = 0.02532 ns                 |


### 5. DRC Rules in Magic

Perform DRC checks on the layout using the following commands in Magic:

1. **Download DRC Tests**:
    ```bash
    cd ~
    wget http://opencircuitdesign.com/open_pdks/archive/drc_tests.tgz
    tar xfz drc_tests.tgz
    cd drc_tests
    gvim .magicrc
    ```

![image](https://github.com/user-attachments/assets/0a396c92-f051-4351-ad84-3df097ed4801)

![image](https://github.com/user-attachments/assets/0bc50206-3564-44d5-b399-9c9fae59a2a9)

Here we can observe that there is a drc error. To fix this add the following commands to the `sky130A.tech` file.

```
spacing xhrpoly,uhrpoly,xpc allpolynonres 480 touching_illegal \
	"xhrpoly/uhrpoly resistor spacing to diffusion < %d (poly.9)"
spacing npres allpolynonres 480 touching_illegal \
	"poly.resistor spacing to N-tap < %d (poly.9)"
```

![image](https://github.com/user-attachments/assets/fb9dc0a6-ccc9-4839-9bd8-2d8eb2947e33)

2. **Load and Check**:
    ```tcl
    tech load sky130A.tech
    drc check
    drc why
    ```

![image](https://github.com/user-attachments/assets/91be0f96-34d1-4f5a-b347-af48e7e60620)
 
</details>

<details>
	<summary>Day 4 : Pre-layout timing analysis and importance of good clock tree</summary>

### 1. **Extracting the `tracks.info` File**
   
Navigate to the required directory and use `less` to view the `tracks.info` file.

```bash
cd Desktop/work/tools/openlane_working_dir/openlane/vsdstdcelldesign
cd ../../pdks/sky130A/libs.tech/openlane/sky130_fd_sc_hd/
gedit tracks.info
```

![image](https://github.com/user-attachments/assets/7fc35868-4cb6-41e0-8382-fb152d9caa32)


### 2. **Setting Grid in tkcon for Local-Interconnect Layer**

In the `tkcon` window, set the grid for the routing tracks on the local-interconnect layer:

```tcl
grid 0.46um 0.34um 0.23um 0.17um
```
![image](https://github.com/user-attachments/assets/58467426-50c1-4994-ba27-8b8d1a534afc)


This sets the pitch for the wires, aligning them with the required guidelines for the local-interconnect layer.

### 3. **Saving and Opening Custom Layout Files**

Save your design with a custom name, then open it with `magic`.

```tcl 
save sky130_achinv.mag
magic -T sky130A.tech sky130_achinv.mag &
```

After saving, type the following in the `tkcon` window to export the LEF file:

```tcl
lef write
```

![image](https://github.com/user-attachments/assets/55d4acaa-fa81-4af3-90c0-10606f6840e5)

![image](https://github.com/user-attachments/assets/d67fc6a6-1335-4560-9170-b4cb662f2366)

### 4. **Modifying `config.tcl` for the OpenLANE Flow**

Modify the `config.tcl` file to set up the environment variables for your design:

```tcl
# Design
set ::env(DESIGN_NAME) "picorv32a"

set ::env(VERILOG_FILES) "./designs/picorv32a/src/picorv32a.v"
set ::env(SDC_FILE) "./designs/picorv32a/src/picorv32a.sdc"

set ::env(CLOCK_PERIOD) "5.000"
set ::env(CLOCK_PORT) "clk"

set ::env(CLOCK_NET) $::env(CLOCK_PORT)

set ::env(LIB_SYNTH) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__typical.lib"
set ::env(LIB_FASTEST) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__fast.lib"
set ::env(LIB_SLOWEST) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__slow.lib"
set ::env(LIB_TYPICAL) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__typical.lib"

set ::env(EXTRA_LEFS) [glob $::env(OPENLANE_ROOT)/designs/$::env(DESIGN_NAME)/src/*.lef]

set filename $::env(OPENLANE_ROOT)/designs/$::env(DESIGN_NAME)/$::env(PDK)_$::env(STD_CELL_LIBRARY)_config.tcl
if { [file exists $filename] == 1 } {
  source $filename
}
```



### 5. **Running the OpenLANE Flow**

Navigate to the OpenLANE directory, start a Docker session, and initialize the OpenLANE flow.

```bash
cd Desktop/work/tools/openlane_working_dir/openlane
docker
./flow.tcl -interactive
```

Add the LEF files to the design:

```tcl
package require openlane 0.9
prep -design picorv32a
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs
run_synthesis
```
![Screenshot from 2024-11-13 00-51-35](https://github.com/user-attachments/assets/a44ad211-1fe4-4edc-bac3-d08cac3f9999)
![Screenshot from 2024-11-13 00-51-46](https://github.com/user-attachments/assets/094c7fa2-955b-417c-9221-21ce0ba81a7a)
![Screenshot from 2024-11-13 00-51-57](https://github.com/user-attachments/assets/a56fecf6-dbe3-43e7-b5a4-4fc29d3bba03)


### 6. **Fixing Slack**

For slack fixing, configure the synthesis strategy and other synthesis parameters before re-running synthesis.

```tcl
./flow.tcl -interactive
package require openlane 0.9
prep -design picorv32a
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs
set ::env(SYNTH_STRATEGY) "DELAY 3"
set ::env(SYNTH_SIZING) 1
run_synthesis
```

![image](https://github.com/user-attachments/assets/9f7afdc0-4c2b-4950-8168-9a85f36d3aa4)
![image](https://github.com/user-attachments/assets/fd5ca063-b79e-4025-9825-694957d9d944)
![image](https://github.com/user-attachments/assets/265e613f-4d9a-4fe0-a5e6-81883ec0ebe6)

### 7. **Floorplanning**

Start the floorplan process. If any issues arise, use individual floorplan commands:

```tcl
	run_floorplan

```

![image](https://github.com/user-attachments/assets/77bcec33-2497-4842-a906-1497863b4ed5)
![image](https://github.com/user-attachments/assets/bc918405-a0c1-4fa1-8c0a-25d103a45a1d)


Alternative commands if facing issues

```tcl
	init_floorplan
	place_io
	tap_decap_or
```

### 8. **Placement**

Continue with the placement step:

```tcl
run_placement
```

![Output of placement run terminal](https://github.com/user-attachments/assets/1a0f35bd-5544-43a9-be1d-31ac5e188664)
![Screen shot generated](https://github.com/user-attachments/assets/a979a6c8-2728-47ed-b9fe-36a134a9e349)


To view the placement in `magic`, open a new terminal and run:

```bash
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/12-11_19-30/results/placement/
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &
```

![Magic output](https://github.com/user-attachments/assets/3a2d7e4f-2b7b-4f48-a7cf-f1639fb520a5)
![IMage shogin custom inverter cell](https://github.com/user-attachments/assets/7cae5409-d6b9-46d9-b73a-ca46c821cb85)

Select the inverter cell by clicking `s`. Now in the console, type expand. 
![image](https://github.com/user-attachments/assets/35fd2dbd-3de1-4d57-8fca-c845636b8c75)

### 9. **Timing Analysis with OpenSTA**

Pre-layout STA will consider clock buffers and RC parasitics. 

If achieving zero WNS after improving timing, perform an initial timing analysis to identify violations in synthesis.

</details>


</details>
