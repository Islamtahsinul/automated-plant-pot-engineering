# V1 Bill of Materials

## Essential

| Item | Qty | Planning price | Selection rule |
|---|---:|---:|---|
| ESP32 DevKit-style board | 1 | £5–£15 | Genuine/reputable board; exposed GPIO; USB programming |
| Analog capacitive moisture sensor | 1 | £2–£8 | 3.3 V-compatible output; repeatable; replaceable |
| 12 V DC mini pump | 1 | £5–£12 | Flow/head proven by experiment |
| Logic-level N-MOSFET | 1 | £0.50–£2 | Low RDS(on) at 3.3/4.5 V gate |
| Flyback diode | 1 | £0.10–£0.50 | Current/reverse voltage appropriate to pump |
| Float switch | 1 | £2–£5 | Normally-open/closed behaviour verified before installation |
| 12 V certified DC adapter | 1 | £8–£15 | Sized after pump current measurement |
| Tubing | 1–2 m | £2–£5 | Fits pump and outlet securely |
| Reservoir | 1 | £0–£5 | Reusable plastic container acceptable |
| Jumper/signal wiring | 1 set | £2–£5 | Existing stock preferred |
| Gate resistor + pulldown | 1 set | <£1 | 100–330 R + 47–100 kR starting values |
| Bulk capacitor | 1 | <£1 | ~220–1000 uF starting range; validate by scope/multimeter behaviour |
| Small fuse/protection | 1 | £1–£2 | Size from measured current and wiring |

**Expected new-parts total:** approximately £25–£50 depending on pump, ESP32 board, supply and what is already available.

## Optional V1

- Breadboard: £3–£7
- Screw/JST connectors: £2–£5
- Simple water outlet/distributor: £1–£3
- Waterproof project box: £5–£10, preferably only after the electronics work

## V2 candidates

- OLED display
- temperature/humidity sensor
- manual watering button
- Wi-Fi telemetry/dashboard
- water consumption measurement
- better enclosure
- PCB instead of breadboard

## V3 candidates

- notifications
- battery operation
- solar charging
- multiple plants
- more sophisticated water-level measurement
- flow-feedback control

## Substitutions

If a suitable 12 V adapter already exists, verify its output voltage, current rating, polarity and certification before reuse. If a pump is already available, characterise it rather than buying another one immediately.

A cheap resistive moisture probe may be used for V0 learning, but it should not become the final sensor without a deliberate corrosion/longevity decision.
