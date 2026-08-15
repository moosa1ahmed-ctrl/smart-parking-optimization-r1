# Smart Parking: Optimization vs. Reinforcement Learning

CP468 (Artificial Intelligence, Wilfrid Laurier University) term project. A driver navigating toward a destination must pick a parking lot from several nearby options, but travel time, lot availability, and cost all shift while they're still en route. The project models this as a graph search / sequential decision problem and compares two solution strategies for choosing (and updating) that decision on the fly.

## Approach

| | Method | Idea |
|---|---|---|
| `notebooks/Optimization.ipynb` | Shortest-path optimization (Dijkstra-based) | Recomputes the best lot as a graph search problem whenever conditions change |
| `notebooks/Reinforcement_Learning.ipynb` | Q-learning | Learns a policy through trial and error that adapts to changing traffic, cost, and availability, subject to a maximum walking-distance constraint |

Both approaches are evaluated across multiple test scenarios; results and comparison metrics are in `results/Results.xlsx`.

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
