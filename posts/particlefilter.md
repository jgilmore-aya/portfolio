# Introduction to Particle Filters

Particle filters can be an effective method when you're dealing with non-Gaussian distributions or nonlinear dynamics that makes Kalman filtering impractical. They are a Monte Carlo approach to state estimation that can be highly useful in certain cases.

## The Core Problem

In the real world, deviations from the Gaussian distributions are common. Noise sources in physical systems come in various form (e.g. pink, shot), financial time series have strong tails, or object tracking has multi-modal distributions.

Particle filters solve this by representing the system state as a cloud of weighted samples (the particles) thereby allowing greater flexibility.

## Implementation Strategy

In their simplest form particle filters have 3 core steps:  
*1. Prediction step:* Move each particle according to a motion model  
*2. Update step:* Weight particles based on observation likelihood  
*3. Resampling:* Eliminate low-weight particles, duplicate high-weight ones

Each particle represents a possible state about where the system is:  

#### Update function in R
update <- function(pf, observation, observation_noise, motion) {
  for (i in 1:pf$n_particles) {
    diff <- observation – (pf$particles[i] + motion)
    #  likelihood update
    pf$weights[i] <- pf$weights[i] * likelihood[i]
  }
  return(pf)
}

The approach has substantial flexibility - you can use any observation model, likelihood, or motion model, without being limited by analytical tractability or requiring a  Gaussian likelihood.

## When They Work
I'd recommend trialling particle filters in these scenarios:
**Complex Dynamics:** Typically in multi modal systems
**Financial modelling:** Regime-switching in abruptly changing markets 
**Object tracking:** Especially with occlusions or cluttered backgrounds 
**Physical and Biological systems:** Changing variables in time with stochastic events 

In general if your system has non-Gaussian characteristics, such as the true posterior distribution being decidedly non-Gaussian, particle filters are worth considering. 

## Performance Expectations

In the past I worked with gravitational n-body simulations, and particle filters share some similar computation characteristics to those types of calculations. In general, tracking lots of particles is computationally expensive. This is the key downside of particle filters and places limitations on their usefulness. In particular, particle filters might not be your best choice in the following cases:
*High-dimensional states: Consider Unscented Kalman Filter instead*  
*Computational constraints: Each particle requires a full state prediction*  
*Smooth, unimodal posteriors: Classical Kalman filtering is more efficient*  

In future posts I will address advanced implementations and technical details for particle filters.  

*The value in understanding particle filters is recognising when assumptions about system behaviour are violated and having methods to handle these cases.*
