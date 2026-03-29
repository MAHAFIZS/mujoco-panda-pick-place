# MuJoCo Franka Panda Pick & Place

This project implements a **Cartesian-space pick-and-place controller** for the
**Franka Emika Panda** robot in **MuJoCo**, with a small **Tkinter GUI** to tune
parameters and trigger the motions.

The controller:
- Automatically **locates the target object** (e.g. `box`) in the MuJoCo scene,
- Moves the end-effector in **smooth Cartesian trajectories** (hover → pre-grasp → grasp),
- **Closes the gripper** to grasp the object,
- Lifts it and **places it at a user-defined XY position**.

---



---

## Features

- 🦾 **Franka Emika Panda model in MuJoCo**
  - Uses `world.xml` + `panda` hand model with finger actuators.

- 🎯 **Automatic object detection**
  - Tries the GUI-specified body name (e.g. `box`).
  - Falls back to auto-detecting the smallest non-robot box/cylinder body.

- 📐 **Cartesian impedance control**
  - Keeps the hand pose at a desired position & orientation (`target_pos`, `target_quat`).
  - Uses Jacobians and a 6D stiffness matrix `K` to compute torques for joints `panda_joint1..7`.

- 🤏 **Gripper control**
  - Opens/closes the gripper via finger position actuators
    (`pos_panda_finger_joint1`, `pos_panda_finger_joint2`).

- 📦 **Pick & Place pipeline**
  - Compute object top height and XY position.
  - Generate waypoints:
    - `hover` (above object),
    - `pregrasp` (5 mm above top),
    - `grasp` (slightly below top).
  - Execute:
    - Move to hover → pre-grasp → grasp  
    - Close gripper and settle  
    - Lift back to hover  
    - Move to place position → lower → open gripper → lift.

- 🖥️ **Tkinter GUI**
  - Fields:
    - Target body name
    - Down distance / time
    - Settle time
    - Up distance / time
    - Place X, Place Y
  - Buttons:
    - **Run Pick & Place** (full sequence)
    - **Pick** (only grasp + lift)
    - **Place** (place at new XY location)

---

## Repository Structure

```text
.
├── world.xml              # MuJoCo scene (Panda + table + box)
├── panda_...xml           # Panda model (hand + finger actuators)
├── panda_pick_place.py    # Main script (Demo class + GUI)
└── README.md              # This file
