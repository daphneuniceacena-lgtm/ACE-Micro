# ACE Micro

500 g sumobot with DIY silicone wheels. My favorite of the lineup.

## Specs

| | |
|---|---|
| Weight | 500 g |
| Controller | Arduino Nano |
| Motor driver | TB6612FNG |
| Motors | JGA25, 620 RPM |
| Battery | 11.1 V LiPo (3S) |
| Opponent sensors | 5x SHARP IR |
| Edge sensors | 2x IR line sensors |
| Start | JSumo start module |
| Wireless | Bluetooth module |
| Logic power | LM2596 buck converter |
| Wheels | DIY silicone |

## Build history

**Chassis.** 3D printed. It took 3 prints to get the chassis right.

**V1.** Pink silicone wheels. Other robots pushed it around easily.

**V2.** Neon green silicone wheels, cast on the third attempt with less hardener. Added a green rear carry handle after seeing other teams use zip ties to lift their bots.

## A bit of math

**Speed.** `v = pi * D * n / 60`. For a 40 mm wheel at the motors' 620 RPM no-load speed that is 1.30 m/s. Real speed under load is lower, so measure it.

**Why the wheel swap helped.** At 500 g the push limit is `F_max = mu * 0.5 * 9.81 = 4.9 N * mu`. Each of the two driven wheels needs `tau = mu * m * g * r / 2` to reach that limit, about 0.5 kg*cm per wheel at `mu = 1` and 40 mm wheels. If the motors can deliver more than that, the wheels slip first and only more grip helps. The Micro V1 to V2 wheel change is a test of this: measure `mu` for both wheel sets and see whether it moved. See [`traction_and_speed.md`](../docs/traction_and_speed.md).

**Driver limit.** The TB6612FNG is rated for 1.2 A continuous and 3.2 A peak per channel (check the datasheet). Compare the JGA25 stall current `V / R` against that before a long push. See [`power_budget.md`](../docs/power_budget.md).

**Sensor distance.** The SHARP output follows `r = a / d + b`, so two measured points give the calibration. See [`sharp_ir_distance.md`](../docs/sharp_ir_distance.md).

## Firmware

`firmware/ace_micro/ace_micro.ino` runs the match logic: avoid the edge first, then charge the opponent (faster as the reading rises), otherwise open with the chosen strategy and search. Bluetooth selects the strategy (`0` spin, `1` charge, `2` arc right) and `r` prints the sensor readings while waiting for the start module. The pins and thresholds are placeholders. Calibrate the SHARP constants with [`sharp_ir_distance.md`](../docs/sharp_ir_distance.md).

## Still to add

- `cad/` with the chassis STL or STEP files
- `photos/` of V1 and V2 side by side
- Wheel diameter, and the measured `mu` table from the docs
