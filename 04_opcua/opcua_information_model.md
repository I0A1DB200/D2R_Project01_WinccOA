# OPC UA Information Model (Digital2Real)

This document defines the OPC UA information model used by **D2R_Project01_WinccOA** as a shared contract between:
- **Node-RED** (simulation + middleware)
- **OPC UA Server** (common data model)
- **WinCC OA** (industrial SCADA)
- **Web SCADA** (IT visualization / portfolio)

The goal is to keep naming, types, and semantics stable across all layers.

---

## 1) Endpoint & Namespace

### OPC UA Endpoint (local default)
- Endpoint: `opc.tcp://127.0.0.1:4841`
- Server Name: `Digital2Real-OPCUA` (Node-RED OpcUa-Server node)

### Namespace
- Namespace index: `ns=1` (assumed for Node-RED server default)
- NodeId format: `ns=1;s=<StringNodeId>`

---

## 2) Naming Convention

### Root prefix
All nodes follow:
- `Digital2Real.<Area>.<Asset>.<Signal>`

### Examples
- `Digital2Real.Motor.Run`
- `Digital2Real.Motor.Speed`
- `Digital2Real.Motor.Setpoint`

### Data types
- Boolean: on/off, states, alarms
- Float: engineering values (speed, setpoint, process values)
- Int32: counters, modes, step numbers (optional)

Units should be documented per signal.

---

## 3) Objects & Variables

### 3.1 Motor (baseline demo asset)

| NodeId (String) | Browse Name | Type | Unit | Access | Description |
|---|---|---:|---|---|---|
| `Digital2Real.Motor.Run` | Run | Boolean | - | R/W | Start/Stop command and state. `true` = running. |
| `Digital2Real.Motor.Setpoint` | Setpoint | Float | % (0..100) | R/W | Operator setpoint. Used by simulation/logic. |
| `Digital2Real.Motor.Speed` | Speed | Float | % (0..100) | R | Simulated/real speed feedback. Follows Setpoint when Run = true. |

**Recommended behavior (simulation):**
- If `Run=false` → `Speed=0`
- If `Run=true` → `Speed` ramps to `Setpoint` (optional ramp)

---

### 3.2 Alarms (minimal)

| NodeId (String) | Browse Name | Type | Unit | Access | Description |
|---|---|---:|---|---|---|
| `Digital2Real.Alarms.Active` | Active | Boolean | - | R | Alarm active summary (true if any alarm is active). |
| `Digital2Real.Alarms.MotorOverSpeed` | MotorOverSpeed | Boolean | - | R | True if Speed > configured threshold (simulation). |

---

### 3.3 Diagnostics (optional but recommended)

| NodeId (String) | Browse Name | Type | Unit | Access | Description |
|---|---|---:|---|---|---|
| `Digital2Real.Diagnostics.Timestamp` | Timestamp | String | ISO-8601 | R | Last update timestamp (portfolio / troubleshooting). |
| `Digital2Real.Diagnostics.Quality` | Quality | Int32 | - | R | Simple quality indicator (0=bad, 1=good). |

---

## 4) Read/Write Ownership (Responsibility Matrix)

| Signal | Written By | Read By |
|---|---|---|
| `Motor.Run` | Web / WinCC OA / Node-RED (test) | Node-RED / WinCC OA / Web |
| `Motor.Setpoint` | Web / WinCC OA / Node-RED (test) | Node-RED / WinCC OA / Web |
| `Motor.Speed` | Node-RED (simulation) OR PLC | WinCC OA / Web |
| `Alarms.*` | Node-RED (simulation) OR PLC | WinCC OA / Web |
| `Diagnostics.*` | Node-RED | WinCC OA / Web |

**Rule:** In production, process feedback (e.g., `Speed`) should be written by PLC/OT source. For the portfolio, Node-RED provides a simulated OT source.

---

## 5) Security Profile (portfolio default)

For local portfolio use:
- Security mode: `None`
- Anonymous: `Allowed`

For hardening (future):
- Enable signing/encryption
- Disable anonymous access
- Use certificates + user authentication

---

## 6) Acceptance Criteria (how to validate)

1. OPC UA server is reachable at `opc.tcp://127.0.0.1:4841`
2. UAExpert can browse and read:
   - `ns=1;s=Digital2Real.Motor.Run`
   - `ns=1;s=Digital2Real.Motor.Setpoint`
   - `ns=1;s=Digital2Real.Motor.Speed`
3. Writing `Run=true` and `Setpoint=50` results in `Speed` changing (simulation)
4. WinCC OA can connect and map these nodes to datapoints
5. Web layer can read values (via Node-RED or direct OPC UA gateway)

---

## 7) Planned Extensions (next iterations)

- `Digital2Real.Motor.Torque` (Float, %)
- `Digital2Real.Motor.Mode` (Int32: 0=Manual, 1=Auto)
- `Digital2Real.Line.Section01.*` hierarchical assets
- OEE metrics namespace:
  - `Digital2Real.OEE.Availability`
  - `Digital2Real.OEE.Performance`
  - `Digital2Real.OEE.Quality`
  - `Digital2Real.OEE.Total`

---

## 8) Change Control

All naming/type changes must be reflected in:
- Node-RED flows (server nodes + simulation)
- WinCC OA datapoints and panels
- Web SCADA bindings

Use pull requests from `develop` → `main` to keep the model stable.
