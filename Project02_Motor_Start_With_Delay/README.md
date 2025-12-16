Project 02 – Motor Start with Delay (CODESYS)
Description

This project simulates an industrial motor start with delay using CODESYS PLC programming.

When the Start button is pressed, the motor does not start immediately.
A TON (on-delay timer) introduces a delay before the motor turns ON.

This behavior is common in real industrial systems to protect equipment and avoid high inrush current.

Main Features

Start / Stop / Emergency Stop logic

Self-holding (latching) control

Start delay using TON timer

Ladder Diagram (LD)

Fully simulated in CODESYS

Logic Summary

Start button activates a run request (latch)

TON timer starts counting (5 seconds)

After the delay, the motor turns ON

Stop or Emergency Stop immediately turns everything OFF

Tools

CODESYS Control Win V3

Ladder Diagram (IEC 61131-3)

Author

Antonio Serrano
Electromechanical Engineering Student
