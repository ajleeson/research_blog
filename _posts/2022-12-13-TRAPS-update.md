## TRAPS Update

This week after unsuccessfully attempting to shift Birch Bay WWTP seawards, I decided to remove the point source entirely. After doing so, LiveOcean was able to run for two days.

I also calculated some simple summary statistics (means and standard deviations) of the climatology values for rivers and point sources. These mean values were then used to replace the climatology of rivers with weird biogeochemistry data. More details below.

---
## Shifting Birch Bay WWTP Seawards

I decided to shift Birch Bay WWTP seawards by two grid cells. Previously, shifting this souce seawards by one grid cell was not enough to eliminate all instabilities. I was hoping that shifting Birch Bay WWTP seawards by two grid cells would be sufficient to prevent ROMS from blowing up.

Unfortunately, this run also blew up at Birch Bay WWTP. In fact, it blew up one hour earlier than it normally does. The surface velocities prior to blow up are shown in Figure 1.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/207182359-7b2051dd-e51d-4f0c-bcf7-1b076292ce5f.png" width="850"/><br>Fig 1. Surface velocity near the Birch Bay WWTP just before model blow up.</p><br>

---
## Removing Birch Bay WWTP Entirely for Now

To prioritize making progress on TRAPS integration, I have opted to remove Birch Bay WWTP for now. After removing Birch Bay WWTP, I let LiveOcean run for two days (2020.01.01-2020.01.02) because I have so far only run the model for one day. This experiment was successful, and ROMS did not blow up on either day.

---
## Ranked Loads

Given that I've removed Birch Bay WWTP for now, it's important to understand how large of a DIN source it was compared to other point sources. Figure 2 shows this comparison. By eye, it seems like Birch Bay WWTP is a mid-sized discharger.

In this case, DIN is defined as the sum of nitrate, nitrite, and ammonium.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/207553622-e9449244-3324-4e8a-9aed-9f023122ae68.png" width="400"/><br>Fig 2. Max daily vs. mean yearly DIN load of all point sources. Note the log-log scale.</p><br>

---
## Climatology Summary Statistics

I did a bit of work compiling averages for the point source and nonpoint source climatology files. The figures below show the average climatology generated from Ecology's point sources (Fig 3) and Ecology's nonpoint sources (Fig 4).

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/207200794-afefda64-0f21-4fc0-98ea-acd5509bc895.png" width="700"/><br>Fig 3. Average climatology for all point sources.</p><br>

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/207488297-1f58d99d-24df-4c0f-95ca-7f9ee982e35f.png" width="700"/><br>Fig 4. Average climatology for all nonpoint sources (rivers).</p><br>

Figure 5 shows a zoomed-in image of average river flowrate.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/207488472-34cb17c7-0dfd-408a-9faa-882ab4aff7ff.png" width="400"/><br>Fig 5. Average river flowrate for all nonpoint sources (rivers).</p><br>

In a prior blog post, I discussed that some rivers in Ecology's dataset have strange biogeochemistry profiles. An example of Tsitika River is shown in Figure 6. I omitted these weird rivers when calculating the average nonpoint source (river) climatology.

---
## Replacing Biogeochemisty Values of Weird Rivers

The original Tsitika River climatology is shown in Figure 6. Tsitika river is one of several rivers that have identical, weird biogeochemistry values in Ecology's dataset.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/202876037-097a10d9-31e9-4532-a24a-a92e466c7e84.png" width="650"/><br>Fig 6. Original Tsitika River climatology values.</p><br>

The flowrate for the weird rivers do look reasonable, however. Thus, I replaced the weird river biogeochemistry values with the average river climatology discussed in the prior section. River flowrate and temperature were kept the same. The new, resulting Tsitika River climatology is shown in Figure 7.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/207489593-d2c0c6b5-f5b0-47db-a173-89a5bdd1297d.png" width="650"/><br>Fig 7. New Tsitika River climatology values after replacing strange values with average climatology for other rivers.</p><br>

There is still room for improvement here. First, I'd like to show Tsitika River flow profiles for all years like is shown in Figure 6. It just happens that the order that I wrote my code does not make it easy to plot flow values for individual years after replacing biogeochemistry values with the average climatology of all rivers. Maybe there's an opportunity for code optimization/restructuring here.

Something else worth mentioning is that I am currently only filling in biogeochemistry values for a list of weird rivers (which I identified as having negative TIC, near zero DO, zero alkalinity, and overall strange values for all biogeochemistry parameters). However, there are also a number of rivers which might have only one strange biogeochemistry profile. Currently, my code isn't dealing with these rivers.

---
## TRAPS Integration Checklist

**New Items**
- Run LiveOcean for two days with the weird rivers added back in (now that their biogeochemistry values have been replaced with the mean climatology values)
- Look for any other rivers that may have weird biogeochemistry values
- Remove code that looks for SSM/LO duplicate rivers from trapsfun.py, since these are now handled in make_climatology_tinyrivs.py

**Open Items**

- Debug why ROMS is crashing
- Verify that the UTM to lat/lon conversion was correct
- Make sure any river mouths located on a narrow strip of land get placed on the correct side of the land (the algorithm is blindly placing rivers on the nearest coastal grid cell without awareness of real geography)
- Compare Ecology's Hood Canal flow data to Mike Brett's data. Conduct other data checking, if possible.
- Determine from which sigma layer each marine point source should discharge, and how to translate this information into the ROMS variable, river_Vshape
- Figure out how to handle climatology for WWTPs that opened after certain year, shut down after certain year, or upgraded treatment type after certain year
- Check SSM flow data for ambiguous duplicate rivers, and decide whether they are or are not actually duplicate rivers
- Handle river mouths that get placed at the same coastal grid cell. Maybe combine the flows?
- Average Feb 28/Mar 1 data to get a Feb 29 value on non-leap years, so leap years have a larger n
- Potentially remove 2013/2014 data from Tahuya and Union rivers, so shift doesn't bias climatology
  - New rivers: *Kitsap_Hood, Kitsap NE, NW Hood, Port Gamble*
  - Check for other rivers with similar issue? Pearson correlation test??
- Have additional logical switch that either does or does not discharge nutrients.
- Why does the Birch Bay treatment plant cause ROMS to blow up??
- Clean up TRAPS integration code and push update to Github
- Take a look at Ecology's new dataset and incorporate it into climatology.
- Write better script to automate process of scraping river/WWTP data from Ecology's website

**Completed Items**

- (2022.12.13) Replace missing or zero values with mean climatology value for tiny rivers or point sources (like the weird rivers)
- (2022.12.12) Calculate overall statistics for climatology.
- (2022.12.07) Verify that my ROMS forcing file generation code does what it should be doing (eg. check my units, timing, etc.)
- (2022.11.30) Why do DIC data sometimes look like a mix of real data and monthly steps?
- (2022.11.29) Modify river name of sources with shared tiny river and point source names. Essentially, add a 'R' at the end of the tiny river name before creating the rivers.nc file.
- (2022.11.16) Add error bars to climatology plots, and save all plots
- (2022.11.15) Decide what to do if there are missing values in climatology. Also answer "why are there missing values in climatology?"
- (2022.11.12) Add logical switches so user can choose to add only tiny rivers or only WWTPs.
- (2022.10.30) Try to run LiveOcean myself!
- (2022.10.22) Create climatology for TRAPS
- (2022.10.22) Think about how to handle forecasting?
  - handled by using climatology files