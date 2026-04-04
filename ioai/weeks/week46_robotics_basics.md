# Week 46 — Robotics and Simulation Basics (Team Challenge Prep)
## Phase 5: Peak Performance Maintenance (Week 46 of 48)

**IOAI 2025 team challenge used Galbot robot arm simulation. IOAI 2026 format unknown — this prepares for robotics/simulation formats.**

---

## Theory + Practice (8h)

---

## PyBullet Basics
```python
import pybullet as p
import pybullet_data
import time

# Initialize
physicsClient = p.connect(p.GUI)
p.setAdditionalSearchPath(pybullet_data.getDataPath())
p.setGravity(0, 0, -9.81)

# Load environment
planeId = p.loadURDF("plane.urdf")
robotId = p.loadURDF("kuka_iiwa/model.urdf", [0, 0, 0], useFixedBase=True)
objectId = p.loadURDF("cube_small.urdf", [0.5, 0, 0.1])

# Print joint info
n_joints = p.getNumJoints(robotId)
for i in range(n_joints):
    info = p.getJointInfo(robotId, i)
    print(f"Joint {i}: {info[1].decode()}, type: {info[2]}")

# Control joints (position control)
target_angles = [0, -0.5, 0, 1.0, 0, -0.5, 0]
for i, angle in enumerate(target_angles):
    p.setJointMotorControl2(robotId, i, p.POSITION_CONTROL,
                             targetPosition=angle, force=500)

# Step simulation
for _ in range(1000):
    p.stepSimulation()
    time.sleep(1./240.)
```

## Inverse Kinematics (move end-effector to target position)
```python
# Calculate joint angles to reach a target position
target_pos = [0.5, 0.2, 0.4]
target_orn = p.getQuaternionFromEuler([0, 0, 0])

joint_angles = p.calculateInverseKinematics(
    robotId,
    endEffectorLinkIndex=6,  # index of end-effector link
    targetPosition=target_pos,
    targetOrientation=target_orn
)

for i, angle in enumerate(joint_angles[:7]):
    p.setJointMotorControl2(robotId, i, p.POSITION_CONTROL, targetPosition=angle)
```

## Pick and Place Strategy
```
1. Move to pre-grasp position (above object)
2. Lower to grasp position
3. Close gripper
4. Lift object
5. Move to target position
6. Lower
7. Open gripper
8. Lift away
```

---

## Sim-to-Real Concepts

- **Domain randomization**: vary physics parameters (mass, friction, lighting) to make policy robust
- **Why it's hard**: real robots have sensor noise, calibration errors, latency — simulation doesn't
- **IOAI relevance**: team challenge scoring is in simulation, so sim accuracy matters more than real-world transfer

---

## Practice

- Solve a pick-and-place task: move a cube from position A to position B using PyBullet
- Implement a simple feedback controller (PD controller) for joint control

---

## Deliverable

`week46_robotics_basics.ipynb` — PyBullet environment setup + working pick-and-place demo

---

*Part of the IOAI Gold Medal 52-Week Training Program — Phase 5: Peak Performance Maintenance*
