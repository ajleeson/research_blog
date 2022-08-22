
## Flat Shelf Upwelling Experiment (Part 2)

This week I returned to the flat shelf upwelling experiment that I began a few weeks ago. Part 1 of this experiment is described in a prior [blog post](https://ajleeson.github.io/research_blog/2022/07/18/flat-shelf-upwelling-part-1.html).

We had identified several improvements that could be made:
- Stratify ocean with salinity rather than temperature
- Turn off atmospheric forcing and simulate wind usings just surface momentum fluxes
- Change salinity and 3d momentum boundary conditions to Gradient instead of Radiation Nudging

I've tried to address all of these suggestions (details below). I have also been slowly teaching myself some GFD so I can hopefully set up a more thoughtfully designed experiment.

---
## Grid

As introduced in Part 1 of this experiment, I am using a flat-shelfed grid with a vertical wall at the coast. The grid is shown in Figure 1. 

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/179569924-35eafbb8-f208-48a7-8d8d-0ed5edba089e.png" width="500"/><br>Fig 1. Flat-shelfed grid.</p><br><br>

---

## Forcing

To simplify this experiment, I have turned off tidal forcing, river forcing, and atmospheric forcing. The only types of forcing present are ocean forcing and surface momentum forcing. 

### Ocean Forcing

The following table lists the ocean forcing used in this model. The ocean does not introduce momentum to the system. Temperature is constant everywhere.

|Variable|Description|Value|
|---|---|---|
|salt*|sea water practical salinity|30 to 33 g kg-1 |
|temp|sea water temperature|10 C|
|zeta|free surface height|0 m|
|ubar|x-dir barotropic velocity|0 m s-1|
|vbar|y-dir barotropic velocity|0 m s-1|
|u|x-dir sea water velocity|0 m s-1|
|v|y-dir sea water velocity|0 m s-1|

*Salinity is used to create a two-layered stratified ocean. The top 15 sigma layers have a salinity of 30 g kg-1, while the bottom 15 sigma layers have a salinity of 33 g kg-1. Since the bathymetry is flat, I can assume that stratifying the ocean with sigma layers will create two flat layers.

The resulting salinity and density profiles are shown in Figure 2. 

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/183492005-97108683-1aa6-4b30-8b9d-c0e5f72ab7ff.png" width="400"/><br>Fig 2. Initial ocean salinity and density.</p>

The salinity and/or temperature of the ocean can easily be changed in the future as needed. I am still learning about the physics of upwelling and have not yet decided on values to use for an extended model run.

### Surface Momentum Stress Forcing

Previously, I defined the BULK_FLUXES option to create atmospheric forcing. This allowed me to define a wind speed, but it also required that I provide solar radiation.

To simplify the model, I have turned off BULK_FLUXES and atmospheric forcing. To simulate wind, I am now forcing ROMS with surface momentum stresses. To achieve this, I needed to do the following:

1. **Undefine ANA_SMFLUX.** ROMS will overwrite .nc surface stress forcing if ANA_SMFLUX is defined. But I want ROMS to use the .nc file I generate.
2. **Create a forcing file to generate .nc files for sustr and svstr.** I called this forcing folder smflux0. I needed to be careful of ROMS's anticipated dimension types for sustr and svstr (u2dvar and v2dvar, respectively).
3. **List surface momentum flux in forcing_list.csv.** I needed to tell ROMS to look for surface stress forcing. 
<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/183495260-20c31d8e-340c-4e68-9294-640bd5314199.png" width="150"/><br></p>

4. **Tell ROMS which files contain surface momentum stress.** This change was made in BLANK.in. I tell ROMS that there are 2 forcing input files, one called sustr.nc and one called svstr.nc. These files are of forcing type smflux and created in make_forcing_main.
<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/183495403-d966adbc-1b7e-429d-bf98-da3f0d9e5360.png" width="560"/><br></p>

The forcing inputs for this model are listed below.

|Variable|Description|Value|
|---|---|---|
|sustr|surface u-momentum stress|0.1 N m-2 |
|svstr|surface v-momentum stress|0 N m-2 |

Again, these values can be changed in the future after more reading.

---
## Boundary Conditions

In the prior version of this experiment, the model seemed to be having issues with boundary conditions. Specifically, it seemed like momentum was circulating within the domain, rather than flowing in and out. 

To fix this problem, I have switched temperature, salinity, and 3d momentum boundary conditions from Radiation Nudging to Gradient. I have also turned off nudging to climatology for these variables. Previously, the model would have nudged all inflows to values provided in climatology (based on the values in ocean forcing, there is zero momentum outside of the domain). Gradient boundary conditions force the inflowing value to be equal to the nearest interior value for each variable.

I have previously written a more detailed [blog post](https://ajleeson.github.io/research_blog/2022/07/15/temperature-bcs.html) about switching from Radiation Nudging boundary conditions to Gradient boundary conditions. While this blog post focuses on temperature, the steps are nearly identical for salinity and 3d momentum.

---
## Preliminary Results

After all of these changes were implemented, I ran the model for 5 days. The video below shows the hourly surface salinity profile during these 5 days.

<p style="text-align:center;"><video src="https://user-images.githubusercontent.com/15829099/183497122-0073a939-b695-4089-b64e-d0e1174d5923.mp4" controls="controls" style="max-width: 700px;">
</video><br></p>

Saltier water does appear to be upwelling at the coast. The net flow of the water appears to be towards the South-East, which seems consistent with the eastward surface stress used to force the model.It seems like there is some flow of momentum in and out of the boundaries, which is a good indication that the boundary conditions are working. However, a longer run later on will make it more apparent whether the boundary conditions are actually working.

---

I am still trying to learn more about upwelling and GFD. In the next week, I am hoping to finalize what values to use to force this upwelling experiment, and to run the model for a longer period of time.

As always, any other suggestions are welcome.