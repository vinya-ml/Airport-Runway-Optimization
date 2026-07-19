# Optimization of Airport Runway Operations: Minimizing Taxiing Time, Reducing Departure Delays, and Optimal Runway Assignment

This repository contains the MATLAB implementation of a unified, three-stage deterministic optimization framework designed to optimize airport ground operations by sequentially minimizing aircraft taxiing time, total departure delays, and runway offset errors.

---

## 1. Description
Airport ground operations (taxiing, runway slot assignment, and departure scheduling) are critical bottlenecks in global aviation. While stochastic metaheuristics like Genetic Algorithms (GA) and Particle Swarm Optimization (PSO) are effective, they are computationally heavy and produce non-reproducible results. 

This project provides a lightweight, deterministic alternative:
* **Stage 1 (Taxiing Time):** Minimizes average taxiing time using the **Hooke-Jeeves Pattern Search** method.
* **Stage 2 (Departure Delay):** Minimizes total squared departure delay using the **Steepest Descent (Gradient Descent)** method.
* **Stage 3 (Runway Offset):** Minimizes runway offset error using **Fibonacci Search**.

The framework is benchmarked against the standard **First-Come-First-Served (FCFS)** industry heuristic as well as **GA** and **PSO** metaheuristics.

---

## 2. Dataset Information (Self-Curated Dataset)
The experiments are conducted on a self-curated synthetic dataset (`AirportData2.xlsx`) designed to replicate realistic airport operations. The workbook contains three sheets:

1. **`Flights` Sheet:**
   * `FlightID`: Unique identifier for each of the 100 flights.
   * `ArrivalTime`: Scheduled arrival time in HHMM format.
   * `DepartureTime`: Scheduled departure time in HHMM format.
   * `GateNode`: The starting gate node (1–20) from which the aircraft taxies.

2. **`Taxiway` Sheet:**
   * A $21 \times 21$ adjacency matrix representing the taxiway network topology.
   * Row/column indices represent taxiway junctions/gates. Node 21 represents the runway threshold.
   * Non-zero cells represent direct transit time (in minutes) between nodes. Zero entries represent no direct connection.

3. **`Runways` Sheet:**
   * `SlotTime`: Available departure slot times in HHMM format. These are expanded by the script into a 15-minute slot grid covering the full operation window.

### Assembly and Source:
This dataset is synthetically generated using structural parameters from medium-sized airports (100 flights, 21-node taxiway graph, 85 runway slots). The data generation constraints (e.g., taxi times between 5 and 25 minutes, departure delays between 2 and 15 minutes) are derived from international civil aviation safety standards.

---

## 3. Code Information
The repository consists of a single primary MATLAB script containing the entire optimization implementation:
* The script loads the dataset from the relative filepath: `"AirportData2.xlsx"`.
* It pre-computes shortest-path taxi distances using Dijkstra's algorithm.
* It runs the FCFS baseline, followed by the proposed three-stage deterministic optimization framework over 10 independent runs.
* It evaluates the comparator GA and PSO algorithms over 10 independent runs.
* It prints a structured comparative performance table directly to the MATLAB command window.
* It generates and displays 9 graphical figures illustrating taxi times, schedule alignment, and optimization convergence curves.

---

## 4. Requirements
* **MATLAB Version:** Standard MATLAB installation (the script utilizes built-in graph functions like `graph` and `shortestpath`).
* **Input File:** The Excel spreadsheet `AirportData2.xlsx` must be present in the same folder as the script.

---

## 5. Usage Instructions
1. Place the dataset spreadsheet `AirportData2.xlsx` and the script file in the same directory.
2. Launch MATLAB and set the folder containing these files as the Current Folder.
3. Open the script file in the MATLAB Editor and press **Run**, or run it from the Command Window.
4. **Console Output:** The MATLAB Command Window will print the loading progress, FCFS baseline statistics, optimization run progress, and a final summary table comparing all four scheduling methods.
5. **Figure Output:** MATLAB will generate and display 9 active plotting figures:
   * **Figure 1:** Optimized Taxiing Time per Flight – Hooke-Jeeves
   * **Figure 2:** Fibonacci Search Convergence
   * **Figure 3:** Optimized Runway Schedule – Fibonacci Offset Correction
   * **Figure 4:** GA Convergence – Best Fitness over Generations
   * **Figure 5:** PSO Convergence – Global Best over Iterations
   * **Figure 6:** Convergence Comparison – GA vs. PSO
   * **Figure 7:** Comparative Performance – All Methods (Grouped Bar Chart)
   * **Figure 8:** Computational Cost Comparison (CPU Time)
   * **Figure 9:** Per-Flight Delay Distribution – Deterministic vs. FCFS

