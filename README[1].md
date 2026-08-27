# Unitree Go2 Web-Based Remote Control — ROS 2

## Overview

This repository contains the **web-control portion** of a Unitree Go2 ROS 2 teleoperation project developed and tested in simulation.

The browser interface connects to ROS 2 through **rosbridge / ROSLIB.js** and publishes robot motion commands using `geometry_msgs/Twist`. The project also includes browser pages for camera viewing, button control, keyboard teleoperation, a virtual joystick, and relay-topic testing.

> **Scope note:** This repository is a portfolio snapshot of the web-control files that were available for sharing. The full working system also relied on a Unitree Go2 Gazebo simulation, a modified camera description, rosbridge, and a Flask/ROS 2 camera-streaming service. Those files are not included in this snapshot unless added separately.

## System Architecture

```text
Browser UI
   |
   |  ROSLIB.js over WebSocket
   v
rosbridge_server :9090
   |
   v
ROS 2 topics
   |------------------------------|
   |                              |
/cmd_vel or /cmd_vel_smoothed   /relay_int
   |                              |
   v                              v
Robot motion                  Relay test topic

Camera path used by the wider project:
Gazebo camera -> ROS 2 image topic -> Python/CvBridge/OpenCV -> Flask :8080 -> Browser
```

## Files

| File | Purpose |
|---|---|
| `web/index.html` | Displays the camera stream served over HTTP on port 8080. |
| `web/button_control.html` | Provides Forward, Backward and Stop buttons and publishes `Twist` commands to `/cmd_vel`. |
| `web/teleop.html` | Provides W/A/S/D keyboard teleoperation and continuously publishes velocity commands while a key is held. |
| `web/joystick.html` | Provides a virtual joystick and converts joystick angle/distance into linear and angular velocity commands. This version publishes to `/cmd_vel_smoothed`. |
| `web/relay.html` | Publishes integer ON/OFF commands to `/relay_int`. |
| `web/roslib.min.js` | Third-party ROS JavaScript client library. |
| `web/nipplejs.min.js` | Third-party virtual joystick library. |

## How the Web Control Works

1. The page reads a `ros_ip` value from the URL query string, for example:
   `?ros_ip=192.168.1.10`.
2. ROSLIB.js opens a WebSocket connection to rosbridge on port `9090`.
3. The page creates a ROS topic object such as `/cmd_vel` with message type `geometry_msgs/Twist`.
4. User input from buttons, keyboard keys, or the joystick is converted into linear and angular velocity values.
5. The browser publishes the command through rosbridge to ROS 2.
6. The robot controller receives the velocity command and moves the simulated Go2.

## Camera Display

The camera pages build a URL in the form:

```text
http://<ros_ip>:8080/video
```

The wider project used a ROS 2 image subscriber and a Flask server to make the simulated camera stream available to the browser.

## Main ROS Topics

- `/cmd_vel` — standard velocity command topic used by the button and keyboard controls.
- `/cmd_vel_smoothed` — velocity command topic used by the uploaded joystick version, intended to pass through a smoothing stage/controller if configured.
- `/relay_int` — integer topic used by the relay-control page (`1` = ON, `0` = OFF).

## My Project Work

This project was completed using **course-provided ROS 2 guidance and an existing open-source Unitree Go2 simulation as the foundation**. My hands-on work involved setting up and running the ROS 2/Gazebo environment, integrating and testing the browser-based controls, configuring the simulated camera workflow, working with ROS topics and rosbridge, and testing teleoperation behaviour.

The files in this repository should therefore be read as a **course-based implementation and integration project**, not as a claim that the underlying Unitree simulator, ROSLIB.js, nipplejs, or all tutorial code was authored from scratch.

## Notable Adaptations in This Snapshot

Compared with the published course reference, this uploaded snapshot includes implementation differences such as:

- `teleop.html` continuously publishes commands at about **20 Hz** while a key is held and publishes a final zero-velocity command on release.
- `joystick.html` publishes joystick commands to `/cmd_vel_smoothed` rather than directly to `/cmd_vel`.

## Typical Runtime Components

A full run generally requires:

- ROS 2 Humble
- Gazebo / Unitree Go2 simulation
- `rosbridge_server`
- Browser with JavaScript enabled
- Camera streaming service on port `8080` if the video pages are used

Example rosbridge command:

```bash
ros2 run rosbridge_server rosbridge_websocket --address 0.0.0.0 --port 9090 --cors-allowed-origins="*"
```

## Attribution

Course reference used during implementation:

- Steven Eu Kok Seng — RoboticSensorROS: https://stevenviewr.github.io/RoboticSensorROS/

Original Unitree Go2 ROS 2 simulation referenced in the coursework:

- https://github.com/anujjain-dev/unitree-go2-ros2

Third-party JavaScript libraries are listed in `THIRD_PARTY.md`.
