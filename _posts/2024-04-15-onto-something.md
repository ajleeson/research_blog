## I think we're on to something

This week I spent some time looking at relationships between hypoxia and depth. In Puget Sound, it seems that bottom hypoxia will only occur if the water column depth is between 10 m and 50 m deep (for the most part), and it generally only appears in terminal inlets. This is exciting! We finally have some spatial description of where hypoxia will most likely occur.

Additionally, I generated TRAPS forcing (with no WWTP loading) for the year 2014. Now that the forcing is done and driver_roms3 is (presumably) done, I am ready to launch the 2014 run whenever there is availability on klone.

I also spent some time comparing Ecology's SUNA nitrate data to the timeseries I am using for rivers. Overall, the SUNA measurements seem reasonable, but the values fluctuate a lot.

Lastly, I included a brief summary of a proposed project for my numerical modeling class. I'm hoping to create an analytical Hood Canal-esque estuary in ROMS, and test the sensitivity of stratification to wind stress direction and mixing scheme.

More details below.

---
## We are on to something: hypoxia's relationship to depth

Last week, Alex and I looked at the number of hypoxic days in our LiveOcean runs. We decided to begin investigating where and when hypoxia is most likely to occur in Puget Sound. One of our hypotheses was that there is some sweet spot of depth in which terminal inlets that are shallow, but not *too* shallow, are most likely to experience hypoxia. This week I generated some figures that seem to agree with our hypothesis.

First, Figure 1 shows a map of the number of hypoxic days in Puget Sound bottom waters for the year 2013. This plot is a repeat figure from last week's [blog post](https://ajleeson.github.io/research_blog/2024/04/09/quantifying-do-differences.html). I am showing it again to highlight that I cropped out data from the Straits for the analysis in Figure 2.

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/da776886-3a9b-439b-9c49-46178d27feaf" width="500"/><br>Fig 1. Puget Sound maps with days in which bottom DO < 6 mg/L, with regions outside of Puget Sound cropped out.</p><br>

