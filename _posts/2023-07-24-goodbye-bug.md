## Goodbye Bug

On July 19th, 2023, after 262 days of blow ups, vertical sources finally ran successfully in LiveOcean. Details below.

---
## Squashing the bug

This week, Parker brilliantly searched for every line in which the variable "Dsrc" appears in the ROMS source code. The result is shown in Figure 1. Note that Dsrc indicates the direction of a source. Dscr = 0 for u-sources, 1 for v, and 2 for w.

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/5746f88f-da94-4a9c-8c58-033e1fbf72e3" width="600"/><br>Fig 1. Result of grep search for "Dsrc" in ROMS source code.</p><br>

What we saw is that in most files (e.g. pre_step3d.F), we have a format of:
```
IF Dsrc == 0 THEN
    ...
ELSE IF Dsrc == 1 THEN
    ...
```

However, in step3d_uv.F (highlighted in Fig 1), we have only:
```
IF Dsrc == 0 THEN
    ...
ELSE
    ...
```

This simple IF-ELSE case is the key to our puzzle. In all other cases, the IF-ELSEIF pattern assures that the code is ascribed to solely u-sources or solely v-sources. However, the ELSE statement in step3d_uv.F means that the code can be ascribed to anything that is not a u-source (i.e. code meant for only v-sources was being ascribed to both v- and w-sources!)

