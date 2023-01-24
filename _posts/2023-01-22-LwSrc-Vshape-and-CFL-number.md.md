## LwSrc, Vshape, and CFL numbers

The first section of this post provides a brief overview of activity since the last KC meeting. The second section provides more detail on this past week's acitivity.

This week I created new plots of CFL number in the with/without WWTP run for both Oak Harbor Lagoon WWTP and Birch Bay WWTP. A CFL condition violation appears to be the cause of the Oak Harbor Lagoon WWTP crash, but it is more ambiguous for Birch Bay WWTP.

Rather than discharging solely to the bottom sigma layer, this week I tested discharging point sources to the bottom 1/3 sigma layers (bottom 10 layers). This run did not blow up at Oak Harbor Lagoon WWTP. However, I was also unable to reproduce the original Oak Harbor Lagoon WWTP blowup issue.

 Later on I realized that re-initializing a run as a *continuation* vs a *perfect restart* changes whether ROMS does or does not crash at Oak Harbor Lagoon WWTP. Thus, it is inconclusive whether redistributing point source discharge actually resolves blowup problems. Repeat-tests are required.

# Summary since last KC update

When we last met with KC, I had shared that I was able to add TRAPS (tiny rivers and point sources) to the LiveOcean grid. For each of these sources, I also generated an average year-long climatology profile for flowrate, temperature, and biogeochemistry variables. Figure 1 shows summary statistics for tiny river climatology. The left panel in Figure 2 shows the location of point sources on the LiveOcean grid.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/208981106-566997f6-6184-43b2-8416-27142192fef6.png" width="700"/><br>Fig 1. Average climatology for all tiny rivers (not includeing pre-existing LiveOcean rivers and omitting rivers with weird biogeochemistry).</p><br>

Since then, I have made several improvements to the TRAPS code. For instance, I consolidated TRAPS that discharge to the same grid cell. Rather than having two sources being input to the same cell, I instead combined their flows and nutrient concentrations and added them as a single source. I also identified several tiny rivers with weird biogeochemistry parameters (shared list with Ben). These rivers had negative TIC values, and near-zero DO. Instead of including these weird biogeochemisty values in the model, I forced the weird rivers with the average climatology of the other rivers (solid purple lines in Fig 1).

Despite this progress, I still have not run LiveOcean for a year and come up with DO results. Why not? WWTPs have been causing ROMS to blow up. For nearly two months, Birch Bay WWTP was generating massive horizontal and vertical velocities that caused the model to blow up on Day 1. The upper right three panels in Figure 2 show the surface velocities near the Birch Bay WWTP. After being unable to resolve the issue, we opted to remove Birch Bay WWTP from the model.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/213941218-9132cd80-850a-45e4-842a-2d290a09ec51.png" width="900"/><br>Fig 2. Point source and point source problems. The left most panel shows where point sources were added to the LiveOcean grid. The three panels in the upper right show surface velocities near Birch Bay WWTP prior to model blow up on 2020.01.01. The three panels in the lower left show surface velocities near Oak Harbor Lagoon WWTP prior to model blow up on 2021.02.19.</p><br>

Afterwards, I attempted a yearlong (2021) LiveOcean run with TRAPS. The model ran smoothly for 1.5 months but then blew up on February 19th at the Oak Harbor Lagoon WWTP. This blowup had similar symptoms to the Birch Bay WWTP blowup, with massive spikes in velocity near the WWTP (see bottom right three panels in Figure 2). It also has a dramatic jump in SSH at the WWTP and sudden vertical homogeneity of biogeochemistry variables. I shared the issue with the ROMS community online, and they suspected a CFL condition violation. WWTPs in the model discharge solely to the bottom sigma layer. Even though their flowrate is small compared to rivers, discharging to only one vertical layer may be enough to cause a CFL violation. Thus, this past week I tried distributing WWTP discharge across the bottom 1/3 sigma layers (10 layers instead of 1). More details are in the next section.

# Updates from the past week

## CFL number of prior runs

Per Alex's suggestion, I created depth vs. CFL number plots over time at the location of the Oak Harbor Lagoon WWTP. Figure 3 shows a comparison of CFL number for a run with WWTPs (that blew up), and a run without WWTPs (that ran just fine).

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/213944497-a99a7c21-cec1-4f48-8e7c-4c3470e3eeef.png" width="850"/><br>Fig 3. CFL number at location of Oak Harbor Lagoon WWTP on 2021.02.19. Depth is on the y-axis, time is on the x-axis. With the WWTP, the run blew up at hour 21. Without the WWTP, the run ran to completion. The dashed line indicates the time when the "With WWTP" run blew up.</p><br>

With the Oak Harbor Lagoon WWTP, the vertical velocity CFL number appears to reach, or exceed, one just before blowup. Based on these results, it appears as though a CFL condition violation is indeed what caused ROMS to crash at Oak Harbor Lagoon WWTP.

## Discharging to bottom 1/3 sigma layers

