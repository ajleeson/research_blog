## Volume-less WWTPs in LiveOcean and ORCA Buoy Comparison

This week I tested a new version of volume-less vertical sources. Although the new version appears to work in the idealized domain, the vertical sources still behave oddly in LiveOcean.

I also re-visited the preliminary DO results from the "WWTP as tiny river" tests we ran for GRC. This week I extracted data from all 6 ORCA buoys and compared their DO timeseries to LiveOcean output.

More details below.

---
## Re-testing volume-less sources

This week Parker shared an improved version of volume-less vertical source code with me. Using this new code for the idealized estuary seemed to resolve issues with the forcing inputs. In other words, the inflowing temperature and salinity matched my expectations. Furthermore, I confirmed that the source does not add volume, and thus does not induce any barotropic velocities. Success!

### Testing in LiveOcean

Next, I tested the improved volume-less vertical source code in LiveOcean. The run did not blow up, but it did appear to get stuck at hour 15. It's worth noting that mox was having issues over the weekend. Thus, it's unclear if the issues with my experiment are due to the vertical sources, mox, or a combination of both.

In any case, I took a look at the surface velocities near Birch Bay WWTP at hour 15. I also started a new LiveOcean run without any WWTPs and looked at surface velocities in the same region. Figure 1 shows a comparison of surface velocities in the runs with and without WWTPs. Even though the "with WWTP" run did not blow up, I still have issues with the surface velocities. In particular, the pattern of "zero then high" velocity just south of the WWTP in panel (b) is a familiar issue we have observed with vertical sources in the past. Therefore, the volume-less vertical sources don't appear to fully resolve issues with vertical sources.

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/bbc2ac90-866d-42ad-a3fb-75a099fc3309" width="600"/><br>Fig 1. Surface velocity near Birch Bay WWTP on hour 15. Panels (a) and (b) correspond to surface u and v with WWTPs included. Panels (c) and (d) correspond to surface u and v with WWTPs excluded.</p><br>

### Volume-less, neutrally-buoyant sources

We have not formally ruled out density instabilities as a potential issue with the WWTPs. This week I have decided to test our hypothesis:

**Hypothesis:** Density instability causes strange WWTP behavior

**Test:** Run LiveOcean with neutrally buoyant discharge (LtracerSrc = F F)

Note that the implementation of this test case means that inflowing water from both WWTPs *and* rivers is neutrally buoyant. If we decide that neutrally buoyant WWTPs is a permanent solution, then I will need to figure out how to only turn off salinity and temperature for WWTPs and not for rivers.

**Result:** The run blew up at hour 20 at Birch Bay WWTP (Fig 2).

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/a8a56ee9-52e0-458d-95a9-26515f417931" width="800"/><br>Fig 2. Surface velocities near Birch Bay WWTP on hour 20 just before blow up. WWTPs are volume-less, and all rivers and WWTPs discharge neutrally buoyant water.</p><br>

This result is very puzzling to me. The WWTPs are volume-less and the water cells are not changing density. At this point, the WWTPs should not be directly influencing the physics. The WWTPs are only altering the biogeochemistry. Thus, these results are pointing towards the biology as a culprit.

However, in a previous test I disabled the NPZD module, but the model still blew up. This previous test points towards the hydrodynamics as the culprit. So what is going on?

I have double-checked the compiled code and log file and have verified that the WWTPs were volume-less, and that the inflowing water is neutrally buoyant.

Next week I will run a new test with volume-less, neutrally-buoyant, biology-less sources. LiveOcean should not even notice that the WWTPs are present, but I'm eager to find out if ROMS will still blow up. Another test idea, similar to what Ben suggested: enable WWTPs, but set discharge to 0 m3/s.

I'm hoping to implement some of the "strong inference" technique discussed in Platt (1964) so I can test and (maybe?) disprove my hypotheses more quickly.

---
## ORCA buoy comparison

This week I also compared model DO output to all of the ORCA buoys. Unfortunately it seems that two of the ORCA buoys do not have bottom DO data for the year 2017. Figure 3 shows the comparison.

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/387ceaa4-d068-4b42-824c-042fdd1556cc" width="800"/><br>Fig 3. Comparison of ORCA buoy observations and LiveOcean output bottom DO.</p><br>

As we have seen at Twanoh station buoy in Lynch Cove, the model tends to overpredict bottom DO concentrations.

It's also worth noting that the "baseline" and "with WW discharge" results are very similar at all locations. At Carr Inlet, the "with WW discharge" appears to have the largest reduction in bottom DO compared to the baseline case. However, this difference is also small.

---
### More information about the UCLA ROMS model

I read more about the UCLA ROMS-BEC model of the SCB in Kessouri et al., 2021. As it turns out, the modelers *are* adding volume with their vertical sources.

The authors also discussed their horizontal and vertical effluent distribution scheme. It seems relatively straightforward if we decide to implement something similar.

In the horizontal, effluent is distributed across a 3x3 square of grid cells. When vertically integrated the center cell receives 4/9 of the effluent, the adjacent cells receive 1/9 of the effluent, and the diagonals receive 1/36.

Vertically, the effluent follows a Gaussian distribution:

$$H(z) = \exp\Big(\frac{-z^2}{d_s^2}\Big)$$

where z is the depth of the plume relative to the surface, and d_s is the vertical scale of the effluent plume.

Once the WWTPs work in LiveOcean, I'd like to experiment with some similar effluent distributions in our model.

---
## What have I been reading?
*(accountability metric)*

Kessouri, F., McLaughlin, K., Sutula, M., Bianchi, D., Ho, M., McWilliams, J. C., Renault, L., Molemaker, J., Deutsch, C., &#38; Leinweber, A. (2021). Configuration and Validation of an Oceanic Physical and Biogeochemical Model to Investigate Coastal Eutrophication in the Southern California Bight. <i>Journal of Advances in Modeling Earth Systems</i>, <i>13</i>(12). https://doi.org/10.1029/2020MS002296 

Platt, J. R. (1964). Strong Inference Certain systematic methods of scientific thinking may produce much more rapid progress than others. <i>Science</i>, <i>146</i>(3642), 347–353. https://doi.org/10.1126/science.146.3642.347

Sutula, M., Ho, M., Sengupta, A., Kessouri, F., McLaughlin, K., McCune, K., &#38; Bianchi, D. (2021). A baseline of terrestrial freshwater and nitrogen fluxes to the Southern California Bight, USA. <i>Marine Pollution Bulletin</i>, <i>170</i>. https://doi.org/10.1016/j.marpolbul.2021.112669