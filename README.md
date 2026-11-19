# BQ-BioRA: A Behavior- and Queue-Aware Bio-Inspired Resource Allocation Framework for Energy-Harvesting Ultra-Dense 6G MEC Networks

## 👥 Authors
- Harshita Patil
- Dr. Kaushlendra Sharma
- Dr. Shishupal Kumar

Department of Computer Science and Engineering, Indian Institute of Information Technology (IIIT) Nagpur, India

## 📌 Overview
This repository contains the source code and simulation environment for the research paper **"BQ-BioRA: A Behavior- and Queue-Aware Bio-Inspired Resource Allocation Framework for Energy-Harvesting Ultra-Dense 6G MEC Networks."**

BQ-BioRA is a three-layer framework for joint computation offloading and resource allocation in ultra-dense Mobile Edge Computing (MEC) networks with energy-harvesting mobile devices. It addresses three sources of uncertainty that make resource allocation difficult in dense 6G deployments:

- 🧠 **Dynamic user behavior**: devices do not always act fully rationally.
- 🚦 **Server-side congestion**: many tasks can arrive at the same MEC server at once.
- 🔋 **Energy-harvesting uncertainty**: harvested energy is intermittent and time-varying.

At each time slot, every mobile device chooses one of three computation modes:

- **Local Execution**: process the task on the device CPU.
- **MEC Offloading**: send the task to one of the MEC-enabled small base stations.
- **Task Dropping**: drop the task when energy or resources are insufficient.

The objective is to minimize the weighted sum of task execution delay and device energy consumption while satisfying battery, power, CPU-frequency, and queue-stability constraints.

## ✨ Key Features

### 🧠 Layer 1: Behavior-Aware Strategy Evolution (SEGT)
Uses **Stochastic Evolutionary Game Theory** with a Moran birth–death process to model how devices choose between local execution and offloading. The model includes bounded rationality and probabilistic decision errors. The stationary distribution of the process gives the effective offloading rate and the aggregate task load at each MEC server.

