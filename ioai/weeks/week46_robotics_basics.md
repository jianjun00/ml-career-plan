# Week 46 — Robotics and Simulation Basics (Team Challenge Prep)
## Phase 5: Peak Performance Maintenance (Week 46 of 48)

**IOAI 2025 team challenge used Galbot robot arm simulation. IOAI 2026 format unknown — this prepares for robotics/simulation formats.**

---

## Theory + Practice (8h)

---

## Reinforcement Learning Basics (required context for robotics team challenges)

IOAI 2025's Galbot challenge required agents to complete manipulation tasks. Understanding RL is essential even if you use classical controllers — you need to understand what a **policy**, **reward signal**, and **episode** are to debug a simulation environment.

```
Core RL concepts:
  Agent: the controller (your code)
  Environment: the simulator (PyBullet)
  State s_t: robot joint angles + object positions at time t
  Action a_t: joint torques or target positions
  Reward r_t: scalar signal (e.g., -1 if not grasped, +10 if object placed correctly)
  Policy π(a|s): function mapping state to action

Goal: maximize cumulative reward Σ γᵗ rₜ (γ = discount factor, 0 < γ < 1)
```

### Simplest working approach: PD Controller (no RL needed for basic tasks)
```python
# Proportional-Derivative controller for smooth joint movement
# No ML required — just physics-based control
def pd_control(current_angle, target_angle, current_vel, kp=100, kd=10):
    error = target_angle - current_angle
    return kp * error - kd * current_vel  # proportional + derivative damping

# Apply at each simulation step:
for step in range(1000):
    joint_states = p.getJointStates(robotId, range(n_joints))
    for i in range(n_joints):
        current_ang = joint_states[i][0]
        current_vel = joint_states[i][1]
        torque = pd_control(current_ang, target_angles[i], current_vel)
        p.setJointMotorControl2(robotId, i, p.TORQUE_CONTROL, force=torque)
    p.stepSimulation()
```

### Policy Gradient (if RL is explicitly required)
```python
# REINFORCE: simplest policy gradient algorithm
# policy network: state → action probabilities
import torch.nn.functional as F

class PolicyNet(nn.Module):
    def __init__(self, state_dim, action_dim):
        super().__init__()
        self.net = nn.Sequential(nn.Linear(state_dim, 64), nn.ReLU(),
                                  nn.Linear(64, action_dim))
    def forward(self, s): return F.softmax(self.net(s), dim=-1)

# Training loop: collect episode, compute returns, update with gradient
def train_episode(policy, optimizer, env):
    log_probs, rewards = [], []
    state = env.reset()
    for _ in range(max_steps):
        probs = policy(torch.FloatTensor(state))
        action = torch.multinomial(probs, 1).item()
        log_probs.append(torch.log(probs[action]))
        state, reward, done, _ = env.step(action)
        rewards.append(reward)
        if done: break
    # Compute discounted returns
    G, returns = 0, []
    for r in reversed(rewards):
        G = r + 0.99 * G; returns.insert(0, G)
    returns = torch.FloatTensor(returns)
    returns = (returns - returns.mean()) / (returns.std() + 1e-8)
    loss = -torch.stack(log_probs) @ returns
    optimizer.zero_grad(); loss.backward(); optimizer.step()
```

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
