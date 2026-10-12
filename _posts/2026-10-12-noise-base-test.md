## Noise Base Test

Before I begin implementing twin variables, it is important to have a test case that we can rapidly test and iterate with. In this blog post, I first remind ourselves of the noise issue that we're dealing with, and then describe the new noise test case that I have developed.

---
## Noise in general exam experiment

For my general exam, I ran an OAE test that was initialized from Whidbey Basin. 
We had added alkalinity to the top eight sigma layers of a single grid cell at the start of every day for one month. After this first month, I stopped adding alkalinity, and I continued to run the model for four additional months for a toal of five months.
At the end of the one-month pulse of alkalinity, we observed noise near the domain boundaries of the model (Fig 1).

<p style="text-align:center;"><video src="/research_blog/figures/2026.10.12/ge_alk_noise.mp4" controls="controls" style="max-width: 800px;"></video><br>Fig 1. Evolution of vertically integrated alkalinity differences from a one-month pulse of alkalinity initialized in Whidbey Basin.</p>
<br>

Our goal is to implement twin variables of TIC and alkalinity to eliminate this noise. I will work towards this goal for the rest of October. Specifically, I will add twin variables to fennel.h via the addition of delta_TIC and detla_alk, based on the code by the Fennel group ([Laurent et al., 2025](https://zenodo.org/records/17871565)).

To test whether twin variables resolves our noise issue, I will run a test case (described in the next section) and evaluate whether we successfully suppressed the noise.

---
## Noise test case

This past week, I designed a simpler test case to evaluate noise. I developed this new test case to simplify some complexities of my general exam experiment. Compared to my general exam test case, this new test case:

1. **Adds alkalinity on only the first day.** For my general exam, I added alkalinity at the start of each day for one month. Now, I only add alkalinity at the start of the first day of run time.
2. **Uses only a single driver.** The purpose of the driver script is typically to get forcing and run ROMS at the start of each day. For my general exam, I used a modified driver that also added alkalinity to the model domain at the start of each day. Then, after the first month of run time, I switched back to our simple driver. Suspiciously, the timing of the boundary noise occurs at about the time I switched drivers. In this new test case, I solely use the simple driver (and I manually added alkalinity to a his file before the first day of run time).
3. **Uses the same compilation of ROMS for the baseline and perturbation runs.** For my general exam, I added a passive tracer dye to the model, which requires compiling a different instantiation of ROMS. However, this meant that my baseline (long hindcast) and perturbation (general exam test with dye) runs used different compilations of ROMS. For this new test case, I opted to NOT add dye, so I can compare baseline and perturbation runs that use the same compilation of ROMS.
4. **Adds alkalinity in a patch closer to the domain boundaries.** Because I added alkalinity to Whidbey Basin for my general exam, it took a month before the alkalinity signal propagated to the domain boundaries and we began to see noise in the domain. For a test case, one month of run time is far too long before we see issues. In the simpler test case, I add alkalinity in a patch closer to the domain boundaries, so noise will hopefully appear sooner.

In my new test case, I added 100 mmol/m3 of alkalinity to the surface sigma layers of a 15x15 patch of grid cells offshore of Oregon.

To my surprise, it still takes nearly a month before I see any boundary noise, because the alkalinity swirls around in an eddy for a little while first (Fig 2). However, the swirl of alkalinity is easier to see now that the pulse of alkalinity doesn't have any nearby land masses. Additionally, the alkalinity swirl *looks* noisy-- it has many positive and negative difference values at the edges of the plume, and I expect the signal to be a primarily positive difference. 

<p style="text-align:center;"><video src="/research_blog/figures/2026.10.12/pert_minus_base_alk.mp4" controls="controls" style="max-width: 800px;"></video><br>Fig 2. Evolution of vertically integrated alkalinity differences from the simple alkalinity injection test case. Alkalinity was added to a 15x15 patch (denoted by the pink circle) at the start of the run. Every frame is a daily average. The left shows the locations where there are differnces between the baseline and perturbation runs at any s-level. It helps visualize how far the alkalinity plume has propagated and whether it has reached the domain boundaries. The right shows the vertically-integrated difference in alkalinty between the baseline and perturbation runs.</p>
<br>

<p style="text-align:center;"><video src="/research_blog/figures/2026.10.12/pert_minus_base_TIC.mp4" controls="controls" style="max-width: 800px;"></video><br>Fig 3. Repeat of Figure 2, except for TIC instead of alkalinity.</p>
<br>

All in all, this test case is at least simpler than my general exam test, and I will use it to test noise moving forward.
 