Figure 2 shows a 2D historgram of water column depth vs. bottom DO concentration from the natural run. The points used in this scatter plot come from every single grid cell highlighted in Figure 1-a, on every single day of the year. (Note that I cropped out data from the Straits so that they don't interfere with any potential signals we might observe in Puget Sound).

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/82bba4c9-02f2-49c0-a97b-cb221914a4a1" width="500"/><br>Fig 2. 2D histogram of water column depth vs. natural bottom DO. The total points on this figure correspond to points from every single grid point on every single day of the year (i.e. number of gridpoints in Puget Sound times 365 days = number of points in this histogram plot). Colorbar indicates the number of points that occupies a location on this figure, with counts reported in log scale. 100 x 100 bins.</p><br>

Based on Figure 2, it is very clear that bottom hypoxia (DO < 2 mg/L) generally only appears if the water column depth is shallower than 50 m. Note that I am only showing data from the natural run, but the 2D histogram for the anthropogenic run looks nearly identical to the natural run.

This is an exciting finding! However, we lose both temporal and spatial resolution in this 2D histogram. To re-gain our spatial understanding, I generated some categorical maps that highlight the co-occurence of bottom hypoxia and shallow water depths. Note that these maps now span over the entire region in the map (including in the Straits). Also note that hypoxic occurence is "True" if the bottom water measured < 2 mg/L for at least one day of the year 2013.

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/e3ad6f7b-e648-46ea-b4d8-7e6f37dad102" width="800"/><br>Fig 3. Categorical maps showing the co-occruence of hypoxia and shallow water depth.</p><br>

Most of the coloring on the map is <span style="color:orange">**orange**</span>, indicating that the water column depth is between 10 - 50 m deep, and the bottom waters did not become hypoxic.

However, several terminal inlets (like Penn Cove, Holmes Harbor, Lynch Cove, etc) are <span style="color:red">**red**</span>, indicating that the depths are between 10 - 50m deep, and the bottom waters became hypoxic at least one day of the year.

Looking at shallower depths, <span style="color:royalblue">**blue**</span> indicates that the water column depth is shallower than 10 m, and the bottom water did not become hypoxic.

The color <span style="color:turquoise">**green**</span> denotes where the water column depth is shallower than 10 m, and the bottom water did become hypoxic. When I squint at this figure, I only see small speckles of green in Lynch Cove and maybe a sliver in Bellingham Bay. Generally, there is much more blue than green on this map, suggesting that very shallow water (< 10 m) tends to not be more resistant to hypoxia.

Lastly, <span style="color:purple">**purple**</span> indicates that the water column depth is deeper than 50 m, and the bottom water became hypoxic. There is almost no purple on these maps, except for a few grid cells in Holmes Harbor (and maybe Point Susan in the anthropogenic run).

Any uncolored water means that the water column is deeper than 50 m, and that the bottom water never became hypoxic. Given that most of the water is uncolored, and there is limited purple, we can conclude that deep water (> 50 m) is unlikely to become hypoxic in Puget sound.

In summary, these findings suggest that bottom hypoxia is most likely to develop in Puget Sound when water column depths are between 10 - 50 m deep, and typically hypoxia seems to only develop in terminal inlets that meet this depth criteria.

Next I'll start looking at DO variability in time. Once I identify the months with the most prevalanet hypoxia, I will begin looking at maps of average bottom DO over the hypoxic season and assess the distance between hypoxic hotspots and large WWTPs. In the coming week I also plan to type up a draft outline of my first paper.

---
## 2014 TRAPS forcing

I believe I am finally ready to launch the 2014 run. This week I finished generating new TRAPS forcing for 2014, and last week I modified driver_roms3 to use Parker's previously generated atm, ocn, and tide forcing. 

When is a good time to run this experiment? Based on our findings from last quarter, I will need to take up all 10 nodes on klone. 

---
## SUNA data

This week I also took a look at SUNA data from Ecology. Parker previously shared an email with me from Christopher Krembs containing links to seven different SUNA monitoring stations in the region. These stations are located on the following river moorings:

- Nooksack River
- Skagit River
- Snohomish River
- Cedar River (Lake Washington)
- Duwamish River (Green River)
- Puyallup River
- Nisqually River

From what I read, the SUNA instrument is intended to measure nitrate (not ammonium), but high concentrations of nitrite can sometimes interfere with measurements. 

The NO3 data I use for TRAPS are technically a "NO3 + NO2" concentration, so comparing those data to the SUNA data seems appropriate. 

Figure 5 shows a timeseries comparison of my TRAPS climatologies to all existing SUNA data for the aforementioned rivers.

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/d9215eb0-8db8-438d-9d4b-5ac337942b54" width="800"/><br>Fig 4. SUNA data comparison to TRAPS climatologies (all data from WA Department of Ecology). Dark and light shading around the TRAPS climatologies represent the +/- 1 and 2 standard deviation profiles, respectively.</p><br>

A few caveats worth pointing out:
1. TRAPS climatologies are based on data from 1999 - 2017, which do not include the years 2023 - 2014 that the SUNA data covers
2. The TRAPS climatologies are based on datasets generated from multiple linear regression (not necessarily on actual NO3 measurements). Thus, there is inherently some amount of guesswork woven into the TRAPS data.
3. TRAPS climatologies are averages, so high frequency fluctuations (if they exist) are filtered out.
4. There are no SUNA data during the summer months, so we can't assess the nitrate minima during the summer.

Given these caveats, it seems like the SUNA data generally agree with the TRAPS climatology values. The SUNA data has much more variability, but the average values don't seem to be that far from what we are already using in TRAPS. 

---
## Class modeling project

For Christie's numerical modeling class, we each need to conduct a numerical modeling experiment. I proposed to build an idealized Hood Canal estuary, with simplified bathymetry. I plan to make Hood Canal a straight estuary (no hook shape), but with approximately the same total length, and roughly the same bottom slope. I'll also add an idealized block sill at the mouth, and one river at the head (with a constant flowrate equal to the sum of the average discharge of all rivers in Hood Canal).

The goal of my experiment is to assess the sensitivity of stratification to wind stress direction (north vs. south) and mixing scheme. I do not intend to add an atmosphere-- just surface wind stress. I could potentially measure the strenght of stratification by looking at transects and roughly estimating the thickness of the halocline. Additionally, I could make maps of the salinity difference between the surface and bottom of the water column. (I hope to conduct relatively qualitative analysis).

I'm hoping to hear your feedback on this project. My goal is to design something that is somewhat useful for my research, but isn't too complicated. I also want to check whether we have any nodes on mox I could use for these experiments, or whether it would be best to design something smaller I can run on my computer, or just run on the STF hyak nodes. Looking forward to hearing your thoughts!

