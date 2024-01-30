## A preliminary look at results

The simulation run is currently in mid-October and still going strong.
This week, I began taking a look at some preliminary results. I also took some time to read a bit more literature and set up the new iPad. More details below.

---
## Figure drawing on iPad

Thank you again for supporting the expansion of my device toolbox! The iPad arrived last Friday, and the Macbook is arriving sometime next week.

Alex, I think I still have a lot of work to do before I can make "**beautiful** figures of estuarine circulation and its influence on DO." But I've started making some...rough-and-ready...figures to summarize key concepts in the literature I'm reading.

Figure 1 shows some of my major takeaways from Li et al. (2016) about the most dominant factors contributing to interannual variability of hypoxic extent in Chesapeake Bay.

Please excuse my poor iPad handwriting.

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/a8813e97-25ad-479d-997e-79307bf35495" width="350"/><br>Fig 1. Schematic summary of Li et al., 2016.
</p><br>

Years with higher river discharge tended to have larger hypoxic volume, not because of changes to physical mechanisms, rather because of increased DIN loading from rivers (and thus more water column respiration).

---
## Hydrodynamics

When the model reached mid-September, I checked for hydrodynamic deviations again.
As shown in Figure 2 and Figure 3, the long hindcast and the N-less run still do not seem to have any hydrodynamic differences.

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/19ba0cc9-891e-4bfb-b7dd-3adb6d8d25d2" width="500"/><br>Fig 2. u difference between long hindcast and N-less run. First panel shows surface difference, second panel shows bottom difference.
</p><br>

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/43f5e86c-a7ae-4d42-b0e6-6892dd0e2f64" width="500"/><br>Fig 3. v difference between long hindcast and N-less run. First panel shows surface difference, second panel shows bottom difference.
</p><br>

---
## Preliminary model results

Once the simulation made it ~halfway through the summer, I began to develop some preliminary plotting scripts. The scripts I developed this week compare monthly average state variable values between the hindcast run and the N-less run. The script generates comparison figures at both the surface and bottom for the state variables:

- DO
- NO3
- NH4
- phytoplankton
- small detritus
- large detritus

(Note: I forgot about the zooplankton but will extract that information next time. Sorry zooplankton!)

So far, I have taken a look at July and August averages. That being said, the script can be run on any month (as long as I run a box extraction first). In theory, I could also create figures with seasonal averages. But for now, I'll focus on some key observations I made in the August results. (Note that there are many more figures, but I am choosing to show the ones that I found to be the most interesting.)

In the "Anomaly" map in the right panel of every figure below, blue means the hindcast had a higher value than the N-less run, and red means the hindcast had a lower value than the N-less run. So, red anomaly in bottom oxygen means the hindcast had worse bottom DO than the N-less run.

### Bottom DO

Figure 4 shows the average August bottom DO in Puget Sound.

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/a8046162-0d1e-4b4a-94e3-811156284f83" width="600"/><br>Fig 4. Average August bottom DO. (a) shows bottom DO in the N-less run. (b) shows the difference in bottom DO between the hindcast and the N-less run.
</p><br>

A few observations jump out at me:

**High and low DO regions (a)**
- High DO regions in N-less run
  - North end of Port Susan (Whidbey Basin)
  - Head of Hammersley, Totten and Eld Inlets in South Sound
- Low DO regions in N-less run
  - Holmes Harbor (Whidbey Basin)
  - Lynch Cove (Hood Canal)
  - Case Inlet (South Sound)

**Changes in bottom DO (b)**
- All changes are of a relatively small magnitude of < 0.3 mg/L (though the monthly average may have filtered out some high variability events)
- Regions with high DO (northern Port Susan and the South Sound inlets) tended to become more oxygenated with WWTP N inputs
- Regions with low DO (Holmes Harbor, Case Inlet) tended to become more deoxygenated with WWTP N inputs
  - The exception being Hood Canal, which appears largely unaffected by WWTP nutrients

