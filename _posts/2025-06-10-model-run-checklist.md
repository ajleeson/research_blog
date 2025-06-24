## Model Run Checklist

This blog post serves as a checklist for all the little loose ends that I need to tie up before launching the new model runs.

- [x] Set up header file and compile ROMS on klone (x11ab)
    - [x] Define AVERAGES and undefine PERFECT_RESTART
    - [x] Use new fennel.h with 50% particle burial and corrected carbon burial 
<br><br>

- [x] Get latest version of ROMS
    - [x] Delete older varinfo files on local pc/perigee/apogee
    - [x] Git pull latest version of ROMS on klone
    - [x] Recompile ROMS after git pull
<br><br>

- [x] Increase nudging to climatology
    - [x] Create nudgcoef.nc file for *all* biogeochemistry variables
    - [x] Update nudgcoef.nc file name in BLANK.in (to nudgcoef_tracer100.nc)
    - [x] Copy nudgecoef_tracer100.nc file into LO_data on apogee
<br><br>

- [x] Set up dot in folder
     - [x] Copy Parker's cas7_t1_x11ab folder to LO_user and rename as cas7_newtraps_x11ab
     - [x] Use newest BLANK.in version with shifted tides
     - [x] Use MPDATA advection scheme for bio tracers in bio_Fennel.in
     - [x] Adjust forcing list (atm00, ocnG00, tide00, trapsN00)
     - [x] Ensure make_dot_in.py is compatible with 320 cores for tiling
     - [x] Make sure make_dot_in is set up for daily averages with continuation
     - [x] Make another copy called cas7_newtrapsnoN_x11ab
        - [x] Specify trapsN01 forcing
<br><br>

- [x] Generate new river/WWTP forcing using updated TRAPS for October 2012 - December 2017 on apogee
    - [x] Final code edits to eliminate any remaining bugs
    - [x] Adjust forcing code to discharge zero NO3 and NH4 from WWTPs
    - [x] Generate one set of forcing with DIN in WWTP effluent (trapsN00)
    - [x] Generate another set of forcing with **zero** DIN in WWTP effluent (trapsN01)
    - [x] Check that forcing is correct
<br><br>

- [x] Make an adjusted driver_roms4 script in LO_user/driver
    - [x] Set up file to look for atmospheric, tidal, and ocean forcing in Parker's apogee account (dat1/parker/LO_output/forcing/cas7)
    - [x] But look for river/WWTP forcing in Aurora's apogee account
<br><br>

- [x] SCP files in LO_data/grids/cas7 into klone?
<br><br>

- [ ] Launch model run
    - [x] Email Parker before-hand to get final confirmation of resources
    - [x] Check with Jilian about resources
    - [x] Use start type "newcontinuation"
    - [x] Use 10 ckpt-g2 slices for each test condition
    - [x] Save output on dat1 in apogee
<br><br>

- [x] Set up crontab so I receive email updates of model progress every few hours

- [ ] Generate script to calculate bit-reproducibility after one day of run time
