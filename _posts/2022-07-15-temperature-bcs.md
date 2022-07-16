
## Open Boundary Conditions for Atmospheric Forcing

As a reminder, this is a snapshot of what the idealized estuary looked like after 2.5 months with atmospheric forcing. Note that wind speed was 0 in this model run.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/179323152-492e11e8-7307-481b-9812-cabdbb59eee3.png" alt="prior_bad_bcs" width="800"/></br>Fig 1. Prior wind experiment with problematic temperature boundary conditions.</br></br></p>

This result was problematic because the boundaries are influencing the model domain. Specifically, the boundaries are introducing colder water into the domain because the atmosphere is not heating water outside of the domain.

After some troubleshooting and testing, I believe I have found appropriate boundary conditions to use with BULK_FLUXES in an idealized estuary. This solution closely followed some suggestions Parker provided previously.

To test different hypotheses, I ran two-day model runs and looked at surface temperature at the end of these two days. Figure 2 shows surface salinity and temperature prior to boundary conditions adjustments. Figure 3 shows the surface salinity and temperature using the final boundary conditions. In Figure 3, cold ocean water no longer seems to be flowing in from the boundaries.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/179325535-6a305c17-06a3-4d87-aa14-fbe52eddff3d.png" width="800"/></br>Fig 2. Surface salinity and temperature after two days using original, problematic boundary conditions.</br></br></p>

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/179325558-b6fba4a2-5d1f-4778-aa4a-a6c9804cf0af.png" width="800"/></br>Fig 3. Surface salinity and temperature after two days using better boundary conditions.</br></br></p>

The final, successful boundary conditions were created using the following combination of changes:

### Change temperature boundary conditions from "RadNud" to "Gra"
I made this modification by creating a unique temperature boundary condition flag in BLANK.in. In make_dot_in.py, I added a line to make open temperature boundary conditions "Gra." The Gradient (Gra) boundary condition forces the gradient of temperature to be 0 at the boundary which forces the exterior value to be equal to the nearest interior value. In contrast, Radiation-Nudging (RadNud) boundary conditions nudges temperature to a pre-defined exterior value for inflows. Figure 4 has an excerpt from the ROMS log file that lists the boundary conditions used.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/179324780-d48b72d6-5661-4784-9882-041b16fb3f6b.png" alt="lateral_bcs" width="500"/></br>Fig 4. ROMS log.txt excerpt for lateral boundary conditions.</br></br></p>

### Turn off Temperature Climatology Processing and Nudging to Climatology
In addition to changing the boundary conditions, I also needed to turn off climatology read/processing and nudging to climatology for temperature. To accomplish this, in BLANK.in I switched LtracerCLM and LnudgeTCLM to FALSE for temperature. Figure 5 has an excerpt from the ROMS log files that shows this change.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/179325374-e54ffb9a-45a4-404c-9b0d-c7869d7e21b6.png" alt="no_climatology" width="500"/></br>Fig 5. ROMS log.txt excerpt for climatology processing.</br></br></p>

---
## Extras

During this troubleshooting process, ROMS produced some fun model results. These might be a good example of "Color Fluid Dynamics" that Alex mentioned. It sure looks pretty, but clearly has some issues.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/179325664-d2cbb1aa-8721-4fb0-af67-628f59702385.png" width="800"/></br>Fig 6. Color Fluid Dynamics.</p>
