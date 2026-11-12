# Smart Campus Navigation and Shortest Path Finder

A runnable Python project based on the DAA report. It models campus locations as vertices and walking paths as weighted edges, then uses Dijkstra's greedy algorithm to find the minimum-distance route. It also selects the nearest reachable facility from a candidate list.

The included map reproduces the report's sample campus and expected 620 m route from Main Gate to Sports Ground. All included distances are hypothetical demonstration values.

## Requirements

- Python 3.10 or newer
- No third-party packages are required

## Run

Open a terminal in this project folder.

On Windows:

    py run.py

On other systems:

    python3 run.py

With no command, the program opens an interactive menu. You can also run individual commands:

    py run.py locations
    py run.py route "Main Gate" "Sports Ground"
    py run.py nearest "Main Gate" --candidates "Central Library" "Cafeteria" "Sports Ground"

The route command prints:

    Shortest route:
    Main Gate -> Central Library -> Cafeteria -> Auditorium -> Sports Ground
    Total distance: 620 m

The nearest-facility command recommends Central Library at 200 m for the sample candidate list.

The module form works as well:

    py -m campus_navigation route "Main Gate" "Sports Ground"

Use --help to see command options:

    py run.py --help

## Project structure

- campus_navigation/graph.py contains graph validation, Dijkstra's algorithm, route reconstruction, and nearest-facility selection.
- campus_navigation/cli.py contains the interactive menu and command-line interface.
- campus_navigation/data/campus.json contains the sample graph data.
- run.py is a simple entry point for direct execution.
- pyproject.toml contains optional packaging metadata.

## Use a different campus map

Copy campus_navigation/data/campus.json and edit its locations and paths. Each path has a non-negative distance in metres. The bundled map is undirected, so each path can be travelled both ways. To represent one-way paths, set directed to true; then each listed path is traversable only from its from location to its to location.

A custom JSON file can be loaded with the global --data option before a command:

    py run.py --data path/to/campus.json route "Main Gate" "Sports Ground"

Every location must have a unique name. Path endpoints must exist in the locations list. Negative, infinite, and invalid edge weights are rejected.

## Algorithm

The application stores the graph as an adjacency list and uses Python's heapq minimum-priority queue. Dijkstra's algorithm requires non-negative edge weights. Its binary-heap implementation has time complexity O((V + E) log V) and space complexity O(V + E), where V is the number of locations and E is the number of paths.

## Limitations

The sample data is not a surveyed campus map. The program does not include GPS, indoor positioning, live closures, accessibility routing, or a graphical map. Replace the illustrative distances with verified path data before using it for real navigation.
