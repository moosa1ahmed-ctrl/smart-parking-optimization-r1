# Smart Parking: Optimization vs. Reinforcement Learning

CP468 (Artificial Intelligence, Wilfrid Laurier University) term project. A driver navigating toward a destination must pick a parking lot from several nearby options, but travel time, lot availability, and cost all shift while they're still en route. The project models this as a graph search / sequential decision problem and compares two solution strategies for choosing (and updating) that decision on the fly.

## Approach

| | Method | Idea |
|---|---|---|
| `notebooks/Optimization.ipynb` | Dijkstra + hill climbing | Dijkstra computes shortest travel time from the start node to every candidate lot; a hill-climbing objective function then scores each *feasible* lot on cost and walking distance to pick the best one |
| `notebooks/Reinforcement_Learning.ipynb` | Q-learning (model-free RL) | An agent learns a routing + parking policy through repeated trial and error, without knowing the graph's costs in advance |

Both approaches are evaluated across multiple test scenarios; results and comparison metrics are in `results/Results.xlsx`.

## How it works

**Problem setup:** a small directed road graph (`S → A/B → C → P1/P2 → D`) where edge weights are travel times, and two candidate parking lots (`P1`, `P2`) each have a cost, a walking distance from the lot to the destination, an availability flag, and a maximum-walk constraint.

**Objective (shared by both methods):**
```
minimize:  travel_time + α · parking_cost + β · walking_distance
subject to: lot must be available, and walking_distance ≤ max_walk
```

**Optimization approach (`Optimization.ipynb`)**
1. Run Dijkstra from the start node `S` to get shortest travel time to every node.
2. For each parking lot, check feasibility (`available == True` and `walk_dist ≤ max_walk`).
3. Hill-climb over the feasible lots using the weighted objective above (`α = 1.0` for cost, `β = 0.01` for walking distance) to pick the single best lot.
4. Deterministic and instant — recomputed from scratch whenever conditions (cost, availability, distance) change.

**Reinforcement learning approach (`Reinforcement_Learning.ipynb`)**
Formulated as an MDP:
- **States:** current node — `{S, A, B, C, P1, P2, TERMINAL}`
- **Actions:** move to an adjacent node, or `park` (only valid at P1/P2)
- **Reward:** `-travel_time` per move; on `park`, `-(cost + β · walk_dist)` if the lot is available and within `max_walk`, otherwise a large penalty (`-1000`) for an invalid choice
- **Policy:** ε-greedy during training (explore vs. exploit), greedy at evaluation

| Hyperparameter | Value |
|---|---|
| Learning rate (α) | 0.1 |
| Discount factor (γ) | 0.9 |
| Exploration rate (ε) | 0.2 |
| Episodes | 5,000 |
| Walking-distance weight (β) | 0.01 |

**Test scenarios (run in both notebooks):**
1. **Baseline** — both lots feasible, agent should pick the better trade-off
2. **P2 too far** — P2's walking distance exceeds `max_walk`, forcing P1
3. **P1 unavailable** — P1 is marked unavailable, forcing P2

The RL agent has to *learn* the same constraint-driven behaviour the optimization approach computes directly — the comparison in `results/Results.xlsx` looks at whether it converges to the same optimal choices, and how it handles conditions changing after training.

## Repo structure
```
├── notebooks/
│   ├── Optimization.ipynb
│   └── Reinforcement_Learning.ipynb
├── results/
│   └── Results.xlsx
├── docs/
│   └── CP468-TP-35-writeup.pdf   # full IEEE-format writeup
├── requirements.txt
└── README.md
```

## Running it
```bash
pip install -r requirements.txt
jupyter notebook notebooks/
```
Each notebook runs independently — no external data files needed, the road network and parking-lot scenarios are defined in-notebook.

## Full writeup
See [`docs/CP468-TP-35-writeup.pdf`](docs/CP468-TP-35-writeup.pdf) for the complete paper: literature review, system model, methodology, and full results analysis.

## Team
Group project for CP468 (Artificial Intelligence), Wilfrid Laurier University.

- Moosa Ahmed
- Lucas Matheson
- Bhavnoor Dhillon
- Reilly MacDonald
