# so101_description (ROS2 Jazzy)

ROS 2 package containing the URDF/xacro description, meshes, and RViz launch
files for the **SO-101** robotic arm.

This package currently provides robot visualization only (RViz + joint state
publishing).

## Build

Clone into your workspace `src` folder and build with `colcon`:

```bash
cd ~/ros2_ws/src
git clone https://github.com/YongQuanz/so101_description.git
cd ~/ros2_ws
colcon build --packages-select so101_description
source install/setup.bash
```
Install missing dependencies with `rosdep` (run from the root of your workspace):

```bash
cd ~/ros2_ws
sudo rosdep init      # skip if already initialized
rosdep update
rosdep install --from-paths src --ignore-src -r -y
```

This reads the `<depend>` tags in `package.xml` and installs everything
needed (`joint_state_publisher_gui`, `robot_state_publisher`, `rviz2`,
`xacro`, etc.) automatically.

## Usage

Launch RViz with interactive joint sliders:

```bash
ros2 launch so101_description so101_display.launch.py
```

This will:
- Parse `urdf/so101.urdf.xacro` and publish the robot description
- Start `joint_state_publisher_gui` for manually moving each joint
- Open RViz pre-configured to display the robot model

> **Note:** If running from VS Code's integrated terminal and using the
> snap-installed version of VS Code, `rviz2` may fail with a
> `symbol lookup error` related to `libpthread.so.0`. This is a known
> snap/glibc conflict — run `rviz2` from a regular system terminal instead,
> or switch to the non-snap (`apt`) build of VS Code.

## Joints

| Joint | Type | Parent → Child | Range (rad) |
|---|---|---|---|
| `shoulder_pan` | revolute | base_link → shoulder_link | −1.92 to 1.92 |
| `shoulder_lift` | revolute | shoulder_link → upper_arm_link | −1.75 to 1.75 |
| `elbow_flex` | revolute | upper_arm_link → lower_arm_link | −1.69 to 1.69 |
| `wrist_flex` | revolute | lower_arm_link → wrist_link | −1.66 to 1.66 |
| `wrist_roll` | revolute | wrist_link → gripper_link | −2.74 to 2.84 |
| `gripper` | revolute | gripper_link → moving_jaw_so101_v1_link | −0.17 to 1.75 |

## Source

The URDF and meshes were taken from:
https://github.com/TheRobotStudio/SO-ARM100/blob/main/Simulation/SO101/so101_new_calib.urdf

## License

Licensed under the [Apache License 2.0](LICENSE).
