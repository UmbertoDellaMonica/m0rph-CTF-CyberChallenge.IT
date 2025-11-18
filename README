# m0rph - CTF Reverse Engineering Solver

## 🚩 Project Overview

This repository contains the solution and the technical documentation for the **m0rph** Capture The Flag (CTF) challenge, originally presented by **CyberChallenge.IT**.

The project was developed as part of the **Secure Programming** university exam. The goal was to reverse engineer a 64-bit ELF binary to uncover a hidden flag. By combining static analysis with advanced dynamic debugging techniques, we developed an automated Python script capable of extracting the flag at runtime by inspecting CPU registers.

## 📑 Table of Contents

- [Challenge Context](https://www.google.com/search?q=%23-challenge-context)
- [Tools Used](https://www.google.com/search?q=%23-tools-used)
- [Technical Analysis](https://www.google.com/search?q=%23-technical-analysis)
  - [Static Analysis](https://www.google.com/search?q=%231-static-analysis)
  - [Dynamic Analysis & Obfuscation](https://www.google.com/search?q=%232-dynamic-analysis--obfuscation)
- [The Solution (Automation)](https://www.google.com/search?q=%23-the-solution-automation)
- [Installation & Usage](https://www.google.com/search?q=%23-installation--usage)
- [Team Members](https://www.google.com/search?q=%23-team-members)

-----

## 🧩 Challenge Context

The target is a binary executable named `m0rph`.

- **Objective:** Find the correct input string (Flag) to pass to the binary to unlock a "Success" state.
- **Difficulty:** The binary employs obfuscation techniques, specifically dynamic memory allocation and indirect jumps (`CALL RAX`), rendering standard static disassembly insufficient.

-----

## 🛠 Tools Used

- **[Radare2](https://rada.re/n/):** Primary framework for reverse engineering and binary analysis.
- **[Cutter (Rizin)](https://cutter.re/):** GUI for Rizin/Radare2, used for flow graph visualization.
- **Python:** Used to script the solver and interact with the debugger pipes.
- **Linux Utilities:** `readelf`, `strings`, `ltrace`, `strace`, `hexdump`.

-----

## 🔍 Technical Analysis

### 1\. Static Analysis

We began by gathering information about the file structure and dependencies.

- **File Type:** ELF 64-bit LSB executable, x86-64.
- **Libraries:** Linked against `libc.so.6`.
- **Findings:** Using `readelf` and `strings`, we identified the memory segments. However, the `main` function appeared deceptively simple. Tracing system calls with `strace` and `ltrace` revealed that the program makes heavy use of `malloc` and `mmap`, suggesting that code or data is being generated or moved during execution.

### 2\. Dynamic Analysis & Obfuscation

Using **Cutter** and **Radare2**, we debugged the application to understand the flow.

- **The Obfuscation:** The program does not execute a linear path. Instead, it utilizes `CALL RAX` instructions to jump to code stored in dynamically allocated memory regions. This breaks static disassemblers, which cannot predict the value of `RAX` before runtime.
- **The Comparison Logic:**
    1. The program takes a user input string.
    2. It iterates through a loop (23 iterations).
    3. Inside the obfuscated segment, it compares the character at the current index of our input against a **specific hexadecimal value** stored in memory.
    4. We discovered that the expected character for the current iteration is temporarily held in the **`RDI` register** (or pointed to by it) just before the comparison.

-----

## 🚀 The Solution (Automation)

Since the comparison values are generated at runtime, a brute-force approach is inefficient. We implemented a **Python Solver** that acts as a debugger wrapper.

### How the Script Works

1. **Initialize Debugging:** The script launches the `m0rph` binary via Radare2 pipes with ASLR (Address Space Layout Randomization) disabled to ensure deterministic address mapping.
2. **Locate Obfuscation:** It scans the assembly to find the address of the critical `CALL RAX` instruction within the main loop.
3. **Set Breakpoints:** A breakpoint is set on the `CALL RAX` instruction.
4. **Runtime Inspection:**
      - The script feeds a dummy input to start the process.
      - When the breakpoint is hit, the script queries the **`RDI` register**.
      - It reads the value pointed to by `RDI`, which corresponds to the *correct* ASCII character for that position.
5. **Flag Reconstruction:** The script appends the found character to a string, resumes execution, and repeats the process until the full 23-character flag is reconstructed.

> **Key Command:** `pd 4 @ rdi` (Print Disassembly/Data at RDI) was crucial to reveal the target value during the debugging session.

-----

## 👥 Team Members

This project was created and presented by:

- **Umberto Della Monica** - [GitHub Profile](https://github.com/UmbertoDellaMonica)
- **Gabriele Vittorio Coralluzzo** - [GitHub Profile](https://www.google.com/search?q=https://github.com/Grizzloo)
- **Piero Agosto** - [Github Profile](https://github.com/pierago01)

-----

### Disclaimer

*This project is for educational purposes only, created for the Secure Programming examination. The m0rph binary belongs to CyberChallenge.IT.*

-----
