# NO-LEACH-VS-LEACH-VS-LEACH-WITH-REMAINING-ENERGY


M.Tech ACN Lab Project — Enhanced Comparative Study of Three Wireless Sensor Network Routing Protocols using Python simulation.


**Table of Contents**

1. Overview
2. Protocols Compared
3. Simulation Parameters
4. Results Summary
5. Key Findings
6. Figures & Plots
7. Project Structure
8. Getting Started
9. References


**Overview**
This project simulates and compares three wireless sensor network (WSN) routing protocols across a 100×100 m field with 100 sensor nodes and a base station placed outside the field at (50, 175) m. All three protocols use identical node deployments (fixed random seed) to ensure a fair comparison.
Performance is evaluated using three standard lifetime metrics:
MetricMeaningFNDFirst Node Dead — when robustness begins to degradeHNDHalf Nodes Dead — useful data coverage thresholdLNDLast Node Dead — total network lifetime

**Protocols Compared**
1. No-LEACH — Direct Transmission
Every alive node transmits directly to the Base Station every round. No clustering, no load balancing. Serves as the baseline.

Energy per round: E_Tx(k, d(node → BS))


2. LEACH — Low Energy Adaptive Clustering Hierarchy

Heinzelman et al., HICSS-33, 2000

Probabilistic Cluster Head (CH) election using a fixed desired fraction p = 0.1. Nodes that haven't been CH in the last 1/p rounds are eligible for election each round.
Threshold formula:
T(n) = p / (1 - p × (r mod 1/p))   if node n ∈ G
     = 0                             otherwise

3.  E-LEACH — Energy-Aware LEACH (Enhanced)
Extends LEACH by making each node's election probability dynamic — proportional to its remaining energy relative to the initial energy.
Dynamic probability:
P_i(r) = P_base × (E_i(r) / E_initial)
High-energy nodes get a higher P_i and are more likely to be elected CH. Low-energy nodes are naturally protected from the expensive CH role.

Simulation Parameters
ParameterValueNumber of Nodes (N)100Field Size100 × 100 mBase Station Location(50, 175) mInitial Energy (E₀)0.5 JElectronics Energy (E_elec)50 nJ/bitAmplifier Energy (ε_amp)100 pJ/bit/m²Data Aggregation Energy (E_DA)5 nJ/bit/signalPacket Size4000 bitsDesired CH Fraction (p)0.1Random Seed42 (same for all protocols)Max Rounds5000

Results Summary
Network Lifetime Metrics
ProtocolFNDHNDLNDFND Gain vs No-LEACHNo-LEACH40691911.00×LEACH1052463982.62×E-LEACH2803904447.00×
Protocol-to-Protocol Improvements
ComparisonFND GainLND GainLEACH over No-LEACH2.62×2.08×E-LEACH over LEACH2.67×1.12×E-LEACH over No-LEACH7.00×2.32×

Cluster Head Election Analysis
MetricLEACHE-LEACHTotal CH elections25921786Avg per node25.9217.86Std deviation8.783.60 
Avg energy at CH election253.3 mJ319.6 mJ 
Min energy at CH election0.033 mJ 
0.145 mJAvg cluster size (members)9.020.7Nodes never elected00

**E-LEACH's lower std deviation confirms better load balancing. Its higher avg energy at election confirms the energy-aware policy is working.**


**Key Findings**

E-LEACH extends FND by 7× over No-LEACH — dramatically delays the first node failure, preserving network coverage longer.
E-LEACH's dynamic P_i protects low-energy nodes — nodes with drained batteries get near-zero election probability, preventing premature death from repeated CH duty.
LEACH's random election can elect near-dead nodes — the minimum energy at CH election was just 0.033 mJ, versus 0.145 mJ in E-LEACH, confirming LEACH's energy-blind weakness.
E-LEACH achieves more uniform CH rotation — std dev of 3.60 vs 8.78 for LEACH means energy load is spread more evenly across nodes.
No-LEACH collapses rapidly — with all nodes transmitting directly to a distant BS, energy drains exponentially and the last node dies at round 191, less than half of LEACH's lifetime.
CH role is significantly more expensive than member role — confirmed by Fig 10's log-scale comparison; the d² amplifier model makes long-range BS transmission the dominant energy cost.
More CH elections → earlier death — Fig 14 shows a negative slope in both LEACH and E-LEACH, directly confirming that heavy CH rotation drains nodes faster.


**Figures & Plots**
FigureDescriptionFig 1Initial Deployment Map — 100 nodes in 100×100 m fieldFig 2Alive Nodes vs. Simulation Round (all 3 protocols)Fig 3Non-Linear Total Network Energy DecayFig 4FND / HND / LND Grouped Bar ChartFig 5CH Election Count per Node (LEACH vs E-LEACH)Fig 6Remaining Energy at Each CH Election Event (scatter)Fig 7Distribution Histogram of Energy at CH ElectionFig 8Average Cluster Size (Members per CH) over RoundsFig 9Average Intra-Cluster Distance (Member → CH) over RoundsFig 10CH Role vs Member Role Energy Cost (log scale)Fig 11CH Count per Round — LEACH vs E-LEACHFig 12E-LEACH: Dynamic P_i Decay over Simulation RoundsFig 13Node Death Round Distribution (graceful vs sudden degradation)Fig 14CH Election Count vs Node Death RoundFig 15Cluster Size Distribution Box PlotFig 16Four-Panel Summary Dashboard

**Getting Started**
Prerequisites
bashpip install numpy matplotlib
Run the Simulation
bashpython leach_simulation.py
All 16 figures are saved automatically as .png files in the working directory.
Modify Parameters
Key parameters are defined at the top of the script under SECTION 1 — GLOBAL PARAMETERS:
pythonNUM_NODES   = 100       # Number of sensor nodes
FIELD_X     = 100       # Field width (m)
FIELD_Y     = 100       # Field height (m)
BS_X, BS_Y  = 50, 175   # Base station coordinates
INIT_ENERGY = 0.5       # Initial energy per node (J)
P_LEACH     = 0.1       # Desired CH fraction
MAX_ROUNDS  = 5000      # Maximum simulation rounds

**References**

Heinzelman, W. R., Chandrakasan, A., & Balakrishnan, H. (2000). Energy-efficient communication protocol for wireless microsensor networks. HICSS-33. DOI: 10.1109/HICSS.2000.926982
Heinzelman, W. B., Chandrakasan, A. P., & Balakrishnan, H. (2002). An application-specific protocol architecture for wireless microsensor networks. IEEE Transactions on Wireless Communications, 1(4), 660–670.
Smaragdakis, G., Matta, I., & Bestavros, A. (2004). SEP: A stable election protocol for clustered heterogeneous wireless sensor networks. SANPA Workshop.


License
This project is submitted as part of the M.Tech Advanced Computer Networks (ACN) Lab curriculum. Feel free to use it for academic and educational purposes with attribution.
