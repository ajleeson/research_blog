## Forcing Debugging and Initial Model Results

This week I attempted to debug the incorrect forcing in the idealized domain. Switching from an analytically-added source to a netCDF-added source does not fix the problem. Thus, forcing continues to be an open issue.

Parker is also helping run the nutrient-less WWTP test case on LiveOcean (in which WWTPs are treated like tiny rivers). This run will take ~2 weeks to complete. In the meantime, I have begun creating scripts that will generate comparison plots.

Finally, I've started to outline some initial ideas for the GRC poster.

More details below.

---
## Debugging Forcing

Last week the V-less vertical sources fixed the vertical velocities in the idealized domain, but the inflowing temperature and salinity were not what they should have been. This week I turned off analytical forcing and added source forcing using a netCDF file instead.

In the first test, the vertical sources still use the volume-adding code. Thus, I fully expected the vertical velocities to behave strangely (which they did). However, I have switched to using forcing from a netCDF file. The intent of this test was to ensure that the netCDF files correctly add a freshwater source with no temperature difference. Figures 1 and 2 show the section salinity and temperature, respectively. These results suggest that the netCDF file does indeed correctly add a freshwater source with no temperature difference.

<p style="text-align:center;"><video src="https://github.com/ajleeson/LO_user/assets/15829099/d8dbe028-c3a7-46c1-9a22-b2db2b9aa46d" controls="controls" style="max-width: 800px;"></video><br>Fig. 1 Section salinity in idealized domain (netCDF forcing, volume adding).</p><br>

<p style="text-align:center;"><video src="https://github.com/ajleeson/LO_user/assets/15829099/ab39e43d-7780-4ca3-8e26-19d4a0b4b12e" controls="controls" style="max-width: 800px;"></video><br>Fig. 2 Section temperature in idealized domain (netCDF forcing, volume adding).</p><br>

Things got a bit weird when I turned on the V-less switch. In this test I continued to use forcing from the netCDF file, but I also compiled ROMS with LWSRC_MASS_ONLY defined, which removes volume-adding code. Figures 3 and 4 show the resulting salnity and temperature profiles, respctively. Even when using forcing from the netCDF files, the inflowing salinity and temperatures are still incorrect!

<p style="text-align:center;"><video src="https://github.com/ajleeson/LO_user/assets/15829099/1ba517e4-1982-4164-854a-b44cbc0a00bd" controls="controls" style="max-width: 800px;"></video><br>Fig. 3 Section salinity in idealized domain (netCDF forcing, V-less).</p><br>

<p style="text-align:center;"><video src="https://github.com/ajleeson/LO_user/assets/15829099/9229e185-2f4f-4459-b44f-57ae3c5fb16f" controls="controls" style="max-width: 800px;"></video><br>Fig. 4 Section temperature in idealized domain (netCDF forcing, V-less).</p><br>

What is strange is that the removed "volume-adding" sections of code do not reference salinity nor temperature. I do not understand how turning the volume off thus changes the inflowing tracer parameters, so I'd like to brainstorm other tests/checks I can conduct.

---
## Preliminary Comparison Plots

As of Monday, the nutrient-less WWTP run has made it to March. Per Parker's advice, I've started to work on some comparison plots to prepare my code for final results, and to confirm that there is a difference between the "nutrient-full" and "nutrient-less" runs.

The next couple of figures show some preliminary results on March 07, 2017. Figure 5 shows a comparison of surface phytoplankton at Whidbey Basin. Figure 6 shows a comparison of bottom DO at Whidbey Basin. In March, DO appears to be fully saturated in both case. In the nutrient-less WWTP run, Oak Harbor appears to have more phytoplankton with a corresponding higher DO concentration. I'm looking forward to watching DO evolve as the model run moves into summer.

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/0cdeaf63-92d9-4f87-8f78-c229d499b6b7" width="500"/><br>Fig 5. Whidbey Basin surface phytoplankton on March 07,2017.</p><br>

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/4f07b679-8c74-424e-9c87-a5b76cabf079" width="500"/><br>Fig 6. Whidbey Basin bottom DO on March 07,2017.</p><br>

Currently, my plotting infrastructure is set up to easily make similar plots for any variable. The coding infrastructure also makes it easy to switch between different regions. In the coming week, I'd like to make additional improvements to the code. Some items include:

- Adding dates to the figures (easy)
- Average values over several days? (med)
- Add labels with total hypoxic volume or total phytoplankton biomass (hard)
  - If I pursue these metrics, I'll need to borrow some of Dakota's expertise
- Make timeseries of bottom DO concentration (or hypoxic volume) throughout the year (hard)

In our intial plan, we also discussed a literature dive to check how other studies in Long Island Sound or Chesapeake Bay were analyzing results from similar experiments. I'm thinking that it may be time to seriously pursue this endeavor.

---
## Poster Outline

I have been thinking more about the GRC poster while making the preliminary comparison plots. In a previous meeting, Alex had also mentioned that it may be a good idea to think of some good "background information schematics" to use for the poster as well. I made progress on both of these efforts this week, and I've drafted a rough poster outline shown in Figure 7. I'd love to hear feedback on the overall content and scope. I'm also not attached to the layout, and will likely change it once I have some final figures.

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/25fe10b6-6412-467b-b556-a2791a6f45d7" width="800"/><br>Fig 7. GRC poster outline.</p><br>

Figure 8 and 9 show a close-up of the hypoxia flowchart and nutrient map, respectively. If we decide that the hypoxia flowchart is a good idea, then I'd like some advice for I can improve it. Currently, nutrient inputs is shown as a single bubble in the flowchart because I wanted to emphasize that nutrients are an important part of the cycle. The issue I have is that nutrient input, although fluctuating, occurs year around. Is there a better way to show inflowing nutrients rather than as a single bubble?

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/88e638d8-c6b2-4505-81a7-8e83c496ac02" width="800"/><br>Fig 8. Hypoxia flowchart.</p><br>

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/d6a667f6-c350-4bec-81c7-3b79579fba63" width="800"/><br>Fig 9. Nutrient map.</p><br>