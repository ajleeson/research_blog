## The perplexing early spring bloom is still perplexing

This week I spent some time investigating factors that could be contributing to the early spring bloom in the no-loading (natural) run. 
I created some depth vs. time property plots to visualize how state variables change in time throughout the water column. I also ran a simple experiment with the 1-D NPZD module. Despite these efforts, I still do not know why we see a larger and earlier spring bloom in the no-loading (natural) run. More details below.

---
## Depth vs. Time Property Plots

Last week, a puzzling question came up. Why are spring blooms starting earlier in the no-loading run compared to the with-loading run? (Fig. 1)

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/10fda08c-61c7-4964-80cd-1466ff3c19a2" width="450"/><br>Fig 1. Main basin surface Chl and bottom DO.</p><br>

The first hypothesis I wanted to test was: The with-loading run has a higher concentration of nutrients deeper in the water column. This causes the spring bloom in the with-loading run to start slightly below the surface. (Recall that the chlorophyll timeseries I showed last week were for *surface* chlorophyll concentrations).

To investigate this hypothesis, I have generated depth vs. time property plots so we can look at how different state variables evolve in time throughout the water column. The state variables shown below include density, NO3, NH4, DO, and Chl. I am looking at the same cast locations as last week, in Hood Canal, Holmes Harbor, and Main Basin (Fig. 2).

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/c7a1167e-2cc9-45f3-9f61-ab4b0f30ad46" width="350"/><br>Fig 2. Locations of mooring extractions.</p><br>

Figures 3 through 5 show the resulting depth vs. time property plots for Hood Canal, Holmes Harbor, and Main Basin, respectively. The first colum shows results from the no-loading (natural) run. The second column shows the difference between the with-loading and the no-loading run (anthropogenic minus natural).

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/c3da45e4-d766-4dbd-9615-288b5f4b868c" width="800"/><br>Fig 3. Hood Canal depth vs. time property plots.</p><br>

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/0a7b30fe-55d5-49d6-af05-6d29fafbe9b6" width="800"/><br>Fig 4. Holmes Harbor depth vs. time property plots.</p><br>

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/74ab42b8-3f5a-4fef-b8cd-52b4417e5a87" width="800"/><br>Fig 5. Main Basin depth vs. time property plots.</p><br>

At all three locations, after the spring bloom, we begin to see a sudden increase in NH4 and decrease in DO at depth. This obervation is indicative of higher respiration after the bloom. Good, this biogeochemistry makes sense to me.

However, at each location there does not appear a deeper phytoplankton bloom in the with-loading (anthropogenic runs), which disproves my hypothesis. The no-loading (natural) condition simply seems to have an earlier and larger phytoplankton bloom (depth does not seem to be an obvious factor).

## 1-D NPZD experiment

To follow-up on this puzzle, I took a dive into the rate equations for the NPZD module and ran an experiment with the 1-D NPZD module. Figure 6 show a timeseries of phytoplankton concentration. The different colored lines indicate different intial NO3 concentrations. As initial NO3 concentration increases, the peak phytoplankton concentration increases and the peak bloom occurs later.

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/4f7cae77-3be3-4f3b-b058-e6b507e64a45" width="500"/><br>Fig 6. Predicted phytoplankton concentration from 1-D NPZD module, with varied initial NO3 concentrations.</p><br>

These results might explain why the with-loading (anthropogenic) run has a later spring bloom than the no-loading (natural) run. HOWEVER, these results also suggest that the spring bloom in the with-loading (anthropogenic) run should be larger than the no-loading (natural) run, which is **not** the case. Thus, there must be some other factor besides NO3 concentrations that is influencing the resulting spring bloom dynamics.

I am already strongly convinced that the hydrodynamic behavior is the same between both model runs, so we can rule out different physical forcing mechanisms. Though, the presence of physical dynamics might still inherentely interact with the biogeochemistry. This interaction could be causing shifts in the timing and magnitude of blooms that can't be replicated easily in the 1-D NPZD module. More investigation remains for the future.