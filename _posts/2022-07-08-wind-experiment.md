
## Wind Experiment using BULK_FLUXES

This week I finally had success running the idealized estuary model using the BULK_FLUXES option to create an atmosphere.

This experiment uses the shortened parabolic estuary grid described in the previous blog post and shown below. 

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/177211507-b6f1b1f1-d0ac-4fcb-adf0-62fca8b6b3b4.png" alt="shortened-grid" width="400"/></p>

I ran this idealized estuary model for 1.5 months (01/01/2020 - 02/15/2020) with three different along-shore wind scenarios. In each scenario, the along-shore wind speed is spatially and temporally constant, but the value of the constant is different between scenarios.

| Scenario | E-W Wind Speed (m s-1) |
|---|---|
|No Wind Baseline|Uwind = 0|
|Eastward Wind| Uwind = 6|
|Westward Wind| Uwind = -6|

---
### Other Forcing

**Atmospheric Forcing**

Most of the atmospheric forcing inputs are spatially and temporally constant. These input values are documented in the following table.

| Variable | Description | Value |
|---|---|---|
| Pair | air pressure | 1013.25 mbar |
|rain|rainfall rate| 0 kg m2 s-1|
|lwrad_down|downwelling longwave radiation| 365 W m-2|
|Tair| air temperature| 10 C|
|Qair| relative humidity| 83%|
|Vwind| N-S wind speed| 0 m s-1|
|EminusP|evaporation minus precipitation| 0  m s-1 |

Shortwave radiation was defined hourly using the curve shown below. The curve corresponds to sunrise at 7 am PT and sunset at 7 pm PT.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/176066486-a95295d8-28bc-41f0-8df8-e61770a1749c.png" alt="swrad" width="400"/></p>

The E-W wind speed (Uwind) is the only input variable that differed between the scenario runs.

**Ocean Forcing**

The ocean input variables are constant everywhere for all time, with the exception of the initial salinity condition.

|Variable|Description|Value|
|---|---|---|
|salt*|sea water practical salinity|30 g kg-1|
|temp|sea water temperature|10 C|
|zeta|free surface height|0 m|
|ubar|x-dir barotropic velocity|0 m s-1|
|vbar|y-dir barotropic velocity|0 m s-1|
|u|x-dir sea water velocity|0 m s-1|
|v|y-dir sea water velocity|0 m s-1|

*At t=0, the salinity of the sea water is 30 g/kg, while the salinity within the estuary is 0 g/kg (image below). Starting with the estuary half fresh hopefully allows the model to reach a realistic state more quickly.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/177622150-9d2610ce-0ff7-4635-9c87-5a9b58a68a63.png" alt="init_salinity" width="330"/></p>

**Tidal Forcing**

Tidal forcing is made up of a simple spring-neap cycle with a solar and a lunar component.

|Variable|Description|Value|
|---|---|---|
|tide_Eamp|tidal elevation amplitude|0.75 m (Lunar)</br> 0.25 m (Solar)|
|tide_period|tidal period|12.42 hrs (Lunar)</br> 12 hrs (Solar)

**River Forcing**

The river forcing introduces one river to the model. River transport is approximately uniform in depth.

|Variable|Description|Value|
|---|---|---|
|river_Vshape|river runoff tranport vertical profile|evenly split between sigma layers |
|river_transport|river volume transport to sea|1000 m3 s-1|
|river_salt|river water salinity|0 g kg-1|
|river_temp|river water temprature| 10 C|

---
## Results

The videos below show three-day (02/12/20 - 02/14/20) surface salinity and surface temperature for all three model scenarios.

When no wind is present, freshwater is mostly diverted toward the west as it exits the estuary. When there is a 6 m/s eastward wind, freshwater is diverted southeast as it exits the estuary. When there is a 6 m/s westward wind, freshwater is diverted towards the west and closely hugs the coast. These observations are consistent with what I would expect given Ekman transport diverting the flow slightly to the right from the directon of the wind.

**No Wind Scenario** 
<video src="https://user-images.githubusercontent.com/15829099/178335075-d38e9bae-d02a-4fba-ba33-da4aee14ec71.mp4" controls="controls" style="max-width: 900px;">
</video>


**Uwind = 6/ms Eastward** 
<video src="https://user-images.githubusercontent.com/15829099/178335104-efc5f6fd-c982-4044-bceb-878a04a9504e.mp4" controls="controls" style="max-width: 900px;">
</video>


**Uwind = 6/ms Westward** 
<video src="https://user-images.githubusercontent.com/15829099/178335111-b74265bf-6f9a-45a5-916e-fe2cb7d62f30.mp4" controls="controls" style="max-width: 900px;">
</video>


The major issue with this model is that the open boundary conditions are influencing the model domain. Specifically, the open boundaries introduce colder water into the model domain because atmospheric forcing is not heating water outside of the model domain.

In the upcoming week, I will read about and experiment with other open boundary conditions to find something more appropriate for this model.

Given that surface water inside of the domain heats ups to 16+ C, I am also suspicious that the downwelling longwave radiation may be too strong. I calculated a downwelling radiation assuming blackbody radiation from a 10 C atmosphere. However, in future models I may lower downwelling radiation from 365 W m-2 to 290 W m-2.

### Example Plots

Below I've included some example plots we can look at once the model is more realistic. The inspiration for these visualizations comes from MacCready et al. (2009)

I created three mooring stations, as shown in the figure below.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/178329999-bac9e4a6-c4cc-4522-9086-5ee3d562f013.png" alt="moor_locations" width="500"/></p>

At each station, we can look at a timeseries of key variables such as water velocity and salinity.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/178330023-e60a25a0-b431-4cba-95e4-5c82da0edee2.png" alt="western_moor" width="600"/></p>

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/178330033-481332a6-cc70-4f2c-9dc5-7c1771d8fca4.png" alt="central_moor" width="600"/></p>

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/178330040-b9fd4643-f15f-49e1-ace9-299cc4833688.png" alt="eastern_moor" width="600"/></p>

Currently, these figures are showing data taken from the surface. Later on I will need to decide at what depth it makes sense to analyze data.

I would also be interested in looking at a temperature profile at these different stations.

I'm also noticing that the surface salinity often hovers around 30 g/kg, which is the ocean forcing salinity. To me, this indicates that I should either move the stations closer to the shore, or I might want to model scenarios with a weaker wind velocity.

Ultimately, I am interesting in observing upwelling or downwelling given different wind conditions.

## References

MacCready, P., Banas, N. S., Hickey, B. M., Dever, E. P., & Liu, Y. (2009). A model study of tide-and wind-induced mixing in the Columbia River Estuary and plume. Continental Shelf Research, 29(1), 278-291.