# Automated Plant Pot Engineering

A low-cost, engineering-first autonomous plant watering system built around an ESP32.

This repository is the project's engineering source of truth. The objective is not to copy a generic Arduino plant-watering tutorial, but to work from requirements and assumptions through component selection, experiments, calculations, prototypes, validation, failure analysis, and iterative improvement.

## Project Goal

Build a small autonomous plant pot that can measure soil moisture, decide when watering is required, automatically run a pump, deliver a controlled amount of water, store water in a reservoir, detect a low/empty reservoir, prevent overwatering/runaway operation, operate unattended, and remain inexpensive, electrically safe, mechanically practical, and maintainable.

## Design Principles

1. **ESP32 first** — use the ESP32 unless a requirement genuinely cannot be met by it.
2. **Measure before deciding** — verify important assumptions experimentally.
3. **Calibrate the real system** — especially soil moisture and water delivery.
4. **Build incrementally** — prove each subsystem before integration.
5. **Design for failure** — handle empty reservoirs, sensor faults, stuck pumps, leaks and reboots.
6. **Keep V1 focused** — dashboards, displays, batteries, solar and multiple plants are deferred.
7. **Document decisions** — record what was chosen, why, and what evidence supports it.

## Development Path

**IDEA → REQUIREMENTS → RESEARCH → CALCULATIONS → COMPONENT SELECTION → EXPERIMENTS → PROTOTYPE → TESTING → ITERATION → FINAL DESIGN**

## Research Incorporated

The current research has been converted into project artifacts rather than remaining as background reading:

- [`requirements/requirements.md`](requirements/requirements.md) — requirements and verification targets
- [`schematics/system-architecture.md`](schematics/system-architecture.md) — V1 architecture and power/water/information flow
- [`bom/component-shortlist.md`](bom/component-shortlist.md) — component options and selection logic
- [`bom/bom-v1.md`](bom/bom-v1.md) — V1 bill of materials and cost targets
- [`calculations/v1-calculations.md`](calculations/v1-calculations.md) — calculation templates and decision variables
- [`experiments/experiment-plan.md`](experiments/experiment-plan.md) — subsystem experiments and decision gates
- [`testing/test-plan.md`](testing/test-plan.md) — requirement-linked validation tests

These documents are living engineering records. Measured results should replace assumptions as the build progresses.

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
                              [ Tubing / Outlet ]
                                      │
                                      ▼
                                    [Plant]

                        WATER / POWER FLOW

 [DC Power Supply]
       ├───────────────> [Pump branch]
       └──> [Regulator] ──> [ESP32 + Sensors]

 [Reservoir] ──> [Pump] ──> [Plant]
```

This is a starting architecture, not the final design. Pump voltage/type, sensor, thresholds, watering volume and power architecture must be validated experimentally.

## V1 Design Direction

The current recommended direction is:

- ESP32 DevKit-style board.
- Analog capacitive soil moisture sensor.
- Small 12 V DC pump selected from measured flow/head/current requirements.
- Logic-level N-channel MOSFET low-side switch.
- Flyback diode for a brushed DC pump unless equivalent suppression is built into the selected hardware.
- Simple float switch for reservoir low-level detection.
- Certified low-voltage DC supply.
- 1–2 L removable plastic reservoir as the initial design range.
- Simple tubing and off-the-shelf pot/reservoir hardware.

This is a recommendation, not a final purchase list. The pump and supply remain decision-gated by experiments.

## Version Strategy

| Version | Purpose | Rule |
|---|---|---|
| V0 | Individual component experiments | No full integration |
| V1 | Measurement/controller proof | Establish reliable sensor data |
| V2 | Pump/controller integration | Prove switching and water delivery |
| V3 | Integrated prototype | Add reservoir protection and robust control |
| V4 | Final physical build | Improve mechanics, wiring, reliability and documentation |

## V1 Feature Boundary

### Included

- ESP32
- One soil-moisture sensor
- One pump
- One reservoir low-level detector
- Pump switching/protection circuit
- Controlled watering duration
- Safety timeout
- Serial diagnostics
- Local autonomous control

### Deferred

- OLED/display
- Phone/web dashboard
- Notifications
- Manual watering button
- Battery/solar
- Multiple plants
- Cloud services
- Flow meter/closed-loop flow control

## Engineering Evidence Rules

Label information as:

- **Verified** — measured on actual hardware/system or directly supported by a manufacturer datasheet/official technical document.
- **Recommendation** — an engineering choice made for this project based on trade-offs.
- **Assumption** — a temporary value adopted so work can proceed.
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
9. Update the design if evidence disagrees with the original assumption.

## Current Status

Project infrastructure is established and the research has now been translated into the core engineering artifacts. **Next gate: run V0 experiments and resolve the pump, sensor and power decisions before buying the final V1 set.**

## License

To be decided when the project reaches a stable reusable release.
