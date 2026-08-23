# NEPTUNE (N-body Exoplanet Prediction using TTV for UNseen Exoplanets)

NEPTUNE is a computational pipeline for investigating unseen planetary companions through Transit Timing Variations (TTVs). Instead of relying on a hidden planet to transit its host star, NEPTUNE looks for its gravitational effect on a planet that does transit: an additional companion can cause successive transits to occur slightly earlier or later than predicted. The project combines observed transit data, N-body simulations, machine learning, and Bayesian inference to constrain the mass, orbital period, and eccentricity of possible perturbing planets.

The repository is organized as a three-stage workflow. Part 1 queries public Kepler data and planetary parameters from the NASA Exoplanet Archive, cleans and detrends the light curves, fits individual transits, and converts their measured mid-transit times into an Observed-minus-Calculated (O–C) timing curve. The notebook uses tools including lightkurve, EXOTIC, Astropy, SciPy, and pylightcurve to take the analysis from archival photometry to the TTV measurements used by the later stages.

Part 2 constructs synthetic two-planet systems and generates their TTV signals using N-body integrations. The simulations explore how changes in parameters such as stellar and planetary mass, orbital period, eccentricity, inclination, and argument of periastron affect the resulting timing signal. Stability-related quantities including the Hill and mutual Hill radii are also calculated, while the simulated O–C curves provide a training set in which the properties of the perturbing planet are already known.

Part 3 uses those simulations to connect measurable properties of a TTV signal back to the planetary system that produced it. RandomForestRegressor models provide initial constraints on candidate planetary parameters, which are then refined using Markov Chain Monte Carlo with emcee. The notebook includes posterior and corner-plot visualization, convergence diagnostics, and clustering tools such as HDBSCAN for examining multiple candidate regions of parameter space. Together, these stages provide an end-to-end experimental framework for moving from observed transit light curves to possible dynamical solutions for an unseen companion.

### Key Features

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