Our new finding is consistent with the problem we identified last week: that even when we presumably turn off all physics associated with w-sources, we still observed large velocities near the source. The reason why is because the WWTP still had v-momentum (which it shouldn't).

### Results

What happens when we change the ELSE statement to an ELSE IF statement?
**The model runs without blowing up!!**

Even when I turn back on the "volume adding" code, the model still runs. Figure 2 shows a two-day, hourly, video of surface velocities near Birch Bay WWTP. They are beautiful.

<p style="text-align:center;"><video src="https://github.com/ajleeson/LO_user/assets/15829099/a0cf438c-ef00-4372-bc18-41b4ca7026e2" controls="controls" style="max-width: 800px;"></video><br>Fig. 2 Two-day video of surface velocities near Birch Bay WWTP (hourly resolution). The WWTPs are volume-adding. The ELSE statement in step3d_uv.F was changed to an ELSEIF statement. The model ran without blowing up.</p><br>

---
## Reflection

Why was this bug so difficult to squash? For one, the problematic bit of code was part of the LuvSrc module, not the LwSrc module. In other words, LwSrc was not directly causing the problem to occur; it only enabled the symptoms to appear.

Given the way the code is written, our issue would only appear if both LuvSrc and LwSrc were enabled. Using either LuvSrc or LwSrc alone would not cause problems, explaining why LwSrc never blew up in the simple, idealized domain.

Even though this bug was (frustratingly) persistent, I sure learned a lot about ROMS. Today I have so much more confidence in my ability to implement new ideas in the model. I also discovered and fixed many small bugs in my code throughout the debugging process.

---
## Next Steps for TRAPS

My next major goal is to run another experiment with and without WWTP DIN. This time we don't need to rush to meet a conference deadline, so I'd like to be more meticulous about how I set up the model runs. Before I run a year-long experiment, there are a few things I'd like to tidy up:

- Upgrade to Ecology's latest dataset
- Deal with WWTPs that opened or closed on a certain year
- Make sure river mouths located on a narrow strip of land get placed on the correct side of the land (the algorithm is blindly placing rivers on the nearest coastal grid cell without awareness of real geography)
- Double check ambiguous duplicate rivers (SSM rivers that may or may not overlap with pre-existing LO rivers)
- Remove 2013/2014 data from climatology for rivers based on Big Beef Creek

There are also a few model configurations I'd like to discuss/plan:

- Implement initial plume distribution (like UCLA ROMS-BEC model)?
- How closely to replicate Ecology's experiments? i.e. should we run an experiment with "natural conditions?"
- Finish model validation and biogeochemistry parameter tuning before next major experiment?
- Model spin-up time?

We have one month before the next KC meeting. I have found that setting goals for this meeting is a good way to motivate new work. What are some realistic goals that I can achieve before the next KC meeting (Aug 17th)?

---
## Key moments in the debugging saga

The very first time I ran LiveOcean with TRAPS, the model blew up on day 1. The following timeline lists key moments in the ensuing debugging process.

### Nov 2022: Identifying that blowups were linked to a WWTP

Using the pan_plot debug plot to look at max/min surface velocities helped pinpoint the problem to Birch Bay WWTP. Identifying that the issue was related to WWTPs helped narrow the scope of the problem.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/200236146-994ce50a-7407-47e3-a883-407d762eb8b7.png" width="600"/><br>Fig 3. Min/max surface velocities of LiveOcean with-TRAPS on hour 20 of 2020.01.01. These velocities come from one of my first model runs with TRAPS.</p><br>

I later discovered that LiveOcean does not blow up on day 1 if I remove Birch Bay WTTP.


### Nov 2022: Resolving issues with the Birch Bay WWTP inputs

We then checked all of our inputs to Birch Bay WWTP. As it turns out, my forcing file actually had several bugs that I needed to fix, including:

- Injecting Nans into the biogeochemistry fields
- Injecting negative DIC into the model
- Using the same name for Birch Bay WWTP and Birch Bay River

However, even after fixing these forcing file problems, Birch Bay WWTP still continued to blow up.


### Jan 2023: Discovering that other WWTPs blow up too

We decided to remove Birch Bay WWTP and let the model run. After two months, we saw a blow up at Oak Harbor Lagoon WWTP in Whidbey Basin (Fig 4). This finding indicated that Birch Bay WWTP was not the sole problem.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/212557264-68abb019-947a-4294-a586-ab7b2f810313.png" width="600"/><br>Fig 4. Comparison of surface velocity profiles near Oak Harbor Lagoon WWTP just before the "with point sources" run blew up.</p><br>

At this point, we brought our issue to the ROMS forum.


### Feb 2023: Hypothesis elimination

We received many good suggestions from the ROMS forum. I then ran test after test to check whether any suggestions could resolve our issue. Some hypotheses that we disproved and some potential solutions that still failed include:

- Discharging to bottom 1/3 sigma layers
- Discharging to all sigma layers
- Discharging to surface layer
- Using different mixing scheme (added mixing along geopotentials)
- Using different turbulence closure model (Kantha-Clayson instead of Canuto-A)
- Removing biogeochemistry
- Using different temperature advection schemes
  - Akima 4th order instead of MPDATA
  - HSIMT instead of MPDATA
- Combining Kantha Clayson and Akima 4th order
- Discharging neutrally-buoyant water (LtracerSrc = FF)
- Changing to different tiling scheme

We also verified that Birch Bay WWTP and Oak Harbor Lagoon WWTP are not especially large dischargers, nor are they located in the shallowest area (Fig. 5)

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/221698619-f13916d8-4760-4e83-8810-faa1d00b5238.png" width="400"/><br>Fig 5. WWTP Depth vs. Discharge Rate.</p><br>


### Mar 2023: Discovering that <i>all</i> WWTPs look funny

We removed both Birch Bay WWTP and Oak Harbor Lagoon WWTP, then ran the model for a month with and without the other WWTPs. Figure 6 shows the resulting hydrodynamic difference between the two model runs.

We discovered that even though the other WWTPs are not blowing up, they do not behave in a physically realistic way.

<p style="text-align:center;"><video src="https://user-images.githubusercontent.com/15829099/223488900-46fa150b-5062-4a88-90a7-e9e4489c6521.mp4" controls="controls" style="max-width: 800px;"></video><br>Fig. 6 Hourly movie of the hydrodynamic difference between the "with WWTP" and "no WWTP" run in Main Basin over the month of February.</p><br>


### Apr 2023: Demonstrating strange velocities in idealized domain

After recognizing that all WWTPs behave strangely, we tried to isolate the issue to a single vertical source in a simple domain. John Wilkin kindly shared a flat-shelf, double-periodic domain which became the basis for idealized experiments.

After a series of tests, we found that the vertical sources indirectly induced barotropic velocities by increasing SSH. The associated vertical velocity, omega, also looked somewhat puzzling.

Parker then developed modifications to the ROMS source code which turned off volume associated with vertical sources. The goal of this modification is to turn off the puzzling hydrodynamics that we observed in the idealized domain. It is worth noting that the vertical source never blew up in the idealized domain.


### May-Jul 2023: Testing volume-less sources in idealized domain

It was important to test the volume-less sources in the idealized domain to ensure that they worked as expected. We identified and corrected issues with forcing inputs because of these idealized tests.

Finally, we demonstrated that volume-less, neutrally-buoyant vertical sources in the idealized domain can discharge tracer concentration without altering the hydrodynamics. The following videos show cross sections of salinity and u from this test. Freshwater is correctly discharging from the source without inducing any velocity.

<p style="text-align:center;"><video src="https://github.com/ajleeson/LO_user/assets/15829099/363cca08-1b5c-48ce-89e0-7e8bc5157ca6" controls="controls" style="max-width: 800px;"></video><br>Fig. 7 Section salinity with volume-less, neutrally-buoyant, freshwater source.</p><br>

<p style="text-align:center;"><video src="https://github.com/ajleeson/LO_user/assets/15829099/95a903a4-1a2d-49bb-b3fd-863b7c257dc4" controls="controls" style="max-width: 800px;"></video><br>Fig. 8 Section u with volume-less, neutrally-buoyant, freshwater source.</p><br>



### Jul 2023: Testing volume-less, neutrally-buoyant sources in LiveOcean

After success in the idealized domain, I tested the volume-less, neutrally-buoyant WWTPs in LiveOcean.

The result of this test produced a key finding: Even when we seemingly turn off all physics associated with WWTPs, they still blow up. In hindsight, this result help illucidate that the model assigns some other hydrodynamic property at the location of WWTPs.

The following day, we found the IF-ELSE conundrum in the LuvSrc module.



---
## What have I been reading

Santana, E. I., &#38; Shull, D. H. (2023). Sedimentary Biogeochemistry of the Salish Sea: Springtime Fluxes of Dissolved Oxygen, Nutrients, Inorganic Carbon, and Alkalinity. <i>Estuaries and Coasts</i>, <i>46</i>(5), 1208–1222. https://doi.org/10.1007/s12237-023-01197-8