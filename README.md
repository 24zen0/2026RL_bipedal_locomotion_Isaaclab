# Point-Foot Locomotion: Reward Design and Simulation Evaluation

Point-foot locomotion experiments in Isaac Lab, covering velocity tracking, external-push recovery, and terrain traversal.

[Technical report](https://24zen0.github.io/files/RL_Locomotion_Technical_Report.pdf) · [Project demos](https://24zen0.github.io/#projects)

![Velocity tracking](media/speed_tracking.PNG)

## Overview

This project investigates how reward design affects locomotion accuracy and recovery behavior. It builds on the [RL bipedal locomotion framework](https://github.com/81578823/RL_bipedal_locomotion_Isaaclab) and uses PPO with an asymmetric actor–critic architecture in Isaac Lab.

## My work

- Compared velocity-tracking reward configurations and implemented an axis-aligned stillness term to reduce unintended motion along uncommanded axes.
- Evaluated external-push recovery under different survival and gait-regularization rewards.
- Configured a more challenging point-foot terrain setup and analyzed the results in the technical report.

These point-foot simulation experiments follow my wheel-legged TRON Camp work. The [project demos](https://24zen0.github.io/#projects) also include the wheel-legged hardware deployment.

## Velocity-tracking results

For the linear-command evaluation, the linear and angular tracking weights were held at 12 and 6. Adding an axis-aligned stillness weight of −0.5 reduced tracking errors in all three reported components:

| Tracking MSE | Without stillness term | With stillness term |
| --- | ---: | ---: |
| Forward velocity | 0.0679 | 0.0323 |
| Lateral velocity | 0.0783 | 0.0333 |
| Yaw rate | 0.0484 | 0.0412 |

The [technical report](https://24zen0.github.io/files/RL_Locomotion_Technical_Report.pdf) presents the full reward comparisons, push-recovery experiments, and terrain evaluation.

## Environment

- Isaac Lab 2.1.0
- Isaac Sim 4.5.0
- Python 3.10

## Installation

Install Isaac Lab using the [version 2.1.0 installation guide](https://isaac-sim.github.io/IsaacLab/v2.1.0/source/setup/installation/binaries_installation.html), then clone this repository:

```bash
git clone https://github.com/24zen0/2026RL_bipedal_locomotion_Isaaclab.git
cd 2026RL_bipedal_locomotion_Isaaclab
conda activate isaaclab

python -m pip install -e exts/bipedal_locomotion
python -m pip install -e rsl_rl
```

Run the following commands from the repository root with the Isaac Lab environment activated.

## Training

```bash
# Flat-ground locomotion
python scripts/rsl_rl/train.py \
  --task=Isaac-Limx-PF-Blind-Flat-v0 \
  --headless

# Rough-terrain locomotion
python scripts/rsl_rl/train.py \
  --task=Isaac-Limx-PF-Blind-Rough-v0 \
  --headless
```

Training options include `--num_envs`, `--max_iterations`, `--save_interval`, and `--seed`.

## Evaluation

```bash
# Flat-ground evaluation
python scripts/rsl_rl/play.py \
  --task=Isaac-Limx-PF-Blind-Flat-Play-v0 \
  --num_envs=10

# Rough-terrain evaluation with a saved checkpoint
python scripts/rsl_rl/play.py \
  --task=Isaac-Limx-PF-Blind-Rough-Play-Near-v0 \
  --checkpoint_path=/path/to/checkpoint.pt \
  --num_envs=1 \
  --video \
  --video_length=800
```

Keyboard controls:

| Keys | Action |
| --- | --- |
| W / S | Forward / backward command |
| A / D | Left / right command |
| 1 / 2 / 3 / 4 | Forward / left / backward / right push |

Push settings are configured in `play.py`.

## Demonstrations

### Multi-terrain traversal

![Multi-terrain traversal](media/multi-terrain-traversal.gif)

### Velocity tracking

![Velocity tracking](media/speed_tracking.gif)

### Push recovery

![Push recovery](media/push.gif)

## Deployment resources

- [TRON1 MuJoCo simulation](https://github.com/limxdynamics/tron1-mujoco-sim)
- [TRON1 policy deployment tools](https://github.com/limxdynamics/tron1-rl-deploy-python)
- [LimX Dynamics deployment guide](https://support.limxdynamics.com/docs/tron-1-sdk/rl-training-results-deployment)

## Upstream code and references

This repository is a fork of [81578823/RL_bipedal_locomotion_Isaaclab](https://github.com/81578823/RL_bipedal_locomotion_Isaaclab). Thanks to the upstream contributors for the locomotion learning framework.

The learning framework draws on [Concurrent Teacher-Student Reinforcement Learning for Legged Locomotion](https://doi.org/10.1109/LRA.2024.3457379), H. Wang, H. Luo, W. Zhang, and H. Chen, 2024.

Report author: **Yitao Zeng**.
