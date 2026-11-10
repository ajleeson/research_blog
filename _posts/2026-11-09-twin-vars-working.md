## Twin variables might be working (!?)

Over the past two weeks, we have made a lot of progress on the twin variables work.

First, both the BC and advection tests finished running. The tests confirm that noise is generated both (a) at the boundaries by the BCs and (b) within the domain by the advection scheme.

Additionally, I have also (finally) completed the twin variables implementation. After a bit of debugging (thanks, co-pilot), the new twin variablea version of ROMS compiles, runs, and appears to produce reasonable results!

More details below:

---
## BC and advection scheme tests

A few weeks ago, I designed a simple OAE test that demonstrates our noise issue within one month. Figure 1 shows the difference in alkalinity between the control and perturbation runs in this test.

<p style="text-align:center;"><video src="/research_blog/figures/2026.10.12/pert_minus_base_alk.mp4" controls="controls" style="max-width: 800px;"></video><br>Fig 1. Base model: Evolution of vertically integrated alkalinity differences from the simple alkalinity injection test case. Alkalinity was added to a 15x15 patch (denoted by the pink circle) at the start of the run. Every frame is a daily average. The left shows the locations where there are differnces between the baseline and perturbation runs at any s-level. It helps visualize how far the alkalinity plume has propagated and whether it has reached the domain boundaries. The right shows the vertically-integrated difference in alkalinty between the baseline and perturbation runs.</p>
<br>

Per Christie's suggestion, I repeated this same test with modified BCs and advection schemes.

First, I changed the BCs for alkalinity and TIC from Radiation & Nudging to Gradient. Figure 2 shows that this change significantly reduced the noise reflections we were seeing at the boundaries. This result suggests that our noise issue is, in part, being amplified by our boundary conditions. Therefore, I am hopeful that using Clamped BCs for dTIC and dTA will help our issue in the twin variables implementation.

<p style="text-align:center;"><video src="/research_blog/figures/2026.11.09/pert_minus_bas_alk_GraBC.mp4" controls="controls" style="max-width: 800px;"></video><br>Fig 2. Repeat of Figure 1, except using Gradient BCs for alkalinity and TIC.</p>
<br>

Next, I reverted the BCs back to Radiation & Nudging, and I changed the TIC and alkalinity advection schemes to U3 (horizontal) and C4 (vertical). Figure 2 shows that this advection scheme combinaton reduces the noise we saw at the edges of the plume boundaries. Thus, part of our noise issue appears to be amplified by the advection scheme via interactions between our alkalinity perturbation and the background alkalinity field. I am again more hopeful that using dTIC and dTA will help our noise issue, because dTIC and dTA will necessarily have a value of zero in the background.

<p style="text-align:center;"><video src="/research_blog/figures/2026.11.09/pert_minus_bas_alkalinity_U3C4.mp4" controls="controls" style="max-width: 800px;"></video><br>Fig 3. Repeat of Figure 1, except using U3 and C4 advection schemes for alkalinity and TIC.</p>
<br>

Although the noise improves, it never completely dissappears. I discuss more thoughts on this in the last section of this blog post.

---
## Twin variable implementation

The first draft of the twin variables implementation is finally complete! So far, I have launched a run using the same OAE initialization as shown above (except I added alkalinity in the dTA field rather than the background alkalinity field). The run is currently on pause during hyak maintenance, but I am already seeing improvements in the first 10 days of the run (Figure 4). 

<p style="text-align:center;"><img src="/research_blog/figures/2026.11.09/day10_dta.png" width="800"/><br>Fig 4. Comparison of alkalinity differences on day 10 of OAE experiment in original run (left) and twin variable run (right). </p><br>

Following Laurent et al (2025)'s example, I used Clamped BCs for dTIC and dTA. Hopeully, this will prevent amplified noise at the domain boundaries when the alkalinity plume finally reaches the edges of the domain.

Implementation notes:
- I compiled the twin variables version of ROMS in LO_roms_user/x11aboaetwin
- The modified ROMS source code with twin variables implementation lives in a new git repo called LO_roms_source_twinOAE. There are 23 files that I had to change as part of this implementation
- To enable the twin variables implementation, I needed to define a new flag called TALK_ADDITION in the header file, following Laurent et al (2025)'s example
- I also defined DIAGNOSTICS_BIO in the header file to output diagnostics, such as air-sea CO2 flux in the control and addition experiments. This functionality appears to be working so far.
- I generated new river and ocean forcing with dTA and dTIC state variables. They have values of zero everywhere.
- I did not initialize OAE using a .nc forcing file. So far, I am still relying on my previous of method of adding alkalinity to a his file (though I now add it in the dTA field rather than the background alkalinity field). 

---
## Next steps

### Verify that the twin variables not only runs, but works

Qualitatively, the twin variables implementation seems to work. Is there a way to be sure that everything is actually behaving as intended? Some ideas:

- Compare dTA to a passive dye addition in the same quantity
- Test OAE in an idealized domain

### More advice for reducing noise

Last friday, I chatted with Alicia Karspeck from CWorthy at a local mCDR workshop. She mentioned that we can never completely get rid of noise. However, there are ways that we can minimize it. She recommends adding alkalinity in a patch using a lateral Gaussian distribution. She also recommends ramping up the alkalinity addition using a logarithmic function in time.

Thankfully, I implemented Laurent et al. (2025)'s alkalinity forcing file code (which we can toggle on by defining TALK_FILE in the header file). It will be much easier to play around with the alkalinity additions using the .nc file functionality.