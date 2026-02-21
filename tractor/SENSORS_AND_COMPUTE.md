# Sensors & Compute for Environmental Perception

## Detection Categories

| Purpose | What to detect | Use case |
|---------|----------------|----------|
| **Collision avoidance** | Obstacles (rocks, furniture, pets, people) | Real-time Nav2 path modification |
| **Work planning** | Acorns, leaves, twigs (debris density) | Prioritize collection areas, skip clean areas |
| **Terrain** | Slopes, holes, wet grass | Speed adjustment, safety limits |

---

## Sensor Options

| Sensor | For | Pros | Cons | Cost |
|--------|-----|------|------|------|
| **2D LiDAR (RPLIDAR A1/A2)** | Obstacles | Nav2 standard, simple | No height, misses low objects | $100-200 |
| **3D LiDAR (LD06, RPLIDAR A3)** | Obstacles + terrain | Height data, better outdoors | Expensive | $300-800 |
| **Intel RealSense D435** | Obstacles + terrain + debris | RGB-D, works in sunlight (mostly) | Close-range only (~3m) | $150-250 |
| **OAK-D / OAK-D Pro** | Obstacles + debris classification | Onboard ML inference, depth + RGB | Learning curve | $200-300 |
| **Stereo camera (ZED 2)** | Obstacles + terrain | Long range, outdoor rated | Expensive, needs GPU | $400-500 |
| **Ultrasonic (HC-SR04)** | Close collision backup | Cheap | No classification, short range | $2-5 each |
| **IMU (BNO055, MPU6050)** | Terrain (slope, tilt) | Odometry fusion | Already in plan | $15-40 |

---

## Recommended Sensor Stack

| Priority | Sensor | Role |
|----------|--------|------|
| 1 | **2D LiDAR (RPLIDAR A2)** | Primary obstacle detection for Nav2 |
| 2 | **OAK-D camera** | Debris classification (acorns/leaves/twigs), depth for low obstacles LiDAR misses |
| 3 | **IMU** | Terrain slope, odometry fusion |
| 4 | **Ultrasonic bumper** | Last-resort collision stop (backup sensor) |

---

## Compute Platform Options

| Option | ML Capability | ROS2 Support | Cost | Notes |
|--------|---------------|--------------|------|-------|
| **Raspberry Pi 5 (8GB)** | Limited (no NPU) | Good | $80 | Add Coral TPU (~$60) for ML |
| **Jetson Orin Nano** | Good (GPU + NPU) | Good | $200-300 | Best for onboard ML inference |
| **Pi 5 + OAK-D** | On-camera ML | Good | $80 + $200 | ML runs on OAK-D, Pi handles ROS |
| **Radxa ROCK 5B** | NPU (6 TOPS) | OK | $80-120 | Alternative to Pi + Coral |

### Recommendation: Pi 5 + OAK-D

- OAK-D runs debris detection models onboard (no Pi GPU needed)
- Pi 5 runs Nav2, sensor fusion
- Simple ROS2 integration via `depthai_ros`
- Keeps cost reasonable

---

## ML Model Approach for Debris Detection

| Option | Description |
|--------|-------------|
| **YOLOv8 (nano/small)** | Fast object detection for acorns/twigs/leaves |
| **MobileNet SSD** | Lighter, good for classification |
| **Semantic segmentation** | Pixel-wise "debris density map" |
| **Pre-trained + fine-tune** | Start with COCO, add custom acorn/leaf/twig labels |

OAK-D supports custom models via DepthAI — compile YOLOv8 or MobileNet to run on Myriad X VPU.

---

## Data Flow

```
[Sensors]                    [Compute]                    [ROS2 Topics]
─────────────────────────────────────────────────────────────────────────
LiDAR (serial/USB)     ──→   Pi 5                  ──→   /scan
                              │
OAK-D (USB)            ──→   │  Nav2 stack         ──→   /camera/depth, /camera/rgb
       │                      │  ↓                        /detection/debris
       └── Myriad X VPU ──→   │  EKF fusion         ──→   /odom_filtered
       (onboard ML)           │  ↓
                              │  Behavior tree      ──→   /cmd_vel
IMU (I2C)              ──→   │                          /terrain/slope
                              │
Ultrasonic (GPIO)      ──→   │  Safety monitor     ──→   /safety/stop
```

---

## Integration Notes

- `depthai_ros` provides ROS2 nodes for OAK-D
- `rplidar_ros` for LiDAR
- `robot_localization` (EKF) for IMU + wheel odometry fusion
- Nav2 costmap layers can ingest debris density for intelligent path planning
- Custom behavior tree node to divert toward high-debris areas (work planning)

