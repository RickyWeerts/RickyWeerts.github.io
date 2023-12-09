---
layout: project
title:  "Cake Icing Robot"
description: "Programming a robot to ice a cake."
categories: Python git ROS Gazebo
permalink: 04-icing-robot
image: assets/images/cake_icing_robot/thumbnail.png
repo: https://gitlab.oit.duke.edu/smw92/finalrobotics
---

<video muted controls width="100%">
    <source src="assets/images/cake_icing_robot/demo_fish.mp4" type="video/mp4">
</video>

I worked with a team of 3 other students to program a UR5 robot to ice a cake in a specified pattern. We used the Robot Operating System (ROS) to communicate with the robo and Gazebo to simulate our final results.

As input, we took SVG files and converted them into lists of points, which we then ordered the robot to trace a cartesian path over those points. We needed to differentiate between different shapes (so that we knew to turn off the icing while moving between them), so we used a list containing other lists, with each "sublist" representing a different shape/path to draw. My portion of the project largely focused on parsing the SVG file and converting it into the list of lists of points that we passed to the rest of the program. I used XML-parsing code already present in Python's standard library and parsed a limited subset of the SVG format. 