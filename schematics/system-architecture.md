# V1 System Architecture

## Design principle

Keep the first system simple enough to understand and instrument. The ESP32 performs sensing, decision-making and pump control. The pump is treated as an electrically noisy load and is isolated from the GPIO by a MOSFET stage.

## Block diagram

```text
                         +----------------------+
                         | Certified DC supply  |
                         |   e.g. 12 V / 1-2 A   |
                         +----------+-----------+
                                    |
                         +----------+-----------+
                         |                      |
                         v                      v
                  +-------------+        +-------------+
                  | Pump power  |        | 5 V / 3.3 V |
                  | rail        |        | regulation  |
                  +------+------+        +------+------+
                         |                      |
                         v                      v
                  +-------------+        +-------------+
                  | DC pump     |        |    ESP32    |
                  +------+------+        +------+------+ 
                         |                      |
                         | water                | GPIO / ADC / I2C
                         v                      |
                  +-------------+               |
                  | Tubing +    |               |
                  | outlet      |               |
                  +------+------+               |
                         |                      |
                         v                      |
                  +-------------+               |
                  | Plant /     |<--------------+
                  | soil        |  moisture data
                  +-------------+

Reservoir --> low-level sensor --> ESP32
ESP32 GPIO --> MOSFET gate --> pump
Pump motor --> flyback diode / suppression --> pump supply
```

## Information flow

1. ESP32 wakes/runs the control loop.
2. Soil moisture is sampled and filtered.
3. Reservoir level is checked.
4. A watering decision is made using calibrated thresholds, hysteresis and lockout time.
5. If watering is allowed, ESP32 enables the MOSFET.
6. Pump runs for a calibrated dose duration or until a hard timeout.
7. Pump turns OFF.
8. System waits for soil moisture redistribution/settling.
9. State and fault information are logged over Serial during development.

## Power architecture

The exact rails are selected after pump measurements. The preferred V1 architecture is a single certified low-voltage supply sized for the pump plus ESP32 regulator input, with the pump on its own high-current branch and the ESP32 on a regulated logic rail. This prevents pump current transients from being allowed to directly determine ESP32 supply voltage.

For a 12 V pump, a 12 V DC supply is the natural starting point. The ESP32 development board is powered through its supported 5 V/USB input or an appropriate regulator. Do not assume a cheap 12 V-to-5 V module is adequate until its output ripple and current capability are tested.

## Sensor architecture

Preferred V1: inexpensive analog capacitive soil sensor feeding an ESP32 ADC-capable input. The DFRobot SEN0193 is a reference-quality option for comparison: it specifies 3.3–5.5 V supply, 0–3.0 V output and 5 mA typical operating current. The project should still test cheaper equivalent probes before committing.

Avoid using the advertised 'percentage' as truth. Store raw calibrated values and derive the watering thresholds for the actual soil/pot/plant combination.

## Actuator architecture

Use a low-side N-channel logic-level MOSFET. The ESP32 drives the gate through a small resistor; a gate pulldown keeps the pump OFF while the ESP32 is booting/reset. The motor has a flyback diode placed physically close to the motor/load wiring.

An IRLZ44N is electrically generous but physically oversized for a small pump; a smaller modern logic-level MOSFET may be cheaper and more appropriate. The selection criterion is low RDS(on) at a gate voltage the ESP32 can actually provide, adequate current margin, and acceptable dissipation.

## Water architecture

Reservoir -> pump inlet -> pump -> tubing -> controlled outlet/distributor -> soil.

A short tube run and modest vertical lift are preferred for V1. Pump flow must be measured under the actual tubing/head condition because catalogue maximum flow is not the delivered flow.

## Deliberately deferred

- Wi-Fi/cloud dashboard
- OLED
- Notifications
- Battery/solar
- Flow meter
- Multiple plants
- Closed-loop PID control

These can be added after V1 demonstrates reliable basic irrigation.
