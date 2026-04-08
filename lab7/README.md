# Laboratory 7 — Coordinate transforms (TF2), robot description (URDF/Xacro), and the RTR manipulator

**Course:** Robotics (ROS 2)  
**Topic:** Spatial transforms, kinematic modeling, and joint-driven TF in ROS 2.


## Procedure

### Build the package

```bash
cd /opt/ws
source /opt/ros/jazzy/setup.bash
colcon build --packages-select lab7 --symlink-install
source install/setup.bash
```

If `docker/Dockerfile` was updated on the repository, rebuild the image on the host and start a new container:

```bash
./scripts/cmd build-docker
./scripts/cmd run
```

### Part A — TF2 broadcaster and listener

**Terminal 1 — broadcaster**

```bash
ros2 run lab7 tf2_broadcaster_demo -- 0.2 0.5 0.35
```

**Terminal 2 — listener** (use the **same** numbers so the analytic check succeeds)

```bash
ros2 run lab7 tf2_listener_demo -- 0.2 0.5 0.35
```

**Optional verification**

```bash
ros2 run tf2_ros tf2_echo world rtr_ee_demo
```



### Part B — URDF/Xacro, joint GUI, and TF

```bash
ros2 launch lab7 rtr_visualize.launch.py
```

### Part C — `joint_state_broadcaster` and TF queries

```bash
ros2 launch lab7 rtr_ros2_control.launch.py
```

Send a position command (revolute joints in **radians**, prismatic **joint_theta2** in **metres**):

```bash
ros2 topic pub --once /forward_position_controller/commands std_msgs/msg/Float64MultiArray \
  "{data: [0.2, 0.6, 0.4]}"
```

Example TF query:

```bash
ros2 run tf2_ros tf2_echo base_link tool0
```

### Automated check (kinematics)

```bash
colcon test --packages-select lab7
```
