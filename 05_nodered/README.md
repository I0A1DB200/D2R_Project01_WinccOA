# Node-RED (D2R Project01) — OPC UA Simulation Layer

This folder contains the Node-RED layer used as an IT/OT middleware and process simulator for the **Digital2Real OPC UA Information Model**.

## Purpose
Node-RED is used to:
- Host an **OPC UA Server** for development/testing.
- Simulate basic process behavior (first-order dynamics).
- Act as a bridge layer between:
  - WinCC OA (SCADA)
  - Web SCADA / dashboards
  - OPC UA clients (UAExpert)

## Current Scope (MVP)
The initial MVP publishes **3 core tags** under a single namespace:

| Tag | NodeId | Type | Direction |
|---|---|---:|---|
| Run | `ns=1;s=Digital2Real.Motor.Run` | Boolean | Write/Read |
| Setpoint | `ns=1;s=Digital2Real.Motor.Setpoint` | Double | Write/Read |
| Speed | `ns=1;s=Digital2Real.Motor.Speed` | Double | Read (simulated) |

## Flow
Flows are stored in:
- `./flows/d2r_motor_simulation.json`

### Import instructions
1. Install Node-RED and the OPC UA nodes:
   - `node-red-contrib-opcua`
2. In Node-RED: **Menu → Import → Clipboard**
3. Paste the flow JSON and click **Import**
4. Click **Deploy**

### OPC UA Endpoint
Default endpoint used by the flow:
- `opc.tcp://127.0.0.1:4841/UA/Digital2Real`

## Simulation Logic (minimal)
- If `Run=false` → `Speed=0`
- If `Run=true` → `Speed` converges to `Setpoint` using a simple first-order response
- Speed is clamped to `[0..3000]` and rounded to 0.1

## Next Steps
- Validate from UAExpert (browse/read/write)
- Connect WinCC OA OPC UA client and build a minimal panel
- Replace simulated values with PLC or industrial simulator values when available
