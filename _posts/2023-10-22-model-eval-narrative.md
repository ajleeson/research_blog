## Model Evaluation Narrative

This week I resolved issues with the ORCA mooring data and re-created the bottom DO comparison figure. I have since been focusing on refining discussion of model evaluation for the NTU talk. More details below.

---
## ORCA Buoy Data

As I commented last week, I had accidentally been using old LiveOcean output in place of ORCA data. This error would explain many of the strange things we were seeing, such as:

- model initial conditions matching "ORCA data" well, but not matching bottle data or CTD casts
- disagreement in bottom DO concentrations between "ORCA data" and bottle data
- disagreement in DO concentrations between "ORCA data" and published ORCA DO climatology

Erin very clearly explained to me where I can find the actual ORCA data. Figure 1 shows a re-hash of the model-ORCA bottom DO comparison figure, which now uses *actual* ORCA data.

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/dfb756a9-0475-4b36-8488-11091650d1c5" width="800"/><br>Fig 1. Model-model-ORCA buoy comparison of bottom DO. "Current LiveOcean" is the cas6_x2b_traps2 model version being used in the current forecast. "Updated model" is the latest cas7_trapsV00_meV00 run.</p><br>

Once I resolved issues with ORCA data, I began reworking my narrative for model evaluation results. Per Parker's recommendation, I tried to incorporate more skepticism towards LiveOcean. The section below provides an overview of my intended narrative for the NTU talk.

---
## Model evaluation narrative

*The following set of images and text provide context and details for the "results" section of my NTU slides. Note that the text do not one-to-one reflect what I intend to say. They merely provide my general thoughts for each slide. I haven't refined this portion of my talk yet, and I am happy to receive feedback and suggestions.*

We care how the model performs relative to observations because we want the model to represent reality to the best of its ability. We can only begin to trust model predictions for different test cases (such as WWTPs with no nitrogen discharge) if the model captures existing conditions well.
<br>

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/b07088ed-a740-422f-8779-0a98de4d1a07" width="800"/><br>First, let's set expectations for our model evaluation. For hydrodynamics, we hope that RMSE and bias are within 1 PSU for salinity, and 1 degC for temperature. Elsewhere, we are exploring to see how well the model's different state variables align with observations. We want to understand where and when the model has greatest uncertainty, because ultimately we want to know the limitations of the model.</p><br>

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/e248a994-5c8a-429a-9105-c60fd1af54c8" width="800"/><br>These figures are property-property plots in which bottle data taken at a specific time and location are matched to corresponding model output. The y-axis is the model value, and the x-axis is the bottle data. The data are grouped into shallow depths (top row) and deep depths (bottom row). Different sub-basins are colored differently, with Main Basin colored dark blue, Hood Canal colored orange and South Sound colored pink. The figures here show property-property plots of salinity (left) and temperature (right)-- an examination of the hydrodynamics performance.</p><br>

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/19a112d1-8c65-4864-9f40-b6897b2de0e1" width="800"/><br>Generally, the model hydrodynamics align well with the observations. Bias and RMSE are close to 1, which is good. However, they are not < 1, so there is still room for improvement in the future.</p><br>

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/a0f40e30-7ba8-47b5-856e-9c3fee05f238" width="800"/><br>In shallower waters, one can also observe more scatter at lower salinities and higher temperatures. In particular, LiveOcean appears to overestimate salinity in the low salinity regime, suggesting that the freshwater inflow may be insufficient. [What can we learn about temperature? That LiveOcean has a harder time getting the location of warm patches of water correct?]</p><br>

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/055b9be1-3fd8-4416-9295-3948c18aab74" width="800"/><br>We can also look at similar property-property plots for biological parameters. In this case, DO is shown on the left, and DIN is shown on the right. Our main question of interest revolves around DO, so we want to represent DO as best as possible. In these property-preoperty plots, DO generally has more scatter than we observed in the salinity and temperature figures earlier. Part of this scatter is due to the modeled DO concentration sitting at the end of a series of hydrodynamic and biogeochemical processes.</p><br>

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/6ac0fbed-be24-47ef-9345-9f9c2c75f2f3" width="800"/><br>We can also consider DIN, as an intermediate biogechemical parameter. Overall, LiveOcean tends to overestimate DIN concentrations. The source of this overestimate is still under investigation.</p><br>

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/e6f926b1-0082-444c-985e-c1c01c245087" width="800"/><br>Focusing solely on bottom DO concentrations now, we can compare a timeseries of bottom DO from moorings (ORCA buoys) to model output. There are data available at five different locations for the 2017 test case. The bottom DO concentrations reported are sourced from the bottom 20% of the water column. The purple dots are observations, and the solid line is the model output.</p><br>

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/fb50fe75-601d-4ce5-90f6-c4ac25bb5c90" width="800"/><br>In Fall, the model appears to decently estimate bottom DO at these 5 station locations. The results look promising. However, we also acknowledge that the model tends to slightly underestimate DO in the first four stations, and appears to struggle more with the Twanoh station at the end of Hood Canal.</p><br>

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/4c9bf091-d0b5-4d53-b73a-06175c1c017c" width="800"/><br>Looking at the beginning of the year, it is also apparent that LiveOcean appears to overestimate DO concentrations in Hood Canal. Observations indicate that DO is low in January (~2.5 mg/L), but LiveOcean starts with an initial condition of ~7.5 mg/L.</p><br>

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/4184e14c-57ff-4ee1-ac2a-9f2f278e5aa8" width="800"/><br>We can also look at DO throughout the water column by comparing modeled DO to bottle data and CTD casts. This figure shows DO profiles near the end of Hood Canal, and each panel shows a different month of the year. (Side note: the units are now in uM instead of mg/L. The conversion is 1 mg/L = 32 uM, which is coincidentally also the conversion rate between the US and Taiwan dollar...so I've been doing this math a lot recently). Despite overestimating initial DO concentrations in Hood Canal, the model does a decent job of capturing DO throughout the water column once the spring bloom dynamics take effect. This is promising. Though we also note that the DO maxima in the model is always at the surface, but in observations it is sub-surface, suggesting that the model phytoplankton dynamics and settling rate still have room for improvement. However, the max DO concentration is usually similar in magnitude between model and observations (yay), except in October when then model underestimates the DO maxima.
</p><br>

Overall, the updated version of LiveOcean shows promising performance when evaluated against observational data. Though we also acknowledge that there are still places where the model performance can be improved, such as better estimates of freshwater inflow, or more accurately capturing DIN concentrations, or improved initial conditions (which Dakota is working on, woohoo!)