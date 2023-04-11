## Simple WWTP Tests in the ROMS Upwelling Grid

This week I simplified the ROMS upwelling test case by removing the surface wind stress and by distributing sigma layers evenly. Then I ran some test cases with a single WWTP in the grid. These test cases include:

- WWTP discharging to bottom sigma layer at 10 m3/s (source)
- WWTP discharging to bottom sigma layer at -10 m3/s (sink)

In the first test, vertical velocity (w) does the opposite of what I would expect it to do: w is predominantly negative with max magnitude at the surface. The second test case produces the same results, but with flipped signs. More details below.

---
## Establishing New Baseline

I made two changes to the ROMS upwelling test case to establish a more simplified baseline. First, I removed the surface wind stress. Second, I distributed the sigma layers evenly throughout the water column. Without any additional forcing, this new model configuration should do nothing throughout the run.

**Removing Surface Wind Stress**

I undefined ANA_SMFLUX in the ROMS header file to remove the default applied wind stress in the ROMS upwelling case.

In addition to this change, I also created my own forcing files for sustr and svstr (surface u- and v-stress, respectively) which were populated with zeros. These forcing files were fed to ROMS in the dot-in files (Fig. 1).

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/230693674-26428dc6-52cf-4945-a5aa-3e97d0413078.png" width="600"/><br>Fig 1. Code to tell ROMS where to look for surface stress forcing files.</p><br>

**Distributing Sigma Layers Evenly Throughout Water Column**

The ROMS dot-in file contained a comment at the end describing how to make sigma layers evenly spaced (Fig. 2).

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/230693667-cc795485-1489-4824-b8c5-e44c7fb935e0.png" width="600"/><br>Fig 2. How to tell ROMS to distribute sigma layers evenly.</p><br>

THETA_S tells ROMS how much to stretch the sigma layers near the surface. THETA_B tells ROMS how much to stretch the sigma layers near the bottom. TCLINE tells ROMS at what depth to begin applying stretching functions. Since TCLINE is large, no stretching is applied.

### Results

The video in Figure 3 shows the surface velocity, surface vertical transport, sea surface height, and surface temperature over the course of 5 days in the simplified ROMS test case. The video in Figure 4 shows the section temperature over 5 days. As expected, these videos are boring-- nothing happens. Thus, simplifying the ROMS upwelling test case was a success!

<p style="text-align:center;"><video src="https://user-images.githubusercontent.com/15829099/231003262-2f910359-f7f8-494d-85b2-66d9610b58f6.mp4" controls="controls" style="max-width: 800px;"></video><br>Fig. 3 Surface velocity, vertical transport, SSH, and temperature in baseline run.</p><br>

<p style="text-align:center;"><video src="https://user-images.githubusercontent.com/15829099/230694106-ed965d27-a40e-4636-90fb-d65545a1c73c.mp4" controls="controls" style="max-width: 800px;"></video><br>Fig. 4 Section temperature in the baseline run.</p><br>

From this point onwards, all other test runs are compared to this new baseline in which there is no surface stress and the sigma layers are evenly spaced.

---
## 10 m3/s WWTP Discharging from the Bottom

After establishing a baseline, I created a model run which added a single WWTP to the center of the domain. This WWTP discharges 10 m3/s to solely the bottom sigma layer (**botwwtp10** = **bot**tom, **wwtp**, **10** m3/s). 

Figure 5 shows differences in surface parameters between this botwwtp10 scenario and the baseline scenario. Note that for SSH (zeta), the deviation from the average difference (per timestep) in SSH is plotted rather than the difference in SSH. This extra calculation is necessary because SSH constantly increases with time as the WWTP discharges, and the magnitude of the SSH increase is greater than the SSH variation over the domain. Subtacting the average over the domain thus allows us to visualize which regions are higher or lower than other regions.

<p style="text-align:center;"><video src="https://user-images.githubusercontent.com/15829099/231003542-40857018-07fc-4916-8a21-446ee0f107fe.mp4" controls="controls" style="max-width: 800px;"></video><br>Fig. 5 Surface plot of the difference between the botwwtp10 scenario and the baseline scenario. Parameters include u, v, w, omega, SSH, and temperature.</p><br>

