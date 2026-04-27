# Adaptive Traffic Signal RL

**Deep Reinforcement Learning for Adaptive Urban Traffic Signal Control**
**應用深度強化學習於都市交通號誌動態控制**

This project proposes a Deep Reinforcement Learning-based adaptive traffic signal control framework that formulates urban intersection management as a Markov Decision Process. The goal is to train an intelligent agent to dynamically control traffic signals based on real-time traffic states, reducing vehicle waiting time, queue length, travel delay, and environmental impact.

---

## Project Overview

Urban traffic congestion is a critical issue in metropolitan areas, especially during rush hours. Traditional fixed-time signal control systems rely on pre-defined timing plans and are often unable to adapt to dynamic traffic demand. Although actuated and adaptive signal systems improve flexibility, many still depend on rule-based logic or heuristic optimization designed by traffic engineers.

This project explores the use of **Deep Reinforcement Learning (DRL)** to enable traffic signal controllers to learn adaptive control policies through interaction with a simulated traffic environment.

The proposed system uses:

* **SUMO** as the traffic simulation environment
* **SUMO-RL** as the reinforcement learning interface
* **Gymnasium** as the standardized environment API
* **Stable-Baselines3** for DRL model training
* **DQN / PPO** as reinforcement learning algorithms

---

## Research Motivation

Traffic signal control is not only related to transportation efficiency but also to road safety and environmental sustainability.

In the Taiwan urban traffic context:

* Major metropolitan areas face severe rush-hour congestion.
* Road traffic accidents remain a significant public safety concern.
* The transportation sector contributes to national carbon emissions.
* Existing smart signal deployments in Taiwan have shown measurable improvements in travel time and delay reduction.

These observations suggest that adaptive traffic signal control is a practical and meaningful research direction. Deep Reinforcement Learning provides a data-driven approach that can further improve the adaptability and long-term optimization capability of traffic signal systems.

---

## Research Objectives

This project aims to:

1. Formulate urban traffic signal control as a **Markov Decision Process (MDP)**.
2. Design a traffic-aware **state representation** for real-time intersection control.
3. Define an action space for adaptive signal phase selection.
4. Develop a reward function that considers efficiency, fairness, and environmental impact.
5. Train and evaluate DRL agents using SUMO-based simulation.
6. Compare DRL-based control with traditional baseline methods.

---

## Proposed Framework

The proposed system contains five major modules:

1. **Traffic Simulation Environment**

   * Built with SUMO
   * Simulates traffic flow, signal phases, vehicle movement, and emissions

2. **State Extraction Module**

   * Extracts real-time traffic features from the simulation environment

3. **Deep Reinforcement Learning Agent**

   * Learns signal control policies using DQN or PPO

4. **Signal Control Module**

   * Applies the selected signal action to the traffic light controller

5. **Reward Evaluation Module**

   * Computes reward based on traffic performance indicators

### System Flow

```text
SUMO Traffic Environment
        ↓
State Extraction
        ↓
Deep RL Agent
        ↓
Signal Control Action
        ↓
Environment Update
        ↓
Reward Calculation
        ↓
Policy Update
```

---

## MDP Formulation

The traffic signal control problem is formulated as a Markov Decision Process:

```text
MDP = <S, A, P, R, γ>
```

Where:

| Element | Description                                                 |
| ------- | ----------------------------------------------------------- |
| `S`     | State space representing real-time traffic conditions       |
| `A`     | Action space representing signal control decisions          |
| `P`     | State transition probability determined by traffic dynamics |
| `R`     | Reward function evaluating traffic performance              |
| `γ`     | Discount factor for long-term reward optimization           |

---

## State Representation

The state vector represents real-time traffic conditions at an intersection.

Example state features include:

* Queue length of each lane
* Average vehicle waiting time
* Current signal phase
* Remaining green time
* Average vehicle speed
* Traffic density
* Vehicle count per direction

Example:

