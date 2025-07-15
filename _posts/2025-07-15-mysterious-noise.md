## Mysterious Noise

This week I ran a few more experiments comparing the NH4 fields between a run with West Point to a run without West Point. Note that in these experiments, one time step corresponds to one minute of model time.

Last week I discovered that the biogeochemistry noise does not obviously appear in the first 10 time steps. I ran the model for 10 timesteps, and I had it write a history file every timestep.

To continue this effort, I ran many more simulations using longer write intervals and allowing the simulation to run further in time. After several tests, I found that I could not reliably detect the noise until I ran for one day (writing history files every hour). The videos below show how the NH4 field evolves over the course of one day.

<p style="text-align:center;"><video src="/research_blog/figures/2025.07.14/pcolormesh.mp4" controls="controls" style="max-width: 800px;"></video><br>Fig. 1 Comparison of NH4 concentration after one day of run time between the two runs (West Point minus no-West Point). The left panel shows surface differences. The right panel shows bottom differences. The pink circle denotes the location of West Point WWTP.</p>
<br>

<p style="text-align:center;"><video src="/research_blog/figures/2025.07.14/binary.mp4" controls="controls" style="max-width: 800px;"></video><br>Fig. 2 Locations where NH4 differs between a model run with West Point and a model run without West Point. Black indicates that there are differences, and blue indicates that there are no differences. The pink circle denotes the location of West Point WWTP.</p>
<br>

Based on all of my experiments, I do not think we have evidence to suggest that the biogeochemistry noise starts at the source (West Point) and propagates away each time step. More so, it seems that anomalies appear spontaneously in the surface NH4 field in Sinclair Inlet.

Let's discuss how to proceed and how to manage timelines for running the Chapter 2 experiments.