---
title: Swarm Robot Coordination
date: 2023-05-22 00:00:00 +0800
categories: [Projects, Robotics]
tags: [swarm, python, simulation]     # TAG names should always be lowercase
author: <author_id>
subsection: "Academic Projects at BITS Pilani"
mermaid: true
pin: false
math: true
image: /assets/images/Thumbnail/swarm_rsz.jpeg
---

# Abstract

Swarm robotics is an emerging field that enables a group of robots to collaborate and achieve complex tasks that are difficult for a single robot to accomplish. One of the key challenges in swarm robotics is the coordination of multiple robots' movements towards a common goal. This report provides a comprehensive overview of swarm robot coordination, with a specific focus on four common tasks: aggregation, dispersion, line formation, and shape formation.

We discuss the different algorithms used for swarm robot coordination, including Potential Fields Algorithm, Centroid based Algorithm, and Line Marching Algorithm, and their respective advantages for each task. Furthermore, we present the simulation results for each task, demonstrating the effectiveness of these algorithms in achieving coordinated movements among the various swarm robots.

# Aggregation

Swarm aggregation refers to the process where a group of agents, often called a swarm, come together to form a single, cohesive group or cluster. This behavior is commonly observed in nature, such as with swarming insects, schooling fish, or flocking birds. In the context of robotics and artificial intelligence, swarm aggregation algorithms enable a group of robots or agents to converge to a single location or gather around a point of interest. The goal of these algorithms is to achieve robust, adaptive, and scalable aggregation, while maintaining simplicity and minimizing communication.

## Fields Algorithm

