# WinCC OA Project (Template) — Digital2Real Project01

This folder contains a **WinCC OA project template** aligned with the Digital2Real OPC UA model and the Node-RED simulation layer.

## Goal
Provide a minimal, scalable WinCC OA structure that can be:
- versioned in Git (portfolio-ready),
- connected to an OPC UA server (Node-RED / PLC),
- extended into a full SCADA application.

## MVP Scope (Phase 1)
Datapoint model for a single motor:
- Run (Bool) — command/state
- Setpoint (Float) — reference
- Speed (Float) — feedback

## Folder structure
- `config/`
  - `datapoints.dpl` → datapoint type definition
  - `opcua_mapping.md` → OPC UA ↔ WinCC OA mapping table
  - `project.conf` → placeholder for project parameters (to be filled when deploying on target PC)
- `panels/` → HMI panels (placeholder until WinCC OA is integrated)
- `scripts/` → DPL scripts (optional, future logic)
- `screenshots/` → evidence for portfolio

## Integration plan
1. Node-RED hosts OPC UA Server at `opc.tcp://127.0.0.1:4841`
2. WinCC OA OPC UA client connects and maps tags to datapoints
3. A minimal panel displays and controls the motor (Run / Setpoint / Speed)

## Notes
This repo stores **template artifacts**. The executable WinCC OA runtime project will be finalized on the target environment (PC personal) once connectivity is validated.
