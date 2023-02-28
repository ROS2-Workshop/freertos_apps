# micro-ROS apps

This repo contains apps demos that are based on freertos and tested for ESP32-devkit-C with the purpose of being a starting point for futher development and workshops demonstrations.


## Quick start using the int32_publisher app


### Dependencies

This package targets the **ROS 2** installation. ROS 2 supported distributions are:

| ROS 2 Distro | State     | Branch     |
| ------------ | --------- | ---------- |
| Humble       | Supported | `humble`     |

Some other prerequisites needed for building a firmware using this package are:

```
sudo apt install python3-rosdep
```
## 1. Clone and setup
```bash
mkdir -p ~/ros2_ws && cd ~/ros2_ws
echo 'export UROS_CUSTOM_APP_FOLDER=~/ros2_ws/src/apps' >> ~/.bashrc
source ~/.bashrc
git clone https://github.com/ROS2-Workshop/micro-ros-demos.git ~/ros2_ws/src/apps
```

---
 
## 2. Setup micro_ros_setup 

For futher details about micro-ros-setup, see [micro-ros-setup](https://github.com/micro-ROS/micro_ros_setup)

```bash
cd ~/ros2_ws
source /opt/ros/$ROS_DISTRO/setup.bash
git clone -b $ROS_DISTRO https://github.com/micro-ROS/micro_ros_setup.git src/micro_ros_setup
sudo rosdep init
rosdep update
rosdep update && rosdep install --from-paths src --ignore-src -y
colcon build
source install/local_setup.bash
```
---

## 3. Create firmware
```bash
# Creating a FreeRTOS + micro-ROS firmware workspace for esp23 micro-controller
ros2 run micro_ros_setup create_firmware_ws.sh freertos esp32
```

---

## 4. Configure firmware

**Connect ESP32 to USB port in PC** 

Check port ID, normally returns 'ttyUSB0':

```bash
ls /dev | grep 'USB'
```

```bash
ros2 run micro_ros_setup configure_firmware.sh int32_publisher -t serial --dev '/dev/ttyUSB0'
```

---

## 5. Building & flash micro-ROS firmware in - terminal window 1

Edit the app and once satifsfied build and flash the app, only need for repeating step 5 and 6 during development.

```bash
ros2 run micro_ros_setup build_firmware.sh
ros2 run micro_ros_setup flash_firmware.sh
```
**NOTE: You must cancel the micro_ros_agent before flashing if started, else the USB port will be busy and also some esp32's are repuired to hold reset button on during the first 1-2 seconds when flashing.**


## 6. Building micro-ROS-Agent in - terminal window 2
The agent acts like a translator between micro-ROS and ROS2

```bash
cd ~/ros2_ws
ros2 run micro_ros_setup create_agent_ws.sh
ros2 run micro_ros_setup build_agent.sh
source install/local_setup.sh
ros2 run micro_ros_agent micro_ros_agent --dev '/dev/ttyUSB0'
```

## 7. Listen to node - terminal window 3

```bash
cd ~/ros2_ws
source install/local_setup.sh
ros2 topic list
```

```bash
ros2 topic echo <topic_name>
```
