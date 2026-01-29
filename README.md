# Bike-Sharing System Discrete Event Simulation

## 📌 Project Overview
This project implements a **Discrete Event Simulation (DES)** from scratch to model the operational dynamics of a large-scale bike-sharing network (similar to Citi Bike or Divvy). Using historical trip data, the simulation tracks the movement of 3,500 riders across 81 distinct stations to evaluate system performance metrics like wait times, bike availability, and station capacity bottlenecks.

The goal was to determine the optimal distribution of bikes required to maintain a **zero-wait-time service level** under stochastic demand.

## 🛠 Tech Stack
* **Language:** Python
* **Libraries:** Pandas, NumPy, SciPy (for confidence intervals), Heapq (for event queue)
* **Concepts:** Discrete Event Simulation, Monte Carlo Methods, Queueing Theory, Stochastic Modeling

## 🔬 Simulation Architecture

### 1. The Engine
Unlike standard data science projects that use pre-built models, this simulation relies on a custom-built **Future Event List (FEL)** pattern:
* **Event Loop:** Uses a min-heap priority queue to process events strictly in chronological order.
* **State Management:** Tracks global system state including `bikes_available`, `waiting_for_bike` (customer queue), and `waiting_to_return` (docking queue) for every station.

### 2. Event Logic
The simulation handles two primary events:
* **Arrival Event (`arrive`):**
    * Rider arrives at a station based on an Exponential distribution ($\lambda = 2.38$).
    * **Logic:** If a bike is available, it is rented immediately. If not, the rider enters a FIFO queue.
* **Return Event (`return_bike`):**
    * Rider travels to a destination based on transition probabilities derived from historical data.
    * **Logic:** If the destination station is full (at capacity), the rider enters a "waiting to return" queue. Upon a successful return, the system checks if any riders are waiting for a bike at that station and immediately services the first in line.

## 📊 Key Findings

### 1. Baseline Performance
Running the simulation with **10 bikes per station** (810 total bikes):
* **Service Level:** ~95.7% of riders successfully rented a bike.
* **Average Wait Time:** ~8.8 minutes.
* **Bottlenecks:** Central stations frequently hit 0 capacity, creating large wait queues.

### 2. Capacity Optimization
We ran a sensitivity analysis to find the "Zero Wait" threshold. By strictly relaxing the station capacity limits and increasing initial fleet size:
* **25 Bikes/Station:** Wait time dropped to ~0.43 minutes.
* **26 Bikes/Station:** Wait time dropped to **0.0 minutes**.

**Conclusion:** To ensure 100% service availability without rebalancing trucks, the system requires a minimum initial distribution of 26 bikes per station.

## 📂 File Structure
* `cx4230-mp3.ipynb`: Main simulation code, including the DES engine, event logic, and statistical analysis.
* `trip_stats.csv`: Historical data used to calculate travel times and destination probabilities.
* `start_station_probs.csv`: Probability distribution defining where riders originate in the network.

## 🚀 How to Run
The simulation is self-contained in the Jupyter Notebook.
1. Ensure the CSV data files are in the working directory (or mounted via Google Drive).
2. Run the cells sequentially to:
    * Load and process station probability data.
    * Initialize the `FutureEventList`.
    * Execute the `simulate()` loop.
    * View the statistical output (Confidence Intervals).
