# Modelling-Waterborne-Cholera-Transmission-within-Refugee-Camps
A compartmental epidemiological model, built with Python/SciPy, simulating the spread of cholera between two connected refugee camp clusters sharing a water source, and evaluating the impact of public health interventions on outbreak severity.

##Toolkit
- **NumPy** — numerical computation
- **SciPy (odeint)** — solving the system of ordinary differential equations (ODEs)
- **Matplotlib** — epidemic curves and intervention comparison plots
- **Google Colab** — development environment

## The Model
The model extends a standard SIR framework with an environmental (waterborne) transmission route and two coupled sub-populations, reflecting how cholera spreads in refugee camp settings where clusters may share contaminated water sources.

Two clusters, each with its own set of compartments:

- **S** — Susceptible
- **Ia** — Infected, asymptomatic
- **Is** — Infected, symptomatic
- **R** — Recovered
- **B** — Bacterial concentration in the local water reservoir

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
- Mass conservation — total population per cluster (S + Ia + Is + R) remains constant over time.
- Non-negativity — all compartments stay ≥ 0 throughout the simulation.

## Outbreak Metrics
For each cluster, the model computes:

- Attack rate — % of the population ultimately infected
- Peak infections — maximum simultaneous infected count, and the day it occurs
- Peak bacterial concentration in the water reservoir, and when it occurs
- Outbreak duration — time the epidemic stays above a defined active-case threshold

## Interventions Modelled
Each intervention is simulated at Baseline / Low / Moderate / High levels, and compared against the no-intervention baseline on attack rate, peak infections, and R₀:

- Chlorination — added water treatment term that increases bacterial decay
- Improved latrine coverage — reduces bacterial shedding into water
- Borehole/water-source suspension — reduces the cross-cluster mixing rate (m), modelling isolating a shared contaminated water source
- Vaccination — reduces the initial susceptible population

## Sensitivity Analysis
A normalised finite-difference sensitivity analysis is run on all core parameters (a1, a2, sigma, r, K, ea, es, mb, m) — perturbing each by ±5% and measuring the resulting proportional change in total infections, to identify which parameters the outbreak size is most sensitive to.

## Outputs / Visualisations
- **Combined and per-cluster epidemic curves (symptomatic vs. asymptomatic infections over time)**

<img width="1160" height="973" alt="image" src="https://github.com/user-attachments/assets/4e8b15c9-785f-4db9-a5bc-dc740a33b303" />

<img width="1389" height="989" alt="image" src="https://github.com/user-attachments/assets/91d6b645-88db-40d0-b96e-f2d72da0fcf3" />


- **Side-by-side comparison plots of active infections under each intervention, at each level, per cluster**
  
<img width="1189" height="1590" alt="image" src="https://github.com/user-attachments/assets/3eb3d139-360b-4de7-b944-bf1949bae843" />


- **Bar charts comparing attack rate across all interventions and levels**
  
<img width="1389" height="495" alt="image" src="https://github.com/user-attachments/assets/97f3a00a-1976-4781-b6c0-0fc100137bb1" />


- **Bar charts comparing R₀ reduction across interventions and levels**
  
<img width="1389" height="593" alt="image" src="https://github.com/user-attachments/assets/0908cdd9-4098-4af6-b46d-1291903a1bbc" />


## Results
Clusters 1 and 2 respond differently to interventions, consistent with theoretical expectations given their different exposure rates. Because cluster 1 (the higher-risk cluster) is unaffected by borehole suspension and its outbreak trajectory instead drives spillover into cluster 2, directly controlling transmission in the higher-risk subcamp — rather than isolating the shared water source alone — appears more effective at reducing transmission camp-wide.

Across all intervention types, increasing coverage consistently reduces both attack rate and R₀. At high coverage, latrine improvements reduce attack rates in both clusters to below the ~0.5% (1,777 cases across a 348,781 population) attack rate observed in the 2015 Dadaab camp cholera outbreak (Centers for Disease Control and Prevention, 2018) — suggesting the model's predicted intervention effects are of a plausible, real-world magnitude.

## How to Run
Open the notebook and run cells top to bottom. All parameters are defined near the top of the notebook and can be adjusted to explore different outbreak scenarios.

## Future Improvements
- Extend to more than two clusters to model larger, multi-site camp networks
- Add stochastic (agent-based or stochastic ODE) simulation to capture variability in small-population outbreaks
