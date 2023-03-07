## WWTP Hydrodynamic Differencing & More

This week I created some hydrodynamic differencing movies using the "with WWTPs" and "no WWTPs" runs from last week. There appears to be quite a significant difference between these two runs.

I also took a look at my past idealized estuary results which included freshwater point sources. At the location of WWTPs, there is a large, negative vertical velocity which suggests that some oddities were present in the idealized tests as well.

Finally, I ran several test cases to see if anything would improve the problem at Birch Bay WWTP. Nothing seemed to do the trick. Tests include:
- Discharging to surface
- Updating ROMS source code
- Using HSIMT advection scheme (instead of MPDATA)
- Changing tiling scheme to 5x16 (instead of 8x10)

I'll note that Parker helped go through the ROMS source code and posed a question to the ROMS community online (thanks Parker). From the responses we received, it sounded like there may be a potential for a bug if a WWTP exists on the boundary of the tiling pattern. Hence, I tested a different tiling pattern. However, changing ntilei and ntilej did not fix the issue.

More details below.

---
## Hydrodynamic Differencing

Per Alex's suggestion, I spent some time this week developing a script that would generate hydrodynamic differencing plots between the "with WWTPs" case and the "no WWTPs" case. Recall that these runs were conducted over the entire month of February 2021.

Figure 1 shows a screenshot of the type of plot/movie I am now able to generate. The region of interest, denoted by the pink square, can be adjusted to look at any location within the domain. To start, I focused on Main Basin near King County because there are a couple large dischargers in this area. This is also the same region we looked at last week.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/223488892-9474028b-eb35-441b-bcbc-2b4fbb1989fd.png" width="600"/><br>Fig 1. Snapshot of new hydrodynamic differencing plot capability.</p><br>

Using this new script, I generated an hourly movie of the hydrodynamic differencing in this Main Basin region (Fig 2). There is very clearly a difference between the "with WWTPs" and "no WWTPs" runs, even in waters several kilometers from a WWTP. Every so often, the velocities and SSH anomaly also appears to become *strikingly* different, suggesting that a WWTP began to have an instability. For instance, take a look at timestamp 1:22 in the video (corresponding to 2021.02.28, 06:00 PST, 0015 hour). Something very strange is happening at West Point WWTP that is highly reminiscent of our problems at Birch Bay WWTP and Oak Harbor Lagoon WWTP.

<video src="https://user-images.githubusercontent.com/15829099/223488900-46fa150b-5062-4a88-90a7-e9e4489c6521.mp4" controls="controls" style="max-width: 800px;"></video><br><p style="text-align:center;">Fig. 2 Hourly movie of the hydrodynamic difference between the "with WWTP" and "no WWTP" run in Main Basin over the month of February.
</p><br>

Creating a video with hourly resolution took *a long long time*. But it did give us some good resolution, and helped point out some weird behavior that I may not have otherwise observed. I'm planning to make more movies of other regions in the coming week.

---
## Past Idealized Estuary Results

When I first added point sources to my idealized estuaries, I only looked at results of salinity because I used freshwater as a tracer.

Given our current problems, I revisited these old results and looked explicitly at velocities and SSH anomaly.

Note that the point sources in the idealized estuaries discharge uniformly to all sigma layers. The transport of each source is 1000 m3/s ("wow that's a lot" was my reaction too). Figure 3 shows the surface velocities over the full idealized estuary domain.

<video src="https://user-images.githubusercontent.com/15829099/223492388-6ff6a8b9-e1b6-4beb-829c-b506088a4ad6.mp4" controls="controls" style="max-width: 800px;"></video><br><p style="text-align:center;">Fig. 3 Hourly movie of the surface velocity in the idealized estuary.
</p><br>

Nothing stands out as being too weird in this video. However, when I take a closer look at the WWTPs, I begin to notice that the vertical velocity near the source is large and negative. Figure 4 below shows a cloe-up of WWTP 4.

<video src="https://user-images.githubusercontent.com/15829099/223493917-677f82fc-eb16-4d77-b962-2026032d11d7.mp4" controls="controls" style="max-width: 800px;"></video><br><p style="text-align:center;">Fig. 4 Hourly movie of the surface velocity in the idealized estuary at WWTP 4.</p><br>

We see the same negative vertical velocity at WWTP 5 below. As an added bonus, this video includes SSH anomaly, though nothing seems to be anomalous about the anomaly.

<video src="https://user-images.githubusercontent.com/15829099/223493214-cd6aaf00-20d1-4d52-9179-09a8b68ede84.mp4
" controls="controls" style="max-width: 800px;"></video><br><p style="text-align:center;">Fig. 5 Hourly movie of the surface velocity in the idealized estuary at WWTP 5.</p><br>

---
## Other Tests

I tried to run a few other tests this week, but still had blow ups at Birch Bay WWTP. The next several figures show surface velocities near Birch Bay WWTP just before the model blows up. Overall, none of these attempts fixed the issue.

With the exception of the first run, all of these runs were conducted without the biogeochemistry module so that the model would run more quickly.

**Discharging to surface**
<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/223488916-1e26b0fd-1074-418b-85e1-4bff323f8565.png" width="750"/><br>Fig 6. Birch Bay WWTP surface velocities for a run in which all WWTPs discharge only to the surface.</p><br>

**Running the model with most up-to-date ROMS source code**
<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/223488922-781bdab2-6e8f-478d-a72b-6c2f0e517dcc.png" width="750"/><br>Fig 7. Birch Bay WWTP surface velocities after updating ROMS.</p><br>

**Switched to HSIMT advection scheme for temperature and salt (instead of MPDATA)**
<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/223488920-f99bda7f-4330-4daa-9473-e85c636a2e46.png" width="750"/><br>Fig 8. Birch Bay WWTP surface velocities using HSIMT advection scheme.</p><br>

**Changed ntilei and ntilej to 5x16 (from 8x10)**
<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/223488911-8d7da3ab-8349-4a2d-97e6-91f32f42e78d.png" width="750"/><br>Fig 9. Birch Bay WWTP surface velocities for a run that uses a different tiling pattern.</p><br>
