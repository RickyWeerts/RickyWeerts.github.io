---
layout: project
title:  "Duke Robotics Club"
description: "Duke's underwater robotics team."
categories: Python git ROS
permalink: 02-duke-robotics
image: assets/images/duke_robotics/oogway.jpg
repo: https://github.com/DukeRobotics/robosub-ros
links:
  - text: Team Website
    url: https://duke-robotics.com/
---

<div class="image-pair">
  <img src="assets/images/duke_robotics/oogway.jpg" />
  <img src="assets/images/duke_robotics/cthulhu.jpeg" />
</div>

For all four years of college, I have been involved with the Duke Robotics Club, first as a member on the Task Planning subteam, and now as the Software Team Lead. As a team lead, I am responsible for making sure the different software subteams are making good progress. It is also my responsibility to review pull requests from different members before they can be merged into the master branch. The majority of the team's programming is done in Python, and we use ROS (Robot Operating System) to communicate between different parts of our codebase.

## The Competition
Every summer, we compete in an international underwater robotics competition known as [RoboSub](https://robosub.org/). Our goal is to complete as many tasks as we can within a time limit. All tasks must be done completely autonomously. Once the robot enters the water, it needs to make decisions completely on its own, with only its sensor inputs for guidance.

Tasks vary from moving through a gate to shooting torpedos into a hole to surfacing in a floating octagon.

## ROS
The Robot Operating System (ROS) is a system that we use to communicate between different parts of our codebase. Different nodes communicate by publishing messages to certain topics that other nodes can subscribe to. These nodes can simply be different modules of code on the same machine or run on completely different devices. Another powerful aspect of this system is that nodes can be written in different programming languages and still communicate. For example, there's a node on an arduino (written in Arduino's C++ variant) that recieves thruster inputs from a node on the main robot computer (written in Python), and then powers the thrusters at the speeds specified.

Here is a general overview of the flow of information through the different nodes and systems of the robot.
![Flowchart showing how information comes from the sensors and works its way through different nodes to the actuators](assets/images/duke_robotics/RoboticsFlowDiagram.png)

## Controls
The controls module takes setpoints (poses or velocities) and adjusts the thruster inputs to reach those setpoints. PID feedback loops are used to determine the amount of "effort" the robot puts towards each axis of movement. These feedback loops need to be carefully tuned to allow for smooth and accurate movements.

I recently changed our controls API from using raw publishing and subscribing to topics, to a wrapper for topics called "actions". Under the old system, setpoints for controls had to be published constantly and at a fixed rate. Actions allow other pieces of code to send a the desired setpoint only once, without any regard for timing. Our controls code will continue trying to reach that setpoint until a new setpoint is received.

## Task Planning
In order for the robot to make decisions we use finite state machines, specifically a library called [SMACH](http://wiki.ros.org/smach). The transition to SMACH was during my time on the team, and I have done most of the programming work in converting our codebase.

![A flowchart showing a small portion of one of our state machines](assets/images/duke_robotics/gate_flowchart.png)

## Sensing
The team uses a combination of sensor inputs to determine where the robot is and what its surroundings are. An IMU and DVL give us readings on our position, rotation, speed, and acceleration. Stereoscopic cameras allow us to detect objects in the water and determine their distance with the help of neural networks. A sonar module gives us additional information on surrounding objects. Hydrophones are positioned across the robot to triangulate the approximate location of acoustic pingers on certain elements of the competition. All these sensor inputs are fused into useful data and taken into account when accomplishing tasks.

When programming tasks, I need to consider the limitations of the sensors I'm using, and account for these limitations in my code. For example, cameras and sonar are great for determining the position of a "buoy" we need to bump into. Once the robot gets close to that buoy, the cameras lose track of it, and we have to make sure to continuing moving towards the last place the buoy could be seen.