Note that omega does not deviate from the baseline run at the surface. Also note that omega in the baseline run was always zero everywhere at the surface. This means that in this botwwtp10 run, omega is also zero at the surface-- there is not net surface transport. More details on this later.

The next series of videos show differencing plots over a section. Note that a single video was created for each of temperature, u, v, w, and omega.

<p style="text-align:center;"><video src="https://user-images.githubusercontent.com/15829099/230693668-bacc33d8-6996-485c-9d56-a5d30c86fa64.mp4" controls="controls" style="max-width: 800px;"></video><br>Fig. 6 Section plot of the difference in temperature between the botwwtp10 scenario and the baseline scenario.</p><br>

<p style="text-align:center;"><video src="https://user-images.githubusercontent.com/15829099/230693669-b360e4dc-3158-4560-bf5b-d3f468b98f06.mp4" controls="controls" style="max-width: 800px;"></video><br>Fig. 7 Section plot of the difference in u between the botwwtp10 scenario and the baseline scenario.</p><br>

<p style="text-align:center;"><video src="https://user-images.githubusercontent.com/15829099/230693671-612e140c-9132-4110-9f65-c7721a0f3ed5.mp4" controls="controls" style="max-width: 800px;"></video><br>Fig. 8 Section plot of the difference in v between the botwwtp10 scenario and the baseline scenario.</p><br>

<p style="text-align:center;"><video src="https://user-images.githubusercontent.com/15829099/230693673-9abc61b2-93fc-41ec-af9e-d8f906a6d617.mp4" controls="controls" style="max-width: 800px;"></video><br>Fig. 9 Section plot of the difference in w between the botwwtp10 scenario and the baseline scenario.</p><br>

<p style="text-align:center;"><video src="https://user-images.githubusercontent.com/15829099/231003999-2f9c2cdc-76ac-4fc4-be70-6ac645cf1802.mp4" controls="controls" style="max-width: 800px;"></video><br>Fig. 10 Section plot of the difference in omega between the botwwtp10 scenario and the baseline scenario.</p><br>

---
## Analysis

The vertical results make no sense to me. It is strange that the largest magnitude velocity is at the surface and is negative, even though the WWTP is a source at the bottom. What is even stranger is that u and v seem to correctly emanate from the bottom sigma layer for the entire duration of the run. What could possibly be happening to w?

What is curious is that in the initial frame (Fig. 11), the section w behaves as I would anticipate it would: w is generally positive throughout the water column, with a max magnitude just above the bottom sigma layer.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/230693676-d9259d07-79e2-48a3-b642-b4200c8eddd5.png" width="700"/><br>Fig. 11 Section w in the first history file of the botwwtp10 run.</p><br>

In the next history file (6 hours later in model time), we see that w is predominantly negative throughout the water column with a max magnitude at the surface (Fig. 12). Also strange is that the negative w value at the surface remains constant with time after this point. 

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/230693678-21398aa8-b206-4ca1-9aa4-49a8b818bc8f.png" width="700"/><br>Fig. 12 Section w in the second history file of the botwwtp10 run.</p><br>

I have observed similar behavior before. However, prior test cases were much more hydrodynamically complex (i.e. with tides, rivers, heat from the atmosphere, etc.). This test setup is simple, so we can have confidence that the issue with w is solely caused by the vertical source.

Now let's look at similar snapshots for omega, the vertical transport. In the initial frame (Fig. 13), omega has a similar expected trend as w: there is max upwards transport just above the wwtp, and generally omega is positive throughout the water column.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/231007398-1c8d3236-907b-4a61-9e45-9992fa2e1810.png" width="700"/><br>Fig. 13 Section omega in the first history file of the botwwtp10 run.</p><br>

Then, in the next history file (Fig 14), omega is in transition and the dominant upwards transport appears to decay. 

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/231007399-f26ae465-a069-497d-96d7-a338f2a49fca.png" width="700"/><br>Fig. 14 Section omega in the second history file of the botwwtp10 run.</p><br>