```text
S_t = [
  queue_length_north,
  queue_length_south,
  queue_length_east,
  queue_length_west,
  waiting_time_north,
  waiting_time_south,
  waiting_time_east,
  waiting_time_west,
  current_signal_phase,
  remaining_green_time,
  average_vehicle_speed
]
```

---

## Action Space

The initial design adopts a discrete action space.

Possible actions include:

| Action | Description                       |
| ------ | --------------------------------- |
| `0`    | Keep current signal phase         |
| `1`    | Switch to north-south green phase |
| `2`    | Switch to east-west green phase   |
| `3`    | Extend current green phase        |

This design is suitable for early-stage experiments using **DQN** and **PPO**.

---

## Reward Function

The reward function guides the learning behavior of the agent.

A simple reward function may minimize total waiting time:

```text
R_t = - average_waiting_time
```

A more comprehensive reward function may include multiple traffic performance indicators:

```text
R_t = - (w1 × QueueLength + w2 × WaitingTime + w3 × Delay + w4 × Emissions)
```

Where:

| Term             | Meaning                                        |
| ---------------- | ---------------------------------------------- |
| `QueueLength`    | Total queue length across all lanes            |
| `WaitingTime`    | Average vehicle waiting time                   |
| `Delay`          | Travel delay compared with free-flow condition |
| `Emissions`      | Estimated CO2 emissions                        |
| `w1, w2, w3, w4` | Weight parameters                              |

The goal is to balance:

* Traffic efficiency
* Queue reduction
* Fairness across directions
* Environmental sustainability

---

## Algorithms

This project plans to evaluate the following Deep Reinforcement Learning algorithms:

### Deep Q-Network (DQN)

DQN is a value-based reinforcement learning algorithm that uses a neural network to approximate the Q-value function.

It is suitable for:

* Discrete action spaces
* Single-intersection control
* Baseline comparison

### Proximal Policy Optimization (PPO)

PPO is a policy-based / actor-critic algorithm that directly optimizes the policy while maintaining stable training updates.

It is suitable for:

* Complex traffic environments
* Stable policy learning
* Future extension to more advanced control settings

---

## Simulation Environment

This project uses **SUMO (Simulation of Urban Mobility)** as the traffic simulation environment.

SUMO supports:

* Microscopic traffic simulation
* Vehicle movement modeling
* Lane changing behavior
* Traffic signal control
* Route generation
* Emission estimation
* Data collection for traffic performance metrics

The project may use **SUMO-RL** to connect SUMO with reinforcement learning frameworks.

---

## Technology Stack

| Layer                  | Tool              |
| ---------------------- | ----------------- |
| Programming Language   | Python            |
| Traffic Simulator      | SUMO              |
| RL Environment Wrapper | SUMO-RL           |
| Environment API        | Gymnasium         |
| RL Library             | Stable-Baselines3 |
| Algorithms             | DQN, PPO          |
| Data Analysis          | Pandas, NumPy     |
| Visualization          | Matplotlib        |

---

## Experimental Scenarios

The project will evaluate the proposed method under several traffic scenarios.

### Scenario A: Single Standard Intersection

A four-way intersection with balanced traffic flow.

Purpose:

* Test basic convergence
* Evaluate basic signal switching ability

### Scenario B: Major Road and Minor Road Imbalance

One direction has significantly higher traffic volume than the other.

Purpose:

* Test fairness
* Avoid excessive waiting on minor roads
* Reduce unnecessary green time allocation

### Scenario C: Peak and Off-Peak Traffic Variation

Traffic demand changes over time.

Purpose:

* Test adaptability
* Evaluate performance under dynamic traffic conditions

---

## Baseline Methods

The proposed DRL-based method will be compared with:

| Baseline           | Description                                      |
| ------------------ | ------------------------------------------------ |
| Fixed-Time Control | Uses pre-defined signal timing                   |
| Actuated Control   | Adjusts signal timing based on vehicle detection |
| Random Policy      | Randomly selects signal actions                  |
| DQN                | Value-based DRL baseline                         |
| PPO                | Proposed main DRL method                         |

