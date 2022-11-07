## TRAPS Debugging Part I

This past week was a bit hectic with the quarterly update and midterms. I resolved the ROMS blowup issue, but have yet to do a deep dive into identifying and debugging other issues. So far, I identified Birch Bay treatment plant as a potential problem point. More details below.

---
## Why did LiveOcean Blow Up with TRAPS?

Last week I ran LiveOcean as-is on 2020.01.01, and I ran LiveOcean with TRAPS on 2020.01.01. The with-TRAPS run blew up on hour 21. Parker has a bit of debug code that plots the min and max surface velocities in the model. I used this plot to compare the as-is and with-TRAPS runs on hour 20 of 2020.01.01.

Figure 1 shows the min/max surface velocities for the as-is run. The u-velocity ranges from -2.6 to 1.9 m/s, and the v-velocity ranges from -1.7 to 1.9 m/s. 

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/200236088-e2924263-4403-4468-8c04-dd1b7095982f.png" width="700"/><br>Fig 1. Min/max surface velocities of LiveOcean as-is on hour 20 of 2020.01.01. This figure shows the full LiveOcean grid.</p><br>

Figures 2 and 3 show the min/max surface velocities for the with-TRAPS run. The u-velocity ranges from -3.2 to 2.5 m/s, and the v-velocity ranges from -3.1 to 5.7 m/s. These velocities are larger than the velocities of the as-is run, but they are on the same order of magnitude. What really stands out to me about the with-TRAPS run is that the min and max u and v-velocities all occur at roughly the same point. My initial guess is that TRAPS added either a river mouth or WWTP at this point.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/200236146-994ce50a-7407-47e3-a883-407d762eb8b7.png" width="600"/><br>Fig 2. Min/max surface velocities of LiveOcean with-TRAPS on hour 20 of 2020.01.01. This figure is zoomed in to Puget Sound. The red box denotes the region shown in Figure 3.</p><br>

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/200236214-bf386e5a-de8a-459d-81c9-33628c505642.png" width="800"/><br>Fig 3. Close-up of the min/max surface velocities of LiveOcean with-TRAPS on hour 20 of 2020.01.01. The region depicted here corresponds to the zone defined in Figure 2.</p><br>

After revisiting the map of all TRAPS in LiveOcean, I discovered that the Birch Bay treatment plant is located suspiciously close to the problematic point (Figure 4). The region depicted in Figure 4 overlaps with the same region depicted in Figure 3. Note that there are no river mouths located near the problematic point.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/200236388-98fbdc90-644a-477f-abc2-d0b0d555c514.png" width="500"/><br>Fig 4. Location of WWTPS located near the problematic point. Note that the region depicted here corresponds to the zone defined in Figure 2, and corresponds to the same zone shown in Figure 3.</p><br>

I removed the Birch Bay treatment plant from the LiveOcean grid and ran the model again. This time, the model ran for a full day with TRAPS and did not blow up!

---
## Next Steps

What is it about the Birch Bay treatment plant that caused ROMS to blow up? Nothing about the flow profile appears out of the ordinary (Figure 5). Note that I only ran the model on January 1st, so the discharge rate from Birch Bay treatment plant is just over 0.4 m3/s.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/200236465-2b4700f1-4c76-48d8-b424-181b00f5e7a3.png" width="600"/><br>Fig 5. Select WWTP flowrate profiles, including Birch Bay. Apologies for the quality of the plot-- it's a single subplot in a large figure I have saved on my computer. Calendar day is on the x-axis. Flowrate (m3/s) is on the y-axis.</p><br>

This upcoming week, I'm planning to conduct a detailed review of all climatology and forcing profiles that I have created. Identifying the blow up issue felt *too* easy, and I suspect that there are several underlying issues that I have yet to dig up. Understanding issues with the Birch Bay treatment plant will be a good starting point for this audit.

I also want to note that I ran LiveOcean with TRAPS for *only* one-day. I would not be surprised if ROMS might still blow-up after a few more days. All in all, there is still quite a bit of debugging left to do, but I'm happy to have made some progress so far.

---
## TRAPS Integration Checklist

New items are *italicized*.

- [X] (2022.10.22) Create climatology for TRAPS
- [ ] Verify that my ROMS forcing file generation code does what it should be doing (eg. check my units, timing, etc.)
- [X] (2022.10.30) Try to run LiveOcean myself!
- [ ] Debug why ROMS is crashing
- [ ] Verify that the UTM to lat/lon conversion was correct
- [ ] Make sure any river mouths located on a narrow strip of land get placed on the correct side of the land (the algorithm is blindly placing rivers on the nearest coastal grid cell without awareness of real geography)
- [ ] Compare Ecology's Hood Canal flow data to Mike Brett's data. Conduct other data checking, if possible.
- [ ] Determine from which sigma layer each marine point source should discharge, and how to translate this information into the ROMS variable, river_Vshape
- [X] (2022.10.22) Think about how to handle forecasting?
  - handled by using climatology files
- [ ] Figure out how to handle climatology for WWTPs that opened after certain year, shut down after certain year, or upgraded treatment type after certain year
- [ ] Check SSM flow data for ambiguous duplicate rivers, and decide whether they are or are not actually duplicate rivers
- [ ] Handle river mouths that get placed at the same coastal grid cell. Maybe combine the flows?
- [ ] Add error bars to climatology plots, and save all plots
- [ ] Average Feb 28/Mar 1 data to get a Feb 29 value on non-leap years, so leap years have a larger n
- [ ] Potentially remove 2013/2014 data from Tahuya and Union rivers, so shift doesn't bias climatology
- [ ] Add logical switches so user can choose to add only tiny rivers or only WWTPs. If WWTPs are added, have additional logical switch that either does or does not discharge nutrients.
- [ ] *Decide what to do if there are missing values in climatology. Also answer "why are there missing values in climatology?"*
- [ ] *Why does the Birch Bay treatment plant cause ROMS to blow up??*