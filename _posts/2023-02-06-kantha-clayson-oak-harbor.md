## I'm a New Fan of the Kantha and Clayson Closure Model

This week I began making my way through some of the suggestions from the ROMS forum. Most feedback recommended that I switch to a different mixing/advection scheme, or that I switch to a different turbulence closure model. I only made it through a few tests so far, but already I have found that Oak Harbor Lagoon WWTP does not blow up when using the Kantha-Clayson turbulence closure model rather than the Canuto A closure model. Unfortunately, Kantha-Clayson does not resolve issues at Birch Bay WWTP. Maybe Birch Bay WWTP was blowing up for different reasons? More details below.

---
## Ninfo

Ninfo specifies how often information is written to the ROMS log file. Units are in number of timesteps. Previously I was running with Ninfo = 90. Feedback from the ROMS forum recommended that I drop Ninfo to 1.

At first when I tested Ninfo = 1, the model occasionally stopped running before I expected it to blow up. I assumed ROMS was simply timing out because the log file became too long. Later on, it was pointed out that ROMS may have stopped earlier because it caught initial symptoms of the blow-up sooner (since it's checking velocity and density every time Ninfo gets printed). Thus, I have since conducted all experiments with Ninfo = 1.

---
## Reverting to Old Mixing Scheme

Parker mentioned that in a previous version of LiveOcean, he had implemented a mixing scheme using a combination of TS_DIF2 (harmonic horizontal mixing) and MIX_GEO_TS (mixing along geopotential surfaces). Since these mixing schemes have already been tested in LiveOcean previously, I decided that the simplest starting point would be to add them back to LiveOcean.

I recompiled ROMS with these mixing options, then initialized the model as a perfect restart beginning 2021.02.18. Unfortunately, ROMS still blew up at Oak Harbor Lagoon WWTP on hour 20 of 2021.02.1.

The surface velocities near Oak Harbor Lagoon WWTP at hour 19 are shown in Figure 1.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/217307512-e2df4ca1-3286-41c9-ae98-add42875c3e3.png" width="750"/><br>Fig 1. Surface velocities at hour 20 on 2021.02.19 near Oak Harbor Lagoon WWTP. This run was initialized on 2021.02.18 with prior LiveOcean mixing schemes, and it blew up.</p><br>

I tested these same conditions at Birch Bay WWTP on 2020.01.01. It blew up as well. Figure 2 shows the surface velocities near Birch Bay WWTP just before blow up.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/217307500-b90bdcd2-84a3-4272-a5a8-c84e56fd6903.png" width="750"/><br>Fig 2. Surface velocities at hour 21 on 2020.01.01 near Birch Bay WWTP. This run was initialized with prior LiveOcean mixing schemes, and it blew up.</p><br>

---
## Kantha-Clayson Closure Model

Another suggestion from the ROMS forum was to switch from a Canuto-A turbulence closure model to Kantha-Clayson. I recompiled ROMS with the new turbulence closure model and reinitialized the run beginning on 2021.02.18. In this case, ROMS ran successfully and did not blow up at Oak Harbor Lagoon WWTP. [Note that for this test, I did not include the prior LO mixing scheme. In other words, the turbulence closure model is the only difference between this run and the orignal Oak Harbor run that blew up.]

Figure 3 shows the surface velocities near Oak Harbor Lagoon WWTP at hour 20 on 2021.02.19, which corresponds to the time that other runs blew up at Oak Harbor Lagoon. Even though Kantha-Clayson prevented ROMS from blowing up, the surface velocity just south of Oak Harbor Lagoon WWTP is rather high. I'm not convinced that these are realistic velocities, and I think there are still some issues with Oak Harbor WWTP.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/217307508-a8216cfc-966d-473b-9118-b063615e308e.png" width="750"/><br>Fig 3. Surface velocities at hour 20 on 2021.02.19 near Oak Harbor Lagoon WWTP. This run was initialized on 2021.02.18 with the Kantha-Clayson closure model, and it did not blow up.</p><br>

I also tested the Kanthan-Clayson Closure Model at Birch Bay WWTP, but Birch Bay WWTP once again blew up on 2020.01.01. There was a *slight* improvement, though, in that ROMS made it one hour longer than it ususually does when Birch Bay WWTP is included. Figure 4 shows the surface velocities just before blow up.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/217307502-f7e44650-e1d9-4248-b73c-d56bce0f376f.png" width="750"/><br>Fig 4. Surface velocities at hour 21 on 2020.01.01 near Birch Bay WWTP. This run was initialized with the Kantha-Clayson closure mode, and it blew up.</p><br>

---
## Next Steps

I'm glad I found some success with Oak Harbor Lagoon WWTP this week. However, there are still a few things to address:
- How do we improve velocities near Oak Harbor Lagoon WWTP?
  - Maybe a combination of Kantha-Clayson and the prior LO mixing scheme would work.
- Investigate Birch Bay WWTP
  - Would other mixing/advection schemes improve conditions at Birch Bay WWTP?
- Dedicate some time towards exploring the ROMS log files for clues

---
## Small aside on loading

While some of these model test cases were running, I revisted prior plots of DIN loads to Puget Sound. The newly updated version is shown in Figure 6.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/217307492-d61a12cd-ec6d-4086-a196-41f3ae1b82ba.png" width="600"/><br>Fig 6. DIN Loads.</p><br>

Per Parker's recommendation, I added a circle showing ocean loads. I pulled this number from Mackas et al. (1997), but this is an older paper. I'd like to know if there are more recent estimates of DIN load entering Puget Sound from the ocean.

I'd also like to add a label of the total yearly DIN loads for all point sources and rivers. The number for point sources is 64,443 kg/d. The number for rivers might be more complicated than simply summing up DIN loads from all rivers. Since rivers are scattered across the full model domain, I imagine it'd be better to look at only a subset of all rivers. My question is, which subset makes sense? I'm thinking to just use rivers that are in the same areas as WWTPs (from Strait of Juan de Fuca up to Vancouver). Another option is to sum up all rivers that are within frame of Figure 6.