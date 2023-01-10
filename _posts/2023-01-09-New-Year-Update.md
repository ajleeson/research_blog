## New Year TRAPS Update

Over the past several weeks I improved tiny river code and attempted a yearlong LiveOcean-with-TRAPS run (Birch Bay WWTP omitted). The yearlong run crashed on February 19th (2021). The cause of failure appears to be related to another point source. I am suspecting that there is a problem with the way I have implemented the point sources. Already I have identified one bug in the point source code. More details are provided below.

---
## River code improvements

For a while I thought that tiny rivers were causing ROMS to crash. This turned out not to be true. However, the misunderstanding gave me an opportuntity to improve tiny river code. The LO_traps repo is up-to-date with all of these improvements.

### Verification of tiny river direction

First, I verified that the directions and indices of tiny rivers are correct by comparing my code to pre-existing LO river code. The tiny river code is based on pre-existing LO river code to begin with. I also verified graphically that tiny river placement is correct. Figure 1 shows a close-up of Eld and Budd Inlets in South Puget Sound.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/211426768-dc14a644-d26a-4ee1-a45f-b7c2e7439aad.png" width="500"/><br>Fig 1. River locations in Eld and Budd Inlets. Arrows indicate the direction of river flow. Pink circles indicate the lat/lon coordinates of the river mouth in Ecology's dataset. These points are connected by a thin line to dark purple circles. These darker circles indicate in which LO gridcell my algorithm places the river mouth. The orange star indicates the location of a pre-existing LiveOcean river. Circled in yellow is an example of two tiny rivers that discharge to the same gridcell. New updates to the TRAPS code combine their flows and add both rivers as a single source.</p><br>

### Consolidating overlapping sources

As an additional improvement, I identified several tiny rivers that discharge to the same gridcell and consolidated their flows. Figure 1 shows an example in which the TRAPS algorithm maps Perry Cr and McLane Cr to the same coastal grid cell.

New updates now combine both rivers into a single sourcce. In this case, the new source would be called "Perry Cr+McLane Cr" in the ROMS forcing file. The flowrate of the new source is the sum of the two individual sources. The temperature and biogeochemistry concentrations are the weighted average (by flowrate) of both sources.

I applied this new consolidation code to point sources as well. The full list of overlapping sources is provided in Figure 2.

