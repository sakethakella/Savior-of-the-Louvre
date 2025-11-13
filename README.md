Savior of the Louvre — Simulation of Thieves (Car A) vs Police (Car B)

## Overview

This repository contains a simulation implemented in a Jupyter notebook (`23EC10006.ipynb`) that models a pursuit scenario on a directed weighted graph. "Car A" (the thieves) attempts to reach an exit node while "Car B" (the police) tries to catch them. The graph includes dynamic events (traffic jams, blockages, one-way changes) that appear during the simulation and temporarily modify edge weights or availability.

## Files of interest

- `23EC10006.ipynb` — Main notebook containing code to load the graph, run Dijkstra pathfinding, simulate dynamic graph events, move the two agents, and visualize the simulation steps.
- `graph_with_metadata.json` — Graph data file (adjacency, node positions, metadata like exit nodes and node count). The loader reads this file to construct the directed graph and metadata.
- `simulation_log.json` — (If present) can store a history of simulation steps/events.

## Key design choices and conventions

- Adjacency matrix representation: The notebook converts between a dict-of-lists adjacency representation and a NumPy adjacency matrix for convenience with dynamic events and visualization.
- Blocked edges: Blocked/unavailable edges are represented as 0.0 in the adjacency matrix and are omitted when converting to the adjacency dict. Dijkstra ignores edges with weight <= 0 or infinite.
- Dijkstra: The notebook includes a Dijkstra implementation that expects an adjacency dict mapping node -> list of (neighbor, weight). If the target is unreachable, the function returns an empty path list.
- Dynamic events: A NumPy-based `dynamic_graph_change(original_adjacency, current_adjacency, active_events, p=0.3)` function simulates events with durations and re-applies active events each step. Supported events: `traffic` (multiplies an edge weight), `blockage` (sets edge weight to 0.0), and `one-way` (removes the reverse edge by setting it to 0.0).

## Dependencies

Install the following Python packages (recommended in a virtual environment):

- Python 3.8+
- networkx
- numpy
- matplotlib

You can install them with pip (PowerShell):

```powershell
python -m venv venv
venv\Scripts\Activate.ps1
pip install networkx numpy matplotlib
```

## How to run

Open the notebook `23EC10006.ipynb` in Jupyter (e.g., VS Code or Jupyter Lab) and run cells in order.

High-level steps performed by the notebook:

1. Load graph and metadata from `graph_with_metadata.json`.
2. Convert adjacency dict into a NumPy adjacency matrix.
3. Initialize Car A (thieves) and Car B (police) states using Dijkstra paths.
4. In a loop, apply `dynamic_graph_change` to update edge weights/availability, convert matrix → adjacency dict, move Car A and Car B using movement functions (`move_carA`, `move_carB`), and record history.
5. Stop when Car A reaches an exit node, Car B catches Car A, or after a max number of steps.
6. Visualize each step with `draw_simulation_step` which colors edges based on active events.

## Notes and troubleshooting

- Node indices: The notebook assumes nodes are integer IDs 0..n-1. Ensure your `graph_with_metadata.json` follows this convention.
- Exit nodes: The loader reads exit node IDs from metadata. If no exit node exists, the simulation will not proceed correctly.
- Blocked edges: Because blocked edges are represented as 0 in the adjacency matrix, conversions to adjacency dicts ignore them. If you prefer `np.inf`, you must update `convert_adj_matrix_to_dict` and the rest of the code accordingly.
- If you encounter index errors:
  - Check `metadata['n_nodes']` matches the actual number of nodes.
  - Check start node indices (e.g., 0 and 49 used by the notebook) are valid for your graph size.

## Suggested next steps / improvements

- Add robust unit tests for `dijkstra`, conversion utilities, and `dynamic_graph_change` using a small synthetic graph.
- Make start nodes configurable via notebook inputs rather than hard-coded indices.
- Add saving/loading of `history` to `simulation_log.json` for replay and debugging.
- Improve visualization to show agent positions along edges (progress), not only node positions.

## Contact

If you want, I can (pick one):

- run the notebook and fix runtime errors,
- add a small smoke test that verifies the core functions,
- or update the notebook to make node starts configurable and save logs.

README generated from `23EC10006.ipynb` by an assistant.
