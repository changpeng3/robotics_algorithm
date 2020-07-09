# Understanding Particle Filters and Its Numerical Implementation in Discrete Domain

By Chang Peng, 2020, 07/08

## I. Fundamentals

## II. High-level 

## III. Steps in implementation

### 1. Initialization of particles
1. Question: What is a particle in definition?

   Answer: 
1. Question: How many particles are necessary?

   Answer: Increasing number of particles would increase computational load

2. Question: Does the number of particles change?

   Answer: It could be set adaptive. However, the implications of complexity needs to be considered.

Increasing  

### 2. Prediction
Prediction is a simple process that implement the process model (also called motion model) in discrete domain.


### 3. Update weights


### 4. Resample
The resample "filter" out particles with lower weights, not by threshold, but by probability. 
With multiple such resample steps, particles with lower weights are essentially eliminated.
This in turn, improves the quality of particles survived the resample step.

Even particles with higher weights could be duplicated after resample at step t, noise will be added to the duplicated particles which then differentiate them.
