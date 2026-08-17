# Project ChaosIndex: Resilient Traffic Signal Control for Non-Standard Urban Mobility

## Abstract
Current Artificial Intelligence (AI) models for traffic signal coordination are predominantly trained on highly structured, rule-compliant datasets. Consequently, these models suffer from severe performance degradation when deployed in developing urban environments characterized by non-standard, chaotic traffic behaviors. This project introduces a novel framework that quantifies abnormal vehicular trajectories into a mathematical variable, denoted as the $Chaos\_Index$, and integrates it as a dynamic reward/penalty function within a Reinforcement Learning (RL) architecture to optimize traffic signal phases and alleviate gridlock.

## 1. Problem Statement
The fundamental flaw in modern Intelligent Transportation Systems (ITS) when applied to emerging megacities is the significant domain shift in driving behavior.
*   **The "Clean Data" Bias:** Existing models assume strict lane discipline, homogeneous vehicle types, and standardized intersection clearing times.
*   **The Reality of Urban Chaos:** Real-world intersections in developing nations experience high volumes of mixed traffic, arbitrary lane-changing, lane-splitting, and complex trajectory conflicts.
*   **System Failure:** When confronted with this noise, conventional computer vision algorithms and static timing models fail to accurately estimate throughput, leading to compounding congestion rather than relief.

## 2. Proposed Methodology

### 2.1. Trajectory Extraction via Computer Vision
Instead of merely counting vehicles (traditional volume metrics), the system utilizes real-time multi-object tracking (MOT) to map the continuous trajectory of each agent within the intersection.
*   **Detection & Tracking:** Extracting bounding boxes and associating them across frames to generate continuous position and velocity vectors.
*   **Behavioral Mapping:** Isolating trajectories that deviate from standardized lane-following patterns.

### 2.2. Mathematical Modeling: The $Chaos\_Index$
We formulate a scalar metric to quantify the degree of intersection disorder. The $Chaos\_Index$ is derived from trajectory variances and spatial conflict rates.

$$Chaos\_Index = \alpha \cdot \sigma^2(\theta) + \beta \cdot \tau_{conflict} + \gamma \cdot \rho_{density}$$

Where:
*   $\sigma^2(\theta)$: Variance in vehicle heading angles (indicating weaving or non-linear movement).
*   $\tau_{conflict}$: Frequency of intersecting trajectories with a critical Time-to-Collision (TTC).
*   $\rho_{density}$: Spatial density of vehicles localized in unstructured clusters.
*   $\alpha, \beta, \gamma$: Empirically defined weight coefficients.

### 2.3. Reinforcement Learning (RL) Controller
The core optimization engine formulates the intersection control as a Markov Decision Process (MDP).
*   **State Space:** Current signal phase, queue lengths across all approaches, and the real-time $Chaos\_Index$.
*   **Action Space:** Extension or termination of the current green light phase.
*   **Reward Function:** A composite function that heavily penalizes a high $Chaos\_Index$. This forces the RL agent to learn policies that actively target and "flush" chaotic clusters, restoring orderly macroscopic flow.

## 3. Experimental Environment & Custom Dataset
To validate the hypothesis and avoid the bias of standard Western traffic datasets, this project builds a proprietary experimental foundation:
*   **Data Acquisition:** Aggregating and processing high-angle video streams from highly unstructured intersections.
*   **Simulation Integration:** Utilizing microsimulation tools (e.g., Eclipse SUMO) combined with the custom dataset to recreate chaotic intersections mathematically. This allows the RL agent to undergo millions of training episodes safely before deployment.

## 4. Academic & Practical Objectives
This research is specifically aligned with the objectives of advanced Intelligent Transportation Systems (ITS) laboratories, particularly those targeting ODA/JICA technology transfer initiatives. By solving the core adaptability issue of AI in non-standard traffic management, this Proof of Concept (PoC) demonstrates a scalable, software-centric approach to urban congestion without requiring massive infrastructure overhauls.

## 5. Repository Structure
```text
├── data/                  # Custom video datasets and annotated trajectories
├── vision_module/         # CV scripts for object detection and MOT
├── chaos_engine/          # Mathematical implementation of the Chaos Index
├── rl_environment/        # Gym environment and SUMO configuration files
├── models/                # Trained RL policy weights
└── requirements.txt       # System dependencies