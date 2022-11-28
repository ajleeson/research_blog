## TRAPS Debugging Part IV

This week, Parker and I had a debug session to understand whether ROMS may be confusing Birch Bay WWTP with Birch Bay river since both are simply named "Birch Bay" in the ROMS forcing file. (During this session, I also learned a bit about list comprehension which seems like a useful Python trick.)

Birch Bay river introduces negative transport in the -6 to -4 m3/s range, whereas Birch Bay WWTP should be introducing positive transport of ~0.04 m3/s. In other words, Birch Bay river has two orders of magnitude more transport, of opposite sign, compared to Birch Bay WWTP. 
If we recall the velocity profile of Birch Bay WWTP, the vertical velocity suddenly became very strong and very negative just before ROMS blew up (Fig 1). Thus, it seems like river and WWTP confusion may be the blow up culprit.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/201598043-b57d302c-97c3-445b-bc1d-11719244560a.png" width="500"/><br>Fig 1. Velocity vs. depth and time at the location of the Birch Bay WWTP.</p><br>

Unfortunately, I'm leaving us on a cliffhanger this week-- I still haven't tested this hypothesis. In the upcoming week, I plan to rename the river from "Birch Bay" to "Birch Bay R" such that ROMS can no longer confuse the river with the WWTP.

I will also note that in addition to Birch Bay, there are four other sources in which a river and WWTP share the same name:
Port Angeles, Port Townsend, Port Gamble, and Gig Harbor.
I will add an "R" at the end of the river counterpart of these pairs of sources as well.

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
- [X] (2022.11.16) Add error bars to climatology plots, and save all plots
- [ ] Average Feb 28/Mar 1 data to get a Feb 29 value on non-leap years, so leap years have a larger n
- [ ] Potentially remove 2013/2014 data from Tahuya and Union rivers, so shift doesn't bias climatology
- [X] (2022.11.12) Add logical switches so user can choose to add only tiny rivers or only WWTPs.
- [ ] Have additional logical switch that either does or does not discharge nutrients.
- [X] (2022.11.15) Decide what to do if there are missing values in climatology. Also answer "why are there missing values in climatology?"
- [ ] Why does the Birch Bay treatment plant cause ROMS to blow up??
- [ ] Clean up TRAPS integration code and push update to Github
- [ ] *Modify river name of sources with shared tiny river and point source names. Essentially, add a 'R' at the end of the tiny river name before creating the rivers.nc file.*
- [ ] *Calculate overall statistics for climatology. Replace missing or zero values with mean climatology value for tiny rivers or point sources.*
- [ ] *Take a look at Ecology's new dataset and incorporate it into climatology.*
- [ ] *Write better script to automate process of scraping river/WWTP data from Ecology's website*