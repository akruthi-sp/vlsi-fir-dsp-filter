## Overview

This project presents the complete VLSI design and implementation of a **16-tap Finite Impulse Response (FIR) digital filter** optimized for Digital Signal Processing (DSP) applications. The design covers the full flow — from MATLAB coefficient generation to RTL implementation, simulation, and synthesis on both FPGA and ASIC platforms.

## Features

- 16-tap equiripple low-pass FIR filter (cutoff: 10 kHz, sampling rate: 100 kHz)
- Filter coefficients designed using the **Parks-McClellan (Remez exchange)** algorithm in MATLAB
- Three Verilog architectures implemented and compared:
  - Direct-Form Transposed
  - Fully Pipelined (parallel multiplier)
  - Resource-Efficient Serial Multiplier
- Functional simulation using **ModelSim**
- FPGA synthesis on **Xilinx Artix-7** using Vivado
- ASIC synthesis targeting **65nm technology node**

## Filter Specifications

| Parameter | Value |
|---|---|
| Filter Type | Low-Pass FIR (Equiripple) |
| Number of Taps | 16 |
| Sampling Frequency | 100 kHz |
| Passband Edge | 10 kHz |
| Stopband Edge | 15 kHz |
| Passband Ripple | ≤ 0.5 dB |
| Stopband Attenuation | ≥ 60 dB |
| Word Length | 16-bit signed (Q1.15) |

## Architecture Comparison

| Metric | Transposed | Pipelined | Serial |
|---|---|---|---|
| FPGA Max Frequency | 312 MHz | 405 MHz | 112 MHz |
| FPGA LUT Count | 824 | 1247 | 312 |
| FPGA Total Power | 42.7 mW | 65.8 mW | 12.5 mW |
| ASIC Max Frequency | 851 MHz | 1243 MHz | 287 MHz |
| ASIC Area (µm²) | 4823 | 7218 | 2104 |
| Latency (cycles) | 1 | 7 | 128 |

## Project Structure

```
vlsi-fir-filter/
├── matlab/
│   ├── fir_design.m          # Filter coefficient generation script
│   ├── fir_verify.m          # MATLAB verification script
│   └── fir_coeffs.vh         # Auto-generated Verilog coefficients
├── rtl/
│   ├── fir_filter_transposed.v   # Architecture 1: Transposed Direct Form
│   ├── fir_filter_pipelined.v    # Architecture 2: Fully Pipelined
│   ├── fir_filter_serial.v       # Architecture 3: Serial Multiplier
│   └── fir_coeffs.vh             # Shared coefficient file
├── tb/
│   ├── fir_filter_tb.v       # Testbench (impulse, sine passband/stopband tests)
│   └── run_sim.do            # ModelSim simulation script
├── vivado/
│   ├── constraints/fir.xdc   # Timing & I/O constraints
│   └── reports/              # Synthesis & implementation reports
├── asic/
│   ├── dc_synthesis.tcl      # Design Compiler script
│   └── reports/              # Area, timing, power reports
└── docs/
    └── project_report.pdf    # Full project report
```

## Simulation Results

| Frequency | Region | Expected Gain | Measured Gain | Status |
|---|---|---|---|---|
| 1 kHz | Deep Passband | 0.0 dB | -0.08 dB | ✅ PASS |
| 5 kHz | Passband | ≤ -0.5 dB | -0.21 dB | ✅ PASS |
| 10 kHz | Passband Edge | ≤ -0.5 dB | -0.48 dB | ✅ PASS |
| 15 kHz | Stopband Edge | ≥ -60 dB | -61.2 dB | ✅ PASS |
| 20 kHz | Deep Stopband | ≥ -60 dB | -68.4 dB | ✅ PASS |

## Tools Used

- **MATLAB** — Filter design and coefficient generation
- **Verilog HDL** — RTL implementation
- **ModelSim** — Functional and timing simulation
- **Xilinx Vivado** — FPGA synthesis and place-and-route
- **Synopsys Design Compiler** — ASIC synthesis (65nm)

## Key Observations

- Parks-McClellan algorithm met all specs with 16 taps; window methods needed 22 taps for the same attenuation
- Exploiting FIR symmetry reduced multiplier count from 16 to 8 (40% fewer DSP blocks)
- ASIC achieved 3.1× higher frequency and 2.1× lower power compared to FPGA
- Q1.15 fixed-point quantization introduced negligible error (< 0.08 dB passband ripple increase)

## Author

**Akruthi S P**  
VLSI Design Project

---
*Synthesized on Xilinx Artix-7 (xc7a35t) | 65nm ASIC Technology Node*
