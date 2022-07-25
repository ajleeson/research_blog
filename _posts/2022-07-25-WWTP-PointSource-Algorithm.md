## WWTP Lat/Lon to Point Source Dischargers

This week I created an algorithm that, given a list of lat/lon coordinates for a WWTP, finds the nearest coastal grid cell and adds a freshwater point source. I created forcing files for these point sources and ran ROMS for two days. Every point source has a discharge flow of 1000 m3 s-1 distributed evenly between the sigma layers. Below are the results:

<video src="https://user-images.githubusercontent.com/15829099/180702152-b46f8fb1-e572-40ca-a926-c1c4cdc531b2.mp4" controls="controls" style="max-width: 900px;">
</video><br><br>

Figure 1 shows a snapshot of the final hour of this run.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/180702396-2340a304-3127-465d-a3d7-8c5f9cc89ead.png" width="600"/><br>Fig 1. Last hour of two-day point source test run.</p><br><br>

There were two major parts of this process. First is the algorithm that finds the nearest coastal grid cell to a WWTP. Second is creating forcing files for the new point sources. Both parts are described in more detail below.

---

## Algorithm: Nearest Coastal Grid Cell to WWTP

The main steps of the algorithm are to
1. Determine in which grid cell the WWTP is located
2. If the WWTP is already in water, the algorithm saves the WWTP grid cell as the point source location
3. If the WWTP is on land, check the surrounding "ring" of grid cells for a water cell
4. If there are no water cells, look in the next ring
5. If there is one or more water cells in the ring, determine the grid cell that is nearest to the WWTP and save the cell location
6. Search in the next ring for water cells that may be nearer to the WWTP
7. If none of the water cells in the outer ring are closer than the previously saved nearest water cell, then the previously saved watercell is the nearest coastal grid cell

The reason why step 6 is necessary is because the grid cells may not all be the same size. As illustrated in Figure 2, it is possible for the nearest coastal grid cell to be several rings away, even if there are coastal grid cells in closer rings.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/180850331-96deab88-2345-493a-8a02-d15caf488d5a.png" width="750"/><br>Fig 2. Nearest coastal grid cell is two rings away, despite the WWTP having a coastal grid cell in the adjacent ring.</p><br><br>
### Summary of Changes

The required changes for this algorithm to work are documented below. Note that I copied all files from LO/pgrid into LO_user/pgrid and modified them within LO_user.

**New Scripts and Files**<br>
1. Create a file called wwtp_loc_info.csv in LO_user/wwtps/[gridname]/wwtp_loc_info.csv with discharger names and lat lon coordinates. The location of the file can be changed later on, and updated in gfun.gstart. Here's an example of what the file looks like: 
<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/180703778-9f27ea75-38d7-4977-8163-0ce922b6ee6c.png" width="250"/></p><br>
   
2. Created a script called place_dischargers.py which is the wwtp equivalent to carve_rivers. This script uses the information in wwtp_loc_info.csv to calculate the nearest coastal grid cell to use as a point source for the model. This information gets saved in LO_output/pgrid/[gridname]/roms_wwtp_info.csv (same as where river info gets saved).
<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/180836886-afab0724-1da4-47d0-ab81-c0bf7252bc05.png" width="240"/></p><br>

**Changes to gfun.py**<br>
1. Added a line in gfun.gstart that defines location of wwtp_dir (which is LO_user/wwtps/[gridname]). This location can be changed later on. Again, wwtp_dir contains the .csv file with WWTP lat lon coordinates.
<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/180837139-046c08ad-f10a-4afc-b6a0-d791224f7686.png" width="800"/></p><br>

2. Added new tag in gfun.increment_filename called "_d" to update the filename after "place_dischargers.py" has been run. The "_d" is for "dischargers"
<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/180837537-d331d24f-6cc7-4cba-97f9-c748863b3c67.png" width="600"/></p><br>

**Changes to start_grid.py**<br>

