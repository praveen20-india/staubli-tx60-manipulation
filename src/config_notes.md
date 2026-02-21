# System Configuration and Motion Primitives

This document outlines the core VAL3 functions and configurations utilized in the Stäubli TX60 manipulation sequence.

## 1. Motion Planning and Control Primitives

### `movej(point, tool, mdesc)`
Executes a joint-space motion to the target Cartesian point. 
- **Joint-space vs Linear**: Joint-space (`movej`) is utilized here instead of linear (`movel`) because the intermediate trajectory between task poses is not strictly constrained, and `movej` generally results in faster, smoother, and more predictable joint coordination without singularity risks.
- **Parameters**: 
  - `point`: Target coordinate (`pos1[0]`, `pos1[1]`, or `pos0`) relative to the base frame.
  - `tool`: Tool coordinate system (`tool0`, `tool0[0]`, `tool0[1]`).
  - `mdesc`: Motion descriptor defining speed and blending (`mNomSpeed`).

### Speed Control (`mNomSpeed.vel`)
Velocity is governed by the `mNomSpeed.vel` attribute, set to an industrial safety standard of **30%** of the robot's maximum nominal speed for this specific sequence. During manual teaching and verification with the hand-held pendant (MCP), velocity is further scaled down to 25% for operator safety.

## 2. Tool Referencing and Tool Center Point (TCP)

The application employs dynamic tool referencing using an array of tool configurations:
- `tool0[0]`: Tool geometry and I/O mapping specific to the first manipulation task.
- `tool0[1]`: Modified configurations (offsets/rotations) applied for the second grasping task.
By switching the defined tool in the `movej` command, the controller automatically handles the TCP offsets and kinematic transformations without requiring manual coordinate recalculation.

## 3. Peripheral I/O Actuation

Control of the end-effector (pneumatic/electric gripper) is synchronized with motion using built-in system I/O primitives:
- `close(tool)`: Actuates the gripping mechanism mapped to the specific tool configuration.
- `open(tool)`: Releases the gripping mechanism.

## 4. Safety and State Management

- `enablePower()`: Initializes servo power. Must be called before executing any motion sequence.
- `resetMotion()`: Immediately clears the motion stack/buffer spanning the arm trajectory planning. Used exclusively in the Stop routine to halt unexpected execution states safely.