---

## Evaluation Metrics

The system will be evaluated using both traffic and algorithmic metrics.

### Traffic Efficiency Metrics

* Average waiting time
* Average queue length
* Average travel time
* Average delay
* Throughput
* Average vehicle speed

### Environmental Metrics

* CO2 emissions
* Idle time
* Stop-and-go frequency

### Learning Metrics

* Cumulative reward
* Convergence speed
* Training stability

---

## Expected Contributions

This project is expected to contribute:

1. A DRL-based framework for adaptive urban traffic signal control.
2. An MDP formulation for traffic signal decision-making.
3. A traffic-aware state representation for intersection control.
4. A joint reward function considering efficiency, delay, fairness, and emissions.
5. A SUMO-based simulation pipeline for model training and evaluation.
6. A comparative analysis between traditional control methods and DRL-based control.

---

## Limitations

This project also has several limitations:

* Simulation results may not fully reflect real-world traffic complexity.
* SUMO may not perfectly model local driving behaviors such as illegal parking, scooter lane filtering, or pedestrian violations.
* The initial design focuses on a single intersection.
* Multi-intersection coordination requires more complex multi-agent reinforcement learning.
* Real-world deployment would require safety constraints, legal compliance, and traffic engineering validation.

---

## Future Work

Potential future extensions include:

* Multi-agent reinforcement learning for multi-intersection control
* Green wave coordination along arterial roads
* Integration with real-time traffic camera data
* Testing with large-scale road networks using CityFlow
* Incorporating pedestrian and public transportation priority
* Sim-to-real transfer learning for real-world deployment

---

## Repository Structure

The expected project structure is:

```text
adaptive-traffic-signal-rl/
│
├── README.md
├── requirements.txt
├── .gitignore
│
├── configs/
│   ├── sumo_config.yaml
│   └── training_config.yaml
│
├── environments/
│   ├── __init__.py
│   └── traffic_signal_env.py
│
├── models/
│   ├── dqn/
│   └── ppo/
│
├── simulations/
│   ├── networks/
│   ├── routes/
│   └── sumo_cfg/
│
├── training/
│   ├── train_dqn.py
│   ├── train_ppo.py
│   └── evaluate.py
│
├── results/
│   ├── logs/
│   ├── figures/
│   └── metrics/
│
└── docs/
    ├── proposal.md
    └── slides/
```

---

## Getting Started

### 1. Clone the Repository

```bash
git clone https://github.com/your-username/adaptive-traffic-signal-rl.git
cd adaptive-traffic-signal-rl
```

### 2. Create Python Virtual Environment

```bash
python -m venv .venv
```

Activate the environment:

```bash
# Windows
.venv\Scripts\activate
```

```bash
# macOS / Linux
source .venv/bin/activate
```

### 3. Install Dependencies

```bash
pip install -r requirements.txt
```

### 4. Install SUMO

Please install SUMO from the official website:

```text
https://sumo.dlr.de/docs/Installing/index.html
```

After installation, make sure the `SUMO_HOME` environment variable is properly configured.

### 5. Run Training

Example:

```bash
python training/train_ppo.py
```

### 6. Evaluate Model

```bash
python training/evaluate.py
```

---

## Suggested Dependencies

Example `requirements.txt`:

```text
numpy
pandas
matplotlib
gymnasium
stable-baselines3
sumo-rl
traci
```

---

## Project Status

This project is currently in the proposal and prototype planning stage.

Planned milestones:

* [ ] Define SUMO simulation scenario
* [ ] Build single-intersection environment
* [ ] Implement state extraction
* [ ] Implement reward function
* [ ] Train DQN baseline
* [ ] Train PPO model
* [ ] Compare with fixed-time control
* [ ] Visualize evaluation results
* [ ] Prepare final presentation

---

## License

This project is intended for academic and educational purposes.

Recommended license:

```text
MIT License
```

---
