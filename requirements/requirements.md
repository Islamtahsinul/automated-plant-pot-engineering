# Engineering Requirements

## Scope
V1 is a single small potted plant with autonomous irrigation. ESP32 is the controller. Temperature/humidity, display, Wi-Fi dashboard, notifications, battery/solar, water metering and multiple plants are deliberately deferred.

## Assumptions

- A small forgiving edible plant is used; cherry tomato, chilli, basil or similar may be considered after checking local growing conditions. Final plant selection is an experiment decision, not a fixed design input.
- Target pot is approximately 150–200 mm diameter with drainage.
- Reservoir target is approximately 1–2 L for V1.
- Indoor or sheltered operation is assumed during development.
- Mains power is acceptable for the prototype, with a certified low-voltage DC adapter kept away from water.
- Watering volume and moisture thresholds must be experimentally calibrated rather than copied from a generic percentage.

## Functional requirements

| ID | Requirement | Verification |
|---|---|---|
| FR-01 | Measure soil moisture at a configurable interval. | Log readings for 24 h and confirm stable acquisition. |
| FR-02 | Decide whether watering is required using calibrated thresholds. | Dry/wet soil test with known threshold crossings. |
| FR-03 | Switch the pump electronically from an ESP32 GPIO. | Repeated 100-cycle switching test. |
| FR-04 | Deliver a controlled water quantity. | Collect pump output and compare with target volume. |
| FR-05 | Store enough water for unattended operation. | Reservoir endurance calculation and practical trial. |
| FR-06 | Detect low/empty reservoir before a watering event. | Empty/near-empty reservoir test. |
| FR-07 | Prevent repeated watering caused by noise or threshold chatter. | Hysteresis and lockout test. |
| FR-08 | Stop the pump on timeout or detected fault. | Force blocked/empty/no-flow conditions and verify shutdown. |
| FR-09 | Recover to a safe state after ESP32 reboot. | Power-cycle and reset tests. |

## Performance requirements

| ID | Requirement | Initial target |
|---|---|---|
| PR-01 | Soil measurement repeatability | Aim for <5% of calibrated dry-to-wet span under unchanged conditions. |
| PR-02 | Watering repeatability | Aim for ±10% of experimentally established V1 dose. |
| PR-03 | Pump maximum continuous runtime | Configurable hard limit; initial target 30 s. |
| PR-04 | Minimum interval between watering events | Configurable; initial target 6 h. |
| PR-05 | Post-watering settling time | Initial target 10–30 min before evaluating moisture again. |
| PR-06 | Reservoir capacity | Target 1–2 L, sized from measured daily water demand. |

## Electrical requirements

- ESP32 GPIO must never directly drive the pump.
- Pump power must be switched with a suitable logic-level N-channel MOSFET or equivalent low-side switch.
- A flyback diode must be fitted across a brushed DC pump motor unless the selected pump/controller already provides equivalent protection.
- Use a certified low-voltage DC supply; no exposed mains wiring in the plant assembly.
- Provide common ground between ESP32 and the pump switching circuit when using a low-side MOSFET.
- Keep pump wiring physically separated from sensitive sensor wiring where practical.
- Add supply decoupling near the ESP32 and pump switching stage.
- Add fuse/current protection appropriate to the selected supply and pump.

## Mechanical requirements

- Electronics must be above the likely water spill level.
- Tubing must be mechanically retained at the reservoir, pump and outlet.
- The reservoir must be removable/refillable without disturbing electronics.
- The pump must be accessible for inspection and replacement.
- The sensor must be removable and inserted at a repeatable depth/location.
- Water must be delivered into the soil, not onto electronics or the plant stem.

## Safety requirements

- Use only SELV/low-voltage DC inside the wet assembly.
- Use a commercially certified mains adapter for development.
- Do not place the mains adapter or mains connectors inside the reservoir/enclosure.
- Test for leaks before energising the complete system.
- A pump fault must fail toward OFF, not continuous watering.

## Cost requirements

- ESP32 remains the default controller.
- Prefer inexpensive, replaceable sensors and pumps during experimentation.
- Avoid buying V2/V3 features before V1 requirements are validated.
- Target a sensible V1 electronics/fluidics spend of roughly £25–£50 excluding tools and the plant/pot; final cost depends heavily on pump and supplier.

## Reliability requirements

- No watering event may occur solely because of one implausible sensor sample.
- Sensor disconnect/open-circuit behaviour must be detectable or conservatively handled.
- Pump operation must be bounded by both volume/time logic.
- Empty reservoir must inhibit watering.
- Firmware startup state must be pump OFF.
- Configuration values must be centralised and documented.

## Requirement status convention

- **Proposed:** design target not yet experimentally validated.
- **Verified:** test evidence exists in `testing/`.
- **Failed:** requirement not met; design revision required.
- **Deferred:** intentionally moved to a later version.
