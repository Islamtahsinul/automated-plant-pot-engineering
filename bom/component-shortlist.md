# Component Shortlist — V1

Prices are planning estimates, not purchase quotes. Recheck supplier price/stock before ordering.

## 1. Controller

### Preferred
**ESP32 development board** — target ~£5–£15 depending on board/supplier.

Why: sufficient GPIO/ADC/I2C, inexpensive, Wi-Fi/BLE available for future V2, strong development ecosystem. There is no compelling V1 reason to move to a more expensive MCU.

### Options
- ESP32 DevKit-style board: cheapest practical learning option.
- Espressif official DevKit: better provenance and documentation, usually more expensive.
- ESP32-C3 DevKit: lower-cost/smaller alternative, but check ADC/GPIO needs and board availability.

Current UK reference: The Pi Hut lists an Espressif ESP32 Development Board - Developer Edition at £14.40 and ESP32-C3 DevKitM-1 at £15 at time of research.

## 2. Soil moisture sensor

### Preferred low-cost class
**Analog capacitive soil moisture sensor**, approximately £2–£8.

Why: avoids the exposed electrodes and progressive corrosion associated with basic resistive probes.

### Reference-quality alternative
DFRobot SEN0193. Manufacturer specifies 3.3–5.5 V input, 0–3.0 V output and 5 mA operating current. It is useful as a known reference against cheaper probes.

### Alternative
Adafruit STEMMA Soil Sensor. More expensive, I2C and polished, but unnecessary for the cheapest V1. Adafruit explicitly notes that its capacitive reading should not be treated as guaranteed volumetric water content.

## 3. Pump

### Preferred architecture
Small **12 V DC pump**, selected only after measuring required flow and head.

For V1, a small diaphragm/peristaltic pump or low-cost mini water pump is acceptable if it can deliver a repeatable dose at the measured head. Peristaltic is attractive because the fluid path is isolated and it can self-prime, but commercial units are expensive; therefore cheap hobby pumps should be characterised experimentally rather than rejected or accepted on name alone.

### Reference
RS lists Verder 12 V peristaltic pumps, but these are industrial products costing roughly £220+ inc VAT, far beyond the project budget. They are useful only as a benchmark for what properly specified pump data looks like.

## 4. Pump switch

### Preferred
Logic-level N-channel MOSFET, selected for:
- RDS(on) specified at <= 3.3/4.5 V gate where possible
- current rating comfortably above measured pump current
- voltage rating above supply with transient margin
- low thermal dissipation

**IRLZ44N** is a safe learning/reference part and is explicitly specified as logic-level, with 35 mΩ max RDS(on) at 4.5 V and 55 V VDS. It is larger than needed for a tiny pump, so a smaller logic-level MOSFET may be the final choice.

## 5. Flyback diode

1N5819 Schottky diode is a reasonable low-cost starting point for small brushed DC loads if its current and reverse-voltage ratings are confirmed against the selected pump. Use a higher-rated diode if the pump's measured current/transients require it.

## 6. Reservoir level sensor

### Preferred V1
Simple float switch, approximately £2–£5.

Why: cheap, simple, binary and easy to understand. V1 only needs a reliable 'do not water' condition when the reservoir is low.

### Alternatives
- Conductive water-level probe: cheaper but water chemistry/corrosion and false readings are concerns.
- Capacitive level sensor: more complex and unnecessary for V1.

## 7. Power

### Preferred
Certified 12 V DC mains adapter sized after pump-current measurement. Initial planning target: 12 V, 1–2 A.

Do not purchase the final adapter until Experiment 3 measures pump current and startup behaviour.

## 8. Tubing

Food-safe or aquarium-grade silicone/vinyl tubing matched to the pump fittings. Initial target ID ~3–4 mm for a small system, but pump fitting compatibility takes priority.

## 9. Reservoir

Off-the-shelf 1–2 L plastic container with secure lid/opening. No reason to 3D-print the water tank for V1.

## 10. Wiring/protection

- Breadboard for low-current logic experiments only.
- Dupont/jumper wires for V0/V1.
- Screw terminals or JST-style connectors for the pump/wet-side wiring.
- 100–330 ohm gate resistor.
- 47–100 kOhm MOSFET gate pulldown.
- 100 nF decoupling near sensor/logic where appropriate.
- 220–1000 uF bulk capacitor near pump supply/switching stage as testing indicates.
- Small inline fuse or resettable protection sized from measured current and wiring capability.

## Cheapest sensible configuration

ESP32 DevKit-style board + cheap analog capacitive probe + inexpensive 12 V mini pump + small logic-level MOSFET + Schottky flyback diode + float switch + existing plastic container + silicone tubing + existing 12 V adapter if suitable.

Planning target: approximately £20–£35 in new parts if bargain components and existing tools/supply are available.

## Recommended learning configuration

ESP32 DevKit + known-good capacitive sensor reference + inexpensive 12 V pump + logic-level MOSFET + float switch + certified 12 V supply + removable reservoir + test tubing.

Planning target: approximately £35–£60 excluding tools and plant/pot.

## Do not buy yet

- OLED
- flow meter
- battery/solar hardware
- Wi-Fi dashboard hardware
- multiple sensors
- custom PCB
- expensive industrial pump
- 3D-printed enclosure

Buy these only when a validated requirement emerges.
