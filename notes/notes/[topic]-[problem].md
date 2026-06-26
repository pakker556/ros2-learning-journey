# Gazebo + AMCL + TF Tree Issues in Andino Navigation

## Date
2026-06-25

## Environment
- ROS 2: Humble
- Simulation: Gazebo + Andino robot
- Running inside: Docker container on VMware Workstation
- Package: robotics_essentials_ros2 (2-slam_and_navigation_demo)

## Problem Description
After launching the simulation, the robot was visible in Gazebo, but it did not appear on the map in RViz. RViz showed the error:  
`No transform from base_link to map`

## Steps to Reproduce
1. Ran `ros2 launch andino_gz andino_gz.launch.py`
2. Opened RViz with the navigation configuration
3. Tried to set initial pose using **2D Pose Estimate**
4. Robot did not appear on the map

## Observed Behavior / Error
- `/odom` topic was not publishing
- AMCL particles remained scattered across the map
- `ros2 lifecycle set /amcl` returned "Unknown transition requested"
- Robot was visible and controllable in Gazebo

## Root Cause
The `/odom` topic was not being published (likely due to lazy bridge or controller_manager not starting properly). Without odometry, AMCL could not localize the robot, so the `map` → `odom` transform was never published.

## Solution / Workaround
- Restarted AMCL using lifecycle commands (partially helped)
- Kept `ros2 topic echo /odom` running to wake up the lazy bridge
- Eventually decided to skip this section temporarily due to repeated issues with the current setup (Docker + VMware)

## Final Status
- [x] Skipped for now
- [ ] Will try again with a cleaner setup later

## References
- https://github.com/henki-robotics/robotics_essentials_ros2
