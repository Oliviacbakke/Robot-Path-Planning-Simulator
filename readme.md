# Robot Path-Planning Simulator

An interactive grid simulator where a robot finds a route across a map with walls and mud. It implements three search algorithms and lets you compare them on the same map.

**[Live demo](https://oliviacbakke.github.io/robot-path-planning-simulator/)**

![Demo of the simulator](demo.gif)

## Features

- Draw maps with walls, mud (5x the cost to cross), a start, and a destination
- Three algorithms: BFS, Dijkstra, and A*
- Optional diagonal movement (8 directions) with a no-corner-cutting rule
- Animated search that shows which squares each algorithm explores before the robot drives the path
- **Compare all** runs every algorithm on your map and reports squares explored, path length, path cost, and runtime

## How it works

- **BFS** finds the path with the fewest steps, but it ignores terrain, so it will happily cross mud.
- **Dijkstra** always expands the cheapest unexplored square, so it finds the lowest-cost path.
- **A\*** is Dijkstra plus a heuristic estimate of the remaining cost (Manhattan distance on 4-way grids, octile distance with diagonals). It finds the same optimal path while exploring fewer squares.
- Diagonal steps cost √2 times the terrain cost, and a diagonal move is blocked if either neighboring square is a wall, so the robot can't squeeze between touching corners.
- Terrain costs stay at 1 or higher, which keeps the heuristics admissible so A* remains optimal.
- The algorithms are pure functions over a grid model, separate from the rendering code. The animation just replays their results.

## Results

Benchmarks from the **Compare all** button on three maps:

| Map | Algorithm | Explored | Cost |
|---|---|---|---|
| Open field | Dijkstra | [N] | [N] |
| Open field | A* | [N] | [N] |
| Mud band with a gap | BFS | [N] | [N] |
| Mud band with a gap | Dijkstra | [N] | [N] |
| Mud band with a gap | A* | [N] | [N] |
| Maze-like walls | Dijkstra | [N] | [N] |
| Maze-like walls | A* | [N] | [N] |

[Two or three sentences on what you found. For example: how much less A* explored than Dijkstra on the open field, whether its advantage shrank in the maze, and how BFS's path cost compared once mud was added.]

## Run locally

Open `index.html` in a browser. There are no dependencies and no build step.

## Next steps

- A robot with a limited sensor radius that builds its own map and replans as it discovers obstacles
- Replanning that reuses earlier work (D* Lite)
- Heading and turning costs instead of square-to-square moves

Built with plain HTML, CSS, and JavaScript.