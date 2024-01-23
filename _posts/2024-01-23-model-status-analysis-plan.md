## Model Status and Analysis Planning

This week, I reinitialized the N-less run on January 1st using a perfect restart on 10 nodes. So far, the model has been running smoothly and has already made it to the end of April. A quick look at mid-April velocities show that we no longer have hydrodynamic deviations between the long hindcast and the N-less run (hooray!).

While the model has been running, I have continued to read. As Alex suggested, I added notes on the "Relevance to Puget Sound" to papers I have already read. This week I have also started to brainstorm figures to make once the run N-less run is complete.

More details below.

---
## N-less run status

To verify that we have eliminated hydrodynamic deviations, I took a look at difference figures between the long hindcast and the N-less run in mid-April.

Figure 1 shows the difference in surface and bottom DIN between the hindcast and the N-less run. Generally, the long hindcast has higher concentrations of DIN (good). Oddly, though, bottom DIN is lower in parts of Whidbey Basin than in the long hindcast. I'm curious to look into this more when we have year-long results.

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/eade82fa-204f-4db5-a9a2-55cd6ddc7d73" width="500"/><br>Fig 1. DIN difference between long hindcast and N-less run. First panel shows surface difference, second panel shows bottom difference.
</p><br>

Figures 2 and 3 show the difference in surface and bottom u and v between the hindcast and the N-less run. Thankfully, there appear to be no differences.

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/6c6b5d8b-c82c-4373-a57a-7c4e9112409c" width="500"/><br>Fig 2. u difference between long hindcast and N-less run. First panel shows surface difference, second panel shows bottom difference.
</p><br>

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/15007de3-1407-44a5-9e8b-4cc8158502f2" width="500"/><br>Fig 3. v difference between long hindcast and N-less run. First panel shows surface difference, second panel shows bottom difference.
</p><br>

---
## Initial analysis ideas

Below is a list of graphics I would like to create once the N-less run finishes. The intent is that observations from these figures will help guide other analyses before Ocean Sciences. Maybe we see surprising trends that we want to dig into more deeply.

I feel confident that I can create all of these figures in one week by relying on some prior scripts I have written. Please let me know if you have additional ideas or think I can skip some of these figures.

- Weekly videos
	- Of DO, DIN, phytoplankton, zooplankton, detritus at surface and bottom over the whole year
	- To see if there are any significant events where things change
	- To see if things evolve in surprising ways
- Difference figures (think GRC money plot, or figures above)
	- Three, month-long averages. July average, August average, and September average
	- Use box extraction to get data
	- Compare bottom DO in Puget Sound, surface phytoplankton, surface zooplankton, surface & bottom DIN
	- Compare long hindcast (baseline) to N-less run
	- long hindcast gets normal pcolormesh map of Puget Sound with actual values
	- The second subplot will be a difference between baseline and N-less run
- Depth vs. time, colored by property
	- Pick a few hypoxic places, or places with extreme differences in nutrients, phytoplankton, etc.
	- Do a cast extraction for the year (daily values)
	- Make a colormap of depth vs. time, colored by property
	- Subplots: first panel is baseline, second panel is N-less run, and they share the same x-axis (time)
	- Look at differences in the different BGC parameters
	- Look at differences in salinity, temperature, eddy viscosity and eddy diffusivity as well
-  Timeseries of DO, DIN, phytoplankton, zooplankton, detritus, stratification all on same axis
	- To see if there are any peaks and troughs that align
	- timeseries over the whole year
	- Need to decide what region to look at, and whether I am looking at surface or bottom
    	- Maybe mixed. e.g. DO is bottom, phytoplankton could be at surface
	- stratification = bottom minus top salinity

---
## To-Do's

### This week

- Continue reading
  - Start to synthesize findings
- Continue brainstorming how to analyze model results
- Keep an eye on model run and re-start if it pauses

### After Ocean Sciences

- Look into new laptop (I got the blue screen of death again this week)
- Re-work driver_roms3 to search for forcing in Parker's apogee folder (except TRAPS forcing which is in my perigee folder)
- Update TRAPS module based on Parker's suggestions
