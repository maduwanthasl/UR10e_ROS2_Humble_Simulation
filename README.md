# 🦾 UR10e ROS2 Humble Simulation Setup 🚀

This repository provides a step-by-step guide to installing and running the Universal Robots UR10e simulation in ROS2 Humble. The setup is tested on Ubuntu-based distributions such as Pop!_OS.

---

## 📋 Prerequisites

Ensure you have the following installed:
- **Ubuntu 22.04 or Pop!_OS** 🐧
- **ROS2 Humble** (Follow the official [ROS2 installation guide](https://docs.ros.org/en/humble/Installation.html)) 🤖
- **Colcon build system** 🛠️
- **Git** 📂

---

## 🏗️ Step 1: Install Dependencies

- First, update your system and install the required ROS2 packages:

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install -y \
  ros-humble-gazebo-ros-pkgs \
  ros-humble-ros2-control \
  ros-humble-moveit \
  ros-humble-joint-state-publisher-gui \
  ros-humble-urdf \
  ros-humble-xacro \
  python3-colcon-common-extensions \
  python3-rosdep
```
- Initialize rosdep:
```bash
sudo rosdep init
rosdep update
```

## 🚀 Step 2: Create a ROS2 Workspace

```bash
mkdir -p ~/Documents/ros2_ur_ws/src
cd ~/Documents/ros2_ur_ws/src
colcon build
```


## 📥 Step 3: Clone Required Repositories
```bash
git clone -b humble https://github.com/UniversalRobots/Universal_Robots_ROS2_Driver.git
```
##  🔧 Step 4: Install Dependencies

- Navigate to the workspace root and install required dependencies:

```bash
cd ~/Documents/ros2_ur_ws
rosdep update
rosdep install --from-paths src --ignore-src -r -y
```

##  🏗️ Step 5: Build the Workspace
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

## 🚀 Step 6: Launch the Simulation

- Try launching the UR10e driver:
```bash
ros2 launch ur_moveit_config ur_moveit.launch.py ur_type:=ur10e use_sim_time:=true
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
![image](https://github.com/user-attachments/assets/33418569-a9ae-4c9b-8f7d-0c0dcc31f636)


Then launch the Gazebo simulation:
```bash
ros2 launch ur_simulation_gazebo ur_sim_control.launch.py
```
## 🎛️ Step 7: Controlling the Robot

- You should now see the UR10e arm in Gazebo. To send commands, use:
```bash
ros2 topic pub /scaled_joint_trajectory_controller/joint_trajectory trajectory_msgs/JointTrajectory "{header: {stamp: {sec: 0, nanosec: 0}}, joint_names: [\"shoulder_pan_joint\", \"shoulder_lift_joint\", \"elbow_joint\", \"wrist_1_joint\", \"wrist_2_joint\", \"wrist_3_joint\"], points: [{positions: [0.0, -1.57, 1.57, 0.0, 0.0, 0.0], time_from_start: {sec: 3, nanosec: 0}}]}"
```
## 🛠️ Troubleshooting

- If Gazebo does not launch, check if gazebo_ros_pkgs is correctly installed. 🔍

- If the UR10e model does not appear, ensure the correct ROS2 Humble branches are used. 🌿

- If joint positions do not update, verify joint_state_publisher and robot_state_publisher are running. 🔄

👥 Repository
- ![Universal_Robots_ROS2_Driver](https://github.com/UniversalRobots/Universal_Robots_ROS2_Driver/tree/humble)
- ![Universal_Robots_ROS2_Gazebo_Simulation](https://github.com/UniversalRobots/Universal_Robots_ROS2_Gazebo_Simulation)