---

# Similar Projects & References

## Agricultural / Lawn Robotics with Perception

| Project | Sensors | Compute | Notes |
|---------|---------|---------|-------|
| **OpenMower** | RTK GNSS, IMU, bumper | Pi 4 | ROS1 Noetic, perimeter-free navigation, no vision |
| **Linorobot2** | LiDAR + depth camera options | Pi/Jetson | **ROS2 native**, 2WD/4WD differential drive, Nav2 ready |
| **FarmBot** | Camera (USB) | Pi | CNC-style precision farming |
| **Donkey Car** | Camera | Pi/Jetson | Self-driving RC car platform, ML training pipeline |
| **Tertill (Franklin Robotics)** | Low sensors for weeds | MCU | Weeding robot, simple sensors |

## Highly Relevant: Linorobot2

**Best match for NutBot** — ROS2 native differential drive robot with Nav2 integration.

- **GitHub**: https://github.com/linorobot/linorobot2
- **ROS Distro**: Jazzy (ROS2)
- **Features**:
  - Supports 2WD, 4WD, Mecanum drive
  - Pre-integrated sensor drivers (RPLIDAR, RealSense, ZED, OAK-D)
  - Nav2 + SLAM Toolbox ready
  - Gazebo simulation included
  - micro-ROS for microcontroller communication
  - Parameterized URDF for custom robot dimensions
  - Docker support

**Installation (2WD with OAK-D + RPLIDAR)**:
```bash
cd /tmp
wget https://raw.githubusercontent.com/linorobot/linorobot2/${ROS_DISTRO}/install.bash
bash install.bash --base 2wd --laser a2 --depth oakd
```

## Relevant GitHub Repositories

| Repo | Description |
|------|-------------|
| https://github.com/linorobot/linorobot2 | **ROS2 differential drive, Nav2 ready — START HERE** |
| https://github.com/ClemensElflein/open_mower_ros | OpenMower ROS1 stack (concepts, not code) |
| https://github.com/autorope/donkeycar | Self-driving RC car, ML training pipeline |
| https://github.com/luxonis/depthai-ros | OAK-D ROS2 integration |
| https://github.com/Slamtec/rplidar_ros | RPLIDAR ROS driver |
| https://github.com/cra-ros-pkg/robot_localization | EKF sensor fusion |
| https://github.com/uls-robocar/f1tenth | Autonomous racing, Nav2 + perception tutorials |
| https://github.com/ultralytics/ultralytics | YOLOv8/YOLO11 object detection (train custom debris model) |

## YOLO for Debris Detection

**Ultralytics YOLO** (https://github.com/ultralytics/ultralytics) — SOTA object detection, easy to train custom models.

```python
from ultralytics import YOLO

# Load pretrained model
model = YOLO("yolo11n.pt")

# Train on custom debris dataset (acorns, leaves, twigs)
model.train(data="debris.yaml", epochs=100, imgsz=640)

# Export for OAK-D (ONNX format)
model.export(format="onnx")
```

**Model sizes for OAK-D**:
- `yolo11n.pt` — Nano, fastest, lowest accuracy (~3M params)
- `yolo11s.pt` — Small, good balance (~9M params)

## Debris / Ground Cover Detection

| Project / Paper | Approach |
|-----------------|----------|
| **Weed detection (various)** | YOLO + segmentation for agricultural robots |
| **Lawn debris (commercial)** | Proprietary — Husqvarna, Worx use simple sensors |
| **Acorn detection research** | Academic papers on forest floor object detection using YOLOv5/v8 |

### Relevant Papers (search terms)

- "acorn detection computer vision"
- "autonomous ground debris detection"
- "agricultural robot weed detection YOLO"
- "lawn mower obstacle detection ROS"

---

# Future Enhancements

- [ ] Train custom YOLOv8 model on acorn/leaf/twig dataset
- [ ] Integrate debris density into Nav2 costmap (bias toward debris)
- [ ] Add hole detection from depth camera (avoid stuck areas)
- [ ] Seasonal model variants (fall leaves vs winter debris)
- [ ] Night operation (IR illumination + camera)

---

# Open Questions

- [ ] Depth range needed for debris detection at operating speed?
- [ ] Is 2D LiDAR sufficient or is 3D needed for terrain?
- [ ] Dataset for training acorn/leaf/twig detection — capture own or crowdsourced?