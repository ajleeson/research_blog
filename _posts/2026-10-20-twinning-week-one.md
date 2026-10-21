## First week of twinning

This past week I have been working on two tasks:
1. **Quick experiments with different BCs and advection schemes**
    - I do not have any results to show, because hyak has been having a rough week. However, Christie and I met to chat about how to run these experiments within the LO ecosystem, and how to run ROMS in general outside of the LO ecosystem. I thought this was a valuable modeling session, and we can look forward to some results next week.
2. **Compiling a list of required changes for a successful twin**
    - I have been going through all of the Fennel group's code (Laurent et al., 2025) and documenting all of the relevant differences between their code and ours. This effort is not complete yet, but I hope to have this wrapped up within the next couple of days so I can begin actual implementation. The sections below summarize some of the key differences I have identified, which will be important for us to implement.

---
## Boundary Conditions

Laurent et al. of the Fennel group did something clever with their BCs. They use Radiation-Nugding boundary conditions for nearly all of their state variables, which is standard and what LiveOcean does as well. However, they use Clamped boundary conditions for their delta TIC (dTIC) and delta total alkalinity (dTA) state variables. This likely prevents some of the weird boundary noise reflections we were seeing in our OAE tests.

---
## State Variables

Inevitably, Laurent et al uses a different state variable file than we do. Their file is customized to include OAE-relevant state variables, such as dTIC, dTA, and particulate alkalinity (since they inject particulate feedstock to their domain, which dissolve to form alkalinity). In contrast, we use the default ROMS state variable file, varinfo.yaml. I will need to create a new version of this file to include dTIC and dTA. 

---
## Diagnostic Outputs

In their header file, Laurent et al enabled DIAGNOSTICS_BIO, and to their diagnostic file they write air-sea CO2 flux and pCO2 for both the baseline and perturbation conditions. They write diagnostics once per day. 

Right now, LiveOcean does not output any diagnostics. However, it will be valuable to turn this feature on so we can get accurate estimates of air-sea CO2 flux. Previously, I had been calculating these fluxes using daily average values of wind speed, temperature, etc., which I suspect could introduce some error to our estimates. 

---
## Alkalinity Injection

Laurent et al injects alkalinity using a python script. This script generates a .nc file of alkalinity addition, which then gets read by their code as an input file. These inputs include information such as the amount of alkalinity added (dTA), horizontal position, depth of addition, time of addition, and feedstock type. Eventually, this file gets processed by their bgc module, by enabling a custom flag they called "TALK_ADDITION."

In contrast, I have been directly modifying his files. Previously, Jilian had also tried to add alkalinity as a surface flux via the atmospheric forcing files. My concern with both of these methods is that it was easy to forget how much alkalinity was added and at what time and where. Thus, I am intrigued by the explicit alk addition script that Laurent et al created, and may try to replicate their alk addition method.  

---
## The Actual Fortran Implementation

Lastly, there are some major revisions to the biogeochemical module, where the twinning (or, deltaing) actually takes place. This is the bit that I am still wrapping my head around.

I have been looking at a diff between our bgc module and Laurent et al's modified version. Figure 1 shows a snapshot of this diff. Everywhere there is code related to the carbonate cycle (i.e., code that modifies alkalinity, TIC, pH, pCO2), they have made an intervention. 

In the code snippet below, lines 1205-1213 are exactly the same in both bgc modules. However, the green lines of code highlight the additions that the Laurent et al. version contains. In this snippet, they introduce pHc and pCO2c, which are equivalent to the values of the base pH and pCO2. The "c" indicates that these are the control values. Then, they repeat the same block of code to obtain pHa and pCO2a, where the "a" indicates addition. They key difference is that instead of inputting TIC and TAlk as inputs to the pCO2_water_RZ function, they input TIC+dTIC and TAlk+dTA. This is how they carry their deltas through the code.

<p style="text-align:center;"><img src="/research_blog/figures/2026.10.20/example_twin.png" width="600"/><br>Fig 1. Example code diff between our bgc module (fennel.h) and the Laurent et al bgc module (reduced_bgc.h). Green highlights indicate code that is in the Laurent et al module, but not our module.</p><br>

For the next couple of days, I will continue interpreting and taking notes on their bgc module twin-variable implementation. Then, I will finally begin making our own interventions.

