# Uniform-Cost Search (Dijkstra) and UGV Path Planning

## Course: Essentials of Artificial Intelligence

## Author
- **Name**: GARIMELLA ANANDITA DAKSHAYANI 
- **Roll No**: SE26MAID034

---

## Objective
1. **Road-distance routing between Indian cities** using Dijkstra's algorithm (a.k.a. uniform-cost search).
2. **Navigation of an Unmanned Ground Vehicle (UGV)** across a 70 x 70 km battlefield grid with obstacles, first when obstacles are static and known a priori, then when they are dynamic and unknown.

---

## Part A: Dijkstra / Uniform-Cost Search on Indian Cities

### Data
- **Source:** Approximate road distances (km) between major Indian cities, taken from open sources
- **Cities used:** Delhi, Mumbai, Bengaluru, Hyderabad, Chennai, Kolkata.
- Stored as a symmetric adjacency matrix `Cities_Distances`, where `0` means "same city / no direct edge".

### Approach / Logic
1. **Graph representation:** cities are nodes, road distances are edge weights.
2. **Priority queue:** a min-heap (`heapq`) keyed by the cheapest known distance from the source.
3. **Relaxation:** pop the closest unvisited node `u`; for each neighbour `v`, if `dist[u] + w(u, v) < dist[v]`, update `dist[v]` and record `prev[v] = u`.

### Complexity
Using an adjacency matrix with a binary heap this runs in O(V^2 log V). With an adjacency list it would be O((V + E) log V).


### Limitations
- Only 6 metros are included, not every city in India. The graph can be extended by adding more rows/columns (or loading a CSV of city pairs).
- Because every pair of cities is directly connected with its **road distance**, most shortest paths are the direct edge. A sparser graph that only contains real highway links between neighbouring cities would give more meaningful multi-hop routes.

---

## Part B: UGV Navigation on a Grid with Static Obstacles

### Problem
A UGV must travel from a user-specified **start** cell to a user-specified **goal** cell on a 70 x 70 km map (1 cell = 1 km). Obstacles are known in advance. The UGV must avoid them and reach the goal by the shortest route.

### Battlefield generation
`battle_field(size, density_lvl, seed, start, goal)` builds a `size x size` NumPy grid (`0` = free, `1` = obstacle) using random placement:

| Density level | Obstacle probability |
|---|---|
| `low` | 10% |
| `med` | 25% |
| `high` | 40% |

The start and goal cells are always forced to be free, and a `seed` can be passed for reproducible maps.

### Algorithm: A* Search
Dijkstra explores uniformly in all directions. A* adds a heuristic so the search is drawn toward the goal:

- `g(n)`: actual cost from start to `n` (each 4-connected move costs 1 km) -- cost 
- `h(n)`: **Manhattan distance** to the goal, `|r1 - r2| + |c1 - c2|` -- heuristic
- `f(n) = g(n) + h(n)`

<!-- Manhattan distance is **admissible** (never overestimates) and **consistent** on a 4-connected unit-cost grid, so A* returns an optimal path and never needs to re-expand a closed node. If no path exists, an empty list is returned. --->



### Measures of Effectiveness (MoE)
| Measure | Meaning |
|---|---|
| **Path found** | Whether a feasible route exists |
| **Path length (km)** | Total distance travelled |
| **Optimality ratio** | Path length divided by the Manhattan lower bound. A value of 1.0 is a perfectly straight route; higher means more detour around obstacles |
| **Nodes expanded** | Size of the explored set, which measures search effort |
| **Run time (s)** | Time for running |

These are reported for each density level (low / med / high) so the effect of clutter on route quality and search cost can be compared.


## Part C: Dynamic and Unknown Obstacles (Design Approach)

> Note: Parts A and## Sensing model
The UGV has a limited **sensor range** (Manhattan distance, e.g. 3 km) around its current position. Obstacles outside this range are invisible to it; only obstacles that come within range are added to its known map. This reflects a realistic onboard sensor rather than an all-seeing one.

### Algorithm: D* Lite (incremental replanning)
A full re-plan with A* every time a new obstacle is sensed is correct but wasteful — it discards all prior search effort and recomputes the entire path from scratch on every update. **D\* Lite** (Koenig & Likhachev, 2002) avoids this by searching backward from the goal and maintaining two cost estimates per cell:

- `g(s)`: current best known cost from `s` to the goal
- `rhs(s)`: one-step lookahead cost, `min` over neighbors `s'` of `cost(s, s') + g(s')`

A cell is *consistent* when `g(s) == rhs(s)`. Only *inconsistent* cells are kept in the priority queue, so when a new obstacle is sensed, only the cells actually affected by that change are reprocessed — not the whole grid. This makes D\* Lite well suited to an agent that repeatedly senses small, local updates to an otherwise mostly-known map, which matches the UGV's sensing model above more closely than starting from zero knowledge each time.

---

## Requirements
- Python 3.x
- `numpy`
- Standard library: `heapq`, `time`

```python
import heapq
import time
import numpy as np
```


## References
- Russell, S. and Norvig, P., *Artificial Intelligence: A Modern Approach* (uniform-cost search, A*)
