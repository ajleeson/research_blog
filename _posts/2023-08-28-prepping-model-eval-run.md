## Preparing a Model Evaluation Run

This week I finally finished refactoring TRAPS. I'm quite pleased because this task was a long haul. In the future, I'd still like to make more improvements, but for now the code is sufficient for the evaluation run.

I have otherwise moved on to creating a test script that will help verify that the TRAPS inputs are correct. Once the script is complete, we can begin the first model evaluation run.

More details below.

---
## Finishing touches for TRAPS

Last week there were a few remaining loose ends in the TRAPS code. This week I wrapped them up, or at the very least brought the code to a "sufficient enough" stopping point for now.

### Follow-up on Big Beef Creek rivers

The Big Beef Creek gage was deactivated sometime during 2012. Several SSM rivers (like Union River) relied on this gage for flow and nutrient data. Our issue was that between mid-2012 through 2014, data for these rivers was filled with a copy of previous years, but shifted by three months. Beginning in 2015, unique data reappears for these rivers.

Last week I had proposed omitting data from mid-2012 through 2014 so that the shifted, duplicate data doesn't bias river climatologies. However, we also discussed whether data from 2015 onwards also biases the climatologies. Per Alex's suggestion, I plotted the climatology of Union River for a case in which mid-2012 through 2014 was omitted, and a case in which mid-2012 onwards was omitted (see Fig. 1).

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/ff759af8-50df-423e-be02-795b6aeebfd6" width="450"/><br>Fig 1. Union River climatology with years 2012-2014 removed compared to 2012-end removed.</p><br>

Overall, the two climatologies are similar. The peaks and troughs are generally in the same spots. Notably, neither climatology has a late-summer peak in discharge, which is ideal. Given the similarity between the two climatologies, I have decided to crop data from July 2012-December 2014, so we keep the unique data from 2015 onwards.

### Courtenay River watershed confusion

Another open issue was related to a trio of rivers on Vancouver Island.

LiveOcean contains one river called **Tsolum River**.
SSM contains two nearby rivers (with the same lat/lon coordinates). One is called **Comox**, the other is called **Vancouver Isl N**.

Our questions are whether Comox or Vancouver Isl N represent the same river as Tsolum River, and whether Comox and Vancouver Isl N contain overlapping information.

Documents from Ian indicated that this region is the Courtenay River watershed. The largest rivers are Tsolum and Puntledge River, which merge to form Courtenay River before discharging into the Strait of Georgia. However, my confusion about our trio of rivers persisted.

