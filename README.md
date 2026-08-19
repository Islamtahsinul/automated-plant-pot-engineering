# Automated Plant Pot Engineering

A low-cost, engineering-first autonomous plant watering system built around an ESP32.

This repository is the project's engineering source of truth. The objective is not to copy a generic Arduino plant-watering tutorial, but to work from requirements and assumptions through component selection, experiments, calculations, prototypes, validation, failure analysis, and iterative improvement.

## Project Goal

Build a small autonomous plant pot that can:

- Measure soil moisture.
- Decide when watering is required.
- Automatically run a water pump.
- Deliver a controlled amount of water.
- Store water in a reservoir.
- Detect a low/empty reservoir.
- Prevent overwatering and runaway pump operation.
- Operate unattended for extended periods.
- Remain inexpensive, electrically safe, mechanically practical, and maintainable.

## Design Principles

1. **ESP32 first** — use the ESP32 as the controller unless a requirement genuinely cannot be met by it.
2. **Measure before deciding** — component ratings and assumptions must be checked experimentally.
3. **Calibrate the real system** — especially soil moisture and water delivery.
4. **Build incrementally** — prove each subsystem before integrating it.
5. **Design for failure** — protect against empty reservoirs, sensor faults, stuck pumps, leaks, and reboots.
6. **Keep V1 focused** — advanced features such as dashboards, displays, batteries, solar, and multiple plants are deferred until the core system is reliable.
7. **Document decisions** — record what was chosen, why it was chosen, and what evidence supports the choice.

## Current Development Path

**IDEA → REQUIREMENTS → RESEARCH → CALCULATIONS → COMPONENT SELECTION → EXPERIMENTS → PROTOTYPE → TESTING → ITERATION → FINAL DESIGN**

## Initial System Concept

```text
                        INFORMATION FLOW

 [Soil Moisture Sensor] ---- ADC ----┐
                                      │
 [Reservoir Float Switch] -- GPIO ---┤
                                      ▼
                                  [ ESP32 ]
                                      │ GPIO
                                      ▼
                              [ N-MOSFET Switch ]
                                      │
                                      ▼
                                  [ Pump ]
                                      │
                                      ▼
                              [ Tubing / Drip ]
                                      │
                                      ▼
                                    [Plant]

                        WATER / POWER FLOW

 [DC Power Supply]
       ├───────────────> [Pump]
       └──> [Buck Regulator] ──> [ESP32 + Sensors]

 [Reservoir] ──> [Pump] ──> [Plant]
```

This is a **starting architecture**, not the final design. Pump voltage, pump type, moisture sensor, thresholds, watering volume, and power architecture must be validated experimentally.

## Engineering Stages

### Stage 1 — Engineering Problem
Define the target plant, pot scale, watering behaviour, operating environment, constraints, assumptions, and success criteria.

### Stage 2 — Requirements
Create functional, performance, electrical, mechanical, safety, cost, and reliability requirements. Every requirement should have a verification method.

### Stage 3 — System Architecture
Partition the system into sensing, processing, actuation, water handling, power, mechanical structure, and optional communications.

### Stage 4 — Component Research
Compare realistic low-cost ESP32 boards, capacitive soil sensors, pumps, MOSFETs, protection parts, level sensors, power supplies, tubing, reservoirs, wiring, and prototyping hardware.

### Stage 5 — Soil Moisture Sensing
Characterise sensor behaviour in the actual soil. Avoid treating advertised percentages as ground truth. Establish useful dry/wet thresholds from measured data.

### Stage 6 — Pump and Water System
Measure flow and current under the real tubing/head arrangement. Determine the relationship between pump runtime and delivered volume.

### Stage 7 — Electrical Engineering
Develop and verify the power budget, voltage rails, pump driver, flyback protection, grounding, wiring, and fault protection.

### Stage 8 — Control System
Implement filtered sensing, hysteresis, minimum watering interval, maximum pump runtime, reservoir interlock, fault handling, and safe boot behaviour.

