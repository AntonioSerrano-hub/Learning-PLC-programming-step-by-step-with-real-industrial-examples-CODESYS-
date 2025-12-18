# Project 04 — Motor Control (State Machine) with Manual/Auto Modes — CODESYS (Ladder)

Industrial-style motor control implemented in **CODESYS (LD / Ladder)** and simulated with **CODESYS Control Win**.

## What this project does
This project controls a motor using:
- **Safety chain** with `STOP_NC` and `ESTOP_NC` (NC signals, TRUE = OK)
- **Exclusive operating modes**: Manual vs Automatic
- **Manual latch** (`MAN_LATCH`) triggered by a START pulse
- **Separated command signals** to avoid “coil overwrite” issues:
  - `RUN_CMD_AUTO`
  - `RUN_CMD_MAN`
  - `RUN_CMD = RUN_CMD_AUTO OR RUN_CMD_MAN`
- **State machine (bits)**:
  - `ST_IDLE`
  - `ST_STARTING` (TON delay)
  - `ST_RUNNING`
  - `ST_FAULT`
- **Start delay** using `TON_START` with `START_DELAY = T#5s`
- Motor output enabled only when **RUNNING** and **SAFETY_OK**.

## Simulation (expected behavior)
1. With safety OK (`STOP_NC=TRUE`, `ESTOP_NC=TRUE`), select **MANUAL**.
2. Pulse `START` → `MAN_LATCH` becomes TRUE → `RUN_CMD` becomes TRUE.
3. System enters **STARTING**, the TON counts.
4. After delay, system enters **RUNNING** and `MOTOR` turns ON.
5. If `STOP_NC` or `ESTOP_NC` opens → `SAFETY_OK=FALSE` → **FAULT** → motor turns OFF.

## Files
- `codesys/` : CODESYS project file (`.project`)
- `media/` : Evidence screenshot (Ladder running)

## Tech
- CODESYS (LD / Ladder)
- Simulation: CODESYS Control Win

