[//]: # (Image References)
[image1]:  ./eqns.png


# CarND-MPC-Project

**Intro**

The idea behind this project is to implement a Model Predictive Controller for the car to follow a trajectory. The problem is formatted into an optimzation problem, and solved using the *IPOPT Solver*. 

The loop is as follow:

1 - Initial State is given
2 - Based on the initial state, model equations, and state constraint, the lowest cost trajectory is computed along with the coressponding control inputs.
3 - Only the first set of control inputs for the next time step are actually implemented
4 - Step 1 again

**Student describes their model in detail. This includes the state, actuators and update equations.**

The states for this system are:

**x**: X position <br/>
**y**: Y position <br/>
**psi**: orientation <br/>
**v**: velocity <br/>
**cte**: Cross-Track-Error, which is computed as the difference between the desired y position, and the current y position <br/>
**epsi**: Orientation Error, which is computed as the difference between the desired angle and the current angle <br/>

Our control inputs are:

**delta**: The steering angle <br/>
**a**: The throttle speed <br/>

A kinematic model, instead of a dynamic one, was used for its simplicity, ease of implementation, and most importantly, the ability to effectively compute it.

The model is as follows:

![alt text][image1]

**Student discusses the reasoning behind the chosen N (timestep length) and dt (elapsed duration between timesteps) values. Additionally the student details the previous values tried.**

An adequate combination between the values of N and dt was chosen, where N was chosen so as not be too large, and dt as small as possible, so that they would the optimizer would not have to take too long, yet at the same time for the environment not to change much.

N was chosen to be 50 and dt was 0.05

**Polynomial Fitting and MPC Preprocessing**

First, the way points of the trajectory were preprocssed by transforming them from map coordinates to vehicle coordinates. They were then fitted to a 3rd degree polynomial, as most roads can be described as such. 

**The student implements Model Predictive Control that handles a 100 millisecond latency. Student provides details on how they deal with latency.**

At each iteration of the loop, the initial states are plugged into the model with dt = 100 milliseconds, which is the same as saying, let the car run with its current inputs for an extra 100 milliseconds, before capturing the updated states and passing them to the solver. The pose (x, y, psi) is all set to zero, since in vehicle coordinates the car is at the origin. 
The optimizer then finds the minimum cost inputs and with which the vehicle is actuated.
