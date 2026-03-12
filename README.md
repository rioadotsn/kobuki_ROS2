Kobuki
======

This repository holds the ROS wrapper of [Kobuki's C++ driver](https://github.com/yujinrobot/kobuki_core) plus various ROS tools and applications.

![Kobuki Logo](http://kobuki.yujinrobot.com/wp-content/uploads/2015/07/iclebo-kobuki-logo-e1437635225432.png)

### Documentation ###

* [Official Web Page](http://kobuki.yujinrobot.com) - home page, sales, specifications and hardware howto.
* [Protocol, Usage and Api Documentation](http://yujinrobot.github.com/kobuki/doxygen/index.html) - in doxygen.
* [Ros Usage and Tutorials](http://www.ros.org/wiki/kobuki) - on the roswiki.
* [Turtlebot Reference Platform](http://www.ros.org/wiki/Robots/TurtleBot) - kobuki has been designed to meet the requirements of [ROS REP #119](http://www.ros.org/reps/rep-0119.html) to support turtlebot.

## Setup repositories

1. Install liborocos-kdl-dev:
   ```bash
   sudo apt install liborocos-kdl-dev
   ```

2. Clone the source repository in src:
   ```bash
   git clone 
   ```

3. Initialize and update rosdep:
   ```bash
   rosdep init
   rosdep update
   ```

4. Install dependencies:
   ```bash
   rosdep install --from-paths src --ignore-src -r -y
   ```

5. Run kobuki_node:
   ```bash
   ros2 run kobuki_node kobuki_ros_node
   ```

# Kobuki Node Configuration Guide

## Modifying Port Configuration

To configure the Kobuki robot port in your ROS2 setup, modify the following code in `kobuki_node/src/kobuki_ros.cpp` at line 213:

```cpp
parameters.device_port = this->declare_parameter("device_port", "/dev/kobuki");

if (parameters.device_port.empty()) {
  throw std::runtime_error("Kobuki : no device port given on the parameter server (e.g. /dev/ttyUSB0).");
}
```

## Adjusting Command Timeout Behavior

To modify the command timeout behavior, update the code around line 325 in the same file:

```cpp
if (kobuki_.isEnabled() && odometry_->commandTimeout(this->get_clock()->now())) {
  if (!cmd_vel_timed_out_) {
    // kobuki_.setBaseControl(0, 0);
    cmd_vel_timed_out_ = true;
    RCLCPP_WARN(get_logger(), "Incoming velocity commands not received for more than %.2f seconds -> zero'ing velocity commands", odometry_->timeout().seconds());
  }
}
else {
  cmd_vel_timed_out_ = false;
}
```
These modifications will help configure the Kobuki robot's port connection and adjust its behavior when command velocity messages time out.


