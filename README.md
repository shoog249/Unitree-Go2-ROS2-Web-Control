# Unitree Go2 Web-Based Remote Control — ROS 2

## Overview

This repository contains the web-control portion of a Unitree Go2 ROS 2 teleoperation project developed and tested in simulation.

The browser interface connects to ROS 2 through **rosbridge / ROSLIB.js** and sends robot motion commands using `geometry_msgs/Twist`. The project includes camera viewing, button control, keyboard teleoperation, a virtual joystick, and relay-topic testing.

## Project Demo

Below is a demonstration of the web-based remote control system used to control the Unitree Go2 robot:

https://github.com/user-attachments/assets/eea8695c-e57e-4d2a-8048-5f6657dc5c8d

## Files

- `index.html` — displays the camera stream served by the ROS/Flask camera service.
- `button_control.html` — Forward, Backward and Stop controls using `/cmd_vel`.
- `teleop.html` — W/A/S/D keyboard teleoperation.
- `joystick.html` — virtual joystick control; this version publishes to `/cmd_vel_smoothed`.
- `relay.html` — ON/OFF relay-topic testing using `/relay_int`.
- `roslib.min.js` — third-party ROSLIB.js client library.
- `nipplejs.min.js` — third-party virtual joystick library.

## How It Works

```text
Browser controls
      |
      | ROSLIB.js / WebSocket
      v
rosbridge_server :9090
      |
      v
ROS 2 topics
      |
      +--> /cmd_vel or /cmd_vel_smoothed --> robot movement
      |
      +--> /relay_int --> relay test
```

The wider project also used a simulated front camera. Its image stream was passed from ROS 2 through a Python/OpenCV/Flask service and displayed in the browser.

## Main Technologies

- ROS 2 Humble
- Gazebo
- rosbridge
- ROSLIB.js
- HTML / JavaScript
- nipplejs
- Python / Flask / OpenCV for the camera-streaming part of the wider project

## My Project Work

This project was completed using course-provided ROS 2 guidance and an existing open-source Unitree Go2 simulation as the foundation.

My hands-on work involved setting up and running the ROS 2/Gazebo environment, integrating and testing the browser controls, configuring the simulated camera workflow, working with ROS topics and rosbridge, and testing the teleoperation system.

The repository is therefore presented as a **course-based implementation and integration project**, rather than as a claim that the underlying Unitree simulator, ROSLIB.js, nipplejs, or all tutorial code was written from scratch.

## Reference Material

- Steven Eu Kok Seng — RoboticSensorROS course material:
  https://stevenviewr.github.io/RoboticSensorROS/
- Unitree Go2 ROS 2 simulation used as a foundation:
  https://github.com/anujjain-dev/unitree-go2-ros2
