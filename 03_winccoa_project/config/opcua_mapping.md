# OPC UA ↔ WinCC OA Mapping (MVP)

## Endpoint (development)
- OPC UA Server: Node-RED
- Endpoint: `opc.tcp://127.0.0.1:4841`
- NodeId format: `ns=1;s=<StringNodeId>`

## Mapping table

| OPC UA NodeId | WinCC OA Datapoint Element |
|---|---|
| `ns=1;s=Digital2Real.Motor.Run` | `Motor_1.Run` |
| `ns=1;s=Digital2Real.Motor.Setpoint` | `Motor_1.Setpoint` |
| `ns=1;s=Digital2Real.Motor.Speed` | `Motor_1.Speed` |

## Notes
- `Motor_1` is the first instance of the `Motor` datapoint type.
- This mapping is intentionally minimal to validate the full chain before scaling.
