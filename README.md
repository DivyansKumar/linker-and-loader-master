# ARM-ELF Toolchain Engine: System Linker & Executable Loader

A low-level system utility developed in **C** for the **ARM Architecture** that simulates the backend of a production-grade toolchain. This project implements manual **ELF (Executable and Linkable Format)** parsing, symbol resolution, and binary relocation to link and execute ARM firmware images on bare-metal targets.

---

## 🎯 Project Objectives & JD Alignment
* **ARM Architecture Mastery:** Implemented instruction-level patching for ARMv7/v8 calling conventions, managing branch offsets and PC-relative addressing during the relocation phase.
* **Toolchain Engineering:** Developed a deep understanding of the **GNU Toolchain** by recreating the internal logic of `ld` (Linker) and `exec` (Loader) for ARM object files.
* **Bootloader Handoff:** Simulated the critical phase of a bootloader where binary segments are transitioned from storage to operating memory before execution.
* **Memory Management:** Engineered logic to handle **LMA (Load Memory Address)** and **VMA (Virtual Memory Address)** mapping, which is essential for SoC memory map design.



## 🛠 Technical Features
* **ELF Parsing Engine:** Decodes ELF headers, section header tables, and symbol tables to extract metadata from `.o` relocatable objects.
* **Global Symbol Resolution:** Handles cross-module function calls and variable references, linking multiple C and Assembly files into a unified execution image.
* **Binary Relocation Logic:** Performs static relocation by patching ARM instructions (such as `BL` and `LDR`) to point to their final resolved memory addresses.
* **Runtime Executive:** Prepares the virtual hardware environment, initializes the stack, and transfers CPU control to the binary's entry point via **QEMU System Emulation**.

## 📂 Project Structure
* **`Linker/`**: Core logic for symbol resolution and instruction patching.
* **`Loader/`**: Memory allocation and binary-to-RAM mapping logic.
* **`Tests/`**: Verification suite using mixed C and ARM Assembly sources.

## 🚀 Build & Integration
This project utilizes the **ARM-GNU Toolchain** to generate source objects for the custom engine.

```bash
# 1. Generate relocatable ELF objects from source
arm-none-eabi-as test1.s -o test1.o 
arm-none-eabi-gcc -r -c -nostdlib -marm test3.c -o test3.o 

# 2. Link and Execute using the ARM-ELF Engine
./run.sh test1.o test3.o
