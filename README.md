# Stäubli TX60 Robotic Manipulation using VAL3

## Overview
Implementation of a highly repeatable, joint-space manipulation sequence on a Stäubli TX60 industrial robot using the VAL3 programming environment. The project encompasses advanced trajectory planning, precise I/O synchronization for end-effectors, and rigorous software-in-the-loop (SITL) simulation testing before physical deployment.

## System Architecture
* **Manipulator**: Stäubli TX60 (6-axis articulated arm)
* **Controller**: CS8 Series Controller
* **Software Environment**: VAL3 via Stäubli Robotics Suite (SRS)
* **Peripheral Integration**: Synchronized Tool I/O control

## Application Logic
The software architecture is robustly segmented into sequential operational phases:
1. **Initialization**: Servo power enablement and system verification.
2. **First Actuation Phase**: Joint-space transition to Task Pose 1, followed by gripper actuation.
3. **Second Actuation Phase**: Transition to Task Pose 2, incorporating alternative tool/TCP offsets.
4. **Recovery & Safe Shutdown**: Controlled return to home position (`pos0`), releasing payloads, and purging the motion buffer.

## Key Technical Concepts
* **Joint-Space Trajectory Planning**: Utilizing `movej` for optimal, non-singular path routing between manipulation points.
* **Tool Coordinate Frames (TCP)**: Dynamic referencing of tool geometries (`tool0[0]`, `tool0[1]`) natively calculating end-effector kinematics.
* **I/O Synchronization**: Deterministic hardware integration for pneumatic valves/grippers.
* **Safety Validation Workflow**: Strict deployment pipeline from simulated digital twin verification to full physical execution.

## Simulation Validation
Extensive testing within the SRS 3D emulation environment:
* **Collision Detection**: Verified absolute clearance across all operational joints.
* **Path Verification**: Pre-calculated joint routes observed for irregularities or extreme joint limits.

## Real Robot Execution
Deployed via the SRS Transfer Manager to the physical CS8 Control Unit:
* **Successful Deployment**: Direct program transfer and interpretation.
* **I/O Validation**: Verified 100% correct logic thresholds triggered at appropriate physical poses.

## Performance
* **Total Cycle Time**: $\approx$ 21.5 seconds per operational loop.
* **Stability**: Highly stable motion execution devoid of hardware faults.
* **Error Rate**: 0 critical runtime errors. Only minor coordinate offset tuning was required initially.

## Applications
This workflow and codebase structure is directly applicable to robust industrial scenarios:
* Industrial Pick-and-Place
* Assembly Line Automation
* General Robotic Cell Integration

## Demonstration
## 🎥 Simulation Run
simulation/Simulation_Video.mp4

## 🤖 Real Robot Execution
real_robot/Robot_Operating_Video.mp4