According to the [ROMS Wiki](https://www.myroms.org/wiki/River_Runoff), horizontal (river) sources and vertical (WWTP) sources can't be applied to the same gridcell. Thankfully there are no situations in which any combination of pre-existing rivers, tiny rivers, or point sources overlap.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/211429610-a3fdf252-332c-4e94-853b-0ca535c8e820.png" width="500"/><br>Fig 2. Complete list of overlapping sources. Tiny rivers only overlap with tiny rivers. Point sources only overlap with point sources. Nothing overlaps with pre-existing LO rivers. (Thank goodness!)</p><br>

### Adding Ecology's biogeochemistry to pre-existing LO rivers

Lastly, per Parker's request, I added biogeochemistry climatology to pre-existing LO rivers for which Ecology has data. Effectively, I left river position, flowrate, temperature, and other physical properties as-is. I only added biogeochemistry parameters such as nitrate. Figure 3 shows the summary statistics of the biogeochemistry climatology for the pre-existing rivers. Figure 4 shows an example individual climatology summary for the Skokomish River.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/211433574-79d74777-6b23-4fc9-a4d7-a61e8018023f.png" width="800"/><br>Fig 3. Summary statistics of biogeochemsitry climatology for pre-existing rivers.</p><br>

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/211433657-e9dd6fb3-4db5-4d71-b825-8ad956089eb5.png" width="800"/><br>Fig 4. Skokomish River biogeochemistry climatology.</p><br>

The list below includes all rivers to which I added biogeochemistry climatologies. The first name is what is used in Ecology's dataset. The second name in parens is what is used in LiveOcean.

Skagit R (skagit), Skokomish R (skokomish), Snohomish R (snohomish), Stillaguamish R (stillaguamish), Sunshine Coast (clowhom), Capitol Lake (deschutes), Clallam Bay (hoko), Dosewallips R (dosewallips), Duckabush R (duckabush), Dungeness R (dungeness), Elwha R (elwha), Fraser R (fraser), Green R (green), Hamma Hamma R (hamma), Howe Sound (squamish), Lake Washington (cedar), Nisqually R (nisqually), Nooksack R (nooksack), Puyallup R (puyallup), Samish_Bell south (samish)

Note that there are 26 overlapping LiveOcean and SSM rivers, but I only added biogeochemistry to 20 of these rivers. The reason is because 6 of the rivers are "weird rivers" with zero DO, negative TIC, etc. I let LiveOcean handle the biogeochemisty for these rivers however it was already handling them.

---
## Attempted one year run

After I realized that tiny rivers were not causing ROMS to crash, I set up LiveOcean to run for one year (2021) with TRAPS. Birch Bay WWTP was omitted for this experiment. This run also uses default pre-existing river biogeochemistry rather than the updated biogeochemistry based on Ecology's data.

The model ran for over a month (!), but crashed on February 19th. I have yet to conduct in-depth debugging.

For now, I have narrowed down the issue to TRAPS in Oak Harbor. There is both a tiny river and a point source near the blowup region (Figure 5). I haven't verified which source is causing the problem, but I'm suspicious of the Oak Harbor Lagoon WWTP given surface velocity similarities to Birch Bay WWTP blowup. Or perhaps the combination of both sources is causing problems. Regardless, more debugging is needed.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/211435533-9472c30d-60f6-4d6d-8e6a-58cfd107b957.png" width="800"/><br>Fig 5. Surface velocity profiles near Oak Harbor just before ROMS crashed. 2021.02.19 at hour 20.</p><br>

---
## New bug discovered

Recently I realized that I don't understand how ROMS uses grid indices to place a point source. I calculate the nearest coastal grid cell for TRAPS using Python which indexes from 0. For tiny rivers, I could then follow pre-existing river implementation to provide ROMS the correct indices. For point sources, I simply fed ROMS the calculated Python indices, which is likely incorrect.

For one, the u-, v-, and w-velocities are discretized differently on the grid. Am I properly adjusting the indices to the w-grid (rho-grid)? I'm not sure. Furthermore, does ROMS expect indices to start at 0 like Python, or to start at 1 like Fortran?

The tl;dr is that I need to explore this further. Despite my confusion, I'm excited to debug this problem. Who knows, maybe it will even help with Oak Harbor and Birch Bay WWTP.

---
## TRAPS Integration Checklist

**New Items**
- Compare added WWTP DIN load to current LiveOcean DIN load
- Run LiveOcean for a year with and without WWTP nutrients
- Improve understanding of ROMS grid indexing and fix point source indexing

**Open Items**

- Debug why ROMS is crashing
- Verify that the UTM to lat/lon conversion was correct
- Make sure any river mouths located on a narrow strip of land get placed on the correct side of the land (the algorithm is blindly placing rivers on the nearest coastal grid cell without awareness of real geography)
- Compare Ecology's Hood Canal flow data to Mike Brett's data. Conduct other data checking, if possible.
- Determine from which sigma layer each marine point source should discharge, and how to translate this information into the ROMS variable, river_Vshape
- Figure out how to handle climatology for WWTPs that opened after certain year, shut down after certain year, or upgraded treatment type after certain year
- Check SSM flow data for ambiguous duplicate rivers, and decide whether they are or are not actually duplicate rivers
- Average Feb 28/Mar 1 data to get a Feb 29 value on non-leap years, so leap years have a larger n
- Potentially remove 2013/2014 data from Tahuya and Union rivers, so shift doesn't bias climatology
  - New rivers: *Kitsap_Hood, Kitsap NE, NW Hood, Port Gamble*
  - Check for other rivers with similar issue? Pearson correlation test??
- Have additional logical switch that either does or does not discharge nutrients.
- Why does the Birch Bay treatment plant cause ROMS to blow up??
- Take a look at Ecology's new dataset and incorporate it into climatology.
- Write better script to automate process of scraping river/WWTP data from Ecology's website
- Look for any other rivers that may have weird biogeochemistry values

**Completed Items**

- (2022.12.31) Handle river mouths that get placed at the same coastal grid cell. Maybe combine the flows?
- (2022.12.28) Clean up TRAPS integration code and push update to Github
- (2022.12.26) Run LiveOcean for two days with the weird rivers added back in (now that their biogeochemistry values have been replaced with the mean climatology values)
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