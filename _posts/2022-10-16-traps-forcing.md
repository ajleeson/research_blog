## TRAPS Forcing Update

This week I debugged my TRAPS placement algorithm and created a preliminary ROMS forcing file with constant values for river and WWTP discharge rates. Parker gave the file a test run on LiveOcean, and it sounds like it worked!
In other words, the plumbing exists and I can now focus on fixing the leaks.

A summary of work, list of remaining issues/next steps, and a tentative one year plan are detailed below.

---
## TRAPS Placement Algorithm

I battled a lot of bugs this week. I won't go into the details, but the main symptoms were that several river mouths were offset from the coast, and others were discharging into land (Figure 1).

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/196086257-f87196c4-d5cb-4b4c-a113-29efb569e776.png" width="250"/><br>Fig 1. Example issues with the river code. In this image, Mill Cr is offset from the coast and Agate East is discharging into land.</p><br>

The add_river_tracks code was helpful for debugging these problems. Figure 2 shows the final result, with river mouths located solely on coastal grid cells and pointing in the correct direction.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/196086305-d231b8a7-70e0-48d7-8503-e134ed7f715b.png" width="600"/><br>Fig 2. River mouths that I now believe to be in the correct location and oriented in the correct direction. This image is a close-up of the Budd, Totten, etc. Inlets on the LiveOcean grid.</p><br>

Note that I wasn't concerned about the WWTP locations for a few reasons:
1. Unlike rivers, WWTPs can be located in the water.
2. WWTPs don't have an orientation since they are vertical momentum sources (rather than horizontal).
3. WWTPs use the same algorithm that I wrote over the summer, which I had tested with some simple test cases.

After fixing the rivers, I created a forcing file with 5 $m^3 s^{-1}$ transport for all rivers, and 4.5 $m^3 s^{-1}$ transport for all marine point sources. Parker helped test the forcing on LiveOcean, and it worked. Now I can work on fixing the other details.

---
## Next Steps (ie. the other details that I need to fix)

In previous blog posts, I wrote about some anticipated issues with integrating TRAPS into LiveOcean. I dealt with some of the issues already, but other new to-do's arose as well. Below is the consolidated list of remaining tasks before TRAPS will be fully functional on LiveOcean:

1. Create climatology for TRAPS (In other words, I need to use real data to force rivers and marine point sources)
2. Verify that the UTM to lat/lon conversion was correct
3. Make sure any river mouths located on a narrow strip of land get placed on the correct side of the land (the algorithm is blindly placing rivers on the nearest coastal grid cell without awareness of real geography)
4. Compare Ecology's Hood Canal flow data to Mike Brett's data. Conduct other data checking, if possible.
5. Try to run LiveOcean myself!
6. Verify that my ROMS forcing file generation code does what it should be doing (eg. check my units, timing, etc.)
7. Determine from which sigma layer each marine point source should discharge, and how to translate this information into the ROMS variable, river_Vshape
8. Think about how to handle forecasting?

Please let me know if there is anything else you think would be valuable to add to this list.

---
## Tentative Plan for the Next Year

This summer we made a tentative one-year plan. Since King County has asked for a one-year schedule, it's probably a good time to revisit our plans:

**Autumn 2022**
- Finish integrating TRAPS into LiveOcean
- Run the model for a year
- Write some READMEs
- Design an experiment with LiveOcean
- Run idealized npzd models on the side
  
**Winter 2023**
- Analyze LiveOcean data from Autumn and determine best method of data analysis
- Continue running experiments in the background
- Dig into literature (to help develop hypotheses for future experiments)
- Maybe reach out to PhD students working on Long Island Sound or Chesapeake

**Spring 2023**
- Begin documenting results
- Continue running experiments in the background
- Prepare for conference? (timing may work if I've started documenting at this point)

We also discussed completinig the first draft of my Master's Thesis by the end of the 2023 calendar year.

This plan still seems reasonable to me. I am interested in discussing what types of experiments seem appropriate for the thesis. Would it be reasonable to run experiments that could be used for intermodel comparisons later on? Or would it make more sense to run more independent experiments?