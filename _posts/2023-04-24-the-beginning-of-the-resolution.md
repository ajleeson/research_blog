## The Beginning of the Resolution

Late last week, Parker and I met with John Wilkin to discuss the strangeness with vertical sources. The meeting went well, and while we don't have a resolution yet, there is certainly interest in getting to the bottom of the problem.

For the last month, I have been focusing my attention on oddities with w. However, I recently learned that w is calculated diagnostically and is only used as an output variable. Rather than w, I should instead be looking at omega, the S-coordinate vertical momentum component.

---
## What is omega?

In prior blog posts, I incorrectly stated that omega is the vertical transport [m3/s]. As it turns out, very old versions of the ROMS code did express the output omega in units of m3/s. Though this is no longer the case. Omega is actually in units of m/s, as clarified by this [ROMS discussion post](https://www.myroms.org/forum/viewtopic.php?f=19&t=1150).

Then how is omega different from w?
w is the vertical velocity in z-coordinates, but omega is the vertical velocity in sigma-coordinates. Omega is orthogonal to the boundaries of the sigma layers. Figure 1 shows my best attempt at depicting omega for a grid cell with slanted sigma layers.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/234327432-9008f7be-740f-4c49-b583-d190dd05e4c0.jpg" width="300"/><br>Fig 1. Depiction of omega through a grid cell.</p><br>

The ROMS source code forces omega = 0 at the bottom and top sigma boundary.

---
## Looking at Omega

John shared some new files in which he set a neutrually buoyant source discharging to a single grid cell located mid-way in the water column. The source discharges at 10 m3/s. He noticed that omega looked unexpected at the end of a 15 minute run, so I've run the model to see if I can reproduce the strange values he observed.

Surface and section profiles for this run are shown in the following videos. Note that omega is plotted in place of w.

Also note that the source is still discharging freshwater. However, John did some magic so that salinity is now a passive tracer of the source, and density is solely a function of temperature. The inflowing source temperature is the same as the water that is already in the grid cell.

<p style="text-align:center;"><video src="https://user-images.githubusercontent.com/15829099/234332777-5aab1def-7aa8-43e2-9d76-8a17e1de9089.mp4" controls="controls" style="max-width: 550px;"></video><br>Fig. 2 Surface velocity and SSH in test case as-is.</p><br>

<p style="text-align:center;"><video src="https://user-images.githubusercontent.com/15829099/234332905-0ce90854-1cdc-4b68-9c62-e4227ed075a5.mp4" controls="controls" style="max-width: 800px;"></video><br>Fig. 3 Section salinity in test case as-is.</p><br>

<p style="text-align:center;"><video src="https://user-images.githubusercontent.com/15829099/234333925-02b5bf0f-3744-4223-8eb3-d2a3033ac635.mp4" controls="controls" style="max-width: 800px;"></video><br>Fig. 4 Section u in test case as-is.</p><br>

<p style="text-align:center;"><video src="https://user-images.githubusercontent.com/15829099/234333999-0c6003e3-9a3f-406e-b0dc-b486f6c481f6.mp4" controls="controls" style="max-width: 800px;"></video><br>Fig. 5 Section v in test case as-is.</p><br>

<p style="text-align:center;"><video src="https://user-images.githubusercontent.com/15829099/234333800-a1edc80c-07ce-4b3a-9770-6fb726a91bd9.mp4" controls="controls" style="max-width: 800px;"></video><br>Fig. 6 Section omega in test case as-is.</p><br>

My omega results do seem to match what John sent.