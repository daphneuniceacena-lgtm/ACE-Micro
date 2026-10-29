# ACE Lineup

Competition robots I design, print, cast, and wire myself: sumobots and a line tracer in different weight classes. All of them run on DIY silicone wheels. This repo holds the firmware, the hardware, the math behind the design choices, and what went wrong on each build.

| Robot | Type | Weight | Controller | Motor driver | Motors |
|---|---|---|---|---|---|
| [ACE Mega](ace-mega/) | Sumobot, aluminum chassis | 1 kg | Cytron Maker RP2040 | 2x BTS7960 | (fill in) |
| [ACE Micro](ace-micro/) | Sumobot | 500 g | Arduino Nano | TB6612FNG | JGA25, 620 RPM |
| [ACE Liner](ace-liner/) | Line tracer, 11-channel bar | 150 g | Arduino Nano | TB6612FNG | N20 |
| [ACE Nano](ace-nano/) | Sumobot, built for juniors and peers to test code | (fill in) | Microbot Rover V1.3 (Nano) | TB6612FNG | (fill in) |

## Math in the lineup

Each robot has a few design questions that turn into short calculations. They live in `docs/`.

| Question | Where | The formula |
|---|---|---|
| How fast does it go, and who wins a push? | [`traction_and_speed.md`](docs/traction_and_speed.md) | `v = pi*D*n/60`, `F_max = mu*m*g` |
| Do the motors or the wheels give up first? | [`traction_and_speed.md`](docs/traction_and_speed.md) | `tau_wheel = mu*m*g*r/2` |
| How far away is the opponent, and can it hide between beams? | [`sharp_ir_distance.md`](docs/sharp_ir_distance.md) | `r = a/d + b`, `d_blind = w/tan(alpha)` |
| Will the fuse, driver, and buck converter survive a push? | [`power_budget.md`](docs/power_budget.md) | `I_stall = V/R`, `P_loss = P_out*(1/eta - 1)` |
| Why does the line tracer wiggle, and how do I fix it? | [`ace_liner_v2_math.md`](docs/ace_liner_v2_math.md) | `zeta = L*sqrt(Kp) / (2*sqrt(v))` |

Every doc ends with a results table. The formulas are there to be checked against measurements, so the tables get filled with real data from the robots.

## The thread through the lineup: wheels

Traction decides most matches, so the silicone wheels are the running experiment. The traction math says the wheel's friction coefficient counts as much as the mass.

| Wheel | Robot | Result |
|---|---|---|
| Blue silicone, attempt 1 | Mega | Bubbles in the cast, no traction |
| Pink silicone | Micro V1 | Got pushed around easily |
| Neon green silicone, attempt 3, less hardener | Micro V2 | (fill in with measured mu) |
| Pink | Liner | Chosen for taste, rarity at competitions, and to match the red sensor LEDs |

## Safety note

A LiPo on the Mega once smoked and its black cover ripped open. Every LiPo build here now has a 15A fuse in line.

## Repo layout

```
ace-lineup/
├── README.md
├── ace-micro/      README.md, firmware/ace_micro/       (add: cad/, photos/)
├── ace-mega/       README.md, firmware/ace_mega/        (add: cad/, photos/)
├── ace-liner/      README.md, firmware/ace_liner_v2/    (add: cad/, photos/)
├── ace-nano/       README.md, firmware/ace_nano/        (add: cad/, photos/)
└── docs/
    ├── traction_and_speed.md
    ├── sharp_ir_distance.md
    ├── power_budget.md
    └── ace_liner_v2_math.md
```

Each robot folder follows the same layout: `firmware/` for sketches (the `.ino` file sits in a folder with the same name), `cad/` for STL and STEP files, `photos/` for build photos.

## Upload (Arduino)

1. Open the `.ino` file inside its folder. The folder name must match the sketch name.
2. Select the board and port, then upload. The Mega uses the Cytron Maker RP2040, which needs the RP2040 board package.
3. Check the pin constants at the top of the sketch against your wiring first. Every pin and threshold in the sketches is a placeholder until you set it.
