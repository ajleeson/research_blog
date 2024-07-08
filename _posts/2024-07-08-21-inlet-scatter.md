## 21 Inlet Scatter

This week I looked at system-wide spatial distributions of biogeochemical variables.
I also conducted mooring extractions in 21 inlets throughout Puget Sound and began looking at which characteristics correlate best with bottom DO.

More details below.

---
## 2014 maps

I have maps for every year between 2014 - 2019, inclusive. However, I am only showing results for 2014 for consistency (I only have mooring extractions for the year 2014).

Figure 1 shows 2014 maps, with values averaged over the hypoxic season (Aug 1 - Oct 31)
The state variables we look at include:
- Bottom DO
- Bottom density minus surface density
- Surface DIN (NO3 + NH4)
- Depth-integrated phytoplankton
- Depth-integrated zooplankton
- Depth-inegrated small detritus
- Depth-integrated large detritus

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/09e77e97-2cc4-4522-ac3b-7a4ed0fc7de5" width="800"/><br>Fig 1. Map of variables averaged over 2014 hypoxia season (Aug 1 - Oct 31).</p><br>

After making this figure, I realized that there must be better ways to look at the model output. The depth-integrated phytoplankton, zooplankton, and detritus  have an underlying pattern that seems to match bathymetry. In hindsight, this seems obvious since I am integrating over depth. 

I wonder whether I should look at the biogeochemical variables in a different ways, such as surface chlorophyll instead of depth-integrated chlorophyll.

However, it also seems significant that Main Basin, which has a high accumulation of chlorphyll and organic matter, does not develop hypoxia. Is this because Main Basin is deep and has more initial depth-integrated oxygen compared to shallower regions, and there is simply too much oxygen in the bottom layer for it to be fully consumed? Or does this finding suggest that physical processes in Main Basin replenish oxygen, despite Main Basin having a large oxygen sink? 

---
## 21 Inlet analysis

Figure 2 shows the locations where I did a mooring extraction in each of the 21 terminal inlets in Puget Sound. If Ecology had a monitoring station within one of the inlets, I did the extract moor at the location of the Ecology station. We have 7 inlets with Ecology data.

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/fdcbf045-2c18-49ba-a924-977cb79e7eda" width="400"/><br>Fig 2. Locations of mooring extraction for the 21 terminal inlets in Puget Sound.</p><br>

### Depth vs. time property plots

In the following figures, I only show results for Lynch Cove. However, I have made the same figures for all 21 inlets.

Figure 3 shows depth vs. time property plots in Lynch Cove. The value of these type of figures is that we resolve the vertical structure of state variables through time.

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/8fc9455d-27ef-479f-b1c5-6a8f87104bc2" width="600"/><br>Fig 3. Depth vs. time property plots in Lynch Cove (2014).</p><br>

In Lynch Cove, we observe that the water column is stratified nearly all year long. During the spring, nitrate concentrations are depleted in the surface, presumably due to algal uptake. Then, there is a corresponding boom in zooplankton at the surface, followed by an increase in small detritus which sinks to the bottom of the water column, and subsequently an increase in large detritus and ammonium in bottom waters (I'm assuming due to remineralization). In the oxygen signature, we observe high surface oxygen concentrations that appear to crorrespond to photosynthesis. Then, in May, we begin to observe DO depletion which quickly evolves into hypoxia.

Although these figures contain a lot of information, it is somewhat difficult to parse the magnitude and timing of key events. Instead, I found that these depth vs. time property plots are more instructive when used as a tool to compare model output to observations.

Figure 4 shows a comparison of the depth vs. time property plot from the model as compared to observations. Note that we only have observational measurements for temperature, salinity, chlorophyll, and DO. 

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/200258f2-26bd-44d9-8073-7c4522a9b74c" width="850"/><br>Fig 4. Side-by-side comparison of model and observation depth vs. time property plots in Lynch Cove (2014).</p><br>

In general, the shape of the DO profile is similar between the model and observations. However, the the modeled hypoxia is more severe than the observed hypoxia. The obervations also suggest that phytoplankton tend to bloom intensely in a narrow band of subsurface waters, which the model does not capture.

I also made a version of this same figure, except showing the difference between model and observations, rather than raw observation measurements (Fig. 5). The benefit I see of looking at the difference is that it becomes apparent where the model overestimates and underestimates different state variable values. However, I also find that the difference plot makes it more difficult to understand what the observations actually look like (which is why I made both types of figures).

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/40e7fb8c-63eb-4083-bfc1-000b2786a853" width="850"/><br>Fig 5. Difference between model and observation depth vs. time property plots in Lynch Cove (2014).</p><br>

In these difference figures, I also notice that the model overestimates surface density, suggesting that observed stratification is stronger than modeled stratification.

My next immediate step is to make simple time series plots of these different state variables for each of the 21 inlets (e.g. bottom DO vs. time; so we no longer resolve vertical structure). These simple time series will make it easier to pick out timing of key biological events.

One question is how do I compress these state variables into a single value across all depths? My original plan was to use depth-integrated values of chlorphyll, zooplankton, etc., but as I saw above, this may not be the best method. 

### Scatter plots and correlation

And finally, the most exciting results....
We have scatter plots with significant correlation!

For these figures, I averaged values in each of the 21 inlets over the hypoxic season (Aug 1 - Oct 31) based on lowpass-filtered daily values. The exception is tidal currents which I averaged over the entire year based on hourly data. 

Every dot in these scatter plots corresponds to one of the 21 inlets. Note, I do not know which dot corresponds to which inlet.

I calculated a Pearson's correlation coefficient (r) for each scatter plot, along with a p-value. Both are labeled on the plot. 

Figure 6 shows the results. So far, I have data for:
- Bottom DO vs. depth (significant, moderate correlation)
- Bottom DO vs. tidal currents (significant, moderate correlation)
- Bottom DO vs. stratification (significant, moderate correlation)
- Bottom DO vs. stratification/depth (no significant correlation)

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/9828b812-97ef-4b7a-a266-fda108d92f53" width="600"/><br>Fig 6. Scatterplots and correlation coefficients between bottom DO and physical characteristics for 21 inlets throughout Puget Sound.</p><br>

Note that it would be better to calculate average values across the entire inlet, rather than at a single point based on the mooring extractions. However, this analysis gives us a good first look at which characteristics are linked to low DO.

Based on these results, it seems that hypoxia does not tend to develop in very shallow water, nor regions with strong tidal currents and weak stratification.

---
## Next steps

These results are simply a step in the process-- I feel like there is still so much more to explore and so much more to wrap my head around!

First, I want to create time series of state variables at each of the 21 inlets so I can finally look at timing of events. If observations are available, I'll be sure to add observed values to these figures as well.

Then, I want to keep exploring characteristics of hypoxic inlets. Some ideas:
- Residence time estimate using tidal prism-- calculate using tef2?? Reach out to Jilian
- Proximity to WWTPs, or quantify WWTP loading somehow

Eventually, I hope that this work will culminate in a parameter space plot, colored by average bottom DO.