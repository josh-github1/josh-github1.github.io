---
layout: post
title: "System Identification via NARMAX"
categories: misc
---
<script
  src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML"
  type="text/javascript">
</script>

## System Identification via NARMAX

Recall Newton's Law, F=ma, and how it maps a relation between force applied to an object and its mass and acceleration. With more math (integrations with initial conditions), one can theoretically get the position of the object with respect to time. This is assuming that we live in an ideal world where everything obeys these physics laws. Objects obey linear/idealized physics laws in general but only up to a certain extent. In most conditions, there will be external forces and conditions that cause the object to not accelerate according to the given equation (i.e. Newton's Law and the ball). It wouldn't be that equation exactly in reality. So what does one do if they need to know the exact location, in a real world condition. They can follow a method called system identification. System identification is a general term for other specific techniques (which I will expound on later).

The meaning of the concept is in the name - the system is being identified. This is done by collecting data on the system of interest and then fitting an equation to that data to come up with a mathematical formula that makes predictions for new or unforeseen inputs. As an example, consider a ball being pushed on a surface that has constant friction at all points in space. If one collects enough data on the balls location (maybe by manually measuring it with a ruler or another technique) and plots the resulting data, they will roughly get a graph that matches Newton's law. However, place the ball in a grass field where it's windy outside. One would find that this plot does not really match F=ma. Another solution is to plot it and choose an alternative method that fits an equation to the resulting data.

Although the ball example is simple, this idea can be extended to seemingly entirely different problems like determining the orientation of a drone in the air, the temperature or chemical concentrations in a batch reactor, the speed of a vehicle on a freeway, or even the state of an economy based on policies, population, and other things. As long as you can measure and collect data on the system, you should be able to identify it. This is considered a data-driven approach and there are significant similarities to other fields like machine learning. In general, we're making predictions on the behaviors and outputs based on past inputs. For mechanical systems like robotics, one would normally get such data by attaching sensors to the system and to a microcontroller. A program running on the electronic device would collect the data being read by the sensor (e.g. an encoder/sensor to measure the resulting angular position/velocity when a robot arm is provided with a command). How the sensor is collecting data is another story, but let's assume for the sake of this article that it is a black box.

Moving on to the actual method, in this article I will expand on a method called NARMAX. NARMAX is an acronym for Nonlinear Autoregressive Moving Average with Exogenous Inputs, a concept co-created by Stephen Billings. The formula -

$$ Y(t) = F(y_{t-1}, y_{t-2}, ..., y_{t-n_y}, u_{t-d}, u_{t-d-1}, ..., u_{t-d-n_x}) + \epsilon _t $$

- $ y_{t-n} $ consists of previous system outputs.
- $ u_{t-n} $ consists of previous system inputs.
- $ \epsilon _t $ consists of measurement noise and uncertainty.

So the current system output depends on previous outputs, previous inputs, and current & previous measurement noise & uncertainties. For those wondering what is meant nx, ny, and ne - these represent the maximum lag for the system output and input. Lag represents the time delay or delta for a system's process variable to output a new value with respect to a control action.

The next problem is how does one encode all this into the function F(...)? There are multiple ways of doing this and this is termed as a NARMAX representation. From looking at the following list of techniques to map unknown functions, one may realize that system identification can be a broad topic:

- generalized additive models
- polynomial basis
- wavelet basis
- radial basis functions
- fuzzy logic-based models
- neural networks

### Example
Since neural networks (NNs) are well-known, I'll cover this method for function approximations with a simple mass-spring-damper system. The following code was used to generate a dataset for such a system [3]:

```python
# Import Visualizer class
from visualizer import Visualizer 

dt = 0.05 # ΔT (sampling period) seconds

# Initial values
position = 15
velocity = 0
acceleration = 0

# Constants
mass = 1 # mass
k = 2.5 # spring coefficient
b = 0.3 # damping coefficient

# Callback Function
def set(arg):
    global dt, position, velocity, acceleration, mass, k, b # Get global variables

    spring_force = k * position # Fs = k * x
    damper_force = b * velocity # Fb = b * x'

    # If we leave the acceleration alone in equation
    # acceleration = - ((b * velocity) + (k * position)) / mass
    acceleration = - (spring_force + damper_force) / mass
    velocity += (acceleration * dt) # Integral(a) = v
    position += (velocity * dt) # Integral(v) = x

    return (position, 0) # Return position
^ Note to self: change 2nd_order.py in your sandbox code

```

Looking at the above code, I've essentially created a dataset for the following system where mass, m, equals 1; damping coefficient, c, equals 0.3; and the spring coefficient, k, equals 2,5:

$$ m \ddot{x} + c \dot{x} + kx = u(t) $$
This is a simulation that generated the dataset, but in practice it would be an actual physical experiment - you would literally have a spring and a dampening device attached to a moveable mass such as a ball, and some way to record the distance of that ball. Regardless, the point of system identification is to build mathematical models from a dataset and measurements so that we can make predictions on what the system will output when provided with new inputs. As a disclaimer, this article does not go into the theory of how neural networks were derived or how to design particular architectures - one uses NNs as tools for system identification and one may often just end up using python libraries such as SysIdentPy.

## NARX Neural Network for SysId
As a note, a NN will only be used to help identify a dynamic system, its original equation is not the NARMAX equation because NNs are for static systems (where the current outputs are dependent on the current inputs).

This image has an empty alt attribute; its file name is network331.png
Figure 1: 3-Layer NN Model

This original NN model is converted into a NARX neural network by adding feedback connections. This is ultimately the method for system identification via NARMAX with NNs.

This image has an empty alt attribute; its file name is dynamic_narx.gif
Figure 2: Dynamic NN Model

To further illustrate this point, the following equation represents the mathematical model for the 3-Layer NN:

#Todo: Add NN equation

To re-illustrate, this is the original NARMAX equation mentioned earlier:

#Todo: Add eqn.

## Short Explanation on NNs and Dynamic NNs
As an aside, neural networks have almost no relation to how our brains actually work and that they are just like any other math problem (and there are much more difficult math problems). A neural network just happens to have the word 'neural' in its name.

So for neural networks, there are many ways to implement them and their performance on a dataset can depend on how they are designed. In summary, they are 'function approximators' in that they approximate what the actual function should be based on repeatedly tuning its own parameters. To understand what those parameters are and how it's tuning it, consider again the 3-layer network model in figure 1.

If you've never seen an NN before, then this might be a lot to look at. However, as with most complicated things, this network is essentially made of simple building blocks and such a block is this single 'neuron':

This image has an empty alt attribute; its file name is singleneuron.png

## References
[1] Introduction to NARMAX Models - SysIdentPy
[2] Basic Usage - SysIdentPy
[3] programming-exercises/demo.py at master · enesdemirag/programming-exercises (github.com)
[4] Unsupervised Feature Learning and Deep Learning Tutorial (stanford.edu)
[ ] Neural NARX - SysIdentPy
[ ] Design Time Series NARX Feedback Neural Networks - MATLAB & Simulink (mathworks.com)
[ ] 2006.02915.pdf (arxiv.org)