### Stage 9 — Mechanical Design
Design the pot/reservoir/pump/tubing arrangement for stability, leak resistance, refill access, maintenance, and safe separation of water and electronics.

### Stage 10 — CAD / Physical Design
Use CAD only where it adds value: custom brackets, mounts, interfaces, and enclosures. Prefer inexpensive off-the-shelf parts for everything else.

### Stage 11 — BOM
Maintain the complete bill of materials, UK prices, supplier references, substitutions, and cost assumptions.

### Stage 12 — Build Strategy
Progress from V0 component experiments through integrated prototypes to a validated final build. Do not add complexity before the previous stage is proven.

### Stage 13 — Experiments
Record controlled experiments for sensor calibration, pump flow, pump current, MOSFET switching, water volume, reservoir detection, and complete watering cycles.

### Stage 14 — Testing and Validation
Trace each requirement to a repeatable test and acceptance criterion.

### Stage 15 — Failure Analysis
Maintain a practical hobby-scale FMEA covering sensor faults, pump faults, leaks, empty reservoirs, blocked tubing, power loss, firmware crashes, and bad measurements.

### Stage 16 — Firmware
Keep the ESP32 firmware simple: sensor acquisition, filtering, state-based watering control, safety limits, error handling, serial diagnostics, and configurable calibration data.

### Stage 17 — Documentation
Keep all engineering evidence in version control, including requirements, research, schematics, calculations, CAD, firmware, experiments, test results, photos, and design decisions.

### Stage 18 — Learning Curriculum
Use the project to learn electronics, embedded systems, sensors, power electronics, motors/pumps, control, fluid flow, CAD, prototyping, testing, and FMEA.

### Stage 19 — Execution Plan
Follow a single chronological build path with explicit decision gates before purchases and before advancing to the next prototype.

### Stage 20 — Final Design
Produce a validated, documented, maintainable physical system rather than a one-off breadboard demonstration.

## Version Strategy

| Version | Purpose | Rule |
|---|---|---|
| V0 | Individual component experiments | No full integration |
| V1 | Sensor + controller proof | Establish reliable measurement |
| V2 | Pump + controller integration | Prove actuation and water delivery |
| V3 | Integrated prototype | Add reservoir protection and robust control |
| V4 | Final physical build | Improve mechanics, wiring, reliability, and documentation |

## V1 Feature Boundary

### Included

- ESP32
- One soil-moisture sensor
- One pump
- One reservoir low-level detector
- Pump switching circuit
- Controlled watering duration
- Safety timeout
- Serial diagnostics
- Local autonomous control

### Deliberately Deferred

- OLED/display
- Phone or web dashboard
- Notifications
- Water consumption logging beyond basic test data
- Manual watering button
- Battery power
- Solar charging
- Multiple plants
- Cloud services

These features can be reconsidered after the core watering system passes validation.

## Engineering Evidence Rules

When documenting a design decision, label information as one of:

- **Verified** — measured on the actual hardware/system or supported directly by a manufacturer datasheet or official technical document.
- **Recommendation** — an engineering choice made for this project based on trade-offs.
- **Assumption** — a value adopted temporarily so work can proceed.
- **Approximation** — an estimate that will be refined by measurement.

Do not silently convert assumptions into facts.

## Repository Layout

```text
.
├── README.md
├── requirements/
├── research/
├── schematics/
├── cad/
├── firmware/
├── calculations/
├── bom/
├── prototypes/
├── experiments/
├── testing/
├── photos/
├── design-decisions/
└── lessons-learned/
```

## Working Method

For every major engineering decision:

1. State the requirement.
2. State the assumptions.
3. Identify candidate solutions.
4. Calculate what can be calculated.
5. Measure what cannot be trusted from a datasheet alone.
6. Select the simplest solution that satisfies the requirement with sensible margin.
7. Record the reasoning.
8. Test it.
9. Update the design if the evidence disagrees with the original assumption.

## Status

Project infrastructure created. The next engineering task is to populate the requirements and research artifacts, then begin V0 experiments before purchasing the final V1 component set.

## License

To be decided when the project reaches a stable reusable release.
