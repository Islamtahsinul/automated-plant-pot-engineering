# V1 Test and Validation Plan

Tests are linked to requirements. Record actual results in dated test logs rather than editing the procedure to fit the result.

| Test ID | Requirement | Procedure | Measurement | Pass/fail criteria |
|---|---|---|---|---|
| T-01 | FR-01 | Run moisture sensor continuously for 30 min in unchanged soil. | Raw sensor readings and spread. | Stable enough for chosen filter; no unexplained jumps. |
| T-02 | FR-02 | Move soil through calibrated dry/wet conditions. | Sensor value and watering decision. | Decision changes at documented thresholds. |
| T-03 | FR-03 | Command 100 pump ON/OFF cycles. | Successful cycles, resets. | 100/100 correct switching; no unsafe boot state. |
| T-04 | FR-04 | Collect output for 10 watering cycles. | Volume per cycle. | Dose within ±10% target initially. |
| T-05 | FR-05 | Calculate and validate reservoir endurance. | Daily water consumption. | Reservoir meets target unattended duration with safety margin. |
| T-06 | FR-06 | Test full/low/empty reservoir states. | Level sensor state and pump command. | Pump inhibited in low/empty state every trial. |
| T-07 | FR-07 | Hold soil around threshold. | Number of watering events. | No rapid repeated cycling; hysteresis works. |
| T-08 | FR-08 | Simulate stuck/blocked/slow pump by restricting outlet or timing. | Runtime and fault state. | Pump stops at hard timeout and enters safe fault state. |
| T-09 | FR-09 | Reboot ESP32 during idle and watering. | Pump state after reboot. | Pump is OFF on boot; system returns to safe control state. |
| T-10 | Electrical safety | Measure supply during pump start/stop. | Voltage/current. | No unacceptable supply collapse or ESP32 resets. |
| T-11 | Mechanical safety | Fill reservoir and run system for 24 h. | Leaks, wet areas, tubing movement. | Zero water reaches electronics. |
| T-12 | Reliability | Run integrated system for >=7 days. | Resets, faults, water use, sensor drift. | No uncontrolled watering, significant leaks or unexplained failures. |
| T-13 | Sensor reliability | Repeat dry/wet calibration after extended operation. | Calibration drift. | Drift is documented and remains within project tolerance or triggers replacement. |
| T-14 | Pump reliability | Perform repeated pump cycles under normal load. | Flow and current over cycles. | No significant degradation or unsafe temperature. |
| T-15 | Failure mode: disconnected sensor | Disconnect sensor during operation. | Firmware state/pump command. | System does not interpret invalid input as 'water now'. |
| T-16 | Failure mode: empty reservoir | Remove water during an allowed watering event. | Pump command/runtime. | System inhibits or terminates watering safely. |
| T-17 | Failure mode: tube disconnected | Remove outlet tubing in a controlled test. | Pump runtime and water location. | Pump remains bounded by timeout; spill remains away from electronics. |
| T-18 | Power loss | Remove and restore power repeatedly. | Pump state and firmware state. | No uncontrolled pump activation; normal recovery after restore. |

## Test record template

For every test record:

- Test ID
- Date/time
- Hardware revision
- Firmware revision
- Configuration/calibration values
- Equipment used
- Procedure deviations
- Raw measurements
- Result
- Pass/fail
- Photos where useful
- Follow-up decision

## Acceptance philosophy

Passing means the evidence supports the requirement under defined conditions. A component that works once is not considered reliable. Repeated tests, boundary conditions and fault injection are part of V1 validation.
