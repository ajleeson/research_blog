## Nutrients Mini Experiment (Part 1)

This week I have made some progress using the idealized estuary to compare bottom DO with and without nutrients in WWTP effluent. There is more analysis work I can do later one, and I have some questions about my boundary conditions. However, I wanted to document and share the current experiment status prior to my trip.

For this experiment, I have been using the grid I developed when writing the WWTP placement algorithm ([documentation in prior blog post](https://ajleeson.github.io/research_blog/2022/07/25/WWTP-PointSource-Algorithm.html)). As a reminder, Figure 1 shows the model grid and locations of point sources.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/189416788-edd51e66-fc4c-4aff-8d0b-495acd5a2bda.png" width="400"/><br>Fig 1. Grid bathymetry and point source locations. Note that this grid also includes a river.</p><br><br>

I let the model run for two months to spin-up without DIN in the WWTP effluent. Then, I ran the model for one additional month under two conditions: (1) the base case in which I let the model continue with no DIN in the WWTP effluent; and (2) the nutrient condition in which I added DIN to the WWTP effluent.

More details about the initial conditions, model spin-up, preliminary results, and boundary conditions are provided below.

---
## Model Spin-Up Initial Conditions

I used four types of forcing during the model spin-up: ocean, atmospheric, tidal, and river forcing. The river forcing indluces forcing conditions for both the river and WWTPs.

### Ocean Forcing

The values used for ocean forcing are listed below. With the exception of salinity, the ocean forcing variables are constants.

The value of variables not listed in the table are is zero (e.g. free surface height or ammonium concentration).

| Variable      | Description               | Value     | Notes|
| ---           | ---                       | ---       | ---  |
| salt          | ocean salinity            | 30 g kg-1 (ocean) <br> 0 g kg-1 (estuary) | Estuary half fresh at t = 0. IC shown in Fig 2.|
|temp           |sea water temperature      |10 C       | Same as air, river, and WWTP effluent temp|
|NO3            | nitrate concentration     |27 mmol N m-3|Salish Sea IC in ocn1/Ofun_bio|
|phytoplankton  |conc of phytoplankton as N |0.1 mmol N m-3| Some small amount to start|
|zooplankton    |conc of zooplankton as N   |0.1 mmol N m-3| Some small amount to start|
|TIC            |conc of total inorganic C  |2037 mmol C m-3|Salish Sea IC in ocn1/Ofun_bio|
|alkalinity     |total alkalinity           |2077 milliequivalent m-3|Salish Sea IC in ocn1/Ofun_bio|
|oxygen         |conc of DO                 |219 mmol O m-3<br>(7 mg L-1)|Salish Sea IC in ocn1/Ofun_bio|

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/189420990-bd2874fb-bd88-4094-bb8e-ee76cb4d04b2.png" width="400"/><br>Fig 2. Ocean salinity initial condition.</p><br><br>


### Atmospheric Forcing

All atmospheric forcing state variables are listed below. All values are constat, except for shortwave radiation which has a daily cycle shown in Figure 3.

| Variable      | Description               | Value         | Notes|
| ---           | ---                       | ---           | ---  |
|Pair           | air pressure              | 1013.25 mbar  ||
|rain           |rainfall rate              | 0 kg m2 s-1   ||
|swrad          |shortwave radiation        |daily cycle with peak of 500 W m-2|Hourly profile shown in Fig 3.|
|lwrad_down     |downwelling longwave radiation|290 W m-2   ||
|Tair           | air temperature           | 10 C          | Same as ocean, river, and WWTP effluent temp|
|Qair           | relative humidity         | 83%           ||
|Uwind          |E-W wind speed             | 0 m s-1       ||
|Vwind          | N-S wind speed            | 0 m s-1       ||

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/189445959-93cdeb6d-08cc-41fe-9f23-f2826f3ebf4e.png" width="400"/><br>Fig 3. Hourly shortwave radiation.</p><br><br>

### Tidal Forcing

Tidal forcing is made up of a simple spring-neap cycle with a solar and a lunar component.

|Variable|Description|Value|
|---|---|---|
|tide_Eamp|tidal elevation amplitude|0.75 m (M2)<br> 0.25 m (S2)|
|tide_period|tidal period|12.42 hrs (M2)<br> 12 hrs (S2)

### River/WWTP Forcing

River forcing includes both the river component and the WWTP component. The river and WWTP forcing differ from each other, but all 5 WWTPs have the same forcing.

Again, any variable that is not listed has an initial value of zero (such as small detritus).

For these forcing conditions, I borrowed several average values for the Skagit River and West Point treatment plant from Ecology's dataset. This way, the forcing conditions will hopefully be somewhat realistic.

**River Forcing**

| Variable      | Description               | Value     | Notes|
| ---           | ---                       | ---       | ---  |
|river_Vshape   |vertical profile           |1/N in each layer|transport evenly split between sigma layers|
|river_transport|volume transport           |1000 m3 s-1||
|river_salt     |river salinity             |0 g kg-1   ||
|river_temp     |river temperature          | 10 C          | Same as ocean, air, and WWTP effluent temp|
|river_NO3      |conc of nitrate            |6.4 mmol N m-3| Average value from Skagit River (Ecology's dataset)|
|river_TIC      |conc of total inorganic C  |455 mmol C m-3| Average value from Skagit River (Ecology's dataset)|
|river_TAlk     |total alkalinity           |410 milliequivalents m-3| Average value from Skagit River (Ecology's dataset)|
|river_Oxyg     |conc of DO                 |362 mmol O m-3<br>(11.6 mg L-1)| Average value from Skagit River (Ecology's dataset)|

**WWTP Forcing**

| Variable      | Description               | Value     | Notes|
| ---           | ---                       | ---       | ---  |
|river_Vshape   |vertical profile           |1 in bottom layer<br>0 in other layers|discharging to bottom layer|
|river_transport|volume transport           |4.5 m3 s-1 |Average value from West Point (Ecology's dataset)|
|river_salt     |river salinity             |0 g kg-1   ||
|river_temp     |river temperature          | 10 C          | Same as ocean, air, and river temp|
|river_NO3      |conc of nitrate            |0 mmol N m-3| Start with no nutrients for model spin-up|
|river_TIC      |conc of total inorganic C  |2639 mmol C m-3|Average value from West Point (Ecology's dataset)|
|river_TAlk     |total alkalinity           |2000 milliequivalents m-3| Average value from West Point (Ecology's dataset)|
|river_Oxyg     |conc of DO                 |184 mmol O m-3<br>(5.9 mg L-1)| Average value from West Point (Ecology's dataset)|

---
## Model Spin-Up Results

The video below shows the daily surface temperature and salinity from the 2-month model spin-up run.

<p style="text-align:center;"><video src="https://user-images.githubusercontent.com/15829099/189448159-d8a7c67e-4ebe-49bc-b16c-9e8cfc2ce6ac.mp4
" controls="controls" style="max-width: 700px;">
</video><br></p>

The next video shows the daily surface phytoplankton and bottom DO from the 2-month model spin-up run.

<p style="text-align:center;"><video src="https://user-images.githubusercontent.com/15829099/189448347-d151a717-1e4f-47fa-acc9-3ac10bd75f45.mp4
" controls="controls" style="max-width: 700px;">
</video><br></p>

The phytoplankton grow very quickly at first, and then suddenly dissappear (I am guessing due to zooplankton population boom). Afterwards, the phytoplankton concentration is low and remains quite low. After the mass phytoplankton consumption, a large hypoxic region develops away from the coast. Near the coast, the river appears to introduce oxygen in the water. There also appears to be more oxygen near the WWTP locations.

---
## Forcing for Experimental Conditions

---
## Preliminary Results

---
## Boundary Condition Questions