1. Needed to update the default filename to include a "_d00" tag for dischargers
<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/180837836-3208366e-5bcb-4351-9326-1149104bbd8d.png" width="340"/></p><br>

**Changes to grid_to_LO.py**<br>

1. Copy point source information into the output directory to prepare for ROMS run
<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/180839335-055d462c-040d-43b9-9a2d-cbc990e391be.png" width="720"/></p><br>
   
**Changes to plot_grid.py**<br>

1. Changed plot_grid to add an if case if the wwtp_loc_info.csv file exists (using the location specified in gfun.gstart). If so, then plot_grid also plots WWTP location.
<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/180839813-f9262d6a-d9e0-477d-86c6-7cda24670ce5.png" width="400"/></p><br>

2. Also added an if case if roms_wwtp_info.csv exists (after place_dischargers.py has been run). If so, then plot_grid shows where the algorithm has placed the point sources.
<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/180839982-9e87516b-8a39-44e2-8134-27e7e367cbc1.png" width="400"/></p><br>

---

## Forcing Files for Point Sources

Creating forcing files was a bit trickier. Right now, the main script that creates wwtp forcing (make_forcing_main.py) does not have the capability to create both wwtp and river forcing. This will need to be dealt with later on.

River forcing uses LuvSrc to introduce momentum from u- or v-faces. However, LuvSrc only allows point sources on coastal cells, and it isn't recommended for cells already in the ocean. Since I had a test WWTP in the ocean, I needed a workaround for this issue.

Instead of LuvSrc, I used LwSrc to introduce momentum from w-faces. This had implications on the eta/xi positions of the source, the direction of the source, and the sign of the source. What I've found is that all three are actually easier to define using LwSrc than LuvSrc. The eta/xi positions of the source are equivalent to the rho-point indices (which were saved in place_dischargers.py). The direction of the source is simply 2 for inflow coming from w-face cells. The sign of the source is always positive for sources (always negative for sinks).

It sounds like we can enable both LuvSrc and LwSrc within the same application, as long as they are not applied to the same grid cell. However, I have yet to combine river and WWTP forcing. Making this work is the next step.

### Summary of Changes

The required changes to set up WWTP forcing are described below.

**Changes to forcing_list.csv**<br>
1. I added a new forcing called wwtp. The name of the wwtp forcing is "wwtpA0" in forcing_list.csv.
<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/180842410-c44e2bae-f28a-477b-bceb-2ecdcc7c835a.png" width="130"/></p><br>

**Changes to BLANK.in**<br>

1. In BLANK.in, I added a line to tell roms to look for wwtp forcing in wwtps.nc. I also commented out the river forcing.
<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/180842669-8530574b-f480-4773-a4d7-8df3f3512a55.png" width="500"/></p><br>
Eventually will need to enable both river and wwtp forcing, but I'm not sure how to do that yet (two different SSFNAME files? Combined into one forcing file?)

2. Turn on logical switch for LwSrc. (In this case I also turnedd of LuvSrc since I didn't have any rivers.)
<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/180845690-ff5d9761-275e-49c6-8e62-5bdac49c3146.png" width="600"/></p><br>

**Changes to make_forcing_main.py**<br>
1. Updated river_Xposition and river_Eposition to always be the rho-point i and j values (this is the case for LwSrc). The river_direction is also always 2 for LwSrc.
<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/180843602-e79790db-5868-4c5c-85a5-732553105f4d.png" width="700"/></p><br> 
   
---
## Helpful Links

Here are some links I found helpful that explains the nuances of LuvSrc and LwSrc:

[ROMS Wiki River Runoff](https://www.myroms.org/wiki/River_Runoff#river_direction) has general information about setting up and using LuvSrc and LwSrc.

[Enhanced Point Sources/Sinks Forcing](https://www.myroms.org/projects/src/ticket/905) helped me understand what to use for river_direction.

[Point Sources Revisited](https://www.myroms.org/projects/src/ticket/860) has a nice comparison between model runs using LuvSrc and LwSrc. The results are similar, which is very reassuring.