**Spatial distribution of DO changes**
- South Sound, Whidbey Basin, and the southern half of Main Basin seem to have the largest DO response to WWTP nutrients. 
  - Why do Hood Canal and the northern half of Main Basin have such a small DO signal?
    - This is a puzzle to me. One of my original hypotheses was that becasue WWTPs discharge from the bottom, then the exchange flow carries effluent from large WWTPs (e.g. West Point) southwards while the plume rises. By the time WWTP nutrients are in photic zone, they are further south. However, when I look at surface nitrate (Fig. 5), I find that all of Main Basin appears to have increased nutrient concentrations. Thus, this spatial pattern is a puzzle to me that I hope to investigate further.

### Surface nutrients and surface phytoplankton

Note that I made figures to look at surface and bottom NO3 and NH4. For brevity, I am showing only surface NO3, which I found to be most interesting.

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/a64d766a-9e23-423c-8955-55872297406c" width="600"/><br>Fig 5. Average August surface NO3. (a) shows surface NO3 in the N-less run. (b) shows the difference in surface NO3 between the hindcast and the N-less run.
</p><br>

**Low surface NO3 regions (a)**
- Hood Canal
- Port Susan
- Holmes Harbor
- Head of Hammersley, Totten, and Eld Inlets

These regions with low surface NO3 also tended to have the highest surface O2 concentrations (not shown). This observation suggests to me that the low NO3 regions have a lot of primary productivity.

Interestingly, these same regions also tended to have the smallest difference in surface NO3 concentrations between the two model runs (Fig. 5-b), such as Port Susan. My hypothesis is that the phytoplankton in these regions have used up nearly all of surface NO3 in both model runs, so the difference in surface NO3 between the model runs is a difference between two numbers close to zero.

Corroborating this hypothesis, I observe that northern Port Susan and Hammersley, Totten, and Eld Inlets have higher surface phytoplankton concentrations in the hindcast run compared to the N-less run, despite both runs having similar (near-zero) surface NO3 concentrations. (See Figure 6-b)

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/f4cb63fa-0c90-4bba-8ed2-6ce9d32e8610" width="600"/><br>Fig 6. Average August surface phytoplankton. (a) shows surface phytoplankton in the N-less run. (b) shows the difference in surface phytoplankton between the hindcast and the N-less run.
</p><br>

Again, there appears to little to no signal in Hood Canal.

### A note on high DO regions

Earlier, I mentioned that northern Port Susan and Hammersley, Totten, and Eld Inlets had high bottom DO which increased further in the presence of WWTP nutrients.

A did a bit more exploration in these regions and have come to the following conclusion:

- These regions are all very shallow (< 10 m depth)
- High surface and bottom phytoplankton concentrations, high surface and bottom DO concentrations, and low surface and bottom NO3 concentrations suggest that primary producers are photosynthesizing throughout the entire water column
- Thus, I conclude that these regions are shallow enough that they are well mixed, and the full water column is in the photic zone.

---
## Next Steps

**This week**
- Make video of surface phytoplankton
  - Where are the blooms occurring and when are they occurring?
- Run mooring extraction at a few key locations of interest
  - Entrance of Hood Canal (right after sill near Dabob Bay)
    - Examine whether there are differences in the amount of nutrients that enter. My hypothesis: relative to other nutrient inputs (i.e. ocean), the WWTPs are contributing very little to Hood Canal.
  - Holmes Harbor in Whidbey Basin
    - What are the major differences between the hindcast and the N-less run? Do nutrients from WWTPs increase the size of the spring bloom (I would assume yes). Do they also change the timing of the spring bloom?
- To answer these questions using data from mooring extractions
  - Create depth vs. time property plots
    - These are pcolormesh color maps
  - Create property vs. time timeseries
    - Where I can plot bottom DO, surface NO3, and surface phytoplankton all on the same axis to check relative timing of peaks and troughs
- Continue brainstorming: How to answer question about spatial distribution of WWTP effects
- Continue reading
  - I'm starting to make connections and distinctions between the different estuaries I am reading about, but not quite to the point where I can write up a summary of the drivers and processes leading to hypoxia in each system. This is a continued effort.

**Next week**
- Set up new laptop
- Make presentation for Ocean Sciences

**After Ocean Sciences**
- Re-work driver_roms3 to search for forcing in Parker's apogee folder (except TRAPS forcing which is in my perigee folder)
- Update TRAPS module based on Parker's suggestions
  