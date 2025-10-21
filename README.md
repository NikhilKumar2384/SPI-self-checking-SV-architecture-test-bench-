# 🧩 SPI Protocol – Transaction-Based Testbench

This repository contains a **SystemVerilog transaction-level testbench (TB)** for verifying the **SPI (Serial Peripheral Interface)** protocol.  
The project demonstrates how to build a modular, reusable **verification environment** using **SystemVerilog OOP concepts** such as classes, transactions, and mailboxes.

---

## 📘 Project Overview

The **SPI Protocol** is a synchronous serial communication interface used for short-distance communication between a **master** and **slave** device.  
This project implements and verifies SPI behavior using a **layered testbench** approach.

The verification environment includes:
- **Driver, Monitor, Generator, Scoreboard, Interface, Environment, and Transaction** classes.
- Separate **Master and Slave** design modules.
- A **Top-Level Testbench** connecting all components.

---

## 🧱 Repository Structure

SPI-Protocol-TB/

│

├── master # SPI Master module (DUT)

├── slave # SPI Slave module (DUT)

│
├── interface tb # Interface connecting DUT and TB

├── transaction tb # Defines SPI transaction (data item class)

├── generator tb # Randomly generates SPI transactions

├── driver tb # Drives signals to DUT using transaction data

├── monitor tb # Monitors DUT outputs and collects data

├── scoreboard tb # Compares expected vs actual results

├── environment tb # Connects all components into one environment

├── test tb # Defines specific test scenario(s)

├── top testbench tb # Top-level TB connecting DUTs and environment

│

└── README.md # Project description (this file)




---

Or you can upload it directly to your repo under `/docs/` or `/waveforms/` and link it here.

---

## 🚀 Key Features

- Transaction-level verification using **SystemVerilog classes and mailboxes**
- Supports **configurable SPI modes** (CPOL/CPHA)
- Modular and reusable **verification components**
- Easy to scale for **multiple SPI slaves**
- Clean **OOP-based structure**

---

## 🧰 Tools Used

- **Language:** SystemVerilog  
- **Simulator:** EDA Playground
- **Waveform Viewer:** EDA Playground
- **Version Control:** Git + GitHub  

---
