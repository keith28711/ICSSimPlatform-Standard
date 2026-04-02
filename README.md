# ICS Sim Platform

Multi-industry Industrial Control System (ICS) simulation platform for cybersecurity training, protocol testing, and network analysis. Simulates 38 subsystems across 9 industrial verticals with realistic process values, alarm behavior, and cross-segment dependencies.

## Features

- **38 subsystems** across 9 industrial verticals with realistic process physics
- **Modbus TCP** server (FC01-FC16) with Float32/Int16 register modes
- **OPC-UA** server with browse, read, write, and subscription support
- **MQTT** publisher with 5 modes: SparkplugB, JSON, Flattened, Plain, UNS (ISA-95)
- **Web dashboard** with live monitoring, alarm management, trend charts, and built-in documentation
- **Alarm simulation** with four severity levels (HH, H, L, LL) and operator acknowledgment
- **Cross-segment dependencies** between related subsystems (e.g., gathering station output drives transmission pipeline)
- **Dual-channel MQTT** with independent publish rates, QoS, and store-and-forward
- **Report-by-Exception (RBE)** filtering for bandwidth-optimized MQTT publishing
- Tiered licensing with demo mode for evaluation
- Self-contained — no runtime installation required

## Supported Industries

| Vertical | Count | Subsystems |
|----------|-------|------------|
| Oil & Gas -- Midstream | 6 | NGGP, NGTS, NGLTF, COGT, RPPT, LNG |
| Oil & Gas -- Upstream | 5 | WELL, ALFT, SEPN, DRIL, OFFS |
| Oil & Gas -- Downstream | 5 | CDU, FCC, BLEN, TERM, PCHM |
| Water / Wastewater | 4 | WTP, DIST, WWTP, LIFT |
| Power Generation | 4 | FGEN, WNDF, SOLR, HYDR |
| Power Transmission | 2 | SUBX, TLNE |
| Power Distribution | 3 | DFDR, DXFM, DER |
| Building Automation | 4 | HVAC, AHU, ELEC, FIRE |
| Manufacturing | 5 | CMPR, BOIL, PKGL, CNC, TANK |

Each subsystem provides 116 Modbus registers with realistic tag names, ranges, and engineering units.

## Quick Start

1. Download the latest release zip from the [Releases](../../releases) page
2. Extract to any directory
3. Run:
   ```
   ICSSimPlatform.exe --system NGGP --devices 5 --alarming t
   ```
4. Open [http://localhost:8090](http://localhost:8090) in your browser

## Examples

| Command | Description |
|---------|-------------|
| `ICSSimPlatform.exe --system NGGP --devices 5` | 5 natural gas gathering & processing devices |
| `ICSSimPlatform.exe --system "NGGP 3, NGTS 2"` | Midstream pipeline with cross-segment dependencies |
| `ICSSimPlatform.exe --system "WTP 1, DIST 3, LIFT 2"` | Water/wastewater system |
| `ICSSimPlatform.exe --system "FGEN 2, SUBX 1, DFDR 3"` | Power generation through distribution |
| `ICSSimPlatform.exe --system CDU --devices 3 --opcua` | Crude distillation with OPC-UA enabled |
| `ICSSimPlatform.exe --system NGGP --mqtt --mqtt-mode Json` | MQTT publishing in JSON format |
| `ICSSimPlatform.exe --system "HVAC 1, AHU 3, ELEC 1"` | Building automation suite |

## Protocols

| Protocol | Port | Default | Notes |
|----------|------|---------|-------|
| Modbus TCP | 502 | Enabled | FC01-FC16, Float32 big-endian |
| OPC-UA | 4840 | Disabled | Enable with `--opcua` |
| MQTT | 1883 | Disabled | Enable with `--mqtt`, requires external broker |
| Dashboard / REST API | 8090 | Enabled | Web UI and JSON API |

## Dashboard

Access at `http://localhost:8090`:

- **Devices** -- Live register values with write support (right-click to write)
- **Alarms** -- Active and historical alarms with acknowledgment
- **Trends** -- Real-time charts for up to 6 tags (10-minute rolling buffer)
- **Health** -- System metrics, protocol status, MQTT bandwidth
- **Docs** -- Built-in subsystem register maps and user guides

## Licensing

| Tier | Max Devices | Subsystems |
|------|-------------|------------|
| Demo | 20 | All 38 (10-minute session limit) |
| Standard | 10 | 14 (Midstream, Water, Building) |
| Professional | 50 | All 38 |
| Enterprise | 9999 | All 38 |

Without a license file the platform runs in Demo mode. Place `license.lic` in the application directory or use `--license <path>`.

## Documentation

See `docs/User_Guide.md` in the release zip for the complete user guide covering configuration, CLI reference, all protocol details, subsystem descriptions, alarm behavior, and troubleshooting.

## License

Copyright (c) 2026 KRB Applied Sciences. All rights reserved.