1. Define constants: NUM_ROBOTS (number of robots), ROBOT_RADIUS (radius of each robot), SWARM_RADIUS (radius of the swarm area), and AGGREGATION_THRESHOLD (a distance threshold below which robots don't move closer to the center of mass).
2. Create the swarm of robots. For each robot, generate random initial positions within the swarm area.
3. Main
    
    a. Calculate the center of mass of the swarm by summing the positions of all robots and dividing by the number of robots.
    
    b. Move each robot towards the center of mass: calculate the direction vector from the robot's position to the center of mass.
    
4. Update the positions of the robots in the scatter plot.

In summary, this code demonstrates a simple swarm robot aggregation behavior by simulating the motion of robots towards the center of mass of the swarm.

### Results

![/assets/images/Swarm%20Robot%20Coordination%20bad71fb1f6434c1f86377bbde4e3c576/image5.png](/assets/images/Swarm%20Robot%20Coordination%20bad71fb1f6434c1f86377bbde4e3c576/image5.png)

![/assets/images/Swarm%20Robot%20Coordination%20bad71fb1f6434c1f86377bbde4e3c576/image1.png](/assets/images/Swarm%20Robot%20Coordination%20bad71fb1f6434c1f86377bbde4e3c576/image1.png)

# Dispersion

Dispersion in swarm robotics refers to a group of robots' capacity to disperse over a wide region while still interacting and coordinating with one another. In other words, it requires evenly distributing robots around a place but still enabling them to cooperate as a group to achieve a shared objective.

Dispersion is a crucial capability in swarm robotics, as it enables the robots to accomplish tasks such as environmental monitoring, search and rescue operations, and surveillance, among others. The robots can cover a broader area more rapidly and effectively when they are dispersed than when they are grouped together in one location. This can also lessen the chance of crashes or congestion, which may happen if the robots are too closely grouped together.

Several algorithms have been developed to perform dispersion, with the Potential Fields Algorithm being one of the most commonly used.

## Potential Fields Algorithm

The Potential Fields Algorithm is a popular method for controlling dispersion of swarm robots. In this method, each robot is represented as a point in a potential field, that is created by a combination of attractive and repulsive forces. While the repulsive forces assist the robots in avoiding objects or other robots, the attractive forces motivate them to go in the direction of a desired place.

The following steps can be taken to implement the algorithm:

1. Define the target dispersion area: First, the target dispersion area needs to be defined. This can be done by defining a virtual boundary around the area where the swarm robots need to be dispersed.
2. Define the attractive force: Next, an attractive force needs to be defined to encourage the robots to move towards the target dispersion area. This can be done by setting up a virtual potential field with a low magnitude force that pulls the robots towards the center of the target area.
3. Define the repulsive force: A repulsive force needs to be defined to prevent the robots from clustering together. This can be done by setting up a virtual potential field with a high magnitude force that pushes the robots away from each other.
4. Update the forces: The attractive and repulsive forces need to be updated continuously based on the position of the robots and the obstacles in the environment. This can be done by using sensors or cameras to detect the position of the robots and obstacles and adjusting the virtual potential field accordingly.
5. Move the robots: Finally, the robots can be moved based on the resultant force vector calculated from the attractive and repulsive forces. This can be done by using differential drive motors or other types of actuators.

### Advantages

The Potential Fields Algorithm has several advantages that make it a popular method for controlling the movement of swarm robots. Some of the key advantages of this algorithm are:

1. Scalability: The algorithm is highly scalable, meaning that it can be used to control the movement of large numbers of robots in a swarm. This makes it ideal for applications where multiple robots need to work together to accomplish a task.
2. Robustness: The algorithm is robust to changes in the environment, as it continuously adapts to changes in the position of obstacles or other robots. This makes it ideal for use in dynamic and unpredictable environments, such as disaster zones or construction sites.
3. It is effective in complex environments with many obstacles and goals, as it provides a smooth and continuous way to move the robots.
4. It can ensure that the robots converge to their goal location and avoid getting stuck in local minima.

### Results

![/assets/images/Swarm%20Robot%20Coordination%20bad71fb1f6434c1f86377bbde4e3c576/image11.png](/assets/images/Swarm%20Robot%20Coordination%20bad71fb1f6434c1f86377bbde4e3c576/image11.png)

![/assets/images/Swarm%20Robot%20Coordination%20bad71fb1f6434c1f86377bbde4e3c576/image6.png](/assets/images/Swarm%20Robot%20Coordination%20bad71fb1f6434c1f86377bbde4e3c576/image6.png)

# Line Formation

Line formation in swarm robotics is a task in which a group of robots organizes themselves into a linear structure, following a predefined path or creating a path as they move. The goal of line formation is to achieve a linear arrangement of robots with a certain level of spacing between them. This task is useful in various scenarios, such as for search and rescue missions, inspection of long pipelines or cables, and monitoring of large areas.

In line formation, robots must be able to move in a coordinated way to avoid collisions and maintain the desired distance between each other. They need to communicate with each other to share information about their positions, orientations, and velocities. Several algorithms have been developed to achieve line formation in swarm robotics, and one of the popular algorithms used for this task is the Line Marching Algorithm.

## Line Marching Algorithm

The Line Marching Algorithm (LMA) is a swarm robotics algorithm used for line formation. It involves coordinating a group of robots to form a line that follows a predefined path. The algorithm involves dividing the line formation task into two sub-tasks:

- Moving forward along the line while maintaining the desired distance between robots.
- Correcting the deviations from the line caused by disturbances or errors in the robot's motion.

To use the Line Marching Algorithm, the following steps can be taken:

1. Define the line: The first step is to define the line that the robots will form. The line can be defined using a series of points, with each point assigned a unique ID. The distance between each point should be small enough to ensure that the robots can move smoothly along the line.
2. Initialize the swarm: Next, the swarm of robots is initialized. Each robot is given a unique ID and placed in a random position on the field.
3. Calculate distance to line points: Each robot calculates its distance to all points on the line and determines the closest point. The robot then moves towards that point.
4. Move towards the line: The robots move towards the closest point on the line using a simple proportional control system. The robot's speed and direction are adjusted based on its distance to the line and the desired speed of the swarm.
5. Maintain distance from neighbors: As the robots move towards the line, they must also maintain a minimum distance from their neighbors. This is done using a repulsion force, which pushes the robot away from any neighbors that are too close. The repulsion force is calculated based on the distance between the robots and the desired minimum separation.
6. Adjust movement: If a robot encounters an obstacle or another robot, it must adjust its movement to avoid a collision. This is done using a simple obstacle avoidance algorithm.
7. Continue to adjust: The swarm continues to adjust its movement until it forms a straight line that follows the path defined by the points.
8. Maintain the line: Once the line is formed, the swarm must maintain it. This is done using a combination of attraction towards the line and repulsion from other robots. Each robot calculates its position on the line and moves towards the point that corresponds to that position. At the same time, it must also maintain a minimum distance from its neighbors.

### Advantages

1. Decentralization: The Line Marching algorithm is a decentralized approach to formation control that does not require a centralized control unit. Each robot only needs to communicate with its nearest neighbors, which reduces the communication overhead and improves the scalability of the system.
2. Robustness to communication delays: The Line Marching algorithm is robust to communication delays, which are common in wireless networks. This is because the algorithm uses local information to compute the desired positions of each robot in the formation, rather than relying on global information.
3. Stability: The Line Marching algorithm is designed to maintain formation stability and avoid collisions between robots. This is achieved by imposing separation constraints between neighboring robots, which ensures that the robots maintain a safe distance from each other.

Code Structure

### Results

![/assets/images/Swarm%20Robot%20Coordination%20bad71fb1f6434c1f86377bbde4e3c576/image9.png](/assets/images/Swarm%20Robot%20Coordination%20bad71fb1f6434c1f86377bbde4e3c576/image9.png)

![/assets/images/Swarm%20Robot%20Coordination%20bad71fb1f6434c1f86377bbde4e3c576/image12.png](/assets/images/Swarm%20Robot%20Coordination%20bad71fb1f6434c1f86377bbde4e3c576/image12.png)

# Shape Formation

Swarm shape formation is a process in which a group of agents, often referred to as a swarm, organizes themselves into specific shapes or patterns. This is often observed in nature, such as with flocking birds or schooling fish, and has been extensively studied in the context of robotics and artificial intelligence. The goal of swarm shape formation algorithms is to enable the agents to coordinate and maintain the desired shape or pattern while maintaining a high degree of robustness, adaptability, and scalability.

There are several algorithms and approaches to swarm shape formation, some of which are outlined below:

**Reynolds' Boids algorithm**: It is based on three simple rules: separation (avoid collisions with neighbors), alignment (align velocity with the average velocity of neighbors), and cohesion (move toward the average position of neighbors). By adjusting the weights of these rules, a variety of swarm behaviors, including shape formation, can be achieved.

**Artificial potential fields**: In this approach, agents in the swarm are influenced by potential fields that push or pull them toward specific positions. These fields can be generated based on the desired shape, and each agent calculates its movement based on the sum of forces from these fields. This method enables swarms to form shapes without explicit communication between agents.

**Graph-based methods**: In these methods, the swarm is represented as a graph where nodes correspond to agents and edges represent connections or relationships between them. Shape formation can be achieved through graph manipulation techniques such as graph transformations, graph matching, or graph drawing. Agents can then follow specific rules to move toward their target positions, guided by the graph structure.

We used Artificial Potential Fields Algorithm

The algorithm works as below:

1. Initialize the parameters, such as the number of robots (n_robots), size of the shape (size), number of iterations (iterations), step size (step_size), shape, and collision threshold (collision_threshold).
2. Randomly initialize the positions of the robots in a 2D space.
3. Define a function (generate_target_positions) to generate target positions for the robots based on the chosen shape.
4. Calculate the target positions for the chosen shape.
5. Define a function (avoid_collision) that computes the direction a robot should move to avoid collisions with other robots, based on a given threshold.
6. Create an update function that executes the following steps for each robot:
    
    a. Find the closest target position.
    
    b. Calculate the direction towards the target position.
    
    c. Normalize the direction vector and scale it by the step size.
    
    d. Add the collision avoidance direction scaled by the step size.
    
    e. Update the robot's position by adding the calculated direction.
    
7. Create a matplotlib animation that iteratively calls the update function, which updates the robots' positions, and plots the updated positions on a 2D scatter plot.

![/assets/images/Swarm%20Robot%20Coordination%20bad71fb1f6434c1f86377bbde4e3c576/image8.jpg](/assets/images/Swarm%20Robot%20Coordination%20bad71fb1f6434c1f86377bbde4e3c576/image8.jpg)

The forces of attraction and repulsion.

In short, this algorithm moves robots towards their target positions while avoiding collisions with each other. The robots form the desired shape over a series of iterations while ensuring that they maintain a safe distance from one another.

### Results

![/assets/images/Swarm%20Robot%20Coordination%20bad71fb1f6434c1f86377bbde4e3c576/image7.png](/assets/images/Swarm%20Robot%20Coordination%20bad71fb1f6434c1f86377bbde4e3c576/image7.png)

![/assets/images/Swarm%20Robot%20Coordination%20bad71fb1f6434c1f86377bbde4e3c576/image3.png](/assets/images/Swarm%20Robot%20Coordination%20bad71fb1f6434c1f86377bbde4e3c576/image3.png)

Start End

![/assets/images/Swarm%20Robot%20Coordination%20bad71fb1f6434c1f86377bbde4e3c576/image4.png](/assets/images/Swarm%20Robot%20Coordination%20bad71fb1f6434c1f86377bbde4e3c576/image4.png)

![/assets/images/Swarm%20Robot%20Coordination%20bad71fb1f6434c1f86377bbde4e3c576/image2.png](/assets/images/Swarm%20Robot%20Coordination%20bad71fb1f6434c1f86377bbde4e3c576/image2.png)

Start End