I reached out to Ecology this week, and Teizeen provided some great notes about the Canadian watersheds in SSM. She shared an [interactive web map](https://waecy.maps.arcgis.com/apps/webappviewer/index.html?id=c7318e19bf3141aca62e980a7e5b53f2#) that highlights the different watersheds associated with each river in SSM. Figure 2 shows a screenshot of this map.

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/9174c4e3-5dc0-4b93-a4c2-264c3c6938fe" width="450"/><br>Fig 2. SSM Canadian watershed domains (Department of Ecology, 2021).</p><br>

Figure 3 highlights the Comox watershed on the same map.

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/080d11c3-0204-4dea-9f4b-3aae69d858ba" width="450"/><br>Fig 3. Comox watershed domain in SSM. This region is a zoom-in of the rectangle drawn in Figure 2 (Department of Ecology, 2021)</p><br>

From what I gather, it seems like Vancouver Isl N and Comox both represent the Courtenay River watershed. The Vancouver Isl N watershed region only encapsulates the watershed area on Vancouver Island, while Comox includes both the region on Vancouver Island as well as Quadra Islands (Fig. 3). It is still unclear to me whether including both Comox and Vancouver Isl N would double-count flow from the Courtenay River.

It seems like both SSM rivers overlap with LiveOcean's Tsolum River. Ecology's data for this region are based on two gages in the watershed: one in the Courtenay river, and one in Oyster river. While Tsolum is separate from Oyster river, Tsolum is a tributary to Courtenay river. Given that both SSM rivers seem to represent *some* portion of LiveOcean's Tsolum River, I have decided to omit both SSM rivers for now.

Last week Alex suggested that I keep the SSM rivers and omit the LiveOcean river. I am considering the opposite because the LiveOcean river has a track carved upstream into the land, whereas I've simply added the SSM rivers as point sources. Thus, keeping the SSM rivers and omitting the LiveOcean river could create a reverse estuary inside of the river track.

I don't think that this is an ideal solution and I would like to have a better resolution to this river confusion problem. But for now, this decision seems like the simplest step so we can get the model running.

### WWTP open/close dates

This week I also finally handled WWTPs that opened or closed sometime between 1999-2017.

I created an excel spreadsheet with approximate open/close dates (yearly resolution). This excel sheet enables users to update the open/close dates and add new WWTPs to the list with ease. The file is saved in LO_data/traps/wwtp_open_close_dates.xlsx (Fig. 4)

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/216b3680-0a1c-4179-92c0-8daa08be61f7" width="300"/><br>Fig 4. User-modifiable excel sheet with WWTP open/close years.</p><br>

The forcing script reads in this excel sheet and converts it into a conditional statement. This conditional statement sets the discharge rate to zero for all years that a WWTP is closed. Otherwise, the forcing script reads the discharge rate from the WWTP climatology.

### Fraser river ammonium

Per Alex's reccomendation, I also reached out to Susan Allen to ask for more information about Fraser NH4. She shared that her group uses a constant concentration of 4.43 uM (mmol/m3), which is the average value measured by Environmental Canada (Olson et al., 2020)

The suspicious value I was previously using was: Fraser NH4 ~ 0.073 mmol/m3.

I have since added a conditional statement to change Fraser NH4 to be 4.43 mmol/m3. (Fig. 5)

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/0d4a3e2d-f433-4d53-a4b9-ea87ff5197e1" width="500"/><br>Fig 5. Conditional statement (hidden in the depths of the forcing script) that sets Fraser NH4 to be 4.43 mmol/m3.</p><br>

Although this adjustment may make Fraser NH4 more realistic, it feels awkard to change one biogeochemsitry variable value in an if-statement (i.e. it feels messy). For now this change may be sufficent, but in the future I would prefer to create more robust code that gracefully and clearly deals with weird nuances. I have more thoughts on TRAPS improvment, which I've expressed in the next section.

### More thoughts on TRAPS code

TRAPS never feels finished. The more I try to refine, the more I discover that more could be refined. I've accepted that TRAPS will always be an unfinished project because there will always be more that I can improve. The tricky part of this realization is that I need to decide when TRAPS is "good enough" for our current use case. Right now, I am deciding that TRAPS is "good enough" to begin model evaluation. However, there are still many improvement I would like to implement in the future. We have discussed some of these ideas previously, some of them are new:

- Replace weird river climatology using average climatology of other rivers in the same watershed
- Automate data checking to screen for weird biogeochemistry and other oddities
- Identify WWTPs that have upgraded their treatment process. Then provide two different climatologies for these WWTPs: one for before the upgrade, one for after the upgrade
  - Example: LOTT

---
## Model evaluation preparation

After wrapping up the TRAPS refactor, I moved on to preparing for our model evaluation run. More details below.

### TRAPS verification script

One topic we discussed in Mike's modeling class was how to verify that our model inputs are correct. As we learned before GRC, I am prone to creating errors in the model input. I'd thus like to write test scripts that verify that the model inputs are as intended prior to starting the model evaluation run.

Figure 6 shows the output from my preliminary test script. First, this script verifies that the rivers are all discharging in the correct direction. I borrowed this section of the code from one of Parker's scripts (which is how we discovered the bug in my GRC inputs). Second, this script counts the number of sources discharging in different directions and verifies that it matches expectations. This week I already caught one bug in my code that was accidentally omitting a point source (which brought the count down from 98 to 97).

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/d727b18d-a227-4d9e-a886-5501b0089210" width="500"/><br>Fig 6. Preliminary output from input verification script.</p><br>

Next I'm planning on expanding my test script to verify the numeric inputs in the forcing file. This portion of the script will help confirm that I haven't, say, mixed up TIC and temperature inputs.

Once I have completed this script and have confidence in the model inputs, I will start the model run using the checklist below.

### Model evaluation run checklist

**ROMS source code**
- [ ] Verify on klone in LO_roms_source/ROMS/Nonlinear that step3d_uv.f uses the revised IF-ELIF syntax

**Updated biology**
- [ ] Verify on klone in LO_roms_source_alt/npzd_banas/ that the new fennel.h is being used (with my AttSW changes)
- [ ] Set biogeochemistry advection scheme to MPDATA in bio_Fennel.in

**Forcing**
- [ ] Use ocean initial conditions from Dakota
- [ ] Check that trapsV00 is being used in forcing_list.csv
- [ ] Run test script on rivers.nc to verify that TRAPS forcing is correct

**General to-dos**
- [ ] Generate forcing with cas7 grid using trapsV00 for year 2017
- [ ] Create new dot_in with gtagex cas7_trapsV00_x2b
- [ ] Compile x2b in klone with correct changes to ROMS source code (see earlier sections)

**Runnning the model**
- [ ] Let Jilian know before I begin so she can pause her runs and I can use 10 nodes on klone
- [ ] Check on run every day (if it pauses, restart using ```perfect restart```)

---
## References

Department of Ecology. (2021). Puget Sound Nutrient Source Reduction Project: Salish Sea Model Results. Retrieved August 24, 2023, from https://waecy.maps.arcgis.com/apps/webappviewer/index.html?id=c7318e19bf3141aca62e980a7e5b53f2#

Olson, E. M., Allen, S. E., Do, V., Dunphy, M., &#38; Ianson, D. (2020). <i>Supporting Information for “Assessment of Nutrient Supply by a Tidal Jet in the Northern Strait of Georgia Based on a Biogeochemical Model.”</i> https://agupubs.onlinelibrary.wiley.com/action/downloadSupplement?doi=10.1029%2F2019JC015766&#38;file=jgrc24099-sup-0001-Text_SI-S01.pdf