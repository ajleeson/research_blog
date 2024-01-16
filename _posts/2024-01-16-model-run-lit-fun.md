## Model Run and Literature Fun

The N-less run has been chugging along on klone. The simulation has reached the end of April, and I expect the full run to finish after another 1.5-2 weeks. At first, the model kept stopping inexplicably (no obvious errors), and I had to restart it several times. However, the model later ran for 5 days or so without any issue. If the stopping issue comes up again then I'll move everything to apogee from perigee.

While the model has been running, I generated plotting scripts to look at initial differences between the long hindcast run and the N-less run. I also submitted a TRAPS pull request to Parker and have begun a hypoxia literature deep dive. More details below.

---
## First look at results

After a month or so of simulation time, I wrote a short script to compare the long hindcast run to the new N-less run. The goal of this script is to verify that the N-less run truly has N-less WWTP effluent. Additionally, I wanted to confirm that the hydrodynamics are the same between both runs (i.e. the only change is to the effluent nutrient concentrations).

The figures shown below each compare 2013.02.16 hour 12 between the two model runs. The colormaps show the long hindcast values minus the N-less run values. The left panel is the surface difference, and the right panel is the bottom difference. Circles indicate locations of WWTPs.

Figure 1 shows the difference in DIN concentrations between the long hindcast and the N-less run. As I hoped, the long hindcast run indeed has has a higher concentration of DIN than the N-less run. Furthermore, the source of excess DIN in the long hindcast appears to be emanating from the WWTPs (as indicated by the red hotspots near WWTPs in the bottom waters). These results are reassuring.

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/757bdfbb-f806-4b0f-a652-4f6e5e386f82" width="650"/><br>Fig 1. DIN difference between long hindcast and N-less run. First panel shows surface difference, second panel shows bottom difference.
</p><br>

Figures 2 and 3 show the difference in horizontal velocities between the long hindcast and the N-less run. Although I did not want to introduce any chnages to the hydrodynamics, there appears to be a few differences just outside of Puget Sound. The source of the differences do not appear to come from the WWTPs, which is good.

My hypothesis is that these differences arise because I initialized the N-less run as a *continuation* rather than a *perfect restart.* I am hoping that these differences do not make a huge difference in the results. 

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/013faf68-430d-4486-9b0f-a55f5bc820c5" width="650"/><br>Fig 2. u difference between long hindcast and N-less run. First panel shows surface difference, second panel shows bottom difference.
</p><br>

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/7f7d1c75-5a92-47e0-b602-5dbbd2a426ba" width="650"/><br>Fig 3. v difference between long hindcast and N-less run. First panel shows surface difference, second panel shows bottom difference.
</p><br>

---
## Literature

This week I began my deep-dive into hypoxia literature. I have also started to take literature notes in Evernote (instead of using the "Comment" tool on the margins of the paper). Wow, what a difference this makes. Something about pretty and well-organized information really makes me happy. Evernote gives me the flexibility to structure and color-code my notes however I like. It's really great, and I am more attentive while reading. Moreover, my new notes are easy to review, so I am hopeful that they will prove helpful in the future.

So far, I have been reading about Long Island Sound. Even though nutrients play a role, the frequency of wind-induced mixing events seem to be a more robust indicator of hypoxia (Wilson et al., 2008). When we read Scully (2013) last quarter, I learned that wind is an important player to Chesapeake Bay hypoxia as well.

Dakota and I led a paper discussion last Friday for our Biological Oceanography course. Preparing for this presentation took up a couple days' time, so I haven't read as much about hypoxia as I would have liked to. However, with my new note-taking system in hand, I am excited to jump back into reading again this week.

---
### To-Do's for this week

As I mentioned above, homework took a bit of time last week. Therefore, I did not get to every task I wanted to complete. Namely, modifying driver_roms3 and researching new laptops. In the upcoming week, my priority is to continue reading literature. During reading breaks, I'll work on these two other tasks.

**Hypoxia literature deep-dive (high priority)**
- *Notes:* Continue reading about drivers and processes leading to hypoxia in different estuarine system to eventually inform hypothesis generation and model analysis work.
- *Anticipated obstacles:* I seem to read scientific literature 10x more slowly than I read anything else. Sometime it's easy to feel defeated.


**Modify driver_roms3 to use long hindcast forcing (lower priority)**
- *Notes:* Having a script that uses the forcing files Parker generated for the long hindcast run will speed up my modeling workflow for furture model runs
- *Anticipated obstacles:* It will be difficult to test this script since the nodes on klone are busy being used for active model runs. I will need to think through the best way to verify that the new script is working.

**Look into new laptop (lower priority)**
- *Notes:* My current laptop is still functional, but it throws a blue screen error often enough that I am concerened about its longevity. This week I will research some alternative laptops that would work well for research.
- *Anticipated obstacles:* I don't understand technology very well, and I'll need to spend some time understanding what specs I should be looking for in a laptop.

---
## References

Scully, M. E. (2013). Physical controls on hypoxia in Chesapeake Bay: A numerical modeling study. *Journal of Geophysical Research: Oceans*, 118(3), 1239–1256. https://doi.org/10.1002/jgrc.20138

Wilson, R. E., Swanson, R. L., & Crowley, H. A. (2008). Perspectives on long-term variations in hypoxic conditions in western Long Island Sound. *Journal of Geophysical Research: Oceans*, 113(12). https://doi.org/10.1029/2007JC004693