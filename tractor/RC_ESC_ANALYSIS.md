# RC ESC Analysis (For Reference)

## Candidate: WP Crawler Brushed 80A ESC

**Source**: AliExpress (RC car ESC)

| Spec | Value |
|------|-------|
| Continuous Current | 80A |
| Peak Current | 400A |
| Input Voltage | 2-3S LiPo / 5-9 Cell NiMH |
| Max Voltage | 8-12.6V |
| BEC Output | 5.8V / 3A |
| Features | Waterproof, includes programmer card |

**Problem**: Voltage mismatch — wheelchair motors are 24V, this ESC is 8-12.6V max.

---

## RC ESC vs RoboClaw Comparison

| Aspect | RC ESC | RoboClaw |
|--------|--------|----------|
| Input | PWM (1000-2000µs pulses) | Serial/PWM |
| Feedback | None (open loop) | Encoder input |
| Control mode | Duty cycle only | Velocity/position PID |
| Current limiting | Often none | Configurable |
| Reverse | Delayed (safety) | Instant |
| ROS integration | DIY via Arduino | Native driver |
| Current feedback | No | Yes |
| Price | $20-80 each | $180-220 |

---

## If Going RC ESC Route

### Requirements

1. **Arduino/MCU** to generate RC PWM from ROS `/cmd_vel`
2. **Separate encoder interface** (Arduino + AS5600) for odometry
3. **24V-compatible ESC** (search "24V brushed ESC" or "RC crawler ESC 6S")

### 24V-Compatible Options

| ESC | Specs | Approx Price |
|-----|-------|--------------|
| **QuicRun WP 880 Dual** | 2× 80A brushed, 6S (22.2V) | ~$50 |
| **Holmes Hobbies BR Solo** | 6S, designed for crawlers, good low-speed | ~$80-100 |
| **Mamba Monster X** | 6S brushed, high quality | ~$150 |

### Trade-offs

**Advantages:**
- Cheaper hardware cost
- Waterproof options available
- Programmer cards for tuning

**Disadvantages:**
- No closed-loop speed control (speed varies with terrain/load)
- More wiring (Arduino for PWM + Arduino for encoders)
- No current feedback (can't monitor motor load)
- Odometry drift on slopes/terrain
- Delayed reverse (safety feature, problematic for precise control)
- More development work for ROS integration

---

## Architecture Comparison

### RoboClaw Architecture (Recommended)

```
[ROS /cmd_vel] → [RoboClaw (serial)] → [Motor]
                        ↑_________________|
                        (encoder feedback)
                        
RoboClaw handles: PID, velocity control, current limiting, odometry
```

### RC ESC Architecture

```
[ROS /cmd_vel] → [Arduino] → [RC PWM] → [ESC] → [Motor]

Separate: [Encoder] → [Arduino] → [ROS /odom]

Arduino must handle: PWM generation, safety, mixing differential drive
Encoder Arduino handles: Quadrature counting, odometry publishing
```

---

## Cost Breakdown

### RoboClaw Path

| Item | Cost |
|------|------|
| RoboClaw 2x60A | ~$180-220 |
| AS5600 encoders ×2 | ~$10 |
| Total | ~$190-230 |

### RC ESC Path

| Item | Cost |
|------|------|
| QuicRun WP 880 Dual | ~$50 |
| Arduino Nano ×2 | ~$10 |
| AS5600 encoders ×2 | ~$10 |
| Wiring/connectors | ~$10-20 |
| Development time | Significant |
| Total | ~$80-100 + dev time |

---

## Recommendation

**RoboClaw** is preferred because:
- Closed-loop velocity control compensates for terrain
- Native ROS driver
- Single integrated solution
- Current monitoring for safety
- Less development time

**RC ESC** path is viable if:
- Budget is primary constraint
- You're comfortable with Arduino development
- Open-loop control is acceptable for your use case
- You don't need current feedback

---

## PWM Interface Code (If Needed)

Arduino sketch to convert ROS `/cmd_vel` to RC PWM:

```cpp
#include <ros.h>
#include <geometry_msgs/Twist.h>
#include <Servo.h>

Servo leftMotor, rightMotor;
ros::NodeHandle nh;

void cmdVelCallback(const geometry_msgs::Twist& msg) {
  float linear = msg.linear.x;
  float angular = msg.angular.z;
  
  // Differential drive mixing
  float left = linear - angular * 0.5;  // adjust factor
  float right = linear + angular * 0.5;
  
  // Convert to PWM (1000-2000 µs, 1500 = neutral)
  int leftPwm = map(constrain(left, -1, 1) * 500, -500, 500, 1000, 2000);
  int rightPwm = map(constrain(right, -1, 1) * 500, -500, 500, 1000, 2000);
  
  leftMotor.writeMicroseconds(leftPwm);
  rightMotor.writeMicroseconds(rightPwm);
}

ros::Subscriber<geometry_msgs::Twist> sub("cmd_vel", &cmdVelCallback);

void setup() {
  leftMotor.attach(9);
  rightMotor.attach(10);
  nh.initNode();
  nh.subscribe(sub);
}

void loop() {
  nh.spinOnce();
  delay(1);
}
```

---

## Notes

- RC ESCs use "double tap" reverse for safety — may need programming card to disable
- Calibrate ESC throttle range before use (1000-2000µs)
- Some ESCs have built-in BEC — can power Arduino from ESC
- Waterproofing is useful for outdoor robot