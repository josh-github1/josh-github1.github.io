---
layout: post
title: "Tutorial on Discretization for Digital Controller Design"
categories: misc
---

<script
  src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML"
  type="text/javascript">
</script>


## Introduction

Systems are 'continuous in time' but, in order to sample and measure in hardware, digital systems must 'discretize' time.  As an example, look at the clock and notice that one of its arms advances forward clockwise per second. In this case, the clock takes a discrete step of 1 second to advance forward in time. The clock allows us to measure time according to discrete steps of 1 second.

We're interested in discretization because control algorithms are often implemented in the digital domain or on computers and microcontrollers, implying that they are discrete-time systems. Discrete-time systems measure time according to the frequency at which it is sampled. Because of this, the equations and outputs are dependent on how time is measured. We connect sensors that measure parameters like temperature, pressure, speed, or orientation to microcontrollers in order to read the outputs and states of dynamic systems, and these values are read at the time they were measured by the computer. The computer has an internal clock that does its computations and readings based on frequency. This is also something you can actually set in software - there would be a file that accesses the hardware's clock value and it would be up to the programmer to do their computations based on some factor of that clock value. 

Thinking about it realistically, you wouldn't be able to contain the values for a purely analog signal in a computer because that would be like storing an infinite amount of numbers in the hardware - it would end up crashing the machine. Philosophically, too much of anything is not usually a good thing.

### Example

So when one designs control algorithms in continuous time (e.g. formulating differential equations for a physical system; taking the Laplace Transform of that system; and developing a control system such that the plant responds based on a set of performance specifications), one would then want to convert that designed algorithm to the discrete time to make it implementable on a digital computer. As an example, the following is the Laplace Transform for a continuous-time PID controller: 

$$\frac{U(S)}{E(s)} = K_ps + K_i\frac{1}{s} + K_d s^2 $$

Assuming that one wants to implement this algorithm on a cyber-physical system (e.g. a microcontroller), the next step would be to convert it to a discrete form:

$$ u[n] = K_Pe[n] + K_I\Sigma_{k=0}^n e[k] + K_D(e[n] - e[n-1]) $$

An example of what this would look like on a microcontroller controlling a DC motor via C++ is the following:

#TODO: insert code here

The above was just a quick implementation that one can glean from knowing how integrals and derivatives are computed on paper (e.g. Trapezoidal Integration and/or the Backward Difference Method). If you're just looking to implement a PID controller without diving too deeply into theory, then that was it. In the next section, I cover a more involved/formalized method to both simulate the discrete-time control system (e.g. to check for stability) and to find the equation that is implementable in hardware. Depending on how complex your control system is, understanding the theory behind discretization and z-transforms would be ideal since highly complex control algorithms can have non-intuitive discrete equations behind them (unlike the classic PID controller).

## Theory
Discretization is actually part of the broader field of applied mathematics and there are multiple ways to discretize a continuous-time system. In this article I'll be covering the Bilinear Transform, also called Tustin's method. I'll cover how it can be used to convert continuous-time systems to discrete-time systems. Once this is done, in order to implement this on a cyber-physical system such as a digital computer, the next step would be to take the inverse Z-Transform of that converted system to identify the equation that will ultimately be implemented in software. So as you can tell, discretization is a multi-step process that is part of the larger multi-step process of control design in hardware. 

### The Bilinear Transform
The bilinear transform is a function or method that converts a transfer function in the continuous-time domain to the discrete-time domain. In particular, when one takes the Laplace Transform of a discrete-time sequence, they are also taking the Z transform of that same sequence except z is equal to the following:

$$ z = e^{sT} $$

So from this equation, one can tell that the bilinear transform is a map between the z-plane and the s-plane. You could say that Laplace transforms...

## References 

[] Control Tutorials for MATLAB and Simulink - Motor Speed: Digital Controller Design (umich.edu)

[ ] PID Controller Discretization and Implementation in Arduino – Fusion of Engineering, Control, Coding, Machine Learning, and Science (aleksandarhaber.com)

[ ] AVR221: Discrete PID controller (uni-osnabrueck.de)

[ ] Bilinear transform - Wikipedia
