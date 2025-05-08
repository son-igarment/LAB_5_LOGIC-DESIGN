# 16-bit Floating Point Unit (FPU)
**Author: Phạm Lê Ngọc Sơn**  
**EE4449 - HCMUT**

## Project Overview

This project implements a 16-bit Floating Point Unit (FPU) compliant with the IEEE 754 standard. The FPU can perform basic arithmetic operations on 16-bit floating point operands, with results rounded to the nearest value.

### Features

- **Operations supported:**
  - Addition
  - Subtraction
  - Multiplication
  - Division
- **IEEE 754 compliant** - Follows the 16-bit half-precision floating point format
- **Exception handling** - Detects and flags overflow, underflow, and other exceptional conditions

## Project Structure

```
.
├── add_sub16bit.sv         # 16-bit FP addition/subtraction module
├── divide.sv               # FP division module using radix-2 algorithm
├── fpu_top.sv              # Top-level module integrating all operations
├── multiplication.sv       # FP multiplication module
├── subtract.sv             # Subtraction module
├── tb_add.sv               # Testbench for addition
├── tb_multiply.sv          # Testbench for multiplication
├── div_tb.sv               # Testbench for division
├── fpu_top_tb.sv           # Top-level testbench
└── other testbenches       # Additional verification files
```

## Technical Details

### IEEE 754 16-bit Format
- 1 bit for sign
- 5 bits for exponent (biased by 15)
- 10 bits for mantissa (with implicit leading 1)

### Implementation Algorithms

#### Addition/Subtraction
1. Decode inputs into sign, exponent, and mantissa
2. Align mantissas based on exponent difference
3. Perform addition or subtraction on aligned mantissas
4. Normalize result and adjust exponent
5. Round to nearest value
6. Check for exceptions
7. Encode back to IEEE 754 format

#### Multiplication
1. Extract sign, exponent, and mantissa from inputs
2. Multiply mantissas
3. Add exponents (subtract bias)
4. Normalize and round result
5. Check for overflow/underflow
6. Encode result in IEEE 754 format

#### Division (Radix-2)
1. Extract components from operands
2. Implement division using the radix-2 restoration algorithm
3. Normalize quotient
4. Round to nearest value
5. Check for exceptions
6. Encode final result

## How to Use

### Simulation
This project is designed for simulation in ModelSim or other SystemVerilog simulators.

1. Load all files into your simulation environment
2. Run the top-level testbench `fpu_top_tb.sv` to verify all operations
3. Individual operation testbenches are provided for focused testing

### Interface
The FPU top module (`fpu_top.sv`) provides the following interface:
```systemverilog
module fpu_top (
  input logic   clk,
  input logic   reset,
  input logic [15:0] a,       // First operand
  input logic [15:0] b,       // Second operand
  output logic [15:0] quotient, product, sum,  // Results
  input logic start,          // Start signal
  output logic [4:0] flags    // Exception flags
);
```

## Verification
The included testbenches verify:
- Basic arithmetic operations with normal values
- Special cases (zero, infinity)
- Overflow and underflow conditions
- Rounding behavior

## Future Improvements
- Pipelining for better performance
- Support for additional operations (square root, etc.)
- Improved exception handling
- Additional rounding modes 