### 🚦 Layer 2: Queue-Aware Performance Modeling (M/M/c)
Models each MEC server as an **M/M/c queue** and computes the following metrics every time slot:
- Server utilization ρ
- Erlang-C waiting probability
- Mean queue length L_q
- Mean waiting time W_q (Little's law)
- Total sojourn time

Congested servers are penalized and less-loaded servers are preferred, so the framework controls congestion before delays build up.

### 🐦 Layer 3: Bio-Inspired Resource Optimization (SBOA)
Uses the **Secretary Bird Optimization Algorithm** to jointly optimize four decisions:
- Task offloading decisions
- Transmission power
- Local CPU frequency
- Device–server association

The search has two phases:
- **Hunting (exploration)**: differential search, then Brownian motion, then Lévy flight.
- **Escape (exploitation)**: camouflage or rapid flight.

A greedy selection step keeps only improved solutions.

### 🔋 Energy-Harvesting Battery Model
Each device has a finite-capacity battery that is replenished by stochastic energy harvesting. Every decision must satisfy energy-causality and battery-capacity constraints.

### 📡 Realistic Ultra-Dense Environment
- 100 mobile devices and 10 MEC servers in a 100 m × 100 m area
- OFDMA uplink with Rayleigh fading
- Random Walk mobility

## 🏗️ System Framework

```
              Energy-Harvesting Mobile Devices
                  (stochastic task arrivals)
                            │
                            ▼
        ┌────────────────────────────────────────┐
        │  Layer 1: SEGT Behavior Modeling       │
        │  Local vs. offload strategy evolution  │
        └────────────────────────────────────────┘
                            │  offloading rate → λ_j(t)
                            ▼
        ┌────────────────────────────────────────┐
        │  Layer 2: M/M/c Queueing Model         │
        │  ρ_j, L_q, W_q, congestion detection   │
        └────────────────────────────────────────┘
                            │  queue-aware delay W_q
                            ▼
        ┌────────────────────────────────────────┐
        │  Layer 3: SBOA Optimization            │
        │  offloading, power, CPU freq, server   │
        └────────────────────────────────────────┘
                            │
             ┌──────────────┼──────────────┐
             ▼              ▼              ▼
          Local          Offload         Drop
             └──────────────┼──────────────┘
                            ▼
          Battery Update & Performance Evaluation
     (cost, energy, queue delay, drop ratio, battery)
```

## 📂 Repository Contents

| File / Folder | Type | Description |
|---|---|---|
| `harshitha.ipynb` | Jupyter Notebook | Simulation of the BQ-BioRA framework: system model, SEGT behavior layer, M/M/c queue model, SBOA optimizer, and performance metrics. |
| `results/` | Folder | Generated result figures (listed below). |
| `README.md` | Documentation | Project description, installation, and reproducibility information. |

### 🖼️ Generated Figures (`results/`)

| File | Description |
|---|---|
| `fig_md_movement.png` | Mobile device movement trajectories (Random Walk) |
| `fig_network_topology.png` | Initial positions of mobile devices and MEC servers with coverage areas |
| `fig15_queue_aware.png` | MEC server utilization and mean queue waiting delay |
| `fig_battery_level.png` | Battery energy level convergence over time |
| `fig_energy_consumption.png` | Average energy consumption per time slot |
| `fig8_bar_energy.png` | Average energy consumption comparison |
| `fig12_energy_efficiency.png` | Energy efficiency (tasks/J) comparison |
| `fig_execution_cost.png` | Average execution cost per time slot |
| `fig7_bar_execution_cost.png` | Average execution cost comparison |
| `fig9_cdf_execution_cost.png` | CDF of execution cost |
| `fig11_scalability.png` | Execution cost vs. number of mobile devices |
| `fig13_convergence.png` | Convergence (running average) of execution cost |
| `fig_task_drop_ratio.png` | Task drop ratio over time |
| `fig14_radar_chart.png` | Multi-metric radar comparison |

## ⚙️ Simulation Parameters

| Parameter | Value |
|---|---|
| Network area | 10,000 m² (100 m × 100 m) |
| Number of MEC servers (SBSs) | 10 |
| Number of mobile devices | 100 |
| Multiple access scheme | OFDMA |
| Channel bandwidth | 1 MHz |
| Carrier frequency | 0.915 GHz |
| Path loss exponent / antenna gain | 3 / 4 |
| Noise power | −130 dBm |
| Task input size | 1 kB |
| CPU cycles per bit | 737.5 cycles/bit |
| Battery energy (min / max) | 0.02 mJ / 2 mJ |
| Max transmission power | 1 W |
| Max CPU frequency | 1.5 GHz |
| Behavior model / queue model / optimizer | SEGT / M/M/c / SBOA |
| Computation modes | Local execution, MEC offloading, task dropping |

## 🆚 Baseline Methods

| Method | Description |
|---|---|
| **LEA**: Local Execution All | All tasks are executed locally on mobile devices. |
| **OA**: Offloading All | All tasks are offloaded to MEC servers. |
| **LYMSD**: Lyapunov-MILP Shortest Distance | Devices are associated with the nearest MEC server. |
| **LYMOC**: Lyapunov-MILP Optimal Cost | A Lyapunov-based MILP minimizes execution cost. |

## 💻 Requirements
- Python 3.9 or later
- NumPy
- Pandas
- SciPy
- Matplotlib
- Jupyter Notebook / JupyterLab

## 🛠️ Installation

### 1. Clone the Repository
```bash
git clone https://github.com/harshitawankhede27/BQ-BioRA.git
cd BQ-BioRA
```

### 2. Create a Virtual Environment (recommended)

**Windows**
```bash
python -m venv venv
venv\Scripts\activate
```

**Linux/macOS**
```bash
python3 -m venv venv
source venv/bin/activate
```

### 3. Install Dependencies
```bash
pip install numpy pandas scipy matplotlib jupyter
```

## ▶️ Running the Code
Launch Jupyter and open the notebook:

```bash
jupyter notebook BQ_BioRA.ipynb
```

Run all cells in order (**Kernel → Restart & Run All**). The notebook performs four steps:
1. Initializes the ultra-dense MEC network, devices, servers, and batteries.
2. Runs BQ-BioRA and the baseline methods over all time slots.
3. Prints the performance summary table.
4. Saves the result figures to the `results/` folder.

The main configuration flags at the top of the notebook are:

```python
USE_OPTIMIZATION = True   # True = SBOA optimization, False = SEGT only
USE_QUEUE_MODEL  = True   # Include M/M/c queue delay in the optimization
NUM_DEVICES      = 100
NUM_SERVERS      = 10
```

## 📈 Results

### Overall Performance Summary

| Algorithm | Execution Cost (ms) | Energy (µJ) | Drop Rate | Battery (mJ) | Queue Wait (ms) |
|---|---|---|---|---|---|
| LEA | 1284.19 | 79.94 | 0.01% | 1.9981 | N/A (local only) |
| OA | 646.27 | 55.32 | 0.00% | 1.9987 | 16.250 |
| LYMSD | 646.37 | 55.17 | 0.00% | 1.9988 | 16.248 |
| LYMOC | 598.85 | **52.51** | 0.00% | 1.9990 | 15.743 |
| **BQ-BioRA** | **586.81** | 52.55 | **0.00%** | 1.9987 | **15.722** |

### Execution-Cost Reduction Achieved by BQ-BioRA

| Compared with | Cost reduction | Energy reduction |
|---|---|---|
| LEA | 54.3% | 34.3% |
| OA | 9.2% | 5.0% |
| LYMSD | 9.2% | 4.8% |
| LYMOC | 2.0% | −0.1% (comparable) |

### Key Findings
- 🏆 **Lowest execution cost** (586.8 ms) of all evaluated methods.
- 🚦 **Lowest mean queue waiting delay** (15.722 ms), with server utilization kept below the high-congestion zone (ρ < 80%).
- ⚡ **Competitive energy consumption** (52.55 µJ), within 0.04 µJ of the best baseline.
- 🔋 **Stable battery levels** close to maximum capacity (~2.0 mJ) throughout the simulation.
- 🌿 **High energy efficiency** of 1.52 × 10⁶ tasks/J, on par with LYMOC and higher than OA, LYMSD, and LEA.
- 📊 **Best scalability**: lowest execution cost at every load from 20 to 100 mobile devices.
- ✅ **Zero task drops** under the evaluated scenario.

> **Note:** These values correspond to the simulation configuration described in the associated manuscript. Results may vary slightly with different random seeds.

## 🔬 Reproducibility
To reproduce the experiments reported in the manuscript:

1. Install the required Python dependencies.
2. Clone or download the repository.
3. Run all notebook cells in order.
4. Check the generated figures in `results/` and the printed summary table.
5. Compare the metrics with those reported in the manuscript.

The experiments were run on an Intel Core i5-13420H processor with an NVIDIA GeForce RTX 5050 GPU, using Python 3.9 and PyCharm 2025.2.

## 📚 Research Context
This framework studies sustainable, congestion-aware resource management for next-generation 6G edge computing. The optimization balances three goals:

**Execution Delay ↓ + Energy Consumption ↓ + Queue Stability ✓**

The approach combines:
- Stochastic Evolutionary Game Theory (SEGT)
- M/M/c queueing theory and the Erlang-C model
- The Secretary Bird Optimization Algorithm (SBOA)
- Energy-harvesting battery modeling
- OFDMA-based communication modeling
- Joint offloading, power, CPU-frequency, and server-association optimization

## 📄 Manuscript
**Title:** BQ-BioRA: A Behavior- and Queue-Aware Bio-Inspired Resource Allocation Framework for Energy-Harvesting Ultra-Dense 6G MEC Networks

**Authors:** Harshita Patil, Dr. Kaushlendra Sharma, Dr. Shishupal Kumar

**Status:** Under review

## ✉️ Contact
For questions about the source code, simulation environment, or reproduction of the results, contact:

**Harshita Patil**
📧 dtea23cse017@iiitn.ac.in

## 📜 License
This repository is intended for research and academic purposes.

If you use the code or methodology in your research, please cite the associated manuscript.
