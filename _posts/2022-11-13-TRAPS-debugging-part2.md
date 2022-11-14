## TRAPS Debugging Part II

This week I continued debugging the Birch Bay WWTP. Unfortunately, I still do not know why this WWTP is causing ROMS to blow up. However, the problem appears to be related to the location of the WWTP in the LiveOcean grid, rather than an inherent problem with the Birch Bay WWTP itself. More details below.

I have also finally published the TRAPS integration code on a public [GitHub repo](https://github.com/ajleeson/LO_traps). Disclaimer: the code isn't too clean and I haven't tested everything thoroughly (unknown bugs!). Despite this, I have shared the TRAPS code with Jilian. I'm looking forward to seeing how she integrates tiny rivers into the Hood Canal model, and I'm interested in hearing her feedback about the code.

---
## Birch Bay Debug Status

Last week I discovered that removing Birch Bay WWTP from LiveOcean eliminates the blow up issue.

This week I ran one more LiveOcean experiment, and I tried to analyze the data I have accumulated from all of my various LiveOcean runs.

### Shifting Birch Bay

Broadly speaking, the issue with Birch Bay WWTP is one of three possibilities:
1. Inherent issue with Birch Bay WWTP inputs
2. LiveOcean is sensitive at the location of the Birch Bay WWTP
3. Both are issues

To test whether something is inherently weird about Birch Bay WWTP inputs, I ran LiveOcean with TRAPS for one day. Except I shifted Birch Bay WWTP further out to sea (shifted seawards in longitude). This model run did not blow up. This tells me that Birch Bay WWTP itslef is not problematic. Rather, the combination of this WWTP and its location in LiveOcean are causing problems.

The table below summarizes all LiveOcean experiments I have conducted so far.

**Summary of Experiments to Date**
| Run | Condition         | Abbreviated Name  | One-Day Run Result    |
|---    | ---               | ---               | ---                   |
| 1     | LiveOcean as is   | as-is             |Success               |
| 2     | LiveOcean with TRAPS | with-TRAPS | ROMS blows up      |
| 3     | LiveOcean with TRAPS <br> excluding Birch Bay WWTP| no-BBW | Success |
| 4     | LiveOcean with TRAPS <br> but Birch Bay WWTP <br> shifted further out to sea | BBW-shifted | Success|

## Mooring Extractions

I extracted state-variable data at the location of the Birch Bay WWTP from both the with-TRAPS and no-BBW LiveOcean runs.

Figure 1 shows the velocity profiles from the with-TRAPS run at the location of the Birch Bay WWTP for every hour in the model run. Figure 2 is the same figure, but for the no-BBW run. Note that the with-TRAPS run blew up at hour 21.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/201598043-b57d302c-97c3-445b-bc1d-11719244560a.png" width="600"/><br>Fig 1. Velocity vs. depth and time at the location of the Birch Bay WWTP in the with-TRAPS run. This model run included Birch Bay WWTP at this location.</p><br>

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/201598062-d9d4e136-32c6-4540-8036-2a89f36f2705.png" width="600"/><br>Fig 2. Velocity vs. depth and time at the location of the Birch Bay WWTP in the no-BBW run. This model run does not have a WWTP at this location. The dotted line denotes the time at which the with-TRAPS run blew up.</p><br>

In the with-TRAPS run, u and v-velocities increase quickly with the rising tides. The w-velocity also decreases suddenly. These trends are not observed in the no-BBW run.

I also took a look at some other state variables from the with-TRAPS and no-BBW runs at the Birch Bay WWTP location.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/201598036-efb49562-56f1-4d31-81c9-368e3c749ba4.png" width="750"/><br>Fig 3. State variables vs. depth and time at the location of the Birch Bay WWTP in the with-TRAPS run. This model run included Birch Bay WWTP at this location.</p><br>

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/201598055-9b6a4ed4-4c4d-423c-a959-dba3407154de.png" width="750"/><br>Fig 4. State variables vs. depth and time at the location of the Birch Bay WWTP in the no-BBW run. This model run does not have a WWTP at this location. The dotted line denotes the time at which the with-TRAPS run blew up.</p><br>

What surprised me most about these results is that past a certain time, all biogeochemistry state variable values turn into nans.

### Surface Velocity Profiles

The mooring extractions were conducted solely at the location of the Birch Bay WWTP. However, I am also interested in the horizontal distribution of velocity near the Birch Bay WWTP. Figures 5, 6, and 7 show the surface velocity profiles near the Birch Bay WWTP for the with-TRAPS, no-BBW, and BBW-shifted runs, respectively. The surface velocities shown are taken from hour 20 of the run, the hour just before the with-TRAPS run blew up.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/201598073-802dc14e-aedd-4b24-a44d-86c296c5dffc.png" width="900"/><br>Fig 5. Surface velocity near Birch Bay WWTP from hour 20 of the with-TRAPS model run. Note that this run blew up at hour 21.</p><br>

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/201598084-98efbd6f-84ef-4b84-a681-0518928dbd5b.png" width="900"/><br>Fig 6. Surface velocity near Birch Bay WWTP from hour 20 of the no-BBW model run.</p><br>

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/201598076-3568cd1b-6324-4d7c-8737-16595ae44aad.png" width="900"/><br>Fig 7. Surface velocity near Birch Bay WWTP from hour 20 of the BBW-shifted model run.</p><br>

The with-TRAPS run has huge surface velocities around the Birch Bay WWTP. The largest velocities appear to be just at the tip of the cusp of land. The surface velocites in the no-BBW and BBW-shifted do not have these large magnitude velocities. The surface velocity profiles of the no-BBW and BBW-shifted runs also look very similar.

### Water Depth

Parker mentioned that the Birch Bay WWTP location may have a particularly shallow bathymetry. Figure 8 shows the sigma layers as a function of time and depth at the location of Birch Bay WWTP. For all time, all sigma layers are less than 1 m thick. At the surface, the sigma layers appear to be ~20 cm thick. Is this considered very thin?

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/201598068-6495347c-caab-4954-ad98-b77a146857fa.png" width="500"/><br>Fig 8. Edges of sigma layers vs. depth and time at the location of the Birch Bay WWTP.</p><br>

### Current Thoughts
Based on results from the BBW-shifted experiment, there doesn't appear to be anything inherently wrong with the Birch Bay WWTP inputs. That being said, I should still double-check the biogeochemistry inputs because it's bizarre that LiveOcean biogeochemistry values turn into nans.

Given that the with-TRAPS run blew up as the tide was rising, and that the sigma layers are rather thin at this location, I suspect that this location is sensitive to rapid change and/or strong flow. Alex mentioned this previously, but perhaps being located at the cusp of land makes this location especially susceptible to blow up as the water curves around.

What should I do next? First, I will follow up on the dissappearing nutrients and verify that I'm not simply inputting nan nitrate concentrations. I would also like to run an expirement with Birch Bay WWTP at its original location, but with 1/10 the flowrate. If this experiment runs successfully, then Birch Bay WWTP may be injecting too much momentum into a sensitive location. Maybe it would be better to shift the location of the Birch Bay WWTP slightly seaward for longer model runs.

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
- [X] (2022.11.12) Add logical switches so user can choose to add only tiny rivers or only WWTPs.
- [ ] Have additional logical switch that either does or does not discharge nutrients.
- [ ] Decide what to do if there are missing values in climatology. Also answer "why are there missing values in climatology?"
- [ ] Why does the Birch Bay treatment plant cause ROMS to blow up??
- [ ] *Clean up TRAPS integration code*