This week I ran the model starting on 2021.02.18 (one day before Oak Harbor Lagoon WWTP blew up), except with WWTPs discharging to the bottom 1/3 sigma layers. The test ran successfully. Figure 4 shows the surface velocities near Oak Harbor Lagoon WWTP at hour 20 of 2021.02.19 (which is when prior runs had blown up).

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/213952831-11874371-20f2-4623-9b1d-7d8c47279285.png" width="750"/><br>Fig 4. Surface velocities near Oak Harbor Lagoon WWTP on hour 20 of 2021.02.19. This model run had WWTPs that discharged to the bottom 1/3 sigma layers rather than solely the bottom sigma layer.</p><br>

At this point, I have had two successful model runs since Oak Harbor Lagoon WWTP first caused a blowup. Last week I had a successful run after removing all WWTPs from the model. This week I had a successful run after updating WWTPs to discharge to more sigma layers.

To be completely sure that the conditions in both of these runs solved the blowup issue, I decided to rerun the model starting on 2021.02.18, but with WWTPs discharging to just the bottom sigma layer. This run uses the exact same conditions that initially blew up. (I double checked the forcing and log files to be sure). I made zero changes to the forcing files that caused a blowup, and simply re-ran the model as a continuation from 2021.02.18. Essentially, I used this run to verify that I can turn the blowup problem on and off.

For some reason, this run didn't blow up. Adding WWTPs back into the model discharging solely to the bottom sigma layer didn't cause a blowup. In fact, the surface velocities for this run on hour 20 of 2021.02.19 (Fig. 5) are nearly identical to when WWTPs discharged from the bottom 1/3 sigma layers.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/213952879-38345fe6-9783-4599-b801-7e67f5b2edf1.png" width="750"/><br>Fig 5. Surface velocities near Oak Harbor Lagoon WWTP on hour 20 of 2021.02.19. This model run had WWTPs that discharged solely to the bottom sigma layer.</p><br>

Now my question is: why didn't this run blow up? Or perhaps, why did the original run blow up? Could the sheer fact that I re-initialized my run as a continuation on 2021.02.18 change the resulting outcome on 2021.02.19?

My other question is: does discharging from the bottom 1/3 sigma layers really improve anything? To answer this, I decided to revisit Birch Bay WWTP.

## Revisiting Birch Bay WWTP

We never truly understood why Birch Bay WWTP was causing ROMS to blowup. I have decided to create a CFL number plot at Birch Bay WWTP to check whether a CFL condition violation may also be a likely culprit (Fig. 6). Unlike Oak Harbor Lagoon WWTP, the CFL numbers don't appear as high near Birch Bay WWTP. This does not mean that a CFL condition violation was not the cause of blowup, but it is also not as clearly the culprit for blowup. Regardless, it will still be valuable to test a new point source distribution at Birch Bay WWTP (especially if I can confirm that Birch Bay still blows up when discharging solely from the bottom sigma layer).

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/213944983-04223119-6dad-4169-8536-e831772340ba.png" width="850"/><br>Fig 6. CFL number at location of Birch Bay WWTP on 2020.01.01. Depth is on the y-axis, time is on the x-axis. With the WWTP, the run blew up at hour 21. Without the WWTP, the run ran to completion. The dashed line indicates the time when the "With WWTP" run blew up.</p><br>

I have made several improvements to TRAPS integration since removing Birch Bay WWTP from the model. Thus, my first test was to add Birch Bay WWTP back and confirm that this point source still causes ROMS to blow up.

Previously, Birch Bay WWTP was causing issues on 2020.01.01. For consistency, I tested the new run on the same day. Thankfully, it blew up.

Afterwards, I distributed point source discharge to the bottom 1/3 sigma layers instead of solely the bottom sigma layer. Birch Bay WWTP still blew up.

(Unfortunately I can't create figures to compare these runs because klone is down right now. When it's back up, I'd be curious to see whether these runs blew up in the same way.)

At least for Birch Bay WWTP, distributing point source transport to the bottom 1/3 sigma layers does not prevent ROMS from blowing up.

## Re-initializing as a perfect restart

I spoke with Jilian about how ROMS gave me different results at Oak Harbor Lagoon, even though my test cases used the same forcing. She recommended that I try to run the model again, but to use a *perfect restart* initialization instead of a *continuation* (two different restart options).

Once again, I restarted a run with WWTPs discharging from solely the bottom sigma layer on 2021.02.18. This time, ROMS blew up on 2021.02.19 as expected (whew).

At this point, it would be good to repeat the following tests starting on 2021.02.18. This time, I will initialize the runs using perfect restart:
- Remove all WWTPs (to re-verify that Oak Harbor Lagoon WWTP, and not Whidbey east tiny river, is causing the blow up issue)
- Discharge point sources to the bottom 1/3 sigma layers

If I can confirm that discharging to the bottom 1/3 sigma layers resolves the Oak Harbor Lagoon WWTP problem, then I'll attempt another one-year run with this solution. I'll also continue to omit Birch Bay WWTP because it is still an unresolved problem.

## Update on placement of LwSrc's on the rho-grid

After much confusion, I've come to the conclusion that the placement and indexing of point sources on the LiveOcean grid has been correct all along. No changes are necessary.