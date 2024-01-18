---
title: Driving Straight with PID
date: 2023/07/31
description: PID robot built using an Arudino
tag: PID, control system, arduino
author: Matia Choi
---

**This website is under construction!**
Story Draft 


Making a robot drive in a straight line is harder than you might think, especially for those lacking technical expertise in this field. From this project, I was able to gain valuable insights into the intricate complexities involved in achieving precise robotic navigation. Without any help from PID. You may be able to keep the robot's heading straight for maybe a few centimeters. However, any further than that you will find it extremely difficult to keep the robot from veering to one side. 

This happens because the motors spin at a different speed from one another. The small manufacturing differences and other random factors cause the difference. However, a system with two motor wheels, two encoders for the motors, and an appropriate controller can be a remedy to this situation. The two encoders are used to create a close feedback loop that controllers the motors. The PID algorithm can implemented to potentially move the robot straight or turning around.

The PID algorithm stands for "Proportional, Integral, and Derivative". Most PID equations come in three terms with each of the terms representing proportional, integral, and Derivative. 

![A diagram of a algorithm Description automatically \n generated](https://images.squarespace-cdn.com/content/v1/5230e9f8e4b06ab69d1d8068/1598233233491-QF7Q47CFR64QE4ZDDHK4/PID+equation.png)

First, a user sets the desired output. Then the error is calculated by finding the difference from the actual output to the desired output. This error value is used to calculate each of proportional, integral, and derivative terms. These terms are then summed and send to a process (in our case, it would be setting the output values of the motors). This will cause the output value to change which is then fed back to the beginning, and the whole cycle repeats. 

## PID SOmething 

**image here**

We can get a better understanding of the PID controller by looking at its equation. As mentioned earlier, it as three terms from proportional, integral, to derivative. The "K"s are the constants for each terms. "e(t)" is the error term. Given by the name, the proportional term just multiplies its constant by the error term. The integral term sums up the errors over time and multiplies by the constant. The derivative term measures the change in the error and multiplies by its constant. All of this is summed up and fed into the motor.


**This website is under construction!**