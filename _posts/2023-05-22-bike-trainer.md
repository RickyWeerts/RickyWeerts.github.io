---
layout: project
title:  "Aelevate Bike Trainer"
description: "The fully integrated, smart, elevating bike trainer."
categories: gui .net
permalink: 03-bike-trainer
image: assets/images/bike_trainer/group_photo.jpg
repo: https://github.com/Brokemia/Aelevate_GUI
---

As part of an independent study project, I worked with 3 other undergraduate engineering students and 6 Master in Engineering Management graduate students to develop a bike trainer with variable resistance and tilt. Existing bike trainers often lack tilting capability, come at a high price, or are sold as separate parts that need to be combined for full functionality. Our product is designed as a single package that transforms the user's existing bike into a fully featured bike trainer.

<div class="image-pair">
  <img src="assets/images/bike_trainer/group_photo.jpg" />

  <video autoplay muted loop>
    <source src="assets/images/bike_trainer/uppy_downy.mp4" type="video/mp4">
  </video>
</div>

## GUI
Every part of the bike trainer is controlled through an intuitive GUI. In the final product, this GUI would be shown on a screen mounted to the handlebars of the bike. The GUI was my main focus, and I took this as an opportunity to learn .NET MAUI (Multi-platform App UI), a relatively new UI framework evolved from Xamarin.Forms. The cross-platform capabilities of .NET MAUI allowed me to prototype quickly without having to worry about where the GUI would eventually be deployed to. I did find a few regards, in which it was clear this framework still needed some work, but it was interesting to work with regardless.

![A screenshot of the GUI in action](assets/images/bike_trainer/bike_gui.png)

This GUI allows users to control resistance and tilt independently, or link them together to control them in a fixed ratio​. It provides graphical visualizations of the current speed and tilt of the trainer. Finally, it provides a central interface for running pre-programmed routes, with the ability to skip, pause, and play the progress of the courses.

## Communication
The GUI communicated with an Arduino through a serial communication protocol. Commands were sent to the Arduino with the following format:

|0xFF   | Command ID (char) | ... Arguments ... |

A single 0xFF byte indicated the start of a command to help ensure the Arduino and GUI stayed in sync, especially when first establishing a connection. Then a single byte (typically represented as a character) indicated what type of information was being sent. Any arguments follow. It was important that 0xFF didn't appear anywhere else in the command to avoid falsely indicating the start of a command, so we limited our arguments to have values from 0 to 254.

In response, the Arduino sent back a far simpler data format. It simply alternated between sending the current number of encoder ticks from the hall effect sensor and a timestamp. Both were encoded as unsigned 32-bit integers. From this information, the GUI determined the current pedaling speed and added it to a graph.

![An overview showing the different parts of the project and how they communicate](assets/images/bike_trainer/logic_diagram.jpg)

## Sensors
We used a potentiometer to detect the actual tilt value of the bike, which combines with a hoist to make a feedback loop that accurately targets our desired tilt angle. We also use a hall effect sensor to detect the number of rotations of the back wheel for use in determining the speed of the bike.

To allow for user interfacing with the product, we developed software to serve a GUI to the user to allow for touch control of the trainer and visual feedback to the user. It was written in C# using .NET MAUI (Multi-Platform App UI)​ framework to allow for quick prototyping and platform compatibility. It allows users to control resistance and tilt independently, or link them together to control them in a fixed ratio​. It provides graphical visualizations of the current speed and tilt of the trainer. Finally, it provides a central interface for running pre-programmed routes, with the ability to skip, pause, and play the progress of the courses.

![A diagram showing how the arduino, sensors, and actuators are wired together](assets/images/bike_trainer/wiring_diagram.jpg)


