## Testing Volume-less Vertical Sources

This week I tested volume-less vertical sources in both the idealized domain from John Wilkin and in LiveOcean.

In the idealized domain, w and omega seem reasonable. However, the inflowing water does not seem to match the specified forcing. In LiveOcean, Birch Bay WWTP still blew up despite having a volume-less source. More details below.

---
## V-less in Idealized Domain

Before running a more complicated V-less test in LiveOcean, I first tried the V-less sources in John Wilkin's simple grid. In this test, the only change I made was to incorporate Parker's V-less code. Everything else remained the same as my last test:
- Source located in the middle of the water column
- Discharges at 10 m3/s (only used to scale tracers, and not added to volume)
- Freshwater source
- Inflowing temperature is the same as water already in the grid cell
- Density is a function of temperature only

An initial look at the surface plots (Fig. 1) show that we no longer see a negative vertical velocity at the surface. Additionally, SSH does not appear to be rising everywhere over time. Thus, the V-less source appears to be working.

<p style="text-align:center;"><video src="https://user-images.githubusercontent.com/15829099/236958981-2a17ae02-cb3d-4b6f-a4cd-85b0f00c844c.mp4" controls="controls" style="max-width: 600px;"></video><br>Fig. 1 Surface u, v, w, SSH in idealized domain.</p><br>

However, a closer look at section plots of salinity and temperature reveal some oddities. Figure 2 shows section salinity. Strangely, freshwater does not appear to be flowing from the source (despite code that tells ROMS to discharge 0 salinity water). Furthermore, Figure 3 suggests that warm water is being discharged from the source, which should not be the case. In the dot_in file, LtracerSrc = F for temperature (inflowing water should be at the same temperature as existing water). I do not fully understand why the forcing seems to be misbehaving.

<p style="text-align:center;"><video src="https://user-images.githubusercontent.com/15829099/236958978-99c0c6b3-c061-4f89-a617-fbdc97f62b4f.mp4" controls="controls" style="max-width: 600px;"></video><br>Fig. 2 Section salinity in idealized domain.</p><br>

<p style="text-align:center;"><video src="https://user-images.githubusercontent.com/15829099/236958982-a0d618de-75c3-429f-9804-948e699e0752.mp4" controls="controls" style="max-width: 600px;"></video><br>Fig. 3 Section temperature in idealized domain.</p><br>

Let's put the strange inflow properties to the side for now so we can focus on velocities. Are the section velocities consistent with a buoyant, warm water plume discharging from the center of the water column? I believe they are!

Figures 4 and 5 show u and v, respectively. Above the buoyant source there is horizontal divergence. Under the buoyant source there is horizontal convergence. These observations are consistent with what I would expect of a buoyant plume.

<p style="text-align:center;"><video src="https://user-images.githubusercontent.com/15829099/236958984-b44e13c3-abe1-43a0-950b-373a7a35a438.mp4" controls="controls" style="max-width: 600px;"></video><br>Fig. 4 Section u in idealized domain.</p><br>

<p style="text-align:center;"><video src="https://user-images.githubusercontent.com/15829099/236958985-c457ebab-4704-4a7d-874e-5372f0525baf.mp4" controls="controls" style="max-width: 600px;"></video><br>Fig. 5 Section v in idealized domain.</p><br>

Figure 6 and 7 show w and omega, respectively. Note that the colorbar scale for w and omega are different. w looks exactly like what I would expect. Vertical velocity is positive throughout the water column, reaching a maximum at the location of the source. Additionally, w is zero at the bottom and surface.

Omega is shown with a saturated colorbar. The oversaturation helps see the smaller magnitude velocities. In general, this video of omega suggests that we are seeing some convective circulation: warm water rises from the source, radiates horizontally outwards, then sinks.

<p style="text-align:center;"><video src="https://user-images.githubusercontent.com/15829099/236958986-da9d8884-9d26-47e7-8b97-19cae6e59b71.mp4" controls="controls" style="max-width: 600px;"></video><br>Fig. 6 Section w in idealized domain.</p><br>

<p style="text-align:center;"><video src="https://user-images.githubusercontent.com/15829099/236958971-14bf066f-2aeb-4f78-bd4e-867549a66a84.mp4" controls="controls" style="max-width: 600px;"></video><br>Fig. 7 Section omega in idealized domain.</p><br>

Overall, these vertical velocities make a lot of sense. This is a huge breakthrough!

---
## V-less in LiveOcean

Although the forcing is behaving strangely in the idealized model, the vertical velocities look great. Thus, I've gone ahead and tested the V-less sources in LiveOcean. I added Birch Bay WWTP back into the model to see if this source would still blow up.

Implementing the V-less sources requires me to recompile ROMS. I took a look at the resulting omega.f90 file generated from the build and confirmed that Parker's changes were correctly incorporated. Figure 8 shows a snippet of the omega.f90 file which indicates that the "volume-incorporating" part of LwSrc has been removed.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/236961226-3a69a56a-2bbd-442f-8739-3dcf10336d65.png" width="500"/><br>Fig 8. Compiled omega.f90 file.</p><br>

So does Birch Bay WWTP still blow up?

Sadly, yes **:(**

Figure 9 shows a snapshot of the surface velocities at Birch Bay WWTP prior to model blow up.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/236961229-dcc0bd1f-2b1e-4c8f-bb88-c94eca8d730f.png" width="750"/><br>Fig 9. Surface velocities near Birch Bay WWTP before blow up.</p><br>

Given that w is still negative at the surface near Birch Bay WWTP, I am inclined to think that the vertical source was not in fact volume-less. However, the build file shown above suggests otherwise. **Could there be some other issue causing an instability at the treatment plants?**

I will also note that I re-ran LiveOcean with the "volume-incorporating" code, and Birch Bay WWTP blew up. The resulting surface velocity plot from this run is identical to Figure 9.