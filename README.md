# MATLAB-Coding-Projects---Undergrad

This repository contains several MATLAB scripts and reports for process modelling, reactor dynamics, and unit design, completed as part of a chemical engineering curriculum.

---

## `Q1_Live_Script_PDF.pdf`: VLE & Flash Separation

### What This File Shows
This MATLAB Live Script performs two key vapour-liquid equilibrium (VLE) calculations:
1.  It determines the **Antoine Coefficients (A, B, C)** for 1,3-Propandiol by fitting raw experimental pressure-temperature data[cite: 2955, 2960].
2.  It models a **three-component flash evaporator** (1,3-Propandiol, Water, Glycerol) operating at 185°C and 2.75 bar. It calculates the vapour-feed ratio (Alpha) and the final mole fractions of the liquid and vapour streams.

### Key Findings
* **Antoine Coefficients (1,3-Propandiol):**
    * A = 4.743261 
    * B = 1948.631342 
    * C = -147.144282
* **Flash Evaporation:**
    * **Vapor-Feed Ratio (Alpha):** 99.15% [cite: 3027]
    * **Vapor Mole Fractions (y_i):**
        * y_PDO: 0.0085 (0.85%) [cite: 3038]
        * y_H2O: 0.9814 (98.14%) [cite: 3040]
        * y_Gly: 0.0101 (1.01%) [cite: 3042]

### MATLAB Methodology
* **Coefficient Calculation:** The script uses the `nlinfit` (non-linear regression) function to fit the raw `Tval` and `Pval` data to a custom Antoine equation function (`Q1AFunct`)[cite: 2955, 2960, 3044].
* **Flash Calculation:**
    1.  Calculates saturation pressures (`P_sat`) for all components using their respective Antoine coefficients[cite: 3009, 3010].
    2.  Determines the V/L equilibrium ratio (`k_i`) for each component using Raoult's Law ($k_i = P_{sat,i} / P_{total}$) [cite: 3012-3014].
    3.  Uses the `fzero` function to find the root of the **Rachford-Rice equation** (defined in the script as `x_total`), which solves for the unknown vapour-feed ratio, `Alpha`[cite: 3020, 3022].
    4.  Calculates the final liquid (`x_i`) and vapour (`y_i`) mole fractions using the solved `Alpha` [cite: 3028-3042].

---

## `Q2_Live_Script_PDF.pdf`: Batch Reactor Dynamics

### What This File Shows
This MATLAB Live Script models the dynamic behaviour (concentration, temperature, and pressure) of a batch reactor under two different scenarios:
* **Scenario A:** A "runaway" case with a **malfunctioning cooling system** (`UA = 0`)[cite: 162, 219].
* **Scenario B:** A "safe" case with a **fully functioning cooling system** (`UA = 2.77e+6`)[cite: 327, 339].

The model simulates a primary reaction and a secondary, high-activation-energy decomposition reaction (of Diglyme, `CS`) that produces Hydrogen gas (`CD`), leading to the pressure increase[cite: 274, 471].

### Key Findings
* **Scenario A (Malfunctioning):**
    * The reactor experiences a **thermal runaway**, hitting the critical pressure (45 atm) [cite: 211] [cite_start]at approximately **3.4 hours** (204 minutes)[cite: 272, 297, 324, 508, 510].
    * At this time, temperature spikes from ~490 K to **540 K** [cite: 281, 282][cite_start], and pressure spikes from 4.4 atm to **45 atm**[cite: 306, 316].
    * This spike corresponds to the rapid decomposition of the solvent `CS` (Diglyme) and production of `CD` (Hydrogen)[cite: 274, 275].
* **Scenario B (Functioning):**
    * The cooling system successfully removes the reaction heat.
    * Temperature peaks safely at ~467 K before settling[cite: 384, 385].
    * Pressure remains stable at the initial **4.4 atm** for the entire process[cite: 415, 431].
    * The runaway decomposition of `CS` is prevented [cite: 357-367].

### MATLAB Methodology
* The script uses the `ode15s` solver to solve a system of stiff Ordinary Differential Equations (ODEs) defined in the function `t_explsn`[cite: 238, 341, 463].
* This function contains the mole balances (`dCAdt`, `dCBdt`, `dCSdt`) [cite: 473-476] [cite_start]and the energy balance (`dTdt`) [cite: 478-482] for the reactor.
* The `UA` value is set as a `global` variable, allowing it to be changed from 0 (Scenario A) to 2.77e+6 (Scenario B) to model the two different cases[cite: 204, 219, 328, 339].
* The ODE function includes `if` statements to model complex process behaviours, such as the vent flowrate (`Fvent`) changing based on pressure [cite: 486-492] and the energy balance equation changing at `T >= 455 K`[cite: 478].
* An `if` statement within the ODE function (`if T >= T_crit || P >= P_crit`) [cite: 504] stops the calculation by setting derivatives to zero (`dydt = 0`) [cite: 505] and uses `fprintf` to output the exact time of the explosion[cite: 510].

---

## `CE30240 - Advanced Modelling Coursework.pdf`: Reactor Design Comparison

### What This File Shows
This is a formal engineering report that presents a conceptual design for a new, single-step process to manufacture **40,000 tonnes/year of Acrylic Acid (AA)**[cite: 3098]. It compares two potential reactor types:
1.  **Fluidised Bed Reactor (FBR)** [cite: 3100]
2.  **Packed Bed Reactor (PBR)** [cite: 3100]

The report details the design calculations, operating conditions, and final dimensions for both reactors and provides a final recommendation.

### Key Findings
* **Recommendation:** The report concludes that the **Packed Bed Reactor (PBR) is the more appropriate choice**[cite: 3070, 3358].
* **Production:** The PBR produces **45,300 tonnes/year** of AA, exceeding the target and the FBR's 41,500 tonnes/year[cite: 3073, 3074, 3219].
  **Yield:** The PBR achieves a higher AA yield of **88.5%**, compared to the FBR's 80.0%[cite: 3075, 3219].
* **Conversion:** Both reactors meet the >85% conversion requirement for propylene (PBR: 88.7%, FBR: 90.0%)[cite: 3076, 3219].
* **Dimensions:** The PBR is more compact (Height: 6m, Diameter: 3.327m) than the FBR (Height: 7m, Diameter: 4.133m), making it better for retrofitting existing plants[cite: 3071, 3254, 3360].
* **Trade-off:** The PBR's main disadvantage is requiring significantly more cooling tubes (**9,429**) than the FBR (**1,862**) to achieve the necessary heat transfer[cite: 3078, 3254].
