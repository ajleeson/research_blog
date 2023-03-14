## Looking at LwSrc in Idealized Runs

First order of business: Happy Pi Day!

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/225079159-077848bc-28fe-45b2-a70a-06f540738ee6.jpg" width="200"/><br>Does a tart count as a pie?</p><br>

This week I tested out a couple of different combinations of mixing coefficients and advectoin schemes, but still nothing seems to improve conditions at Birch Bay WWTP.

I then focused my attention on the idealized estuary. I re-ran the model for one day with point sources discharging at a rate of 1 m3/s (rather than 1000 m3/s). I also disabled LtracerSrc so that the inflowing water enters at the same temperature and salinity as the water already in the grid cell. The results are not as extreme as what we observe at point sources in LiveOcean, however there are still interesting vertical velocity patterns in the water column. More details below.

---

## Mixing Coefficients and Advection Schemes in LiveOcean

I tested two different combinations of mixing coefficients and advections chemes. In both cases, ROMS still blew up at Birch Bay WWTP.

First, I used MPDATA (pre-existing LiveOcean advection scheme) with TNU2 = 5.0 m2/s (lateral, harmonic, mixing coefficient). Figure 1 shows the surface velocities near Birch Bay WWTP prior to blow up.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/225081841-77dbaac9-5fde-4a7a-81cc-889ec0d3e20d.png" width="750"/><br>Fig 1. Surface velocities near Birch Bay WWTP prior to model blow up. Advection scheme: MPDATA; TNU2 = 5.0 m2/s</p><br>

Per John Wilkin's recommendation, I then tested the Akima 4th order advection scheme (A4) with TNU2 = 5.0 m2/s. Unfortunately this combination still did not resolve the issue. Figure 2 shows the surface velocities near Birch Bay WWTP prior to blow up.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/225081837-dd728eca-683a-4cbe-a2a7-6f8bfc352e4d.png" width="750"/><br>Fig 2. Surface velocities near Birch Bay WWTP prior to model blow up. Advection scheme: A4; TNU2 = 5.0 m2/s</p><br>

The surface velocities may appear to look more extreme in the A4 case, but note that the A4 run ran for an hour longer than the MPDATA run. Thus, the A4 surface velocity snapshots were taken one hour later than the MPDATA snapshot.

---
## LwSrc in Idealized Estuary

Given the still ongoing issue with vertical sources in LiveOcean, I have shifted my attention back to the idealized estuary.

Last week I shared results from Summer 2022 in which I had run an idealized estuary with point sources discharging 1000 m3/s of freswhater. This is a huge flowrate, and the freshwater may also have created buoyancy effects that obscured the direct influence of vertical sources on the surrounding water.

Thus, I re-ran the idealized estuary for one day with point sources discharging with 1 m3/s distributed across all sigma layers. I disabled LtracerSrc*, so the inflowing water came in at the same temperature and salinity as the water that is already in the grid cell. Thus, we should not be conflating buoyancy effects with vertical source effects.

*Note: I also ran LiveOcean with LtracerSrc disabled, but it still blew up.

As a reminder, the bathymetry of the idealized estuary is provided in Figure 3.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/225081827-f591d8e1-4fe6-4df2-a3c1-2964ec84d9c8.png" width="450"/><br>Fig 3. Idealized estuary bathymetry. Colorbar shows depth in meters.</p><br>

An hourly video of surface velocities and SSH anomaly is shown in Figure 4 below. Note that SSH anomaly is the difference between a grid cell's SSH and the average SSH over the full domain at a given timestep.

<video src="https://user-images.githubusercontent.com/15829099/225086143-fb119226-d446-45ef-8029-bb62e2caac73.mp4" controls="controls" style="max-width: 800px;"></video><br>Fig. 4 Hourly movie of surface velocities and SSH anomaly in the idealized estuary.
<br>

There are some weird wave effects in the beginning when the tides start to flood. Aside from this, I am not observing anything strange on the surface. The point sources don't appear to have a huge effect on the surrounding water.

The video below shows the vertical velocity along a transect that crosses over WWTP4.

<video src="https://user-images.githubusercontent.com/15829099/225086162-796c0fbb-4f57-46f8-93dc-49d659af2d8d.mp4" controls="controls" style="max-width: 800px;"></video><br>Fig. 5 Hourly movie of vertical velocity (w) over a transect that overlaps with WWTP4 (the open ocean vertical source). Note that the dark triangle denotes the location of the treatment plant in the section.
<br>

The vertical velocity at the point source appears to oscillate between positive and negative at a similar frequency as the tides. However, w at the source has a larger magnitude than w elsewhere, and oscillations of w at the source appear to lag behind w elsewhere.

The time lag is what I find fascinating. At the vertical source, we may see a negative w, but everywhere else we may observe a positive w. I would expect these velocity patterns to create a large horizontal shear in w. **Could large shear be the start of an instability?**

The video below shows a section plot of the N-S velocity. Again, what stands out is that v at the vertical source lags compared to the tide.

<video src="https://user-images.githubusercontent.com/15829099/225110068-6ce09c68-35b1-41ce-b7dd-12446b5a8037.mp4" controls="controls" style="max-width: 800px;"></video><br>Fig. 6 Hourly movie of N-S velocity (v) over a transect that overlaps with WWTP4 (the open ocean vertical source). Note that the dark triangle denotes the location of the treatment plant in the section.
<br>

Figure 7 below shows a depth vs. time profile of velocity at the open ocean point source (WWTP4).

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/225110437-d531fab2-4f6c-491a-bf96-0984a68bec39.png" width="600"/><br>Fig 7. Depth vs. time profile of velocity at WWTP4.</p><br>

Generally, velocity appears to vary with a tidal frequency.
I would be interested in whether the magnitude of these velocities evolves over time.