## TRAPS Debugging Part V

This week I modified several river names to differentiate between rivers and point sources that share the same name. Most notably, Birch Bay was one such source in which both the river and WWTP shared the same name. However, this change did not fix the Birch Bay WWTP blow up problem.

I also took a closer look at Ecology's DIC data and discovered why the DIC profiles look like monthly averages with daily variability.

More details are below.

Next week, I'd like to move Birch Bay WWTP to a more stable location on the LO grid so I can make progress on other TRAPS action items.

---
## Fixing Rivers and WWTPs with Duplicate Names

In [last week's blog post](https://ajleeson.github.io/research_blog/2022/11/27/TRAPS-debugging-part4.html), I discussed how there are several pairs of sources in which the river and WWTP share the same name. 'Birch Bay' is one of these duplicates. We hypothesized that ROMS may have been confusing Birch Bay river and WWTP, and thus applying the river transport at the WWTP location.

To fix the duplicate issue and to test our hypothesis, I appended a 'R' at the end of the river names. Thus, Birch Bay river became 'Birch Bay R,' while the WWTP remained 'Birch Bay.' This change ensures that ROMS will not mix up the two sources.

I was feeling confident that this naming issue was the source of the Birch Bay blow up problem. Sadly, that does not seem to be the case. After running LiveOcean with corrected names (and verifying that the names were indeed corrected in the forcing file), the model still blew up at hour 21 at the Birch Bay WWTP. Figure 1 shows the surface velocities at hour 20.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/204687140-68d1d8ef-0e28-40e2-b88a-8c88175cab58.png" width="850"/><br>Fig 1. Surface velocity near the Birch Bay WWTP just before model blowup. In this run, duplicate river and WWTP names have been differentiated.</p><br>

---
## LiveOcean with *only* Birch Bay WWTP

Is the Birch Bay WWTP alone, with its 0.04 m3/s transport, really the issue? Could other rivers or WWTPs be interacting or getting mixed up with Birch Bay WWTP and causing the blow up issue? To test this hypothesis I ran LiveOcean with pre-existing rivers, but no TRAPS except for the Birch Bay WWTP.

The simpler outcome would be if this run still blows up at hour 21, indicating that Birch Bay WWTP alone is the problem.

The more troubling outcome would be if this run does not blow up. Not blowing up would indicate that Birch Bay WWTP alone is not the problem. Rather, some other sources are interfering or interacting with Birch Bay WWTP causing the blow up issue.

Luckily, this run blew up at hour 21. Figure 2 shows the surface velocity profiles at hour 20.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/204698272-0052f2be-88b6-40ca-bf20-5472094e41a1.png" width="850"/><br>Fig 2. Surface velocity near the Birch Bay WWTP just before model blowup. This run includes pre-existing LiveOcean rivers and Birch Bay WWTP, but no other TRAPS.</p><br>

Birch Bay WWTP alone, at its particular location, is the problem.

---
## What to do with Birch Bay WWTP?

At this point, what is the best thing to do with Birch Bay WWTP? Shifting the point source out to sea by one grid cell was not sufficient to prevent blow up issues. Is it worth shifting the source seawards by two grid cells, or perhaps shifting it north or south by one grid cell? Or is it better to pursue the specific reason that Birch Bay WWTP is causing ROMS to blow up?

I would prefer to shift Birch Bay WWTP to a more stable location so I can focus on cleaning up other parts of the TRAPS integration code.

---
## Follow-up on DIC

When we met a while back, there were some questions about tiny river DIC profiles that looked like a strange combination of monthly averages and real data. This week I did some digging into how Ecology came up with their DIC values.

Pelletier et al. (2017) describes how acidification was added to SSM, and with it, inorganic carbon chemistry. It seems that DIC for rivers are calculated from pH and total alkalinity.

As it turns out, Ecology's dataset often contains daily values for pH, but only monthly values for alkalinity. Thus, DIC profiles appear to have a combinaton of daily variability as well as large monthly jumps. Figure 3 shows an example of DIC, pH, and total alkalinity profiles for Miller Creek.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/205527259-e0bebb00-3bc4-4209-bdf9-c9c880ceb209.png" width="450"/><br>Fig 3. Miller Creek DIC, pH, and alkalinity profiles.</p><br>

A few rivers have only monthly profiles for both pH and alkalinity. The resulting DIC profiles for these rivers thus only look like monthly averages. Figure 4 shows an example of such a river in Victoria that discharges into the Strait of Juan de Fuca.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/205527264-b0eb6c03-b256-45d0-85ff-b9402788a487.png" width="450"/><br>Fig 4. Victoria, Strait of Juan de Fuca DIC, pH, and alkalinity profiles.</p><br>

---
## TRAPS Integration Checklist

**Open Items**

- Verify that my ROMS forcing file generation code does what it should be doing (eg. check my units, timing, etc.)
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
- Calculate overall statistics for climatology. Replace missing or zero values with mean climatology value for tiny rivers or point sources.
- Take a look at Ecology's new dataset and incorporate it into climatology.
- Write better script to automate process of scraping river/WWTP data from Ecology's website

**Completed Items**

- (2022.11.30) Why do DIC data sometimes look like a mix of real data and monthly steps?
- (2022.11.29) Modify river name of sources with shared tiny river and point source names. Essentially, add a 'R' at the end of the tiny river name before creating the rivers.nc file.
- (2022.11.16) Add error bars to climatology plots, and save all plots
- (2022.11.15) Decide what to do if there are missing values in climatology. Also answer "why are there missing values in climatology?"
- (2022.11.12) Add logical switches so user can choose to add only tiny rivers or only WWTPs.
- (2022.10.30) Try to run LiveOcean myself!
- (2022.10.22) Create climatology for TRAPS
- (2022.10.22) Think about how to handle forecasting?
  - handled by using climatology files

---
## References

Pelletier, G., Bianucci, L., Long, W., Khangaonkar, T., Mohamedali, T., Ahmed, A., & Figueroa-Kaminsky, C. (2017). Salish Sea Model Ocean Acidification Module and the Response to Regional Anthropogenic Nutrient Sources. https://fortress.wa.gov/ecy/publications/SummaryPages/1703009.html
