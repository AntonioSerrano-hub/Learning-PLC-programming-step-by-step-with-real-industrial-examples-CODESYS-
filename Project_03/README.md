# PLC Project 03 – Motor Start with Delay and Status Lights

**Platform:** CODESYS Control Win V3
**Language:** Ladder Diagram (LD)

##  Project Overview

This project simulates an industrial motor starter using PLC logic in **CODESYS**, including safety handling, start delay, and visual status indicators.

The objective is to implement a **realistic and industry-oriented control sequence**, fully simulated without physical hardware.

---

##  Functional Description

The motor starts only when all safety conditions are met and after a configurable time delay.

### Inputs (Simulated)
- `START` – Normally Open pushbutton
- `STOP_NC` – Normally Closed stop pushbutton
- `ESTOP_NC` – Normally Closed emergency stop

### Outputs
- `MOTOR` – Motor command output

### InternalLogic
- `RUN_LATCH` – Run seal-in (latching logic)
- `TON_START` – Start-up delay timer (TON)

### Status Indicators
- 🟡 `L_STARTING` – Motor is in start delay
- 🟢 `L_RUN` – Motor running
- 🔴 `L_FAULT` – Stop or emergency stop activated

##  Control Logic Summary

- The motor can only start if `STOP_NC` and `ESTOP_NC` are TRUE.
- Pressing `START` sets the `RUN_LATCH`.
- A TON timer introduces a delay before motor startup.
- While the timer is counting: 
- Yellow light (Starting) is ON.
- When the timer ends: 
-Engine turns ON. 
- Green light (Running) is ON.
- If STOP or ESTOP is pressed: 
-Engine turns OFF. 
- Fault light (Red) is activated.

##  Simulation Environment

- CODESYS Control Win V3
- Fully simulated I/O
- No forced outputs (input-driven logic only)

##  Screenshots

See the **/images** folder for:
- Ladder logic networks
- Simulation showing Starting / Running / Fault states

##  Future Improvements

- Auto/Manual mode
- Fault reset logic
- Timeout fault if engine does not start
-HMI integration
