---
title: Proportional Integral Derivative Control
layout: post
---


## Introduction

A PID controller is an acryonym for Proportional-Integral-Derivative Control. When we want to control the output of a system (e.g. the angle or angular velocity of a motor), we would first develop a mathematical model representing the system's outputs as a function of its inputs. The controller provides the inputs necessary to give us the desired outputs. An example of a system is a brushed DC motor. These are controlled via 'direct current', hence the DC in its name. The angular-position, -velocity, and -acceleration are a function of the current applied to the motor's windings. 

To understand why the PID controller works the way it does, first look at an example of a system without any feedback or algorithm at all:

This image has an empty alt attribute; its file name is img_9229.jpg
Assume that 'Controller' is just a value of 1. The required/desired process response is directly entered into the process model, and the output is what it is regardless of whether it actually matches the desired process response. In the context of a control system implemented in hardware, this means that there is no sensor connected to the system and back to the electronics or at least there is no logic to handle what our sensor is sending and what we want the system to do. That system could be a DC motor where we are actuating it with PWM signals but there is no encoder to read in motor speed.

Now look at the system model with PID and feedback:

### <insert diagram>

Notice the arrow pointing back towards the summing block from the output of the plant. This is to compute for the error, the output value of that same summing block. Conceptually what's happening is that the PID controller is modifying our input to the system based on the error dynamics. 

Again, that system could be a variety of things such as the temperature or pressure in a bioreactor; the yaw angle of an aircraft; the vibrational response of a building structure to an earthquake (provided that the building has a vibration control system); or the acceleration of a vehicle when pressing on its gas pedal. For most systems, one could get feedback (and subsequently the error) through electronic devices or sensors such as accelerometers, temperature/pressure sensors, or magnetometers and gyroscopes - this is to get the measured value to compute the error.

### Formula
With u(t) as the controller input and e(t) being the error between the actual or measured output value of the plant and the desired output value for the plant, the equation for a PID controller is as follows:

$$ u(t) = K_p e(t) + K_I \int e(t)dt + K_d \frac{de}{dt} $$

### Controller gains

- $$K_P$$ is proportional gain where $$e(t) = y_{desired} - y{actual}$$
- $$K_I$$ is integral gain.
- $$K_D$$ is derivative gain.

These are coefficients that we update and tune to change system behavior based on different error dynamics. Tuning for each coefficient has a different meaning because of how the error is being processed.

#### Proportional Term
The proportional term causes the plant to produce an output value proportional to the current error or difference between the desired response and actual response.


## Example
As this is a classical control strategy, the conventional way of implementing a PID controller on paper is to pair it's own transfer function with the plant transfer function for a dynamical system. This is done through a series of taking the Laplace Transform of both the controller's and plant's differential equations.

As an example, the following is a dynamic model for a vehicle which is simplified as a bicycle model: