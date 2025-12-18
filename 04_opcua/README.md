# OPC UA Information Model — Digital2Real Project01

## Scope
Minimal OPC UA model for validating end-to-end integration:
Node-RED → OPC UA → WinCC OA → Web SCADA.

## Namespace
- Namespace Index: `ns=1`
- Namespace URI: `urn:digital2real:project01`

## Objects

### Motor
Represents a single motor instance.

| Variable | Type | Access | Description |
|--------|------|--------|-------------|
| Run | Boolean | R/W | Start/Stop command |
| Setpoint | Float | R/W | Speed reference |
| Speed | Float | R | Actual speed feedback |

## NodeIds (String-based)

- `ns=1;s=Digital2Real.Motor.Run`
- `ns=1;s=Digital2Real.Motor.Setpoint`
- `ns=1;s=Digital2Real.Motor.Speed`

## Notes
This model is intentionally simple and frozen for MVP validation.
