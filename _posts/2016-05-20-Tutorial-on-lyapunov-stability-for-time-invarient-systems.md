---
layout: post
title: "Lyapunov Stability for Time-Invariant Systems"
author:
- Joshua Ramayrat
meta: "Springfield"
---
<script
  src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML"
  type="text/javascript">
</script>

## Introduction

Analyzing the stability of any control system is essential in the control design process. It is faulty to provide just any input to a system without considering whether the system itself can handle it. 

### Motivations & Physical Examples

A physical example is voltage or power supplied to a motor for tracking or regulation. If your controller demands a voltage rating higher than what the motor can handle, it will destroy the system. That is, the motor might break or catch fire. Also consider a person balancing a pendulum where the person's hand is the controller and the pendulum is the plant. If the person is running at full speed to keep the pendulum upright, you could say the system or pendulum is stable up until the point where the person trips and falls. The system is then no longer stable. 

Another drastic scenario, taken from Slotine et al. [1], is an airplane (perhaps with passengers) subject to wind gusts mid-flight. If we actuate the wings in such a way to keep the plane stable against these wind gusts, will this new state the plane is in later on result in instability, caused either by violent vibrations or even a wing breaking off and the plane crashing? These scenarios (and of course there are many more) highlight the significance of stability analysis - philosophically, we do not want to apply forcing functions or inputs that cause systems to break either in the immediate- or far future. This can be intuitive for linear systems with simple controllers, but less obvious with more complex and highly nonlinear systems. One tool that we have out our disposal is Lyapunov Stability.  



### Introduction
When it comes to nonlinear control design, an understanding of the concept of Lyapunov Stability is fundamental and should be the first step. This is because, in addition to understanding system stability, Lyapunov stability serves as the foundation for designing nonlinear control algorithms such as robust, adaptive, or robust-adaptive controllers. This will become clear later, but one must come up with a Lyapunov function as a prerequisite to coming up with the control algorithm/function. Knowing valid Lyapunov functions (and how to come up with them) requires an understanding of Lyapunov stability (there are also other math fields such as functional analysis or differential geometry that assist with creating such functions, but this article does not cover that). 

For starters, the following is a linear differential equation that represents the physics of how a mass-spring-damper works when subject to no external forces (this could literally be a ball attached to a spring and damper): 

$$m\ddot{x} + c\dot{x} + kx = 0 $$

This is linear because the state variables $latex \ddot{x}$, $latex \dot{x}$, and $latex x$ are added and single variables that are not multiplied with each other. The following is a non-linear version of the mass spring damper: 

$$ m\ddot{x} + c\dot{x}|\dot{x}| + k_0x + k_1x^3 = 0 $$

If you were to give an input to this system, such as by u(t), the output of the system would be disproportionate. Nonlinear systems also come in two types - autonomous and non-autonomous. This should not be confused with literal autonomous systems such as robots or autonomous vehicles (although one can find ways to apply this math to such systems). In the context of nonlinear control, autonomous simply means time-invariant - it does not depend on time or the parameters of the system equation are time-independent. Non-autonomous is the opposite in that the system or differential equation does depend on time - it is time-variant. 

$$ \dot{x} = f(x) $$

$$ \dot{x} = f(x, t) $$

Notice how the first function, f(x), does not have the variable time, t, as an argument while the second function, f(x, t), does. 

As a result, Lyapunov stability is handled differently for autonomous and non-autonomous systems. This article covers the former. In reality, all systems are actually time-dependent because there is no parameter that is truly constant and unchanging with time. However, in a lot of cases we can simplify and assume that the parameters are slowly varying to the point of being practically time-independent. As in, the masses of most objects in question don't change significantly in the span of 1 second to an hour, so we can assume mass is a constant parameter.

### Illustrating Stability with Equilibrium Points
To understand what we mean by whether a system is 'stable', we must start with the system's equilibrium points which is defined as follows: 

Definition: If x(t) is equal to x* and remains equal to x*, then x* is an equilibrium state / equilibrium point of the system. 

To further illustrate, consider a physical interpretation of an equilibrium point. The following is a nonlinear differential equation that describes the motion of a pendulum:

$$ MR^2 \ddot{\theta} + b\dot{\theta} + MgRsin(\theta) = 0 $$

Thinking about it physically and intuitively, the pendulum has 2 possible locations of coming to a complete stop - either in the vertical up position or the vertical down position. These locations represent the equilibrium points of the pendulum and its equation of motion. It is with this knowledge that we can understand the stability of the system - will the system keep oscillating out of control or will it eventually settle down to an equilibrium state? Under no external forces, the pendulum will eventually settle down and thus we can conclude that the system is stable. 

#### References

[1] Slotine, Jean-Jacques E., and Weiping Li. Applied nonlinear control. Vol. 199. No. 1. Englewood Cliffs, NJ: Prentice hall, 1991. 
