## Noise and NaNs

I have made a little bit of progress on a few research tasks this week. See below for more details.

---
## Long Hindcast

One of my primary research tasks is to debug the long hindcast. I undefined OMEGA_IMPLICIT in the ROMS header file, recompiled, and re-ran the model starting on 2014.06.13, which is five days prior to the dreaded blow-up date. 

Despite re-launching several days in advance, the model still blows up on hour 19 of 2014.06.18. I do not have figures to show this week, but next week I'm planning to plot some vertical profiles that will help us understand the source of the blow-up issue. 

---
## Noise

As a follow-up to my [last blog post](https://ajleeson.github.io/research_blog/2025/10/07/noise-evolution.html) on noise evolution, I re-made the noise videos given Alex and Parker's feedback.

In the new iteration of noise videos, I have plotted:

- The difference between the loading and no-loading run
- of the vertical integrals of total nitrogen (TN)
    - *Note: these are no longer volume integrated-- simply vertically integrated*
- Normalized by the same value in every frame of the video
    - This value is the maximum value of the difference in vertically integrated TN observed over the course of a year

<p style="text-align:center;"><video src="/research_blog/figures/2025.10.22/full_domain_year.mp4" controls="controls" style="max-width: 600px;"></video><br>Fig. 1 Evolution of normalized vertically-integrated TN differences at a weekly interval for one year. Colorbar limits of the pcolormesh range from -10% to 10%.</p>
<br>

Near the boundaries of the model domain, I still observe noise on the order of 10% of the max signal (Fig 1). Again, this might be problematic for future OAE experiments.

Over the course of a year in the Salish Sea, I do not observe any noise on the order of 10% of the max signal (Fig 2).

<p style="text-align:center;"><video src="/research_blog/figures/2025.10.22/salish_sea_year.mp4" controls="controls" style="max-width: 600px;"></video><br>Fig. 2 Evolution of normalized vertically-integrated TN differences at a weekly interval for one year. Colorbar limits of the pcolormesh range from -10% to 10%.</p>
<br>

During the first two weeks after initializing the model run, I observe some noise on the order of 0.1% in Puget Sound. By the end of the two-week period, the signal (red) appears to dominate over the noise. 

<p style="text-align:center;"><video src="/research_blog/figures/2025.10.22/salish_sea_twoweeks.mp4" controls="controls" style="max-width: 600px;"></video><br>Fig. 3 Evolution of normalized vertically-integrated TN differences at a daily interval for two weeks. Colorbar limits of the pcolormesh range from -0.1% to 0.1%.</p>
<br>

It's difficult to evaluate the size of the noise in Puget Sound since the signal dominates over the noise after two weeks of run time. The remaining questions are: How much is this signal "off" due to noise? 
If the noise remains on the order of 0.1% of the max signal, is that acceptable?

*Side note: I could not detect noise on the order of 1% of the max signal in Puget Sound (video not shown). Thus, I think that noise is closer to 0.1% of our max signal.* 

---
## OAE module test

As a bonus figure, I've included a recent video from my OAE module testing in LiveOcean.
LiveOcean runs without throwing an error when I activate the new OAE module, but very quickly the carbonate chemistry fields are swallowed by a wave of nans coming from the Northeast corner of the model domain (Fig 4).

<p style="text-align:center;"><video src="/research_blog/figures/2025.10.22/oae_movie.mp4" controls="controls" style="max-width: 600px;"></video><br>Fig. 4 Surface alkalinity and DIC in LiveOcean + OAE module. The video spans one day of run time at an hourly interval. No additional alkalinity was added to the domain (alkalinity concentrations are ambient values).</p>
<br>

Despite this problematic behavior, I think we are okay to check off the OAE module integration task for now as we were able to run the OAE module without any blow-ups or other errors. 
