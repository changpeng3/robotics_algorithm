# Understanding Particle Filters and Its Numerical Implementation in Discrete Domain

By Chang Peng, 2020, 07/08

## I. Fundamentals

## II. High-level 

## III. Steps in implementation

### Step 1: Initialization of particles
1. Question: What is a particle in definition?

   Answer: A particle is a belief, which is a location/pose of the ego object in the case of a localization problem.
   Depending on the degrees of freedom, the particle is in the form of a set of float numbers. 
   In the case of 2D, it could be three numbers: x-coordinate, y-coordinate, heading.
   In the case of 3D, it could be a set of six numbers: x, y, z, yaw, pitch roll.
   
1. Question: How many particles are necessary?

   Answer: Increasing number of particles would increase computational load
   
   "For this choice I often think about the trade-off between computational cost and the variance of the resulting estimates. As you increase the number of particles or sample size the former increase, while the latter decreases.
    
    Often I do a simple computational experiment:
    
    I create a grid of potential numbers of particle (say 102, 103 and 104).
    
    I do the filtering N times using each sample size.
    
    I plot the sample variance of the quantity I'm interested in (for example the variance of the estimated likelihood) on the Y axis, with the number of particles on the X axis.
    
    You should get a convex curve, that becomes flat as the number of particles increases. Generally I just look at it, and choose a number of particle that seems reasonable in the sense that increasing the number of particles further wouldn't reduce the variance by much.
    
    Obviously this is just a practical approach, maybe there are more rigorous ways of looking at the problem."

2. Question: Does the number of particles change?

   Answer: It could be set adaptive. However, the implications of complexity needs to be considered.

Increasing  

### Step 2: Prediction
Prediction is a simple process that implement the process model (also called motion model) in discrete domain.

A sigma (standard deviation) array is passed into prediction function, together with time step size and control variables.
Control variables are velocity and yaw rate.

Then it loops thru all particles by applying process model, after which Gaussian noise is added.

### Step 3: Update weights
There are three nested loops in this step.
#### Loop 1: through all particles.
Each particle has an associated weight as well as one set of observation in homogeneous coordinates.
#### Loop 2: through all items in the observation.
Each observation is a list of different measurements with regard to the landmarks in a given map.
The size of list differ from one time step to another.
#### Loop 3: through all landmarks.
For one item from the list of one observation, it is compared to all landmarks in a give map.

### Step 4: Resample
The resample "filter" out particles with lower weights, not by threshold, but by probability. 
With multiple such resample steps, particles with lower weights are essentially eliminated.
This in turn, improves the quality of particles survived the resample step.

Even particles with higher weights could be duplicated after resample at step t, noise will be added to the duplicated particles which then differentiate them.

In C++, std::discrete_distribution is used to simplify the resample code.
https://en.cppreference.com/w/cpp/numeric/random/discrete_distribution