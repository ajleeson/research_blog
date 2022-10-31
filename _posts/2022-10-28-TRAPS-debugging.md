## TRAPS Debugging

This week I ran LiveOcean for one day as-is, I created ROMS forcing using the TRAPS climatology, and I tried to run LiveOcean for one day with TRAPS forcing. With TRAPS forcing, ROMS blew up on hour 21 of the day, and I have yet to debug what went wrong.

---
## Running LiveOcean

It took a while to find all of the files I needed to generate river, tidal, ocean, and atmospheric forcing for LiveOcean. I copied Parker's grid files for cas6, the current LiveOcean grid, from klone. After I tracked down all of these missing pieces, I ran LiveOcean as-is for one day. Figure 1 shows a snapshot of hour 20.
<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/198935960-5bd08855-a754-4219-bdee-9855fa2e6730.png" width="600"/><br>Fig 1. Snapshot of hour 20 of a one-day, as-is, LiveOcean run. Data from January 01, 2020.</p><br>

---
## Adding TRAPS Forcing

To create TRAPS forcing, I used the same structure of code that is used to create river forcing. River forcing includes a python script called rivfun, which contains helper scripts that convert climatology into ROMS forcing. I borrowed this code structure and created the analagous "trapsfun" to convert the TRAPS climatology into ROMS forcing.

TRAPS forcing is used in place of river forcing. Thus, TRAPS forcing really includes TRAPS plus pre-existing LiveOcean rivers. The TRAPS make_forcing_main script is really a copy of the riv00 make_forcing_main script with TRAPS forcing generation appended at the end. Pre-existing LiveOcean river and TRAPS forcing values all get saved into a single rivers.nc file for ROMS. I did a cursory check of the resulting forcing file to make sure things generally looked okay. A more thorough check is probably warranted, though.

After generating TRAPS forcing, I created a new dot_in folder using a copy of cas6_v00_uu0mb (LiveOcean as-is). I needed to make only two changes to this folder to enable TRAPS. First, in forcing_list.csv I changed riv forcing from riv00 to traps00 (Figure 2). Second, in BLANK.in, I enabled LwSrc since WWTPs are added as vertical momentum sources. (Figure 3). 

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/198936974-882fd5fe-d698-4004-8e28-0ac571e1c471.png" width="150"/><br>Fig 2. forcing_list.csv change required to use the traps00 forcing instead of riv00 forcing.</p><br>

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/198936980-ac6e9ac8-ecab-41e6-b55c-db79ca076155.png" width="800"/><br>Fig 3. BLANK.in change required to enable LwSrc for WWTPs.</p><br>


---
## Running LiveOcean with TRAPS

LiveOcean ran for 20 hours of one day before ROMS blew up. I do not yet know the root of the problem. However, I do have a list of possible reasons. Some things to check include graphically verifying that all rivers point towards water and away from land. I should also somehow combine two or more river mouths that get mapped to the same LiveOcean grid cell. It would also be good to verify that I have truly consolidated duplicate SSM/LiveOcean rivers. Finally, I should more thoroughly confirm that the ROMS forcing files I created don't have have any errors.

Figure 4 shows hour 20 of the LiveOcean with-TRAPS run. The surface salinity and surface temperature plots of the LiveOcean as-is and LiveOcean with-TRAPS runs look nearly identical. If I squint, I can convince myself that Lynch Cove looks slightly fresher in the with-TRAPS run.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/198934539-96e45743-c69a-48d6-9cd5-3056d9e09c1c.png" width="600"/><br>Fig 4. Snapshot on hour 20 of a one-day LiveOcean run with TRAPS forcing. This is the last hour that LiveOcean ran successfully before ROMS blew up. Data from January 01, 2020.</p><br>

---
## TRAPS Integration Checklist

New items are *italicized*.

**Priorities before KC meeting**
- [X] (2022.10.22) Create climatology for TRAPS 
- [ ] Verify that my ROMS forcing file generation code does what it should be doing (eg. check my units, timing, etc.)
- [X] (2022.10.30) Try to run LiveOcean myself!
- [ ] Debug why ROMS is crashing

**All remaining items**
- [ ] Verify that the UTM to lat/lon conversion was correct
- [ ] Make sure any river mouths located on a narrow strip of land get placed on the correct side of the land (the algorithm is blindly placing rivers on the nearest coastal grid cell without awareness of real geography)
- [ ] Compare Ecology's Hood Canal flow data to Mike Brett's data. Conduct other data checking, if possible.
- [ ] Determine from which sigma layer each marine point source should discharge, and how to translate this information into the ROMS variable, river_Vshape
- [X] (2022.10.22) Think about how to handle forecasting?
  - handled by using climatology files
- [ ] Figure out how to handle climatology for WWTPs that opened after certain year, shut down after certain year, or upgraded treatment type after certain year
- [ ] Check SSM flow data for ambiguous duplicate rivers, and decide whether they are or are not actually duplicate rivers
- [ ] *Handle river mouths that get placed at the same coastal grid cell. Maybe combine the flows?*
- [ ] *Add error bars to climatology plots, and save all plots*
- [ ] *Average Feb 28/Mar 1 data to get a Feb 29 value on non-leap years, so leap years have a larger n*
- [ ] *Potentially remove 2013/2014 data from Tahuya and Union rivers, so shift doesn't bias climatology*
- [ ] *Add logical switches so user can choose to add only tiny rivers or only WWTPs. If WWTPs are added, have additional logical switch that either does or does not discharge nutrients.*