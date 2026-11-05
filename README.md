# Generalized Klotski

A self-contained [Jupyter notebook](klotski.ipynb) solving six sliding-block
puzzles with **BFS** and **A***, using both cell and slide moves.

## Problem

Move the rectangular target **A** to the bottom exit in as few actions as possible,
or prove that no solution exists. Pieces cannot rotate, overlap, or leave the
board. The target finishes inside the board, covering the exit columns.

| Rule | One action, costing 1 |
| --- | --- |
| `cell` | Translate one piece by exactly one cell. |
| `slide` | Translate one piece any positive distance along a clear straight path. |

Turning requires another action. Both rules have the same reachable states, but
can have different optimal costs, so they are solved separately.

## Approach

- **State representation:** group non-target pieces by oriented shape and sort
  their anchors, merging equivalent permutations while retaining labels for replay.
- **BFS:** explore in increasing path length; unit action costs make the first
  reached goal optimal.
- **A*:** prioritize path cost plus an admissible, consistent lower bound: target
  movement plus the number of non-target pieces overlapping its final footprint.
  Target movement is Manhattan distance for `cell`, or the number of mismatched
  coordinates for `slide`.
- **Validation:** compare BFS/A* costs and replay every returned path to check
  legality and goal attainment. Complete exhaustion establishes unsolvability.

All six boards and the complete implementation are embedded in the notebook.
No local solver package or external puzzle file is required.

## Results

Both algorithms agree on the optimal number of actions:

| Problem | Slide optimum | Cell optimum |
| --- | ---: | ---: |
| p01 | 7 | 8 |
| p02 | 13 | 18 |
| p03 | 36 | 45 |
| p04 | 90 | 116 |
| p05 | 150 | 179 |
| p06 | No solution | No solution |

For **p06**, all four searches exhaust **833 canonical states** without finding
a goal. The notebook includes saved outputs, complete numbered solution paths,
and per-run search measurements. Single-run timings are not a speed ranking.

## Run

Requires **Python 3.10+**. From the project root:

```sh
python3 -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
jupyter lab klotski.ipynb
```

Select the environment's Python kernel and **Restart Kernel and Run All Cells**.
Beyond Jupyter/IPython, the notebook uses only the Python standard library.

## Branches

- **`main`**: the standalone notebook, this README, and minimal setup files.
- **`dev`**: the solver package, tests, web UI, experiments, and original
  documentation, preserved for later work.

Use `git switch dev` or `git switch main` to change versions; commit or stash local
edits first. Generated outputs, virtual environments, and caches stay untracked.
