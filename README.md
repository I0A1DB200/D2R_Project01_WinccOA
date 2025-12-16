# D2R_Project01_WinccOA

Industrial SCADA portfolio project built with **Siemens WinCC OA**, integrated with **Node-RED** as IT/OT middleware and **OPC UA** as the common data model.  
Goal: deliver a clean, maintainable, and demonstrable Digital Twin/SCADA stack, with a path to publish selected live telemetry to a web UI.

## Why this project
This repository is designed as a real-world engineering portfolio:
- Clear separation between **SCADA**, **middleware**, and **OPC UA data model**
- Structured project layout (maintainability first)
- Reproducible setup and documentation
- Version-controlled artifacts (flows, nodeset, configs, screenshots)

## High-level Architecture

**WinCC OA (SCADA/HMI)**
- Operator screens, alarms, trends, faceplates
- Connects to OPC UA endpoint(s)

**Node-RED (Middleware)**
- Simulation of process data (when PLC is not available)
- OPC UA Server / Client bridge
- Data shaping and publish/subscribe patterns
- Optional web API exposure (REST/WS) for external visualization

**OPC UA (Data Model)**
- Common namespace, tags, and types
- Vendor-neutral interface to SCADA and web

## Repository Structure

- `01_docs/`  
  Documentation, setup guides, screenshots, notes.

- `02_architecture/`  
  Diagrams, interfaces, naming conventions, tag list specs.

- `03_winccoa_project/`  
  WinCC OA project skeleton:
  - `config/` project configs (tracked)
  - `panels/` UI panels (tracked)
  - `scripts/` CTRL scripts (tracked)
  - `images/` icons/assets (tracked)
  - `dp/` datapoint model assets (tracked)
  - `data/` runtime data (ignored)
  - `logs/` runtime logs (ignored)

- `04_opcua/`
  - `nodeset/` OPC UA nodeset / export (tracked)
  - `certs/` certificates (ignored)

- `05_nodered/`
  - `flows/` exported Node-RED flows (tracked)
  - `docs/` notes and configuration exports (tracked)

- `06_web_integration/`
  Web integration notes, endpoints, examples (future scope).

## Scope (MVP)
1. Create a minimal WinCC OA project:
   - datapoints + basic faceplate
   - alarm + trend example
2. Provide an OPC UA model (namespace + tags)
3. Run Node-RED as OPC UA Server (simulation) and/or bridge
4. Connect WinCC OA to OPC UA and validate live updates

## Roadmap
- **Phase 1 (MVP)**: tags + WinCC OA screens + Node-RED OPC UA Server
- **Phase 2**: alarm/trend strategy + naming conventions + templates
- **Phase 3**: web publication (read-only) via Node-RED API/WS
- **Phase 4**: optional PLC integration (TIA Portal/PLCSIM)

## Requirements
- Windows 10/11
- WinCC OA (recommended: keep the same version across machines for consistency)
- Node.js + npm
- Node-RED
- Node-RED OPC UA nodes (node-opcua based)
- OPC UA client tool (UAExpert or equivalent) for validation

## Quick Start (Node-RED OPC UA Server)
1. Install Node-RED
2. Install OPC UA nodes in Node-RED palette
3. Create an OPC UA Server node:
   - Endpoint example: `opc.tcp://127.0.0.1:4841`
   - Security mode: None (MVP)
   - Allow anonymous: enabled (MVP)
4. Add `OpcUa-Item` nodes representing tags, deploy the flow

## Validation
- Use an OPC UA client (UAExpert) to browse:
  - namespace
  - tags under the configured node path
- Confirm tag value updates live when Node-RED changes payloads

## Notes on Multi-PC Development
This repo is designed to be used across multiple machines:
- Keep tool versions aligned where possible (WinCC OA, Node-RED)
- Do not commit runtime folders (logs/data/certs)
- Export Node-RED flows and commit them to `05_nodered/flows/`

## License
This repository is intended as a personal portfolio project. See `LICENSE` if added later.
