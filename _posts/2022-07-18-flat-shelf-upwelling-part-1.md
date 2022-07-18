

## Flat Shelf Upwelling Experiment (Part 1)

This week I started running upwelling experiments using a flat-shelfed grid. As a reminder, the motivation for this experiment is to compare numerical simulations of wind-induced upwelling to an analytical solution. <br>
This experiment is still a work in progress. Currently, the model is mostly set up, and I've run a trial run. However, the experimental conditions need some refinement, and I have yet to set up a comparison to analytical solutions. More details in the "Preliminary Results" section below.
<br>

---
## Model Set Up
<br>

For this experiment, I have switched to a simpler flat-shelfed grid (Figure 1). The shelf is perfectly flat with a depth of 100 m, and a vertical wall at the coastline.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/179569924-35eafbb8-f208-48a7-8d8d-0ed5edba089e.png" width="600"/><br>Fig 1. Flat-shelfed grid.</p><br><br>

To simplify this experiment, I have only included ocean and atmospheric forcing. In other words, I have removed tidal and river forcing. This step was somewhat tricky. To remove tidal forcing I needed to:

- In ROMS header file, undefine SSH_TIDES and UV_TIDES then recompile ROMS
  - SSH_TIDES allows user to impose tidal elevantion
  - UV_TIDES allows user to impose tidal currents
- In BLANK.in, comment out line that lists .nc file for TIDENAME
  - TIDENAME tells ROMS where to find input file for tidal forcing

To remove river forcing I needed to:
- In BLANK.in, turn off LuvSrc and LtracerSrc
  - LuvSrc is the logical switch that activates horizontal momentum transport and mass point sources/sinks
  - LtracerSrc is the logical switch that activates tracer point sources/sinks
- In BLANK.in, comment out line that lists .nc file for SSFNAME
  - SSFNAME tells ROMS where to find input file for source/sink forcing

<br>

### Atmospheric Forcing

Atmospheric forcing is almost identical to what was used in the prior blog post, "Wind Experiment using BULK_FLUXES." For convenience, I've copied and pasted the same information below. One difference is that I did not provide an EminusP (surface freshwater flux) input for this experiment.

| Variable | Description | Value |
|---|---|---|
| Pair | air pressure | 1013.25 mbar |
|rain|rainfall rate| 0 kg m2 s-1|
|lwrad_down|downwelling longwave radiation| 365 W m-2|
|Tair| air temperature| 10 C|
|Qair| relative humidity| 83%|
|Vwind| N-S wind speed| 0 m s-1|
|Uwind*|E-W wind speed| 6 m s-1|

Shortwave radiation was defined hourly using the curve shown in Figure 2. The curve corresponds to sunrise at 7 am PT and sunset at 7 pm PT.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/176066486-a95295d8-28bc-41f0-8df8-e61770a1749c.png" width="400"/><br>Fig 2. Daily shortwave radiation input.</p>

Uwind is the E-W wind velocity. Ideally, I would vary this between different experiment scenarios. However, I only ran one scenario this week using a 6 m/s eastward wind.
<br>

### Ocean Forcing

The ocean input variables used for this experiment are listed below.

|Variable|Description|Value|
|---|---|---|
|salt|sea water practical salinity|30 g kg-1|
|temp*|sea water temperature|two layer, 10 C and 5 C|
|zeta|free surface height|0 m|
|ubar|x-dir barotropic velocity|0 m s-1|
|vbar|y-dir barotropic velocity|0 m s-1|
|u|x-dir sea water velocity|0 m s-1|
|v|y-dir sea water velocity|0 m s-1|

I created a two-layer stratified ocean using two different temperatures. Figure 3 shows the temperature profile imposed. In hindsigt, I wish I had made the ocean both temperature and salinity stratified. This can be an improvement for the next attempt.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/179575593-e56f3807-c36e-4dd8-a688-07f456aed346.png" width="400"/><br>Fig 3. Initial ocean temperature profile.</p>


### Boundary Conditions

For open boundary conditions, I used the conditions discussed in the blog post "Open Boundary Conditions for Atmospheric Forcing." In brief, velocity and salinity used RadNud, but temperature used Gra. I've also turned off temperature processing of, and nudging to, climatology. This is necessary to prevent cold water outside of the domain (and thus not subject to heating from the atmosphere) from flowing into the experimental domain.

<br>

---
## Preliminary Results <br>

As a trial experiment, I ran the model with a 6 m/s eastward wind for one month. A video of the daily surface salinity and temperature profile is shown below.

<video src="https://user-images.githubusercontent.com/15829099/179579016-02f9fa1d-7ef1-452d-9af6-d5388175e512.mp4" controls="controls" style="max-width: 1200px;"><br>Fig. 4 One month daily surface salinity (left) and temperature (right).
</video>

Some observations:
- There does appear to be upwelling. Colder water seems to surface along the coast.
- Cold water that has risen to the surface appears to heat up over time (from radiation, or mixing with warmer surface water, or migrating westward?)
- Warmer surface water appears to be moving towards the east, while the colder deep water is moving to the west. I am suspicious of my boundary conditions. In particular, there is no wind outside of the domain, so there may not be transport of warmer surface water flowing into the domain from the west.
- The surface slainity isn't very interesting since the ocean forcing has uniform salinity.

What I'd like to accomplish in the upcoming week is to refine this experiment by adding some salinity stratification. I may change the boundary conditions as well to prevent colder deep water from pooling in the west. Then I can run a with wind and without wind scenario and compare the vertical velocity profiles to analytical solutions.

I also wonder if the most interesting upwelling physics is happening during the first few days of the model run. A month-long simulation may be too long. I've included an hourly video of the first five days below.

<video src="https://user-images.githubusercontent.com/15829099/179580674-dba2b062-9185-46f2-9a59-7d44048ce8db.mp4" controls="controls" style="max-width: 1200px;"><br>Fig. 5 Five day hourly surface salinity (left) and temperature (right).
</video>

Observations:
- Over the first five days, I see more and more cold water appear along the coastline. Upwelling is much easier to observe.
  - Maybe I am able to compare results from the first five days to an analytical solution. 
- The surface temperature warms and cools daily.
- Towards the end of this video I start to observe the boundary effect where cool water pools on the western edge, and warmer water pools on the easter edge.
