# AES Verilog Implementation for FPGA

This repository contains a hardware implementation of the Advanced Encryption Standard (AES) algorithm written in Verilog HDL. The design targets FPGA platforms and supports all three key sizes: 128-bit, 192-bit, and 256-bit.

## About This Project

This implementation provides a complete hardware solution for AES encryption and decryption. The design follows the FIPS 197 standard and includes all necessary components for a functional cryptographic accelerator on FPGA hardware.

## Technical Overview

The AES algorithm operates on 128-bit data blocks using symmetric keys of varying lengths. The encryption process consists of four main transformations applied over multiple rounds:

1. Byte substitution using S-box lookup tables
2. Row shifting operations
3. Column mixing in Galois field
4. Round key addition via XOR

The number of transformation rounds varies by key size: 10 rounds for 128-bit keys, 12 rounds for 192-bit keys, and 14 rounds for 256-bit keys.

## Component Architecture

The implementation is modularized into the following Verilog components:

**Core Cryptographic Modules**
- SubTable.v and InvSubTable.v: S-box and inverse S-box lookup tables
- SubBytes.v and InvSubBytes.v: Byte substitution layers
- ShiftRows.v and InvShiftRows.v: Row transformation layers
- MixColumns.v and InvMixColumns.v: Column mixing operations
- KeyExpansion.v: Round key generation from master key
- AddRoundKey.v: State-key combination

**Top-Level Integration**
- AESEncrypt.v: Encryption state machine controller
- AESDecrypt.v: Decryption state machine controller
- AES.v: Main module integrating all components

**Display Utilities**
- DisplayDecoder.v: Seven-segment display driver
- Binary2BCD.v: Binary to decimal conversion for output

## Hardware Requirements

- FPGA development board (DE1-SoC or compatible)
- Quartus Prime Lite Edition or higher
- ModelSim-Altera or compatible simulator
- USB blaster for FPGA programming

## Setup Instructions

1. Obtain the source files from this repository
2. Launch Quartus Prime and create a new project
3. Add all .v files to the project directory
4. Set AES.v as the top-level design entity
5. Configure pin assignments according to your FPGA board documentation
6. Run synthesis and place-and-route
7. Program the FPGA using the Quartus Programmer tool

## Simulation Guide

To verify the design before hardware deployment:

1. Create a ModelSim project
2. Add all Verilog source files
3. Compile the design hierarchy
4. Run testbenches for individual modules (files ending in _DUT)
5. Execute the top-level AES_DUT testbench
6. Compare simulation outputs against NIST test vectors

## File Organization

```
AES-Verilog/
├── Core Components
│   ├── SubTable.v / InvSubTable.v
│   ├── SubBytes.v / InvSubBytes.v
│   ├── ShiftRows.v / InvShiftRows.v
│   ├── MixColumns.v / InvMixColumns.v
│   ├── KeyExpansion.v
│   └── AddRoundKey.v
├── Control Logic
│   ├── AESEncrypt.v
│   ├── AESDecrypt.v
│   └── AES.v
├── Display Interface
│   ├── DisplayDecoder.v
│   └── Binary2BCD.v
└── Documentation
    ├── README.md
    └── LICENSE
```

## Design Specifications

### Key Schedule
The key expansion module generates round keys dynamically:
- AES-128: 44 words (176 bytes) of expanded key material
- AES-192: 52 words (208 bytes) of expanded key material
- AES-256: 60 words (240 bytes) of expanded key material

### Encryption Flow
1. Add initial round key
2. Execute main rounds (9/11/13 iterations):
   - Substitute bytes via S-box
   - Shift rows cyclically
   - Mix columns in GF(2^8)
   - Add round key
3. Final round (excludes column mixing)

### Decryption Flow
1. Add final round key
2. Execute inverse main rounds (9/11/13 iterations):
   - Inverse shift rows
   - Inverse substitute bytes
   - Add round key
   - Inverse mix columns
3. Initial round key addition

## Testing and Verification

Each module includes a corresponding testbench for functional verification. Testbenches follow the naming convention {ModuleName}_DUT.v. Comprehensive testing should include:

- Individual module verification
- Integration testing of encryption/decryption pairs
- Known-answer tests using standard test vectors
- Timing analysis for target clock frequency

## Additional Resources

- NIST FIPS 197: Advanced Encryption Standard specification
- Intel/Altera FPGA documentation
- DE1-SoC board reference materials
- Verilog HDL programming guides

## License

This project is released under the MIT License. Refer to the LICENSE file for detailed terms and conditions.
