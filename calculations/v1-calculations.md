# V1 Engineering Calculations

These are calculation templates. Measured values replace assumptions as the project progresses.

## 1. Water dose from pump flow

Measure collected water volume `V` over time `t`.

`Q = V / t`

For example, if 100 mL is collected in 20 s:

`Q = 100 / 20 = 5 mL/s = 0.30 L/min`

For a target dose `Vdose`:

`t_dose = Vdose / Q`

Do not use the pump's advertised maximum flow as `Q`.

## 2. Head pressure

Approximate static head:

`H_static = vertical lift from reservoir water surface to outlet`

The actual pump must overcome static head plus tubing/friction losses. For V1, keep the vertical lift small and measure actual delivered flow at the real geometry.

## 3. Daily water requirement

Measure reservoir mass/volume loss over several days while the plant is healthy.

`Daily demand = water added - unused reservoir water change`

For an intended unattended duration `D` days and safety factor `SF`:

`Reservoir volume >= Daily demand × D × SF`

Initial design SF: 1.5–2.0, then revise from evidence.

## 4. Pump energy/power

Measure pump voltage and current while pumping under the actual load.

`P_pump = V_pump × I_pump`

For an estimated daily runtime `t_day`:

`E_day = P_pump × t_day`

Because watering is intermittent, average power can be much lower than instantaneous pump power.

## 5. Supply sizing

For a supply voltage `Vs`:

`I_supply >= I_pump_startup + I_logic_margin`

Use measured startup current, not only running current. Target at least ~25–50% current headroom for a hobby prototype.

## 6. MOSFET dissipation

Once `RDS(on)` is known at the actual gate voltage:

`P_MOSFET = I_pump^2 × RDS(on)`

For example, at 0.5 A and 0.1 ohm:

`P = 0.5^2 × 0.1 = 0.025 W`

The actual MOSFET must be evaluated using its datasheet RDS(on) condition. Do not use the headline current rating alone.

## 7. Diode selection

Check:
- reverse-voltage rating > pump supply with margin
- average forward-current rating >= relevant motor current
- pulse/transient capability appropriate to motor switching

The selected diode must be checked against the actual pump before finalising.

## 8. Soil calibration

Record raw sensor readings at:

1. sensor in air/dry reference;
2. sensor in the actual dry potting mix;
3. soil at the desired 'water now' condition;
4. soil immediately after watering;
5. soil after settling.

For an approximately linear mapping between two reference readings:

`moisture_index = 100 × (raw_dry - raw) / (raw_dry - raw_wet)`

Clamp the result to 0–100% for display only. The control algorithm should preferably use raw/calibrated thresholds rather than pretending this is true volumetric water content.

## 9. Hysteresis

Define:

- `DRY_THRESHOLD`: watering permitted below this point.
- `WET_THRESHOLD`: watering lockout above this point.

Require `DRY_THRESHOLD < WET_THRESHOLD` in the chosen moisture scale. This prevents rapid pump cycling around a single threshold.

## 10. Watering safety limits

Every pump activation must have:

- target dose time;
- hard maximum runtime;
- minimum time since previous watering;
- reservoir-OK condition.

A first-pass hard timeout of 30 s is deliberately conservative and should be revised after pump characterisation.

## 11. Measurement uncertainty

For each important measured quantity record:

`value ± estimated uncertainty`

Examples: pump volume ± cylinder resolution; current ± meter accuracy; sensor reading ± observed repeatability.

## 12. Calculations still required before final purchase

- pump flow at selected tubing/head;
- pump startup and steady-state current;
- required watering dose;
- expected watering frequency;
- reservoir endurance;
- MOSFET dissipation;
- supply current margin;
- fuse/protection rating;
- ADC/input-voltage safety for the selected sensor;
- enclosure and reservoir dimensions.
