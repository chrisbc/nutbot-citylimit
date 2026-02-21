# Motor Controller Options

## Motor Specifications

| Parameter | Value |
|-----------|-------|
| Type | 2x 24VDC brushed motors |
| Startup current | ~80A per motor |
| Normal current | 5-10A per motor |
| Drive | Differential drive, direct to pneumatic tyres |

## Controller Recommendations

| Controller | Specs | ROS Integration | Approx Cost |
|------------|-------|-----------------|-------------|
| **RoboClaw 2x60A** | 60A/ch continuous, 120A peak | Native ROS driver, encoder inputs, PID | $180-220 |
| **Sabertooth 2x60A** | 60A/ch continuous, 120A peak | Serial/PWM input, simple | $160-200 |
| **Roboteq SBL1360** | 60A/ch, programmable | Good ROS support, CAN/serial | $250-300 |

## Recommendation: RoboClaw 2x60A

**Advantages:**
- Built-in encoder inputs
- Internal PID control loop (~1kHz)
- Handles peak currents
- Native ROS package (`ros_roboclaw`)
- Current limiting for motor/battery protection
- Reports velocity, position, current via serial

**Control modes:**
- **Velocity mode**: send target speed, RoboClaw maintains via PID
- **Position mode**: send target encoder count, drives to position

## Budget Alternative

Dual IBT-2 / BTS7960 modules (~$15 each, 43A peak)
- Need 4 units (2 per motor parallel)
- No built-in ROS driver
- Add your own encoder handling
- More wiring complexity

---

# Encoder Options

## Current Status

Motors have **no built-in sensors**. Encoders required for closed-loop control and wheel odometry.

## Options

| Method | Cost | Difficulty | Notes |
|--------|------|------------|-------|
| Wheel hub encoder (magnetic) | $15-40 | Medium | Optical or magnetic on wheel shaft |
| Motor shaft encoder | $20-60 | Medium-Hard | Requires disassembly or end-cap mod |
| External encoder bracket | $30-80 | Medium | Coupled to exposed shaft or wheel hub |
| Hall sensor add-on | $10-20 | Hard | Mount sensors near motor windings |

## Recommended: Magnetic Wheel Hub Encoder

**Option A: Pre-built rotary encoder**
- Mount on wheel axle or exposed shaft
- 100-360 PPR typical
- Direct quadrature output to RoboClaw

**Option B: DIY magnetic encoder**
- Diametric magnet glued to wheel hub
- AMS AS5600 or AS5048 sensor mounted ~2mm away
- 12-14 bit resolution (4096-16384 counts/rev)
- Requires Arduino/MCU to output quadrature (or I2C to ROS)

**Parts list (DIY magnetic option):**
| Item | Cost | Source |
|------|------|--------|
| AS5600 magnetic encoder module | ~$5 | AliExpress, Amazon |
| Diametric magnet (6mm) | ~$2 | Amazon, Digikey |
| Arduino Nano or ESP32 | ~$5-10 | AliExpress |

## Open-Loop Fallback (No Encoders)

RoboClaw works in simple serial/PWM mode without encoders:
- Send duty cycle commands
- No speed feedback
- Odometry from IMU only (drifts)
- Speed varies with terrain

**Better:** Add wheel odometry separately using Arduino + encoders publishing to ROS `/odom` topic.

---

# Encoders per Revolution Calculation

For wheel-mounted encoder:
- Wheel diameter: ~350mm
- Wheel circumference: ~1100mm
- 100 PPR × 4 (quadrature) = 400 counts/rev
- Resolution: ~2.75mm per count

Higher PPR = better low-speed control and odometry resolution.

---

# References

- RoboClaw datasheet: https://www.basicmicro.com/downloads
- ros_roboclaw: https://github.com/sonyccd/ros_roboclaw
- AS5600 encoder: https://ams.com/as5600