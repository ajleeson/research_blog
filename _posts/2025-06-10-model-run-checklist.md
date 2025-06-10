## Model Run Checklist

This blog post serves as a checklist for all the little loose ends that I need to tie up before launching the new model runs.

- [x] Set up header file and compile ROMS on klone (x10ab)
    - [ ] Define daily averages
    - [ ] Use new fennel.h with 50% particle burial and corrected carbon burial 

- [ ] Get latest version of ROMS
    - [x] Delete older varinfo files on local pc/perigee/apogee
    - [ ] Git pull latest version of ROMS on klone <span style="color:red">(Do this once hyak maintenance is complete!)</span>

- [ ] Increase nudging to climatology
    - [ ] Create nudgecoef.nc file for *all* biogeochemistry variables

- [ ] Set up dot in folder
     - [x] Use newest BLANK.in version with shifted tides
     - [ ] Use MPDATA advection scheme for bio tracers in bio_Fennel.in
     - [ ] Adjust forcing list (atm00, ocn00, tide00, new TRAPS) <span style="color:gray">(Note that we use old ocean forcing with HYCOM boundaries)</span>
     - [ ] Ensure make_dot_in.py is compatible with 320 cores for tiling

- [ ] Generate new river/WWTP forcing using updated TRAPS for October 2012 - December 2017 on apogee
    - [ ] One forcing with DIN in WWTP effluent
    - [ ] One forcing with **zero** DIN in WWTP effluent
    
- [ ] Adjust driver_roms4
    - [ ] Set up file to look for atmospheric, tidal, and ocean forcing in Parker's apogee account (dat1/parker/LO_output/forcing/cas7)
    - [ ] But look for river/WWTP forcing in Aurora's apogee account

- [ ] Launch model run
    - [ ] Use start type "new average"
    - [ ] Use 10 cpug2 slices in coenv group for one test condition
    - [ ] Use 10 ckpt group slices for other test condition
    - [ ] Save output on dat1 in apogee

- [ ] Set up crontab so I receive email updates of model progress every few hours

- [ ] Generate script to calculate bit-reproducibility after one day of run time
