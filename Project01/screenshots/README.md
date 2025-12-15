# Project 01 – Motor Start/Stop with Safety (CODESYS)

## 📌 Description
This project implements a basic industrial motor control using **CODESYS PLC** programming.
The logic includes **Start/Stop control, emergency stop, and self-holding (latching)** behavior,
commonly used in real industrial environments.

This project was developed as part of my learning path in **PLC programming and industrial automation**.

---

## ⚙️ Implemented Logic
- Start push button (Normally Open)
- Stop push button (Normally Closed)
- Emergency Stop (Normally Closed)
- Self-holding memory (RUN_LATCH)
- Motor output controlled by safety conditions

---

## 🧠 Control Philosophy
- The motor starts when the **START** button is pressed.
- The motor remains running using a **latching circuit**.
- The motor stops immediately if:
  - STOP is pressed
  - Emergency Stop is activated

---

## 🛠 Tools Used
- **CODESYS Control Win V3**
- Programming Language: **Ladder Diagram (LD)**

---

## 📂 Project Files
- `Proyecto 01.project` → Complete CODESYS project file
- `screenshots/` → Step-by-step simulation and logic verification

---

## 📸 Screenshots
Screenshots show:
1. Initial state (motor OFF)
2. Start pressed (motor ON)
3. Latching active
4. Stop / Emergency behavior

---

## 🎯 Skills Demonstrated
- PLC programming fundamentals
- Industrial motor control logic
- Safety-oriented design
- Simulation and debugging in CODESYS
- Technical documentation using GitHub

---

## 👤 Author
**Antonio Serrano**  
Electromechanical Engineering Student  
PLC & Industrial Automation

