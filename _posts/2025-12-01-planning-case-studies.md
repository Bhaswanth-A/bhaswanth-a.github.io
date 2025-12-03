---
title: Case Studies of Planning and Decision-Making in Robotics
date: 2025-12-01 00:00:00 +0800
categories: [Blog, Robotics]
tags: [cmu, planning, manipulators, cbs]     # TAG names should always be lowercase
author: <author_id>
mermaid: true
pin: false
math: true
image: /assets/images/ldr.png
---

*In progress*

# Autonomous Driving

# Mobile Manipulators

# Legged Robots

# Coverage, Mapping, and Surveyal

## Frontier-based Planning

A frontier is any region that lies at the boundary between explored space and unexplored space.
Moving toward a frontier guarantees that the robot will gather new information.

This idea applies equally well to both mapping and coverage, even though the environments differ in how much initial knowledge the robot has.

In coverage, the map is already known, but the robot must visit every area to “cover” it with its sensors. In mapping, the environment is unknown beforehand, and the robot’s sensors reveal the map gradually. Yet in both scenarios, the frontier represents the next most informative or unobserved area for the robot to visit.

## Coverage

In the coverage scenario, the robot has access to a known map but must physically move to all parts of the environment to observe or “cover” it. Coverage tasks include vacuum cleaning, lawn mowing, painting a surface with a sprayer, or security patrolling.

Initially, simple patterns like lawn-mower (boustrophedon) patterns are commonly used. These are efficient in open areas, but they degrade quickly in environments with irregular shapes, tight corridors, or obstacles. The robot may skip regions, require manual stitching of patterns, or need a complex global plan.

Frontier-based planning offers a more adaptive and principled alternative. The robot maintains a record of which cells or regions it has already covered, and at every step, it identifies frontier cells—those cells that have not yet been visited but are adjacent to visited ones. The robot then chooses the nearest such frontier and computes a path to it. Once it reaches that frontier, new cells become observed, the set of frontiers changes, and the robot repeats the process. Eventually, all free cells will have been visited, guaranteeing complete coverage.

### Why it works?

Frontier-based coverage works well because it always moves the robot toward “new” portions of the environment. Since frontier cells are, by definition, the closest areas that remain unobserved, the robot makes steady progress toward the coverage goal without repeatedly visiting the same areas. This reduces unnecessary overlap and ensures that the robot moves in a purposeful manner.

Moreover, frontier-based coverage naturally adapts to obstacles and environmental geometry. Unlike lawn-mowing patterns, frontiers reorganize themselves around walls, corners, and narrow passages. This allows the robot to navigate complex spaces efficiently, making the method widely used in real-world applications such as vacuum robots or cleaning drones.

## Mapping

Mapping differs from coverage in one crucial way: the robot begins with no prior knowledge of the environment. As the robot moves and senses its surroundings, it gradually constructs an occupancy grid or map of free and occupied space. However, at any moment, most of the environment remains unknown.

Frontier-based planning for mapping uses the same principle — Always move to the closest frontier: the boundary between what has already been scanned and what is still unknown.

In mapping, every frontier visit provides maximum information gain, because the robot's sensors reveal new cells beyond the frontier. As the robot moves and senses, the set of frontiers changes. Some frontiers disappear as the robot explores them, and others appear as new unknown spaces are exposed.

The process continues until no frontiers remain, meaning the entire reachable environment has been mapped.

## Using Multi-Goal A* for Frontier Navigation

One subtle but important detail is how to choose the closest frontier. Since the robot may have many frontier cells at once, finding the nearest one requires solving a multi-goal shortest-path problem.

Rather than running A* separately for each frontier (which would be inefficient), we perform a graph transformation that allows A* to check all frontiers in a single search. This is based on the multi-goal A* technique.

The idea is:

- Treat all frontier cells as goal nodes in one A* search.
- Expand the search until the first frontier is reached.
- This frontier is guaranteed to be the closest in terms of path cost.

## Surveyal

Surveyal refers to a class of robotics tasks in which the robot must visit a set of predefined points of interest in the environment and perform sensing actions at each one, such as taking photos, scanning objects, or collecting data. Unlike simple navigation problems, surveyal does not ask the robot to reach a single goal state. Instead, it requires the robot to construct a least-cost route that visits all required waypoints, in any order, while respecting motion constraints and avoiding obstacles. This makes surveyal a combination of combinatorial reasoning (deciding the order in which to visit waypoints) and motion planning (finding feasible paths between them).

