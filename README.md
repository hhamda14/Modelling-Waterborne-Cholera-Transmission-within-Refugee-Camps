# Modelling-Waterborne-Cholera-Transmission-within-Refugee-Camps
A compartmental epidemiological model, built with Python/SciPy, simulating the spread of cholera between two connected refugee camp clusters sharing a water source, and evaluating the impact of public health interventions on outbreak severity.

##Toolkit
- **NumPy** — numerical computation
- **SciPy (odeint)** — solving the system of ordinary differential equations (ODEs)
- **Matplotlib** — epidemic curves and intervention comparison plots
- **Google Colab** — development environment
- 
## The Model
The model extends a standard SIR framework with an environmental (waterborne) transmission route and two coupled sub-populations, reflecting how cholera spreads in refugee camp settings where clusters may share contaminated water sources.

Two clusters, each with its own set of compartments:

S — Susceptible
Ia — Infected, asymptomatic
Is — Infected, symptomatic
R — Recovered
B — Bacterial concentration in the local water reservoir

Symptomatic and asymptomatic individuals shed bacteria into the water at different rates (es, ea), and infection risk in each cluster depends on the local bacterial concentration relative to a saturation constant (K). A mixing parameter (m) allows bacterial contamination to spread from Cluster 1's water source into Cluster 2's, representing shared or nearby water infrastructure.

## Key parameters:
- **a1, a2** - Exposure rate per cluster
- **sigma** -	Proportion of infections that are symptomatic
- **r** -	Recovery rate
- **K** - Water bacterial saturation constant
- **ea, es** - Bacterial shedding rate (asymptomatic / symptomatic)
- **mb** - Natural bacterial decay rate
- **m** - Cross-contamination/mixing rate between clusters

The basic reproduction number (R₀) is calculated analytically per cluster from these parameters, alongside numerically solving the ODE system with scipy.integrate.odeint.

## Validation

Before analysis, the model is checked with basic unit tests on the simulated output:

Mass conservation — total population per cluster (S + Ia + Is + R) remains constant over time.
Non-negativity — all compartments stay ≥ 0 throughout the simulation.
Outbreak Metrics

For each cluster, the model computes:

Attack rate — % of the population ultimately infected
Peak infections — maximum simultaneous infected count, and the day it occurs
Peak bacterial concentration in the water reservoir, and when it occurs
Outbreak duration — time the epidemic stays above a defined active-case threshold
Interventions Modelled

Each intervention is simulated at Baseline / Low / Moderate / High levels, and compared against the no-intervention baseline on attack rate, peak infections, and R₀:

Chlorination — added water treatment term that increases bacterial decay
Improved latrine coverage — reduces bacterial shedding into water
Borehole/water-source suspension — reduces the cross-cluster mixing rate (m), modelling isolating a shared contaminated water source
Vaccination — reduces the initial susceptible population
Sensitivity Analysis

A normalised finite-difference sensitivity analysis is run on all core parameters (a1, a2, sigma, r, K, ea, es, mb, m) — perturbing each by ±5% and measuring the resulting proportional change in total infections, to identify which parameters the outbreak size is most sensitive to.

Outputs / Visualisations
Combined and per-cluster epidemic curves (symptomatic vs. asymptomatic infections over time)
Side-by-side comparison plots of active infections under each intervention, at each level, per cluster
Bar charts comparing attack rate across all interventions and levels
Bar charts comparing R₀ reduction across interventions and levels
Results

Add a short summary once finalised, e.g. which intervention was most effective at reducing R₀ below 1, and the parameters the sensitivity analysis flagged as most influential.

(Drop in a screenshot or two of the epidemic curve / intervention comparison plots here, e.g. ![Epidemic Curve](images/epidemic_curve.png))

How to Run

Open the notebook and run cells top to bottom. All parameters are defined near the top of the notebook and can be adjusted to explore different outbreak scenarios.

Future Improvements
Fit parameters to real outbreak data rather than illustrative values
Extend to more than two clusters to model larger, multi-site camp networks
Add stochastic (agent-based or stochastic ODE) simulation to capture variability in small-population outbreaks
