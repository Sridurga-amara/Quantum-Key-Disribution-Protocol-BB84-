# Implementation of Quantum Key Distribution Protocol: BB84 on FPGA

## Abstract

This project presents the implementation of the **Quantum Key Distribution (QKD) Protocol: BB84** on an FPGA. A quantum communication methodology has been designed using a cryptographic protocol along with the parallel processing power of FPGAs. A hierarchical module structure has been used to simplify the realization of the algorithm.

- The first submodule handles basis selection and qubit generation at the transmitter (Alice).
- The second submodule deals with basis selection and comparison of received qubits at the receiver (Bob).
- A **spy module** is introduced between Alice and Bob to simulate and analyze various real-world eavesdropping scenarios.
- The third submodule estimates the deviation in received messages and determines whether the communication channel has been hijacked.

The major advantage of using **FPGAs** is their high processing speed. Verilog further aids in flexible and efficient algorithm development.

**Keywords**: Quantum, FPGA, BB84, Cryptographic, Transmission, Verilog

---

## Introduction

With advancements in communication technology, there is an increasing demand for secure encryption techniques to prevent data leakage, especially over classical communication channels.

Quantum cryptography solves this problem by generating a secure key through **Quantum Key Distribution (QKD)** protocols like **BB84**, introduced by **Bennett and Brassard in 1984**. This protocol helps prevent eavesdropping and enhances the security of communication by using **qubits (quantum bits)** transmitted over a quantum channel.

This project presents a Verilog-based hardware implementation of the **BB84 protocol** on **Altera Cyclone IV FPGA**, demonstrating the significance of FPGAs in the field of quantum cryptography.

---

## BB84 Protocol Overview

### A. Communication Channels and Qubits

BB84 uses two communication channels:
- **Quantum Channel**: Used to transmit qubits (polarized photons)
- **Classical Channel**: Used to compare basis choices and validate key bits

The quantum state of each photon depends on:
- The basis chosen (rectilinear or diagonal)
- The bit stream to be transmitted

### B. Data Transmission (Alice to Bob)

- **Alice** selects two bit streams:
  - **A**: Actual data bits
  - **B**: Basis selection bits (0: rectilinear, 1: diagonal)
- Alice encodes each bit into a photon (qubit) based on its basis
- Bob receives these qubits and chooses his own random basis (**B′**) to measure them
- A classical channel is then used to compare Alice's and Bob's basis choices

### C. Error Estimation and Key Validation

- Alice and Bob compare their basis choices over the classical channel
- If the basis matches, the corresponding bits are retained to form the final key
- Error percentage is calculated using Exclusive-NOR comparison between original and received bits
- A high error rate indicates possible **eavesdropping** (based on the **No-Cloning Theorem**), and the session is discarded

---

## Algorithm Architecture

To utilize FPGA parallelism, each function is implemented as an independent module:

### 1. Main Module

- Integrates all submodules and ports
- Inputs:
  - Alice's Bit Streams (A, B)
  - Bob’s Basis Stream (B′)
  - Spy control signal
- Outputs:
  - Received bit stream A′
  - Error analysis result

### 2. Alice Module

- Generates qubits by assigning polarization states to photons
- Takes input bit stream A and basis stream B
- Outputs qubit sequence for transmission

### 3. Bob Module

- Receives qubits and applies randomly selected bases (B′)
- Extracts bit stream A′ from measured qubits
- Incorporates spy logic to simulate eavesdropping by flipping qubit states if the spy signal is high

### 4. Error Estimation Module

- Compares Alice’s original bit stream A and Bob’s received stream A′ using XNOR gates
- Calculates mismatch count to determine presence of a spy
- If error percentage exceeds a defined threshold, the channel is flagged as insecure

---

## Simulation

Simulation was conducted using Verilog testbenches. The waveform results demonstrate:

- Correct generation and transmission of qubits
- Proper bit extraction by Bob
- Accurate detection of errors and eavesdropping scenarios
---

## Advantages

- Hardware-level implementation for faster execution
- Parallel module design increases efficiency and reusability
- Real-time simulation of quantum encryption and eavesdropping scenarios
- Secure communication through quantum-based randomness

---

## Applications

- Quantum-safe encryption systems
- Research in post-quantum cryptography
- Secure government and military communication
- Academic projects exploring the intersection of quantum physics and digital hardware
- Prototyping real-world QKD systems for future telecom networks

---

## Future Improvements

- Implement a complete handshake mechanism for secure key agreement
- Add support for multi-qubit state handling and dynamic basis selection
- Extend to other quantum protocols like E91 and B92
- Improve spy simulation using probabilistic logic instead of inversion
- Interface with external optical hardware (e.g., polarizers and photon detectors)

---

## Tools Used

- **HDL**: Verilog
- **Platform**: Altera Cyclone IV FPGA
- **Simulation**: ModelSim / Vivado
- **Development Tools**: Quartus Prime, ISE, or equivalent

---

## References

1. Bennett, C. H., & Brassard, G. (1984). Quantum cryptography: Public key distribution and coin tossing.  
2. Nielsen, M. A., & Chuang, I. L. (2000). Quantum Computation and Quantum Information.  

