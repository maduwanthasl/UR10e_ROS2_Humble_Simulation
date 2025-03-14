# 🦾 UR10e ROS2 Humble Simulation Setup 🚀

This repository provides a step-by-step guide to installing and running the Universal Robots UR10e simulation in ROS2 Humble. The setup is tested on Ubuntu-based distributions such as Pop!_OS.

---

## 📋 Prerequisites

Ensure you have the following installed:
- **Ubuntu 22.04 or Pop!_OS** 🐧
- **ROS2 Humble** (Follow the official [ROS2 installation guide](https://docs.ros.org/en/humble/Installation.html)) 🤖
- **Gazebo and RViz2** (Installed with ROS2 Humble) 🎮
- **Colcon build system** 🛠️
- **Git** 📂

---

## 🚀 Step 1: Create a ROS2 Workspace

```bash
mkdir -p ~/Documents/ros2_ur_ws/src
cd ~/Documents/ros2_ur_ws/src
```


## 📥 Step 2: Clone Required Repositories
```bash
git clone -b humble https://github.com/UniversalRobots/Universal_Robots_ROS2_Driver.git
```
##  🔧 Step 3: Install Dependencies

- Navigate to the workspace root and install required dependencies:

```bash
cd ~/Documents/ros2_ur_ws
rosdep update
rosdep install --from-paths src --ignore-src -r -y
```

##  🏗️ Step 4: Build the Workspace
```bash
colcon build --cmake-args -DCMAKE_BUILD_TYPE=Release
```

- Source the workspace after building:
```bash
source install/setup.bash
```
- To automatically source it each time, add it to .bashrc:
```bash
echo "source ~/Documents/ros2_ur_ws/install/setup.bash" >> ~/.bashrc
source ~/.bashrc
```

## 🚀 Step 5: Launch the Simulation

- Try launching the UR10e driver:
```bash
ros2 launch ur_bringup ur_control.launch.py ur_type:=ur10e use_sim_time:=true
```
If you get an error about a missing robot_ip, you are trying to launch a real robot driver instead of the simulation. Use Gazebo instead.


## 🎮 Gazebo Simulation

- If ur_gazebo is missing, install it first:
```bash
git clone -b humble https://github.com/ros-simulation/gazebo_ros_pkgs.git
cd ~/Documents/ros2_ur_ws
rosdep install --from-paths src --ignore-src -r -y
colcon build
source install/setup.bash
```

Then launch the Gazebo simulation:
```bash
ros2 launch ur_gazebo ur_gazebo.launch.py ur_type:=ur10e
```
## 🎛️ Step 6: Controlling the Robot

- You should now see the UR10e arm in Gazebo. To send commands, use:
```bash
ros2 topic pub /scaled_joint_trajectory_controller/joint_trajectory trajectory_msgs/JointTrajectory "{header: {stamp: {sec: 0, nanosec: 0}}, joint_names: [\"shoulder_pan_joint\", \"shoulder_lift_joint\", \"elbow_joint\", \"wrist_1_joint\", \"wrist_2_joint\", \"wrist_3_joint\"], points: [{positions: [0.0, -1.57, 1.57, 0.0, 0.0, 0.0], time_from_start: {sec: 3, nanosec: 0}}]}"
```
## 🛠️ Troubleshooting

- If Gazebo does not launch, check if gazebo_ros_pkgs is correctly installed. 🔍

- If the UR10e model does not appear, ensure the correct ROS2 Humble branches are used. 🌿

- If joint positions do not update, verify joint_state_publisher and robot_state_publisher are running. 🔄

👥 Repository
- https://github.com/UniversalRobots/Universal_Robots_ROS2_Driver/tree/humble
- https://github.com/UniversalRobots/Universal_Robots_ROS2_Gazebo_Simulation
