# ACE Micro V2

**A 500 g autonomous sumobot built to compete, learn, and push the limits of a compact robot.**

ACE Micro V2 is part of the ACE competitive robotics lineup, featuring a **black-and-neon-green color scheme**, custom 3D-printed chassis, and DIY silicone wheels. It runs on an Arduino Nano, uses Sharp IR sensors for opponent detection, and prioritizes edge avoidance before committing to an attack.

It's also my favorite robot in the ACE lineup — a project shaped by iteration, experimentation, and the lessons that come from building hardware that has to perform under pressure.

Got any questions? Get in touch with me via:
email: daphneuniceacena@gmail.com 
instagram: @d_4cena 

---

## Overview

| | |
|---|---|
| **Project** | ACE Micro V2 |
| **Category** | Autonomous sumobot |
| **Weight class** | 500 g |
| **Controller** | Arduino Nano |
| **Chassis** | Custom 3D-printed |
| **Design** | Matte black and neon green |

## Hardware Specifications

| Component | Specification |
|:--|:--|
| Microcontroller | Arduino Nano |
| Motor driver | TB6612FNG dual H-bridge |
| Drive motors | JGA25, 620 RPM rated no-load speed |
| Battery | 11.1 V LiPo, 3S |
| Opponent detection | 5 × Sharp IR sensors |
| Edge detection | 2 × IR line sensors |
| Start module | JSumo start module |
| Wireless interface | Bluetooth module |
| Logic power | LM2596 buck converter |
| Wheels | DIY silicone wheels |
| Target weight | 500 g |

## Design Evolution

ACE Micro wasn't built perfectly on the first try. Each revision was an opportunity to identify a weakness and make a deliberate improvement.

### V1 — Finding the Weakness

The first version used pink silicone wheels. During testing, other robots could push ACE Micro around too easily. That made traction a key area to investigate rather than simply assuming that more motor speed would solve the problem.

### V2 — Improving Traction and Handling

V2 introduced several changes:

- **Neon-green silicone wheels:** Cast on the third attempt, using less hardener in the wheel mixture.
- **Refined chassis:** The chassis took three 3D prints to get right.
- **Rear carry handle:** Added a green handle after noticing other teams using zip ties to lift their robots.
- **Consistent visual identity:** Black and neon green became the defining colors of the ACE Micro.

The wheel change is also an opportunity to investigate the relationship between wheel material, grip, and pushing performance through measurements rather than assumptions.

## Engineering Notes

ACE Micro is not just about assembling components. Its design involves understanding the physical limits of the drive system, power electronics, and sensors.

### 1. Wheel Speed

The theoretical linear speed of a driven wheel is

\[
v=\frac{\pi Dn}{60}
\]

where:

- \(v\) = linear speed in m/s
- \(D\) = wheel diameter in meters
- \(n\) = wheel speed in RPM

For a 40 mm wheel running at the motors' rated 620 RPM no-load speed:

\[
v=\frac{\pi(0.040)(620)}{60}
\approx 1.30\text{ m/s}
\]

This is a theoretical estimate, not a measured robot speed. Actual performance depends on motor loading, wheel slip, battery voltage, and drivetrain losses.

### 2. Traction and Pushing Force

The maximum theoretical friction force available from the wheels can be approximated by

\[
F_{\max}=\mu mg
\]

For a 0.5 kg robot:

\[
F_{\max}=\mu(0.5)(9.81)
=4.905\mu\text{ N}
\]

Here, \(\mu\) is the effective coefficient of friction between the wheels and the competition surface.

This relationship helps explain why motor torque alone does not determine pushing performance. If the motors can supply more torque than the wheels can transfer to the ground, the wheels will slip before the motors reach their full potential.

For two equally loaded driven wheels with a 20 mm radius, the approximate torque per wheel required to reach the traction limit is

\[
\tau_{\text{wheel}}=\frac{\mu mgr}{2}
\]

At \(\mu=1\), this is approximately \(0.049\text{ N·m}\), or \(0.50\text{ kgf·cm}\), per wheel.

**Next experiment:** Measure the grip of the V1 and V2 wheel sets under consistent conditions, then compare the results with observed pushing performance.

See [`docs/traction_and_speed.md`](docs/traction_and_speed.md).

### 3. Motor Driver and Power Budget

The TB6612FNG controls the two drive motors through its dual H-bridge outputs. Its current limits must be evaluated against the actual motor specifications and operating conditions.

Important considerations:

- Motor stall current at the relevant terminal voltage.
- Driver current limits and thermal dissipation.
- Battery voltage under load.
- Buck converter output stability.
- Supply decoupling and electrical noise from the motors.

A useful first estimate of winding stall current is

\[
I_{\text{stall}}\approx\frac{V}{R}
\]

where \(V\) is the motor terminal voltage and \(R\) is the winding resistance. This is an approximation; the actual current depends on the motor, wiring, and supply conditions.

Do not assume that the driver's peak-current rating is a sustainable operating current. Verify the datasheet, board implementation, and motor specifications before extended pushing tests.

See [`docs/power_budget.md`](docs/power_budget.md).

### 4. Sharp IR Sensor Calibration

Sharp IR sensors provide nonlinear distance-dependent analog readings. A simple model for an inverse-distance response is

\[
r\approx\frac{a}{d}+b
\]

where:

- \(r\) = sensor reading
- \(d\) = target distance
- \(a,b\) = calibration constants

This is an approximate model, not a universal calibration curve. Two measured points can determine its constants, but additional measurements across the intended operating range are needed to evaluate its accuracy.

Calibrate each sensor under consistent conditions before relying on distance thresholds for opponent detection.

See [`docs/sharp_ir_distance.md`](docs/sharp_ir_distance.md).

## Firmware

The main Arduino sketch is located at:

[`firmware/ace_micro/ace_micro.ino`](firmware/ace_micro/ace_micro.ino)

The firmware implements a priority-based match strategy:

1. **Edge avoidance:** Detect the boundary and prioritize staying inside the ring.
2. **Opponent engagement:** React to opponent sensor readings and initiate an attack.
3. **Search behavior:** Execute the selected strategy when no opponent is detected.

### Strategy Selection

The Bluetooth interface selects the robot's search and attack strategy.

| Command | Strategy |
|:--|:--|
| `0` | Spin |
| `1` | Charge |
| `2` | Arc right |
| `r` | Print sensor readings while waiting for the start module |

The current firmware includes placeholder pins and thresholds. These must be verified against the physical wiring and calibrated sensor readings before competition use.

## Repository Structure

```text
ACE-Micro/
├── firmware/
│   └── ace_micro/
│       └── ace_micro.ino
├── docs/
│   ├── traction_and_speed.md
│   ├── power_budget.md
│   └── sharp_ir_distance.md
├── cad/                  # Planned: chassis CAD files
├── photos/               # Planned: build and testing photos
└── README.md
```

## Philosophy

**Keep Acing**

ACE Micro V2 represents the engineering process behind competitive robotics: identify a weakness, form a hypothesis, make a change, and test whether it actually works.

---

*ACE Micro V2 — part of the ACE competitive robotics lineup.*
