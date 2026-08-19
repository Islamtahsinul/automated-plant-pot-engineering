# Experiment Plan

The project is developed experimentally. Do not integrate a component merely because it works in a tutorial.

## E-01 — ESP32 GPIO and ADC baseline

**Objective:** establish a known-good development environment and learn raw ADC behaviour.

**Equipment:** ESP32, USB cable, potentiometer or known voltage source, multimeter.

**Procedure:** read an adjustable analog voltage and log raw readings; test several GPIOs; record noise and repeatability.

**Measure:** raw counts, voltage, sample-to-sample spread.

**Pass:** stable readings with no unexplained resets or unsafe input voltages.

**Decision:** select the ADC input and sensor interface.

## E-02 — Soil sensor characterisation

**Objective:** compare sensor repeatability and establish actual calibration points.

**Procedure:** measure air/dry, dry potting mix, progressively watered soil, saturated soil, then repeat at the same depth after settling.

**Measure:** raw sensor value, soil state, insertion depth, time, temperature if available.

**Pass:** readings move consistently with moisture and repeat within the project target.

**Decision:** keep, replace or modify sensor; define control thresholds.

## E-03 — Pump current and startup

**Objective:** determine electrical load before selecting the final supply/MOSFET/fuse.

**Procedure:** measure pump startup current and steady-state current at nominal voltage with the real tubing/head arrangement.

**Measure:** voltage, startup current, steady current, duration.

**Pass:** repeatable startup and no supply collapse.

**Decision:** supply and protection ratings.

## E-04 — Pump flow characterisation

**Objective:** determine actual delivered flow.

**Procedure:** collect water for 10, 20, 30 and 60 seconds using final tubing and approximate final head. Repeat at least five times.

**Measure:** volume, time, head, tubing, pump voltage.

**Pass:** enough repeatability to estimate dose time.

**Decision:** pump suitability and dose-control method.

## E-05 — MOSFET switching

**Objective:** verify safe pump switching from ESP32 GPIO.

**Procedure:** connect ESP32 -> gate resistor -> MOSFET -> pump; add gate pulldown and flyback diode. Toggle repeatedly while observing pump and supply.

**Measure:** gate state, pump current, supply voltage, MOSFET temperature.

**Pass:** pump OFF during reset/startup, clean switching, no ESP32 resets, acceptable MOSFET temperature.

**Decision:** retain or replace MOSFET/protection design.

## E-06 — Reservoir level sensor

**Objective:** ensure empty/low condition reliably inhibits watering.

**Procedure:** test full, normal, near-low and empty states repeatedly; include cable movement.

**Measure:** digital state and false transitions.

**Pass:** no false 'water allowed' state when reservoir is below the defined minimum.

**Decision:** sensor placement and software debounce.

## E-07 — Water dose calibration

**Objective:** establish the repeatable amount delivered per pump activation.

**Procedure:** run calibrated pump times and collect output into a graduated container. Repeat at least 10 cycles around the selected dose.

**Measure:** volume per cycle.

**Pass:** target dose within ±10% initially.

**Decision:** fixed-time dose, pulse-based dose or future flow feedback.

## E-08 — Soil response after watering

**Objective:** determine how long the soil takes to redistribute water and how the sensor responds.

**Procedure:** trigger a known dose; log moisture readings immediately, then at 1, 5, 10, 20, 30, 60 minutes.

**Measure:** moisture versus time.

**Pass:** identifiable settling behaviour and no immediate repeated watering.

**Decision:** post-water lockout/settling time.

## E-09 — Automatic cycle

**Objective:** validate the complete control algorithm without leaving it unattended.

**Procedure:** integrate moisture sensor, reservoir switch, pump and firmware. Force dry, wet, low-water and fault conditions.

**Pass:** watering occurs only when permitted; faults inhibit pump; startup is safe.

**Decision:** proceed to integrated prototype.

## E-10 — Long-duration endurance

**Objective:** discover failures that short tests miss.

**Procedure:** run for 7 days minimum, logging moisture, watering events, reservoir level and faults.

**Pass:** no leaks, runaway pump, unexplained resets or persistent sensor drift.

**Decision:** V3 mechanical/electrical changes and final design.
