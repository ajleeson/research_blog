## Adding a WWTP to ROMS Upwelling

Over break I was able to poke around the ROMS upwelling case and add a vertical source to the system. This proved to be slightly tricky because the ROMS upwelling case is outside of the LO system. However, I was able to learn more about ROMS during the process. As it turns out, my prior idealized estuary work provided enough background that I could figure out how to intervene.

First, I share some results of the upwelling case as-is. Then, I show some preliminary results of the upwelling case with a single vertical source in the center of the grid. The most notable observation is that the vertical velocity, w, appears to be negative throughout the water column for all time at the location of the source. Lastly, I share some details of how I intervened in the upwelling code (mostly for my own future reference).

---
## The ROMS Upwelling Case

The upwelling case was the first thing I ran when I installed ROMS. At the time, I had no idea what I was running. I simply knew that *something* had run and ROMS was working. However, I now needed to understand how the upwelling case is set up to be able to intervene and add my own vertical sources.

The ROMS upwelling grid is an 80 km x 41 km domain with a U-shaped channel that reaches a max depth of 150 m. There are 16 vertical layers. The channel is periodic on the E-W boundaries, and a uniform wind stress is applied from the east to the west. The domain is seemingly in the Southern hemisphere as surface transport is deflected to the south and waters upwell in the north. The water column is initially vertically stratified by temperature. Salinity seems to be uniform. The upwelling case runs for 5 days by default.

To help visualize what is going on, the video below shows a surface plot and cross section of temperature over the 5 days (with one frame every 6 hours).

<video src="https://user-images.githubusercontent.com/15829099/228300330-2305c335-5450-4117-a9ca-3439c2868af0.mp4" controls="controls" style="max-width: 800px;"></video><br>Fig. 1 Section temperature in the default ROMS upwelling case.
<br>

The following three videos show section plots of u, v, and w for the default upwelling case.

<video src="https://user-images.githubusercontent.com/15829099/228301699-80fb6139-6250-49c3-b70e-e1e140d35703.mp4" controls="controls" style="max-width: 800px;"></video><br>Fig. 2 Section u (E-W velocity) in the default ROMS upwelling case.
<br>

<video src="https://user-images.githubusercontent.com/15829099/228301701-f992324e-9e5d-4d92-9500-e156716117f2.mp4" controls="controls" style="max-width: 800px;"></video><br>Fig. 3 Section v (N-S velocity) in the default ROMS upwelling case.
<br>

<video src="https://user-images.githubusercontent.com/15829099/228301703-91b73d81-7760-42a7-85f8-aee478033294.mp4" controls="controls" style="max-width: 800px;"></video><br>Fig. 4 Section w (vertical velocity) in the default ROMS upwelling case.
<br>

---
## Adding a WWTP to the ROMS Upwelling Case

After piecing apart the ROMS upwelling case, I then added a vertical source in the middle of the domain. I'll call our source a WWTP. I left LtracerSrc = F F, meaning that water from the WWTP enters the domain at the same temperature and salinity as water already in the grid cell. Thus, the WWTP only adds volume to the system.

The WWTP discharges at a rate of 10 m3/s, distributed uniformly across all 16 sigma layers. I had originally tested 1 m3/s, but the difference was so small that I could not see much going on. Also, 10 m3/s is still relatively small compared to the grid cell size. Each cell is 1 km x 1 km in the horizontal, and roughly 10 m deep. Discharging 1/16 of 10 m3/s into a grid cell is thus tiny compared to the pre-existing volume.

I do not yet have differencing plots, and by eye I did not pick up on changes to temperatue, u, and v. However, there is clearly a consistent negative vertical velocity throughout the water column at the location of the WWTP. Video below.

<video src="https://user-images.githubusercontent.com/15829099/228304481-5af722fc-7530-4863-a567-d97e823c751c.mp4" controls="controls" style="max-width: 800px;"></video><br>Fig. 5 Section w (vertical velocity) in the ROMS upwelling + WWTP case.
<br>

I think we're narrowing in on the problem. I'll need help brainstorming what to do next, but my ideas now are to:

- Create difference plots to show [with WWTP] minus [no WWTP] cases
- Increase discharge rate until something blows up (maybe)

Once we have something really exciting, I can share the findings with John Wilkin and the ROMS community.

---
## Notes on Code Intervention

This section is mostly for my own notes so in the future I will have an easier time intervening in ROMS test cases.

**Forcing for new source**

First, I created a forcing file called wwtp.nc, which was based off of the rivers.nc file used in LiveOcean runs. This forcing file contains the position, discharge rate, etc. for the single vertical source. I saved this file in LO_user/forcing/upwelling, and modified the roms_upwelling.in file to search for this forcing.

**Changes to the dot in file in LO_roms_user/upwelling**

I made a copy of the original roms_upwelling.in file, then modified it to accomodate a vertical source.

First, I renamed the output files so that they would not be confused with output from the default ROMS upwelling test (Fig 6).

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/228308351-ccc6d674-936d-4628-8db5-4d28f75125ab.png" width="300"/><br>Fig 6. Changed output file names.</p><br>

Then I needed to enable vertical sources by setting LwSrc to true (Fig 7).

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/228308346-abaa804a-783c-49d9-b117-085b07c1eb51.png" width="600"/><br>Fig 7. Enable vertical sources.</p><br>

Lastly, I added the filepath to the forcing file I created. This tells ROMS where to look for information on the WWTP.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/228308359-72f214a4-34d9-4c16-a842-643201b892fd.png" width="600"/><br>Fig 8. Tell ROMS where to look for source/sink forcing.</p><br>

**Changes to the shell script**

Then I made a copy of klone_batch0.sh, the shell script that runs the upwelling case. I renamed the generated log file, and also told the script to use the new dot in file with my WWTP changes (Fig 9).

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/228308356-1835fc05-a2aa-4a65-b894-102a37f6b0e6.png" width="600"/><br>Fig 9. Tell klone to run the new upwelling case which includes the WWTP.</p><br>

**Results and Analysis**

Just in case I can't find it later, all analysis and plots were generated in LO_user/upwelling-tests. The forcing file was created in LO_user/forcing/upwelling.