---

## 6. Methodology Summary
The unified optimization pipeline runs sequentially in three distinct stages:
* **Stage 1: Taxiing Time Minimization (Hooke-Jeeves Pattern Search):** Initiates taxi times using Dijkstra's shortest-paths. It uses pattern moves (exploratory steps of size $\delta$ halved recursively) to optimize gate-to-runway routes, incorporating random Gaussian perturbations to simulate real-world taxiway noise.
* **Stage 2: Departure Delay Minimization (Steepest Descent Method):** Assigns aircraft to available runway slots using a greedy nearest-neighbor approach, then optimizes the departure times using gradient descent with a learning rate of $\alpha = 0.02$ to minimize total quadratic delay.
* **Stage 3: Runway Offset Minimization (Fibonacci Search):** Minimizes the offset error (the global timeline misalignment) by using Fibonacci line search to find the optimal uniform offset time $\Delta t$ over unimodal absolute deviations.

---

## 7. Citations & References
If you use this code or dataset in your research, please cite the following paper:
> **Aluvala Sai Vinya, Sree Sruthi Alur, and Mamatha T. M.** (2026). *Optimization of airport runway operations: minimizing taxiing time, reducing departure delays, and optimal runway assignment*. PeerJ Computer Science (Manuscript under review).

### Literature References:
* **[1]** C. Ding, J. Bi, and Y. Wang, "A hybrid genetic algorithm based on imitation learning for the airport gate assignment problem," *Entropy*, vol. 25, no. 4, Art. no. 565, 2023.
* **[2]** F. Guédán-Pecker and C. Ramírez-Atencia, "Airport take-off and landing optimization through genetic algorithms," *Expert Systems*, vol. 41, no. 8, Art. no. e13565, 2024.
* **[3]** H. Zhou and X. Jiang, "Multirunway optimization schedule of airport based on improved genetic algorithm by dynamic time window," *Mathematical Problems in Engineering*, vol. 2015, Art. no. 854372, 2015.
* **[4]** M. Zhang, Q. Huang, S. Liu, and H. Li, "Multi-objective optimization of aircraft taxiing on the airport surface with consideration to taxiing conflicts and the airport environment," *Sustainability*, vol. 11, no. 23, Art. no. 6728, 2019.
* **[5]** W. Deng, H. Zhao, L. Zou, G. Li, X. Yang, and D. Wu, "Study on an improved adaptive PSO algorithm for solving multi-objective gate assignment," *Applied Soft Computing*, vol. 59, pp. 288–302, 2017.
* **[6]** C. Huang, "Hybrid particle swarm optimization and Q-learning for airport parking space allocation and scheduling," *Informatica*, vol. 49, no. 31, pp. 71–86, 2025.
* **[7]** M. Battipede, G. Sirigu, J.-P. Clarke, and P. Gili, "Hybrid particle swarm optimization with parameter fixing: application to automatic taxi management," *Journal of Air Transportation*, vol. 28, no. 2, pp. 36–48, 2020.
* **[8]** J. Yin, M. Zhang, Y. Ma, W. Wu, H. Li, and P. Chen, "Prediction and analysis of airport surface taxi time: classification, features, and methodology," *Applied Sciences*, vol. 14, no. 3, Art. no. 1306, 2024.
* **[9]** G. Lian, Y. Zhang, J. Desai, Z. Xing, and X. Luo, "Predicting taxi-out time at congested airports with optimization-based support vector regression methods," *Mathematical Problems in Engineering*, vol. 2018, Art. no. 7509508, 2018.
* **[10]** R. Hooke and T. A. Jeeves, "Direct search solution of numerical and statistical problems," *Journal of the ACM*, vol. 8, no. 2, pp. 212–229, 1961.
