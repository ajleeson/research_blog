## Simple WWTP Tests in the ROMS Upwelling Grid

This week I simplified the ROMS upwelling test case by removing the surface wind stress and by distributing sigma layers evenly. Then I ran some test cases with a single WWTP in the grid. These test cases include:

- WWTP discharging to bottom sigma layer at 10 m3/s (source)
- WWTP discharging to bottom sigma layer at -10 m3/s (sink)

In the first test, vertical velocity (w) does the opposite of what I would expect it to do.

I also checked compared the total volume in the domain to the expected volume given the discharge rate of the WWTP.

---
## Establishing New Baseline

I made two changes to the ROMS upwelling test case to establish a more simplified baseline. First, I removed the surface wind stress. Second, I distributed the sigma layers evenly throughout the water column. Without any additionally forcing, this new model configuration should do nothing throughout the run.

**Removing Surface Wind Stress**

I undefined ANA_SMFLUX in the ROMS header file to remove the default applied wind stress in the ROMS upwelling case.

In addition to this change, I also created my own forcing files for sustr and svstr (surface u- and v-stress, respectively) which were populated with zeros. These forcing files were fed to ROMS in the dot-in files (Fig. 1).

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/230693674-26428dc6-52cf-4945-a5aa-3e97d0413078.png" width="600"/><br>Fig 1. Code to tell ROMS where to look for surface stress forcing files.</p><br>

**Distributing Sigma Layers Evenly Throughout Water Column**

The ROMS dot-in file contained a comment at the end describing how to make sigma layers evenly spaced (Fig. 2).

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/230693667-cc795485-1489-4824-b8c5-e44c7fb935e0.png" width="600"/><br>Fig 2. How to tell ROMS to distribute sigma layers evenly.</p><br>

THETA_S tells ROMS how much to stretch the sigma layers near the surface. THETA_B tells ROMS how much to stretch the sigma layer near the bottom. TCLINE tells ROMS at what depth to being applying stretching functions. Since TCLINE is large, no stretching is applied.

### Results

The video in Figure 3 shows the surface velocity, surface temperature, and sea surface height over the course of 5 days in the simplified ROMS test case. The video in Figure 4 shows the section temperature over 5 days. As expected, these videos are boring-- nothing happens. Thus, simplifgying the ROMS upwelling test case was a success!

<p style="text-align:center;"><video src="https://user-images.githubusercontent.com/15829099/230694128-9068311a-7c64-477f-b221-94e7f9c82856.mp4" controls="controls" style="max-width: 800px;"></video><br>Fig. 3 Surface velocity, temperature, and SSH in baseline run.</p><br>

<p style="text-align:center;"><video src="https://user-images.githubusercontent.com/15829099/230694106-ed965d27-a40e-4636-90fb-d65545a1c73c.mp4" controls="controls" style="max-width: 800px;"></video><br>Fig. 4 Section temperature in the baseline run.</p><br>

From this point onwards, all other test runs are compared to this new baseline in which there is no surface stress and the sigma layers are evenly spaced.

---
## 10 m3/s WWTP Discharging from the Bottom

After establishing a baseline, I created a model run which added a single WWTP to the center of the domain. This WWTP discharges 10 m3/s to solely the bottom sigma layer (**botwwtp10** = **bot**tom, **wwtp**, **10** m3/s). 

Figure 5 shows differences in surface parameters between this botwwtp10 scenario and the baseline scenario. Note that for SSH (zeta), the deviation from the average difference (per timestep) in SSH is plotted rather than the difference in SSH. This extra calculation is necessary because SSH constantly increases with time as the WWTP discharges, and the magnitude of the SSH increase is greater than the SSH variation over the domain. Subtacting the average over the domain thus allows us to visualize which regions are higher or lower than other regions.

<p style="text-align:center;"><video src="https://user-images.githubusercontent.com/15829099/230693675-3f892f67-c498-46b6-b819-07090714c045.mp4" controls="controls" style="max-width: 800px;"></video><br>Fig. 5 Surface plot of the difference between the botwwtp10 scenario and the baseline scenario. Parameters include u, v, w, temperature, and SSH</p><br>

The next series of videos shows differencing plots over a section. Note that a single video was created for each of temperature, u, v, and w.

<p style="text-align:center;"><video src="https://user-images.githubusercontent.com/15829099/230693668-bacc33d8-6996-485c-9d56-a5d30c86fa64.mp4" controls="controls" style="max-width: 800px;"></video><br>Fig. 6 Section plot of the difference in temperature between the botwwtp10 scenario and the baseline scenario.</p><br>

<p style="text-align:center;"><video src="https://user-images.githubusercontent.com/15829099/230693669-b360e4dc-3158-4560-bf5b-d3f468b98f06.mp4" controls="controls" style="max-width: 800px;"></video><br>Fig. 7 Section plot of the difference in u between the botwwtp10 scenario and the baseline scenario.</p><br>

<p style="text-align:center;"><video src="https://user-images.githubusercontent.com/15829099/230693671-612e140c-9132-4110-9f65-c7721a0f3ed5.mp4" controls="controls" style="max-width: 800px;"></video><br>Fig. 8 Section plot of the difference in v between the botwwtp10 scenario and the baseline scenario.</p><br>

<p style="text-align:center;"><video src="https://user-images.githubusercontent.com/15829099/230693673-9abc61b2-93fc-41ec-af9e-d8f906a6d617.mp4" controls="controls" style="max-width: 800px;"></video><br>Fig. 9 Section plot of the difference in w between the botwwtp10 scenario and the baseline scenario.</p><br>

The vertical results make no sense to me. It is strange that the largest magnitude velocity is at the surface and is negative, even though the WWTP is a source at the bottom. What is even stranger is that u and v seem to correctly emanate from the bottom sigma layer for the entire duration of the run. What could possibly be happening to w?

What is curious is that in the initial frame (Fig. 10), the section w behaves as I would anticipate it would: w is generally positive throughout the water column, with a max magnitude just above the bottom sigma layer.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/230693676-d9259d07-79e2-48a3-b642-b4200c8eddd5.png" width="700"/><br>Fig. 10 Section w in the first history file of the botwwtp10 run.</p><br>

In the next history file (6 hours later in model time), we see that w is predominantly negative throughout the water column with a max magnitude at the surface (Fig. 11). Also strange is that the negative w value at the surface remains constant with time after this point. 

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/230693678-21398aa8-b206-4ca1-9aa4-49a8b818bc8f.png" width="700"/><br>Fig. 11 Section w in the second history file of the botwwtp10 run.</p><br>

I have observed similar behavior before. However, prior test cases were much more hydrodynamically complex (i.e. with tides, rivers, heat from the atmosphere, etc.). This test setup is simple, so we can have confidence that the issue with w is solely caused by the vertical source.

[Look at omega!!!!!!]