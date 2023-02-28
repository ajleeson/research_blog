## WWTP Hydrodynamics Comparison

This week I ran some tests that will allow us to compare LiveOcean hydrodynamics with and without WWTPs. First, I ran a one month test (February 2021) that includes all TRAPS except Birch Bay WWTP and Oak Harbor Lagoon WWTP. Then, I repeated the run but removed all WWTPs. Though I haven't yet conducted a thorough comparison between these two runs, I have already observed that WWTPs cause strange hydrodynamic behavior.

While these models were running I generated a couple figures that we discussed in last week's meeting:
1. **WWTP depth vs. average discharge** to see whether Birch Bay or Oak Harbor Lagoon WWTP are an extreme
2. **Distance from WWTP to nearest coastal cell vs. average discharge** to see how much WWTPs would change if I implemented them as tiny rivers

More details below.

---
## WWTP Depth vs. Average Discharge

Figure 1 shows the depth of every point source in LiveOcean vs. their average discharge rate. Neither Birch Bay WWTP nor Oak Harbor Lagoon WWTP appear to have a particularly high flowrate at a particularly shallow depth compared to other WWTPs. It really is perplexing why these two treatments plants are causing problems.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/221698619-f13916d8-4760-4e83-8810-faa1d00b5238.png" width="400"/><br>Fig 1. WWTP Depth vs. Discharge Rate.</p><br>

---
## WWTP Distance to Coast vs. Average Discharge

A potential solution that Alex suggested a couple of weeks ago was to treat point sources like tiny rivers rather than vertical sources. The main issue with this approach is that all point sources will be forced to the coast. This week I tried to visualize how many point sources would be affected, and how far they would need to move.

Figure 2 plots the distance from a WWTP's current location to the nearest coastal cell vs. its average discharge rate.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/221698623-10705948-6ae0-4030-91ba-a7b5d3d6110d.png" width="400"/><br>Fig 2. WWTP Distance to Coast vs. Discharge Rate.</p><br>

There are two point sources that stand out to me as being both large and far away from shore: South King and West Point.

South King is the fourth largest discharger and is ~3 km from shore. West Point is the third largest discharger and is 1.5 km from shore. Both of these plants are King County plants that discharge right into Main Basin.

I'll note that I am not worried about Annacis because this plant discharges into the Fraser River. Thus, I'm assuming that moving Annacis WWTP by 0.5 km is negligible, and that the Fraser flow will dominate how Annacis nutrients enter the Salish Sea.

I'll further note that the largest discharger is already located at a coastal grid cell, so it does not need to move at all. 

---
## One Month With and Without Point Sources

As I mentioned aboved, I ran LiveOcean with TRAPS from 2021.02.01 through 2021.03.01. This should roughly give us information over a full spring neap cycle. I ran two test conditions:
1. All TRAPS *except* Birch Bay WWTP and Oak Harbor Lagoon WWTP
2. No point sources, but still included tiny rivers

Again, neither run blew up. Both runs were initialized as a perfect restart using the restart file from my original 2021 attempt to run LiveOcean (which blew up on 2021.02.19 at Oak Harbor Lagoon WWTP).

The model runs only recently finished so I don't have extensive analysis yet. However, I did make some quick surface velocity plots in just a snapshot of time (hour 21 on 2021.02.25). Both Figure 3 and Figure 4 below show surface velocities in Puget Sound Main Basin in King County.

**With Point Sources**
<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/221714353-c19aadcd-330f-4583-8b87-59fdcbe86344.png" width="750"/><br>Fig 3. With point sources run: Surface velocities on hour 21 of 2021.02.25 in Main Basin.</p><br>

**Without Point Sources (markers denote where the "would-be" WWTPs are located)**
<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/221714348-306ab9e3-caf2-453f-af3d-24dc2392ceb4.png" width="750"/><br>Fig 4. No point source run: Surface velocities on hour 21 of 2021.02.25 in Main Basin. Note that this run still includes tiny rivers.</p><br>

Even though the "with WWTP" condition did not blow up, I am still seeing some higher surface velocities near the WWTPs in Figure 3. Surface velocities just south of Bainbridge Island City WWTP are particularly strange. Higher velocities just south of the WWTP also match what I was observing at Oak Harbor Lagoon WWTP (ROMS was blowing up just south of the WWTP rather than exactly at the WWTP). What this suggests to me is that many, if not all, point sources are causing strange behavior. But so far only Birch Bay WWTP and Oak Harbor Lagoon WWTP have been strange enough to cause a blow up. Either way, there's something strange happening.