Soon afterwards, the section profile for omega appears to reach a more steady state in which omega is largely zero throughout the water column. The exception is a slight upwards transport one sigma layer above the wwtp, and a slight negative transport two sigma layers above the wwtp. Figure 15 shows the last history file from this model run, demonstrating the aforementioned observation.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/231007402-a104d50e-10db-4ac8-92b4-b997bbf21827.png" width="700"/><br>Fig. 15 Section omega in the last history file of the botwwtp10 run.</p><br>

So, what's going on here? To me, it appears as if omega is always forced to be zero at the surface. However, the presence of the vertical source causes SSH to rise everywhere. Figure 16. below shows a video of the difference in SSH between botwwtp10 and the baseline run (without subtracting out the average SSH difference).

<p style="text-align:center;"><video src="https://user-images.githubusercontent.com/15829099/231008651-e19b6c27-0e4a-432d-bdfa-be3e00231820.mp4" controls="controls" style="max-width: 200px;"></video><br>Fig. 16 Difference in SSH between the botwwtp10 run and the baseline run.</p><br>

My theory is that for omega to remain zero at the surface despite the rise in SSH, then there must be a downwards vertical velocity (w) to compensate for the rising sea surface. Recall that w was a constant -8.8e-6 m/s at the surface for all time (except the first timestep).

To test this hypothesis, I fit a line to a plot of SSH vs. time at the location of the WWTP. If SSH is rising at a rate equal and opposite to -8.8e-6 m/s (surface w at the WWTP), then our strange w values are likely an artifact of an imposed "omega=0" rule at the surface.

Figure 17 shows zeta vs. time (at location of WWTP), with its calculated slope. Zeta increases at a rate 3 orders of magnitude more slowly than the surface w. Thus, the negative surface w doesn't seem to directly compensate for the increasing SSH. My hypothesis for the negative surface w has been rejected.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/231012085-974d389d-fa5f-434e-b9c8-5233adcc772e.png" width="400"/><br>Fig. 17 SSH vs. time at the location of the WWTP.</p><br>

I've thought about this problem in more depth, and have realized that the dynamics are complicated (and I'm confused). We have zeta increasing at the surface, but w at the surface is also negative. Omega must be zero because there cannot be transport through the surface, or else the surface would no longer be the surface. To make things more complicated, the number of sigma layers is constant. Thus, as SSH increases, the grid cells are also stretching vertically. This is a lot to think about! With all of these dynamics in consideration, w, to me, is still the most problematic parameter.

---
## -10 m3/s Sink at the Bottom

As I mentioned previously, I also tested how ROMS would respond to a sink of volume. This run is exactly the same as botwwtp10, except the discharge rate is *negative*. Thus, I've called this run **botwwtp-10** (**bot**tom, **wwtp**, **-10** m3/s). 

Based on a quick look at the results, it seems as if this test case behaves exactly the same as botwwtp10, except that the signs of the velocity are flipped. 

Figure 18 shows a differencing video of the surface parameters (botwwtp-10 minus baseline).

<p style="text-align:center;"><video src="https://user-images.githubusercontent.com/15829099/231054701-fd215718-bf42-49f2-b5be-8bf67eb3f466.mp4" controls="controls" style="max-width: 800px;"></video><br>Fig. 18 Surface plot of the difference between the botwwtp-10 scenario and the baseline scenario. Parameters include u, v, w, omega, SSH, and temperature.</p><br>

For additional comparison, Figure 19 shows a section of the difference in w between this botwwtp-10 run and the baseline run. Perfectly opposite to the botwwtp10 run, the sink create a max *positive* velocity at the surface with magnitude 8.8e-6 m/s.

<p style="text-align:center;"><video src="https://user-images.githubusercontent.com/15829099/231056391-196473d1-6dde-46c8-99b6-7a1f7d0fc669.mp4" controls="controls" style="max-width: 800px;"></video><br>Fig. 19 Section plot of the difference in w between the botwwtp-10 scenario and the baseline scenario.</p><br>



