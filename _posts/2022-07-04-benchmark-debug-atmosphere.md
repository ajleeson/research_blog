
## Benchmark, Debugging, Atmosphere, and Nutrients

---
### Benchmark

This week I tested the run speed on klone with different cores using the idealized estuary model that I have been running. The results are shown below.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/177211951-884f7e6a-b6ef-4e3a-9f06-e546d6893d4f.png" alt="benchmark-results" width="475"/></p>

Previously, I had been running with 40 cores. Using 80 cores reduces the run time for each day from 8:40 to 5:40. Moving forward, I will run the idealized estuary model with 80 cores.

---
### Debugging

This week I also shortened the idealized estuary grid and attempted to run the model for a longer period of time.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/177211507-b6f1b1f1-d0ac-4fcb-adf0-62fca8b6b3b4.png" alt="shortened-grid" width="400"/></p>

I grew a lot as a modeler while working on this new grid (a.k.a. I caught a number of errors I had made previously)

1. Open boundary conditions were defined at the wrong edges
2. Ocean forcing file was setting the estuary to be half fresh at the start of each day, instead of solely the start of the model run
3. Initial conditions for ocean forcing set part of the shelf to fresh, instead of just the estuary to be fresh (image below)

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/177213623-8c77c69c-3a6c-4b24-8729-74e3de377a41.png" alt="bad-ocean-forcing" width="275"/>
<img src="https://user-images.githubusercontent.com/15829099/177213628-829b8c04-c921-43df-94ef-d7822ab84dd6.png" alt="good-ocean-forcing" width="275"/></p>

*Solutions to these issues*

1. Double-check forcing list before running the model.
2. I added an if statement in the forcing file to check if the date is the first day of the model run. If so, I define the estuary to be half fresh. If not, the ocean water is salty everywhere.
3. Previously, I had set the switchover latitude value by multiplying the minimum latitude value by a scale. This became a problem once I shortened the estuary. Instead, I can now input the exact latitude value at which I want the ocean forcing to switch from salty to fresh water.

---
### Atmospheric Forcing I: BULK_FLUXES

The TL;DR version is that I was unable to run atmospheric forcing using the BULK_FLUXES option. I've instead tried to add wind by simply defining surface stress input variables. This method appears to work so far.

For the last 2 weeks I have been attempting to run atmospheric forcing using BULK_FLUXES. However, the model kept blowing up despite many debugging attempts. For the most part, I ran with constant (spatially and temporally) forcing inputs.:

| Variable | Description | Value |
|---|---|---|
| Pair | air pressure | 1013.25 mbar |
|rain|rainfall rate| 0 kg m2 s-1|
|lwrad_down|downwelling longwave radiation| 300 W m-2|
|Tair| air temperature| 10 C|
|Qair| relative humidity| 83%|
|Uwind*|E-W wind speed| 0 m s-1|
|Vwind| N-S wind speed| 0 m s-1|
|EminusP|evaporation minus precipitation| 0  m s-1 |

  *In prior runs, I had set Uwind = 6 m s-1 for all time everywhere. As part of the debugging procedure, I later set wind speed to be zero everywhere.

Shortwave radiation was defined hourly using the curve shown below. This curve should map to sunrise at 7 am PT and sunset at 7 pm PT.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/176066486-a95295d8-28bc-41f0-8df8-e61770a1749c.png" alt="swrad" width="400"/></p>

**Tricky Points with BULK_FLUXES**

Here's a growing list of tricky issues I've encountered while trying to use BULK_FLUXES:

- When I define BULK_FLUXES, I need to undefine analytical surface fluxes such as ANA_SSFLUX, ANA_SMFLUX, and AND_STFLUX.
- The model would not run until I also defined an EminusP variable, the net flux of fresh water from the ocean
- I need to be careful of the expected dimensions of the forcing variables. For instance, air temperature is defined at the surface and is thus a 2D array. However, ocean temperature is defined in 3D. It's also important to pay attention to whether the variables are defined at the rho points or u/v points. Double check the units that ROMS is expecting as well. All of this information can be found in varinfo.yaml
- When I defined atmospheric forcing, I also needed to modify BLANK.in to accept atmospheric forcing. The way this manifested was to tell BLANK.in to look for .nc files that contain the forcing information.
- BULk_FLUXES is very sensitive and can blow up very easily!

---
### Atmospheric Forcing 2: Defining Surface Fluxes

To make progress, I decided to simplify atmospheric forcing by simply defining surface stress. In this case, I am not using BULK_FLUXES or any other atmospheric option (such as LONGWAVE_OUT). Instead, I have enabled analytical surface flux options in the ROMS header file. In my atmospheric forcing file, I have defined only two input variables, sustr and svstr. Both variables are constant everywhere for all time:

| Variable | Description | Value |
|---|---|---|
| sustr | E-W surface stress | 0.1 N m-2 |
|svstr|N-S surface stress| 0 N m-2|

This model did run successfully for two days without blowing up. Two days is not enough to produce meaningful results, but it gives me confidence that this method will work.

Here's a snapshot after running for two days:
<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/177213176-3c60d5dd-a645-4178-86d5-a620e0e86f61.png" alt="two-day-wistress" width="650"/></p>

It looks like the model is doing something. Whether it's doing what I would expect, I'm not sure. There is more model running and reading to be done.

Later on, I will still also need to add sunlight to the model.

 <span style="color:red">
07/05/2022 Update: It turns out that this model run was not reading the surface stress inputs.
</span>

---
### Nutrients

This week I have also spent some time reading through Ecology's nutrient load summary for Puget Sound.

One major questions I have is:

How should we decide which nutrient loads to use as inputs to the model? Is this a decision that will be unique to LiveOcean? Or is there instead some coordination and standardization we should agree to with other modeling groups?
