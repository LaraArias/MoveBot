# MoveBot — Your Personal Movement Instructor

**MoveBot turns any movement video into a robot instructor.** Pick a discipline, watch a Unitree G1 humanoid robot demonstrate the moves in simulation, and learn step by step.

> Built at [Nebius Build SF 2026](https://cerebralvalley.ai/events/~/e/nebius-build-sf) hackathon.

## Why This Matters

Physical inactivity is a global health crisis hiding in plain sight.

**In the United States:**
- 22% of adults report zero physical activity outside of work. Only 1 in 4 adults meet physical activity guidelines.
- Adults earning under $25,000 have inactivity rates **4x higher** than those earning $150,000+. Those without a high school diploma have rates **3x higher** than college graduates.
- Over 100 million American adults live with obesity. Healthcare costs: **$173–260 billion per year**.
- 8.3% of deaths among nondisabled adults 25+ are directly attributable to physical inactivity.

**In Mexico:**
- Adult physical inactivity rose from 16.5% to 19.8% between 2018 and 2022 — and it's accelerating.
- **7 out of 10 students** aged 10-14 don't meet basic physical activity recommendations. 91% of adolescents spend 2+ hours daily on screens.
- Only **1.5% of children** meet all three movement behavior recommendations (activity, sedentary time, sleep).
- Obesity is projected to reach **48% by 2040**. Overweight already decreases Mexico's GDP by 5.3% — the worst in the OECD.

**The access problem:** Personal instruction costs $50-100/hr per discipline. The people who need movement education most — low-income families, underserved communities, kids glued to screens — are the ones who can least afford it.

## The Solution

**One robot. Infinite instructors.** Upload any video of a human performing a movement, and MoveBot:

1. **Extracts 3D body pose** from the video using PromptHMR (SMPL-X)
2. **Retargets the motion** to the Unitree G1's 29 joints using GMR
3. **Trains a reinforcement learning policy** (PPO) in MuJoCo Warp on Nebius Cloud
4. **Delivers a robot instructor** that performs the move and teaches it step by step

Why hire 5 teachers when one inference gives everyone in the house their own personal instructor? Dad learns karate, mom does yoga, kid takes ballet — all from the same robot, trained from video. At $3/hr of GPU compute instead of $100/hr per instructor.

## Demo

The live demo connects to a Unitree G1 simulation running on an NVIDIA H200 GPU on Nebius Cloud via the [viser](https://github.com/nerfstudio-project/viser) web viewer.

### Running locally

```bash
python3 -m http.server 3000
# Open http://localhost:3000
```

Creative Dance → Dance Routine No. 1 connects to the live MuJoCo simulation.

## Architecture

```
Video (YouTube, TikTok, camera)
    ↓
video2robot: PromptHMR → SMPL-X pose extraction → GMR → Unitree G1 motion retargeting
    ↓
robot_motion.csv (LAFAN format, 29 DOF)
    ↓
mjlab: csv_to_npz → MuJoCo Warp (GPU-accelerated physics) → PPO training (512 parallel envs)
    ↓
Trained policy (.pt checkpoint)
    ↓
MuJoCo simulation via viser (web-based 3D viewer)
    ↓
MoveBot UI — learn moves, try them yourself, quiz
```

## Tech Stack

| Component | Technology |
|-----------|-----------|
| **GPU Compute** | NVIDIA H200 (141GB VRAM) on Nebius Cloud |
| **Pose Extraction** | [PromptHMR](https://github.com/yufu-wang/PromptHMR) (SMPL-X) |
| **Motion Retargeting** | [GMR](https://github.com/YanjieZe/GMR) |
| **Physics Simulation** | [MuJoCo Warp](https://github.com/google-deepmind/mujoco_warp) (GPU-accelerated) |
| **RL Training** | PPO via [RSL-RL](https://github.com/leggedrobotics/rsl_rl) |
| **Robot** | Unitree G1 (29 DOF humanoid) |
| **3D Viewer** | [viser](https://github.com/nerfstudio-project/viser) |
| **Pipelines** | [video2robot](https://github.com/DavidDobas/video2robot-hackathon), [mjlab](https://github.com/DavidDobas/mjlab-hackathon) |
| **Frontend** | Vanilla HTML/CSS/JS |

## Disciplines

- **Karate** — Basic Kata (6 forms with step-by-step instructions)
- **Ballet** — Basics (4 positions)
- **Calisthenics** — Basic Workout (5 exercises)
- **Yoga** — Basic Stretches (5 poses)
- **Creative Dance** — Community Routines (LIVE simulation)

## Training Stats

- **Training time**: ~21 minutes for 2000 iterations
- **Parallel environments**: 512
- **GPU**: NVIDIA H200 on Nebius Cloud
- **Iteration time**: 0.63s
- **Motion source**: LAFAN1 motion capture dataset

## Impact Potential

- **Cost reduction**: $3/hr GPU compute vs $100/hr personal instructor
- **Accessibility**: Any video becomes an instructor — democratizes movement education across income levels
- **Scalability**: One Unitree G1 robot serves unlimited disciplines for an entire household
- **Sim-to-real path**: Trained policies are validated for transfer to physical G1 hardware (proven in published research: ASAP, ExBody2, SONIC)
- **Community-driven**: Anyone can upload a video and create a new lesson — the library grows with its users

## Data Sources

- US physical inactivity & obesity: CDC/BRFSS, Americas Health Rankings, TFAH
- Mexico physical inactivity & obesity: ENSANUT 2006-2022, INEGI, OECD
- Unitree G1 capabilities: Published research (ASAP/CMU, ExBody2, SONIC, HumanUP)

## Team

Built by [Carlos Lara](https://github.com/LaraArias) at Nebius Build SF 2026.

## License

MIT
