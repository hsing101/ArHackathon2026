# Warehouse Robot Routing 🏆 1st place, Amazon Robotics Hackathon 2026

My winning solution for routing **drive units**, the robots that carry inventory pods across an Amazon fulfillment-center floor, from storage to pick stations as fast as possible.

The floor is a weighted graph with narrow aisles (edge capacity), limited station docks (node capacity) and robots that can carry more than one pod. Each pod's score decays the longer it takes to deliver, so the goal is to deliver every pod, quickly, without traffic jams.

## Results

| Metric | Result |
|---|---|
| Pods delivered | **100%** |
| End-to-end completion time | **79% faster** than the baseline driver |
| Score | **44% higher** than the baseline |
| Routing decision time | **< 1 ms** per call (limit: 1 s) |

*Baseline = the provided example driver, which greedily takes the cheapest nearby aisle.*

## How it works

**1. Congestion-aware Dijkstra.** Shortest paths are computed with Dijkstra's algorithm on edge costs that account for current aisle traffic, so robots route around busy, capacity-limited corridors instead of queuing in them.

**2. Global task assignment.** Although the engine asks about one robot at a time, my router plans globally: it ranks robot-to-pod batches by **expected reward per unit of travel time** (matching the hackathon's exponentially decaying score), so each robot takes the pickup that adds the most score soonest.

**3. Cached shortest paths.** Paths are cached and reused between calls, which keeps every routing decision under 1 ms.

## Repository layout

- `ar_hackathon/api/routing.py` – **my solution** (`drive_unit_next_move`)
- Everything else (simulation engine, models, visualizer, test cases) is the starter framework provided by Amazon Robotics for the hackathon.

## Run it

```bash
python3 -m pip install -e .
python3 scripts/run_game.py test_cases/level3/test_case_1.json      # run my router
python3 scripts/visualize.py test_cases/level3/test_case_1.json     # animate it in the browser
```

Python standard library only, per hackathon rules.
