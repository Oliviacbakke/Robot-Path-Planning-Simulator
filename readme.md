# Robot Path-Planning Simulator

An interactive grid simulator where a robot finds a route across a map with walls and mud. It implements three search algorithms and lets you compare them on the same map.

**[Live demo](https://oliviacbakke.github.io/Robot-Path-Planning-Simulator/)**

## Features

- Draw maps with walls, mud (5x the cost to cross), a start, and a destination
- Three algorithms: BFS, Dijkstra, and A*
- Optional diagonal movement (8 directions) with a no-corner-cutting rule
- Animated search that shows which squares each algorithm explores before the robot drives the path
- **Compare all** runs every algorithm on your map and reports squares explored, path length, path cost, and runtime

## How it works

- **DFS** searches far and wide for any path, but almost never is the shortest path.
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
| Open field | DFS | 23 | 22 |
| Open field | BFS | 210 | 22 |
| Open field | Dijkstra | 201 | 22 |
| Open field | A* | 23 | 22 |
| Mud band with a gap | DFS | 23 | 46 |
| Mud band with a gap | BFS | 210 | 46 |
| Mud band with a gap | Dijkstra | 276 | 44 |
| Mud band with a gap | A* | 198 | 44 |
| Maze-like walls | DFS | 66 | 64 |
| Maze-like walls | BFS | 182 | 56 |
| Maze-like walls | Dijkstra | 182 | 56 |
| Maze-like walls | A* | 137 | 56 |

**Some notable findings:**

1. Depth-First Search always resulted in very few cells being explored because the destination was always put in the same row as the start. But often it missed the best route because of how quickly it found the destination.  

2. The algorithms found that once the mud band became 6 cells wide, it is more effective to go around the mud than through it. DFS never checked that route, as it does not care about the weight of the path.

3. DFS was suprisingly effective in the maze, which again is due to the placement of the start and destination cells. 

## Run locally

Open `index.html` in a browser. There are no dependencies and no build step.

## Next steps

- A robot with a limited sensor radius that builds its own map and replans as it discovers obstacles
- Replanning that reuses earlier work (D* Lite)
- Heading and turning costs instead of square-to-square moves

Built with plain HTML, CSS, and JavaScript.