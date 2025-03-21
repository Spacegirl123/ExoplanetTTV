# ExoplanetTTV

NEPTUNE is a pipeline designed to detect and characterize hidden planetary companions using Transit Timing Variations (TTVs). By combining N-body simulations, machine learning, and Bayesian inference, NEPTUNE addresses the challenges of detecting long-period or non-transiting exoplanets and refining the orbital parameters of known systems.


Hybrid Approach: Merge N-body simulations, machine learning (Random Forest), and Bayesian inference (MCMC) to detect unseen planets and determine their mass, orbital period, and eccentricity.
Efficient Parameter Estimation: Use machine learning models trained on an extensive grid of synthetic TTV signals to provide informative priors for Bayesian sampling methods.
Scalable & Flexible: Adaptable to both resonant and non-resonant systems, with built-in tools to handle observational uncertainties and noise.
Code Overview
Simulation Module

Parameter Space: Executes and manages N-body simulations over an 11-dimensional parameter space (e.g., planet masses, periods, eccentricities).
TTV Extraction: Processes simulation outputs to generate TTV signals for each configuration.
Machine Learning Module

Data Preprocessing: Augments synthetic TTV signals with observational noise.
Random Forest Training: Learns relationships between TTV features and planetary parameters, producing priors for orbital fitting.
Bayesian Inference Module

MCMC Analysis: Performs Markov Chain Monte Carlo sampling using the machine learning–derived priors.
Posterior Distributions: Extracts parameter estimates (mass, orbital period, eccentricity), quantifying uncertainties.
Validation & Testing

Pipeline Integration: Includes diagnostic tools for ensuring seamless data flow between simulation, ML, and Bayesian components.

False Positive Checks: Employs statistical tests to confirm that derived TTV signals correspond to real planetary companions.