The core difficulty in surveyal lies in the fact that the robot cannot teleport between waypoints — it must travel along feasible trajectories. This means that the true “cost” of going from waypoint A to waypoint B is not simply Euclidean distance, but the cost of a real motion-planning solution that accounts for obstacles, robot dynamics, vehicle orientation, and constraints such as minimum turning radius. Therefore, surveyal cannot be solved by simply treating it as a Traveling Salesman Problem (TSP) over Euclidean distances. Instead, we must combine low-level continuous motion planning and high-level discrete graph search into one unified planning pipeline.

### Constructing the Surveyal Search Space

To solve the surveyal problem using graph search, we must construct a discrete search space that captures two key aspects of the robot’s progress:

1. Which waypoints have already been visited, and
2. Where the robot currently is (and possibly its orientation)

To encode this, we define the search state as:

$$
v = \{ \alpha, \Omega \}
$$

Here, $\alpha$ is a vector of $M$ bits, where each bit corresponds to one waypoint. A bit is set to `1` if the corresponding waypoint has been visited, and `0` otherwise. This bitmask representation allows the planner to concisely describe all possible “progress states” of the mission. For example, if the robot has three waypoints to visit, then $\alpha=$ `101` means the robot has visited waypoints `0` and `2` but not `1`.

$\Omega$ encodes the robot’s current position in the waypoint graph. For holonomic robots (which can move in any direction), $\Omega$ is simply the index of the current waypoint. For non-holonomic robots (cars, drones with yaw constraints, differential-drive robots), $\Omega$ may also include the robot’s orientation at that waypoint. Orientation is important because the robot’s ability to travel to the next waypoint may depend strongly on how it is oriented when it leaves its current waypoint.

This representation guarantees that each search state contains all relevant information that affects future motion, which means that the state space satisfies the Markov property. From any state `{α, Ω}`, the planner can determine exactly which next waypoints are allowed and what costs are involved in reaching them.

### Goal States in Surveyal

The goal of the surveyal problem is to visit all required waypoints. Therefore, the goal condition is simply:

$$
\alpha = [1, 1, \dots, 1]
$$

Once all bits in $\alpha$ are set to one, the robot has successfully surveyed every waypoint, and the search can terminate. Importantly, the robot’s orientation at the final waypoint does not affect goal satisfaction — only the fact that every waypoint has been visited.

### Role of Low-Level Motion Planning

Before graph search can be performed, the system must know the cost of traveling between each pair of waypoints. These costs cannot be assumed — they must be computed by invoking a continuous motion planner (e.g., A* on a grid, Hybrid A*, RRT*, lattice planning).

For every ordered pair of waypoints (A, B), a low-level planner is used to determine:

1. Whether a feasible path exists between A and B
2. The cost of this feasible path
3. The resulting orientation of the robot at waypoint B

Thus, edges in the high-level surveyal graph correspond to actual robot trajectories, not simple straight lines. This is what makes surveyal fundamentally richer than classical TSP.

### High-Level Graph Search: Where A* Comes In

Once feasible motion edges are computed, surveyal becomes a graph search problem over the state space `{α, Ω}`. Each node in this graph represents a partially completed mission. Each outgoing edge from that node corresponds to:

- Selecting an unvisited waypoint $i$
- Following a feasible low-level path from the current waypoint to $i$
- Updating the visitation bitmask $\alpha$
- Updating orientation (if applicable)

Graph search algorithms such as A* or Dijkstra explore this state space to find the minimum-cost path from:

$$
{ \alpha = 0\ldots0, \space \Omega = \text{start waypoint} }

\quad \rightarrow \quad

{ \alpha = 1\ldots1, \space \Omega = \text{any waypoint} }
$$

This is where graph search actually happens. The search determines the optimal sequence of waypoints to visit, accounting for true motion constraints and path costs.

This formulation elegantly integrates:

- Combinatorial reasoning about waypoint order
- Continuous geometric motion planning
- Robot kinematic/dynamic constraints
- Obstacle avoidance
- Optimality of the final surveyal tour

By encoding both “what has been visited” and “where the robot is now,” the search graph becomes fully Markovian. The planner can reason about the mission using a well-structured state space while relying on low-level planners to guarantee physical feasibility.

Because of this, surveyal planning becomes a clean example of how discrete search and continuous planning must work together to solve real robotic tasks.