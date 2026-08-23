# NEPTUNE
## (N-body Exoplanet Prediction using TTV for UNseen Exoplanets)

**NEPTUNE** is a computational pipeline for investigating unseen planetary companions through **Transit Timing Variations (TTVs)**. Instead of requiring a hidden planet to transit its host star, NEPTUNE searches for its gravitational effect on a planet that does transit: an additional companion can cause successive transits to occur slightly earlier or later than predicted.

The project combines **observed transit data, N-body simulations, machine learning, and Bayesian inference** to constrain the mass, orbital period, and eccentricity of possible perturbing planets.

## Workflow

### Part 1 — Transit Extraction and O–C Construction

The first notebook queries public **Kepler** transit data and planetary parameters from the **NASA Exoplanet Archive**.

It then:

- downloads the available Kepler light curves
- cleans and detrends the photometry
- fits individual transit events
- measures the mid-transit time of each event
- compares the measured times with a linear ephemeris
- constructs an **Observed-minus-Calculated (O–C) timing curve**

The notebook uses tools including:

- `lightkurve`
- `EXOTIC`
- `Astropy`
- `SciPy`
- `pylightcurve`

The resulting O–C curve contains the transit-timing variations used by the later stages of NEPTUNE.

### Part 2 — N-body Simulation of Planetary Systems

The second notebook generates synthetic two-planet systems and calculates the TTV signals produced by their gravitational interactions.

The simulations explore changes across planetary-system parameters including:

- stellar mass
- inner-planet mass
- outer-planet mass
- inner and outer orbital periods
- orbital eccentricities
- orbital inclinations
- arguments of periastron
- orbital period ratios
- planetary mass ratios

The code also calculates quantities such as the **Hill radius** and **mutual Hill radius** to help evaluate the dynamical configuration of simulated systems.

Each simulation produces a synthetic O–C curve for a system whose underlying planetary parameters are known. These simulations therefore form the training dataset used by the machine-learning stage.

## TTV Signal Analysis

Transit-timing variations can contain signals operating on different timescales.

NEPTUNE analyzes these timing curves to identify periodic structure associated with gravitational interactions between planets. In particular, the pipeline can examine both longer-period resonant TTV behaviour and shorter-period synodic signals produced by repeated planetary conjunctions.

Using multiple components of the timing signal is important because different combinations of planetary mass and eccentricity can sometimes produce similar dominant TTV curves. Additional periodic information can therefore help distinguish between otherwise degenerate dynamical solutions.

### Part 3 — Machine Learning and Bayesian Inference

The third notebook uses the simulated systems to learn the relationship between measurable TTV properties and the physical parameters of the perturbing planet.

A **Random Forest regressor** is trained on features extracted from the simulated timing curves and is used to generate initial estimates for parameters such as:

- orbital period
- eccentricity
- planetary mass

These predictions provide informed starting regions for a subsequent **Markov Chain Monte Carlo (MCMC)** analysis using `emcee`.

Rather than searching the entire parameter space blindly, the Bayesian sampler begins near regions identified by the machine-learning model and then explores the posterior distribution to refine the solution and estimate parameter uncertainties.

## Posterior Analysis and Diagnostics

The inference notebook includes tools for examining the resulting parameter space and determining whether a stable solution has been found.

These include:

- MCMC trace and convergence analysis
- posterior distributions
- corner plots
- percentile-based visualizations
- candidate-solution clustering
- `HDBSCAN` and `DBSCAN` clustering tools
- goodness-of-fit comparisons

The clustering tools are particularly useful when the timing data permit multiple possible dynamical solutions, allowing separate regions of parameter space to be identified and examined individually.

## Validation

NEPTUNE was tested using the **Kepler-46** system, where transit-timing variations of Kepler-46b are known to reveal the non-transiting planet Kepler-46c.

The pipeline recovered a solution for the unseen companion consistent with the known system parameters, demonstrating that the combined simulation, machine-learning, and Bayesian approach can recover a hidden planet from transit timing information.

NEPTUNE was also applied to **Kepler-1710**, a system exhibiting significant transit-timing variations. The analysis produced a possible solution involving an additional Neptune-mass companion near a mean-motion resonance. This represents a candidate dynamical solution to the observed TTV signal rather than a confirmed planetary detection.

## Purpose

The broader goal of NEPTUNE is to make dynamical TTV analysis more **automated, reproducible, and computationally accessible**.

By combining archival transit observations with simulation-based learning and Bayesian inference, the pipeline provides a framework for investigating planetary systems in which additional planets may be difficult or impossible to detect directly through transits.

## Key Features

Hybrid Approach: Combine N-body simulations, machine learning (Random Forest), and Bayesian inference (MCMC) to detect unseen planets and determine their masses, orbital periods, and eccentricities.

Efficient Parameter Estimation: Use machine learning models trained on an extensive grid of synthetic TTV signals to provide informative priors for Bayesian sampling methods.

Scalable & Flexible: Adaptable to both resonant and non-resonant systems, with built-in tools to handle observational uncertainties and noise.

Parameter Space: Executes and manages N-body simulations over an 11-dimensional parameter space (e.g., planet masses, periods, eccentricities).

TTV Extraction: Processes simulation outputs to generate TTV signals for each configuration.

Data Preprocessing: Augments synthetic TTV signals with observational noise.

Random Forest Training: Learns relationships between TTV features and planetary parameters, producing priors for orbital fitting.

MCMC Analysis: Performs Markov Chain Monte Carlo sampling using the machine learning–derived priors.

Posterior Distributions: Extracts parameter estimates (mass, orbital period, eccentricity), quantifying uncertainties.
Validation & Testing

Pipeline Integration: Includes diagnostic tools for ensuring seamless data flow between simulation, ML, and Bayesian components.

False Positive Checks: Uses statistical tests to confirm that derived TTV signals correspond to real planetary companions.
