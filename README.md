# ICS Sim Platform

### Multi-Industry Industrial Control System Simulation Platform

**Version 1.7.5-rc1 | April 2026**

---

## Download

Download the latest release from the [Releases](../../releases) page. The zip contains:

- `ICSSimPlatform.exe` — the simulator (self-contained, no runtime install needed)
- `ICSPoller/` — companion bandwidth benchmarking tool
- `docs/` — user guide, task playbooks, register maps, and feature references
- `LICENSE_AGREEMENT.md` — end user license agreement

## Quick Start

```bash
# Run with 3 gas gathering devices (Modbus TCP on port 502, dashboard on port 8090)
ICSSimPlatform.exe --system NGGP 3

# Open the dashboard
# http://localhost:8090

# Run with MQTT SparkplugB publishing
ICSSimPlatform.exe --system NGGP 3 --mqtt --mqtt-broker localhost:1883

# Run a custom CSV-defined simulation
ICSSimPlatform.exe --custom-sim my_equipment.csv

# See all options
ICSSimPlatform.exe --help
```

The platform runs in demo mode (all features, 10-minute session limit) without a license file. See the [Licensing](#licensing) section for tier details.

## What It Is

ICS Sim Platform is a software-based industrial control system simulator that generates realistic process data across eight major industry verticals. It communicates over the same protocols used by real PLCs, RTUs, and edge gateways — Modbus TCP, OPC-UA, and MQTT/SparkplugB — making it indistinguishable from physical field devices to upstream SCADA, historian, and analytics systems.

The platform runs as a single lightweight executable on Windows, Linux, or macOS. No proprietary hardware, no virtual machines, no complex infrastructure. Configure it, run it, and connect your tools.

---

## The Problem It Solves

Building, testing, and demonstrating industrial software requires process data. Getting that data traditionally means:

- **Physical test labs** — expensive, space-consuming, limited to one or two process types
- **PLC simulators** — vendor-specific, require matching hardware or licensed software
- **Static data files** — no protocol interaction, no write-back, no live behavior
- **Production systems** — risky, access-restricted, and never available when you need them

ICS Sim Platform eliminates these barriers. It provides live, interactive, protocol-correct process simulation that can be started in seconds on any workstation.

---

## Who It's For

**System Integrators** — Develop and test SCADA configurations, HMI screens, and historian connections without waiting for physical hardware. Simulate the customer's exact process environment before going on-site. Use the Custom Simulation feature to define project-specific equipment from a CSV file — no source code changes needed.

**Software Vendors** — Demonstrate your ICS/OT software against realistic multi-industry data. Show prospects how your product handles oil & gas, water, power, BAS/HVAC and manufacturing — all from one platform. Use ICS Poller alongside the simulator to benchmark polling strategies and quantify bandwidth trade-offs for customer proposals.

**Cybersecurity Teams** — Build OT security training labs with realistic protocol traffic. Test IDS rules, practice network forensics, and run tabletop exercises with data that looks and behaves like the real thing. Inject cyberattack scenarios (setpoint manipulation, sensor spoofing, alarm suppression) and validate detection capabilities.

**Training Organizations** — Teach ICS/SCADA fundamentals with hands-on protocol interaction. Students can browse OPC-UA address spaces, decode Modbus registers, and analyze SparkplugB payloads against live simulation.

**Control Engineers** — Prototype and validate control logic against simulated process data before deploying to real equipment. Test alarm configurations, setpoint changes, and dependency behavior. Define custom sequences of operation using the StateMachine behavior for project-specific SOO validation.

**Network Architects** — Evaluate OT-over-WAN designs with simulated cellular, satellite, radio, MPLS, and SD-WAN link characteristics. The Communications Infrastructure overlay adds link health telemetry (latency, jitter, packet loss, RSSI, bandwidth utilization) to every device. Use the dual-channel MQTT architecture and the bundled ICS Poller to measure edge vs. centralized polling bandwidth before committing to a network design.

**AI/ML Engineers** — Build, train, and validate OT-specialized anomaly detection models using the integrated AI testbed pipeline. Generate labeled training data from fault and cybersecurity scenarios, export in standard fine-tuning formats, fine-tune local LLMs on NVIDIA or Apple Silicon, and deploy with continuous monitoring. The entire workflow runs on the same platform that produces the simulated telemetry.

---

## Industry Coverage

38 subsystem definitions organized across eight industry verticals. Each subsystem models the complete register map of a real-world field device with 116 process tags (70 holding registers, 16 coils, 14 input registers, 16 discrete inputs), realistic value ranges, engineering units, and inter-tag dependencies.

| Vertical | Subsystems |
|----------|-----------|
| **Oil & Gas — Upstream** | Wellhead (WELL), Artificial Lift (ALFT), Separator (SEPN), Drilling (DRIL), Offshore Platform (OFFS) |
| **Oil & Gas — Midstream** | Gas Gathering/Processing (NGGP), Gas Transmission (NGTS), NGL Fractionation (NGLTF), Crude Oil Gathering/Transport (COGT), Refined Products Pipeline (RPPT), LNG Facility (LNG) |
| **Oil & Gas — Downstream** | Crude Distillation (CDU), Fluid Catalytic Cracking (FCC), Blending (BLEN), Terminal Operations (TERM), Petrochemical (PCHM) |
| **Water & Wastewater** | Water Treatment Plant (WTP), Distribution System (DIST), Wastewater Treatment (WWTP), Lift Station (LIFT) |
| **Power Generation** | Fossil/Gas Turbine (FGEN), Wind Farm (WNDF), Solar Array (SOLR), Hydroelectric (HYDR) |
| **Power Transmission & Distribution** | Substation (SUBX), Transmission Line (TLNE), Distribution Feeder (DFDR), Distribution Transformer (DXFM), Distributed Energy Resources (DER) |
| **Building Automation** | HVAC Central Plant (HVAC), Air Handling Unit (AHU), Electrical Distribution (ELEC), Fire Alarm/Suppression (FIRE) |
| **Manufacturing** | Compressed Air (CMPR), Industrial Boiler (BOIL), Packaging Line (PKGL), CNC Machine Center (CNC), Storage Tank Farm (TANK) |

Subsystems can be mixed freely. Run a single gas compressor, a full water utility with treatment and distribution, or a combined power generation and transmission grid — all in one instance.

---

## Custom Simulation

Define your own Modbus TCP devices entirely through CSV files — no source code changes, no recompilation. 18 behavior templates across three tiers:

- **Tier 1** — Simple value generators (RandomWalk, SineWave, Mirror, Constant, BoundedRandom, Toggle)
- **Tier 2** — Parameterized equipment templates (PumpVFD, PumpDOL, MOValve, SOValve, Compressor, Setpoint, CircuitBreaker, AnalogActuator, StagedEquipment, TapChanger, Totalizer)
- **Tier 3** — Generic state machine for custom sequences of operation with actions, conditions, and timed transitions

Multi-device support (up to 10 devices per CSV), LLM prompt templates for AI-assisted CSV authoring, and a validate-without-running CLI flag (`--custom-sim-validate`). Use `--custom-sim-list-behaviors` to see every template with its required and optional parameters.

See `Custom_Simulation_Reference.md` for the full specification and `Custom_Simulation_Prompt_Templates.md` for LLM-assisted authoring.

---

## Protocol Support

### Modbus TCP

The industry workhorse. ICS Sim Platform presents as a Modbus TCP server supporting all standard function codes (FC01 through FC16). Connect any Modbus master — Autosol ACM/eACM, Ignition, FactoryTalk, Geo SCADA, or a custom driver — and poll registers exactly as you would a real PLC. The dashboard can generate ready-to-import tag definition files for Autosol eACM and Ignition SCADA, eliminating manual tag configuration.

- Float32 and Int16 register modes with configurable byte order (BigEndian, LittleEndian, MidEndianBig, MidEndianLittle)
- Shared port (unit ID addressing) or per-device port allocation
- Up to 50 concurrent connections
- Full read/write support with immediate process response

### OPC-UA

The modern standard. The platform exposes a fully browsable OPC-UA address space organized by subsystem and device. Tags appear with correct data types, engineering units, and descriptive metadata.

- Browse, Read, Write, and Subscription services
- Hierarchical address space: Platform > Subsystem > Device > Tags
- Configurable security policies (None, Basic256Sha256, or Both)
- Compatible with UA Expert, Prosys Browser, Ignition OPC-UA, KEPServerEX, and other standard clients

### MQTT / SparkplugB

Purpose-built for modern edge and cloud architectures. The platform publishes process data as a SparkplugB edge node — the same topology used by Ignition MQTT Engine, Cirrus Link Chariot, and AWS IoT SiteWise.

- **Full SparkplugB v2.2 lifecycle**: Birth certificates, data messages, death certificates, and command write-back (DCMD/NCMD)
- **Five publish modes**: SparkplugB (Protobuf), structured JSON, flattened topics, plain scalar values, or ISA-95 Unified Namespace (UNS)
- **Dual-channel architecture**: Real-time data delivery (QoS 0) alongside guaranteed historian delivery (QoS 1/2) on independent channels
- **DDATA and NDATA modes**: Per-device publishing (DDATA, default) with individual DBIRTH/DDATA/DDEATH per device, or node-level publishing (NDATA) in the Red Lion Crimson-style edge gateway pattern with all devices as node metrics in a single message
- **SparkplugB metric aliasing**: Birth messages carry name + alias; data messages use numeric alias only, reducing payload size by 40-60%
- **Report-by-exception (RBE)**: Deadband-based filtering publishes only changed values, reducing bandwidth by 80-95% during steady-state operation
- **Broker failover**: Automatic failover between primary and backup brokers with SparkplugB birth storm on reconnect and `bdSeq` session tracking
- **UNS + SparkplugB integration**: Parris-method ISA-95 GroupId encoding and dual-channel hybrid (SpB primary + UNS secondary) for Industry 4.0 architectures
- **Topic flattening bridge**: Parallel plain MQTT client republishes SparkplugB data as individual topics for legacy consumers
- **Network disconnect simulation**: Dashboard-controlled outage simulation with store-and-forward queuing (QoS 1/2) and queue drain on reconnect

### Write-Back Across All Protocols

Every protocol supports bidirectional communication. Write a setpoint via Modbus, toggle a valve via OPC-UA, or send a SparkplugB device command — the simulation responds immediately. Process dependencies propagate: opening a valve increases downstream flow, changing a speed setpoint shifts vibration and temperature values.

---

## Communications Infrastructure Overlay

Simulate the network layer alongside the process layer. The CI overlay adds 30 communication health tags to every device — latency, jitter, packet loss, RSSI, bandwidth utilization, VPN status, and link-up/down indicators — at realistic baselines for five link types:

| Link Type | Typical Latency | Use Case |
|-----------|----------------|----------|
| Cellular (4G/LTE, 5G) | 30-120 ms | Pipeline RTUs, distributed grid assets |
| Satellite (VSAT/LEO) | 500-1500 ms | Remote wellsites, offshore platforms |
| Radio (licensed/mesh) | 20-200 ms | Pipeline corridors, substations |
| MPLS | 5-40 ms | Refineries, large fixed plants |
| SD-WAN | 10-60 ms | Managed WAN for multi-site enterprises |

The `multi` mode auto-assigns realistic link types per subsystem (pipeline corridors get cellular/radio/satellite; refineries get MPLS/SD-WAN). Link failure simulation drives random latency spikes, packet loss events, and disconnects.

---

## ICS Poller — Companion Bandwidth Benchmarking Tool

ICS Poller is a standalone Modbus TCP polling application bundled with ICS Sim Platform. It connects to ICS Sim (or any Modbus TCP server) and measures per-device polling performance:

- **Latency**: Per-poll round-trip time
- **Success rate**: Percentage of successful polls vs. timeouts
- **Bandwidth**: Bytes sent and received per device
- **10-minute rolling charts**: Live visualization of all metrics

Use ICS Poller alongside ICS Sim to benchmark edge vs. centralized polling architectures, validate bandwidth budgets for cellular or satellite backhaul, and produce before/after comparisons for network design decisions.

ICS Poller has its own web dashboard, its own configuration, and runs independently of ICS Sim. It is not license-gated.

---

## Cross-Segment Process Interaction

Subsystems don't operate in isolation. When multiple subsystems run together, the cross-segment dependency engine creates realistic process coupling:

- Gas gathering plant output feeds gas transmission pipeline throughput
- Power generation units drive substation bus voltage
- Water treatment plant output feeds distribution system pressure
- Industrial boiler steam production drives tank farm heating
- HVAC chilled water flows through air handling units
- Fire alarm activation triggers AHU emergency shutdown

These relationships mean that writing a value to one device produces observable, physically-plausible effects on related devices — exactly as in a real industrial process.

---

## Browser Dashboard

A built-in web dashboard provides real-time visibility and control without any additional software.

- **Device overview**: All running devices with subsystem identification, status tags, and alarm counts
- **Register inspection**: Full register tables showing Holding Registers, Coils, Input Registers, and Discrete Inputs with live values, Modbus reference numbers, raw register values, and engineering units
- **Trend charting**: Select tags for real-time trend visualization with 10-minute rolling buffer
- **Alarm management**: Active alarm list with color-coded severity (HH/H/L/LL), right-click acknowledgment, and alarm history
- **MQTT control panel**: Per-channel connection status, message counters (published/dropped/queued/filtered), and disconnect/reconnect simulation controls
- **Protocol status**: Header badges showing Modbus connections, OPC-UA sessions, MQTT state, and license tier at a glance
- **Tag export**: Download ready-to-import tag files — Autosol eACM (.xlsx), Ignition SCADA Modbus (.json), Ignition SCADA OPC-UA (.json), or generic CSV. Addresses are pre-calculated for the active register mode
- **Interactive writes**: Right-click any writable tag to modify its value — coils toggle, holding registers accept numeric input with range validation
- **Documentation viewer**: Built-in markdown reader for user guides, register maps, task playbooks, and the EULA — no need to leave the dashboard
- **Health monitoring**: System metrics including CPU, memory, uptime, protocol connection counts, MQTT bandwidth tracking, and historian statistics

The dashboard is accessible from any browser on the network at the configured port (default 8090).

---

## Steady-State Simulation & Bandwidth Optimization

Real industrial processes don't oscillate continuously. Most of the time, values hold steady at operating setpoints with minor instrument noise. The platform's steady-state simulation mode replicates this behavior:

- Tags hold at realistic setpoints with small deadband noise
- Periodically, values transition smoothly to new setpoints (cosine-interpolated ramp)
- Transition timing is randomized and independent per tag, creating natural-looking process variation

Combined with MQTT report-by-exception filtering, SparkplugB metric aliasing, and NDATA node-level publishing (as an alternative to the default per-device DDATA mode), the full optimization stack reduces MQTT bandwidth by 80-95% compared to unconditional polling. This is exactly how production edge gateways optimize bandwidth over constrained cellular or satellite links.

---

## AI/ML Testbed Capabilities

The platform includes a complete AI/ML pipeline for training and deploying OT-specialized language models. AI features require a Professional or Enterprise license (demo mode includes a 10-minute trial).

- **Time-Series Historian** — InfluxDB 3 Core integration for continuous telemetry capture with event-driven alarm and write-command recording
- **Scenario Engine** — 8 built-in scenarios (normal baseline, 4 operational faults, 3 cyberattacks) with ground-truth labels for supervised training
- **Unified Namespace (UNS)** — ISA-95 topic hierarchy with self-describing JSON payloads for AI-friendly data discovery, plus SparkplugB integration via the Parris method and dual-channel hybrid
- **Training Data Export** — Export labeled training datasets from historian data in standard LLM fine-tuning formats, with configurable sliding-window parameters, normal/fault ratio, and scenario labels
- **LLM Connector** — OpenAI-compatible API integration (Ollama, vLLM, LM Studio, OpenAI, Anthropic) with continuous monitoring mode that surfaces anomaly alerts in real time
- **Fine-Tuning Pipeline** — Unsloth (NVIDIA) and MLX (Apple Silicon) scripts for producing domain-specialized OT-analyst models, with cloud GPU support (RunPod, Lambda Labs)

The end-to-end workflow — from simulated telemetry to deployed fine-tuned model — runs on one platform. See `AI_Testbed_Guide.md` for the complete walkthrough.

---

## Example Use Cases

### SCADA Integration Testing

> *"We're deploying Ignition to a gas gathering station with 6 compressor units and need to validate our tag configuration before going on-site."*

Run `ICSSimPlatform.exe --system "NGGP 6" --mqtt --mqtt-mode SparkplugB` and point Ignition MQTT Engine at the broker. All 6 compressors appear with birth certificates, live data updates, and DCMD write-back support. Test your screens, alarm pipelines, and historian connections against realistic data.

### Custom Equipment Simulation

> *"Our project has non-standard equipment that doesn't match any of the 38 built-in subsystems. We need a pump skid and a metering skid with specific tag layouts."*

Write a CSV file defining both devices with their tags and behaviors. Run `ICSSimPlatform.exe --custom-sim my_project.csv` and the simulator creates exactly what you specified — custom addresses, custom behavior templates, multi-device support — all without touching source code.

### OPC-UA Driver Validation

> *"We need to verify our OPC-UA client handles 20 devices across 4 different subsystem types with correct data types and subscriptions."*

Run `ICSSimPlatform.exe --system "WTP 5, DIST 5, LIFT 5, WWTP 5" --opcua` and connect your client to port 4840. Browse the address space, subscribe to value changes, write setpoints, and verify your driver handles Float, Boolean, and Integer tag types correctly across 2,320 tags.

### Cybersecurity Training Lab

> *"We're running a 2-day ICS security training course and need realistic Modbus traffic for students to analyze with Wireshark and write Snort rules against."*

Run `ICSSimPlatform.exe --system "SUBX 2, DFDR 4, DXFM 3"` on the training network. Students see live Modbus TCP traffic on port 502 — register reads, function code patterns, and response timing that match real power distribution RTUs.

### AI Anomaly Detection Development

> *"We want to build an on-prem LLM that detects compressor valve degradation and cyberattacks in our OT telemetry."*

Use ICS Sim's AI testbed pipeline: generate baseline data, run fault and cyberattack scenarios with the historian recording, export labeled training data, fine-tune a 3B model on a cloud GPU ($2-6), deploy via Ollama, and validate detection rates — all from the same platform that produced the telemetry.

### Edge Gateway Bandwidth Analysis

> *"We need to show a customer how SparkplugB report-by-exception reduces bandwidth over their satellite link compared to flat Modbus polling."*

Configure steady-state simulation with RBE and aliasing enabled. Use ICS Poller to measure centralized polling bandwidth, then compare against the MQTT dashboard's published vs. filtered message counts. The combined measurement — poller bandwidth in, MQTT bandwidth out — gives a complete picture of the optimization.

### Cybersecurity Data Diode PoC/Evaluation

> *"We need to evaluate data diode solutions at the OT edge with realistic ICS traffic volumes and protocol diversity."*

Run `ICSSimPlatform.exe --system "NGGP 10, SUBX 3, WTP 5" --mqtt` to generate live Modbus, OPC-UA, and MQTT traffic across 18 devices and 2,088 tags. Place your data diode between the simulation network and your monitoring infrastructure and validate one-way data flow, protocol translation, and throughput under sustained load.

### Historian Backfill Testing

> *"We need to verify our historian correctly handles store-and-forward backfill when a remote site reconnects after a network outage."*

Configure dual-channel MQTT with QoS 1 on the secondary channel. Use the dashboard to simulate a network disconnect. During the outage, messages queue with original timestamps. Reconnect and watch the queue drain — your historian should show continuous data with the correct gap timestamps from the outage period.

### Multi-Protocol Interoperability

> *"Our gateway product translates between Modbus, OPC-UA, and MQTT. We need all three protocols serving the same data to validate translation accuracy."*

Run `ICSSimPlatform.exe --system "FGEN 2, SUBX 1" --opcua --mqtt`. All three protocols serve the same simulation state simultaneously. Read a value via Modbus, verify it matches OPC-UA, and confirm the SparkplugB payload agrees. Write via one protocol and observe the change reflected across all three.

---

## Licensing

| Tier | Devices | Subsystems | AI Features | Duration |
|------|---------|------------|-------------|----------|
| **Demo** | Up to 20 | All 38 | Yes (10-min trial) | 10-minute sessions |
| **Standard** | Up to 10 | 14 selected | No | Perpetual |
| **Professional** | Up to 50 | All 38 | Yes | Perpetual |
| **Enterprise** | Unlimited | All 38 | Yes | Perpetual |

The platform runs in full-featured demo mode without a license file. Demo mode has a 10-minute session timer with a graceful countdown — enough time to evaluate all features including AI capabilities. Licensed operation removes the time restriction and enforces tier-appropriate limits. AI features (historian, scenario engine, LLM connector, training export) require a Professional or Enterprise license.

---

## System Requirements

- **Operating System**: Windows 10/11, Linux (x64), macOS (ARM64)
- **Runtime**: Self-contained — no .NET SDK or runtime installation required
- **Memory**: ~50 MB base + ~2 MB per simulated device
- **Disk**: ~70 MB (ICS Sim Platform) + ~150 MB (ICS Poller)
- **Network**: Ports 502 (Modbus), 4840 (OPC-UA), 8090 (Dashboard) — all configurable
- **MQTT**: Requires an external MQTT broker (e.g., Mosquitto, HiveMQ, EMQX, Chariot) when MQTT is enabled
- **Historian**: Requires InfluxDB 3 Core when historian features are enabled

---

## Roadmap

The following capabilities are planned for future releases:

- **HMI Process Screens** — SVG-based process schematics for each subsystem type, rendered in the browser dashboard with live data binding
- **DNP3 Protocol** — DNP3 outstation support for power and water subsystems (code complete, pending compatibility testing)
- **BACnet/IP Protocol** — BACnet device support for building automation subsystems with standard object types
- **Docker Containerization** — Multi-architecture Docker images with compose files for single-instance, multi-instance, and cybersecurity lab deployments
- **Additional Protocols** — Emerson ROC/ROC Plus (oil & gas), AB EtherNet/IP CIP (all industry verticals)

---

*For technical documentation, protocol configuration details, task playbooks, and register mapping specifications, see the documentation included with the platform distribution or browse the built-in Docs tab in the dashboard.*
