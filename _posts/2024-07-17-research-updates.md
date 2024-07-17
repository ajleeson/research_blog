## Weekly research updates

I did not get nearly as much done this week as I wanted to get done, but I thought it would still be best to share what I have so far so I can get feedback.

This past week, I finished my model-observation comparison figures at the twenty one different inlets. I also made timeseries of different biogeochemical state variables at each inlet.

On my to-do list which I still haven't gotten to: using tef2 to calculate tidal prism and inlet flushing time. My goal is to make a scatter plot of bottom DO vs. flushing time and report their correlation. 

I have also been thinking much more about timescales of relevant biological processes, relative to physical processes, which influence hypoxia. I reached out to Lily Engel about her PhD work on timescales. She shared her paper manuscript with me, as well as a review paper on timescales (Lucas & Deleersnijder, 2020). I haven't had the brain capacity to read either yet. However, I'm wondering if this type of timescale analysis may be insightful for my work. Could some of this analysis, or something like Buckingham Pi, help identify relevant nondimensional parameters that constrain hypoxia development?

---
## What's up with depth?

Before I dive into new results, I want to revisit an old question that I was never able to resolve.

Figure 1 shows a 2D histogram of depth of DO minima vs. DO minima concentration. We realized that hypoxia does not tend to develop in the extremely shallow depths (i.e. < 10 m). However, I was always puzzled that hypoxia does not develop at depths deepth than ~180 m. This depth cutoff is so sharp, and I could not explain why. 

<p style="text-align:center;"><img src="https://github.com/user-attachments/assets/542ad26b-3b82-4660-bf0c-db4dca4e5914" width="600"/><br>Fig 1. 2D histogram of depth of DO minima vs. DO minima concentration for all cells and days in Puget Sound with DO < 6 mg/L (100x100 bins).</p><br>

Last week, I re-made my categorical map of depth and hypoxia using data for 2014 (Fig. 2). I observe that in shallower waters ( < 10 m), hypoxia rarely develops except at the boundary of shallow, non-hypoxic water (orange) and deep, hypoxic water (pink) such as northern Port Susan, Bellingham Bay, Lynch Cove. Otherwise, hypoxia most commonly develops at depths between 10 m < z < 125 m (pink). Deeper water (z > 125 m) is only ever hypoxic in Hood Canal (red). Otherwise, the deeper water in Main Basin and South Sound does not become hypoxic. As it turns out, the max depth of Hood Canal is 183 m. Thus, the strange depth cutoff I observed in Figure 1 seems to just be the max depth of Hood Canal. Hood Canal is simply the deepest location in Puget Sound that develops hypoxia.

<p style="text-align:center;"><img src="https://github.com/user-attachments/assets/9b841eaf-8a42-47f4-9c91-c608d5325ee8" width="375"/><br>Fig 2. Categorical map showing relationship between depth and hypoxia across Puget Sound.</p><br>

---
## Model-observation comparison

This week I also wrapped up model-data comparison at 7 inlet sites which have observational data. As a reminder, Figure 3 shows the 21 inlet locations, and we have observational data at the purple inlets.

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/fdcbf045-2c18-49ba-a924-977cb79e7eda" width="400"/><br>Fig 3. Locations of mooring extraction for the 21 terminal inlets in Puget Sound.</p><br>

At each of these seven inlets, I compare time series of the top 5 m and bottom 5 m for temperature, salinity, chlorophyll, and oxygen. I also look at property-property plots and report bias and RMSE. Figure 4 shows an example plot from Lynch Cove.

<p style="text-align:center;"><img src="https://github.com/user-attachments/assets/a524363f-4717-45eb-b3d3-6a9b0fc751ff" width="750"/><br>Fig 4. Model data comparison of temp, salinity, chlorophyll, and DO at Lynch Cove. Data come from Ecology CTD casts.</p><br>

At Lynch Cove, the model does a decent job of replicating temperature. We slightly overestimate surface salinity, however. This is important because the model may be overestimating stratification, which could influence vertical mixing and bottom DO. 

The model also appears to overestimate surface chlorophyll during the spring and summer, but underestimates bottom chlorophyll. These results suggest that the model is missing a subsurface chlorphyll bloom. 

Finally, the model appears to underestimate bottom DO. In other words, the predicted bottom DO is worse than it really is. In general, the model tends to underestimate DO at all 7 inlets, except for Hammersley. Figure 5 summarizes the bias and RMSE of each state variable at each inlet location (apologies for the poor, handwritten quality). I have yet to compare these results to the full LiveOcean model-data comparison in Jilian's manuscript, but plan to comment on this topic in my thesis.

<p style="text-align:center;"><img src="https://github.com/user-attachments/assets/e55c4a00-7166-4f54-980b-59b817ad80ad" width="450"/><br>Fig 5. Summary of bias and RMSE at each site that has Ecology CTD data. Note that an average DO bias of -28.8 uM is roughly equal to -0.9 mg/L. </p><br>

As a side note, if an inlet location was shallower than 10 m, then I did not plot individual time series for the surface 5 and the bottom 5 m. Instead, I plotted a single time series averaged over the full water column depth. Figure 6 shows an example at Totten Inlet.

<p style="text-align:center;"><img src="https://github.com/user-attachments/assets/521c371c-31e3-4213-a26c-21b8648026a1" width="750"/><br>Fig 6. Model data comparison of temp, salinity, chlorophyll, and DO at Totten Inlet. Data come from Ecology CTD casts.</p><br>

---
## Biogeochemistry time series at 21 inlets

Last week, we also had a very productive discussion about how to compress the vertical dimension of biological state variables. I have decided to look at the following values:
- NO3: average surface 20 m
- NH4: average surface 20 m
- phytoplankton: average surface 20 m
- zooplankton: average surface 20 m
- small detritus: average surface 80 m
- large detritus: average bottom 5 m

All of the state variable values are reported in units of uM (mmol/m3). Note that I only conducted this analysis on my 21 mooring extractions.

It was taking far too long to make these calculation across Puget Sound (and I estimated that the processing time would take between 5-10 hours...so I opted not to do that). To make Puget Sound maps, I instead looked at surface layer concentrations of every state variable, excpet large detritus in which I looked at bottom layer concentration (Fig. 7). For me, the color differences make it difficult to see what is going on. Addtionally, we lose information on how these state variables evolve with time. Thus, I am only including this figure for posterity, and I prefer to look at individual time series at the 21 inlets. 

<p style="text-align:center;"><img src="https://github.com/user-attachments/assets/6ee73678-cb9e-4aa2-8ea5-8a2e1b49332c" width="900"/><br>Fig 7. Map of variables averaged over 2014 hypoxia season (Aug 1 - Sep 30).</p><br>

Alright, so what do the state variables look like at the 21 inlets? Figure 8 shows an example time series at Dabob Bay. I chose to show Dabob Bay because we observe a nearly linear decline in bottom DO which seems to coincide with the onset of stratification and the accumulation of large detritus in early April. Note that the biogeochemical state variables in panel (a) are plotted on a log-scale y-axis so we can observe changes in all state variables. 

<p style="text-align:center;"><img src="https://github.com/user-attachments/assets/781f3997-2c38-403b-96ba-b043968cfb48" width="600"/><br>Fig 7. Time series of biogeochemical state variables (a) in units of mmol N per m3, and bottom DO and bottom-surface-density-difference (b) at Dabob Bay for the year 2014.</p><br>

At this point I need to admit that I haven't spent much more time analyzing the timing of different events in these time series. Partially because covid has made my brain feel fuzzy. Partially because the results are simply confusing-- it's difficult to see what is going on and which variables are causing which responses. For instance, in shallow inlets (< 20 m deep), NH4 could be changing due to both phytoplankton uptake and subsurface/bottom remineralization. Additionally, in inlets like Similk Bay (Fig 9.), local peaks in delta rho seem to align with local peaks in bottom DO (weird).

<p style="text-align:center;"><img src="https://github.com/user-attachments/assets/8fc683f5-399a-4d6c-aa8e-0db5c7a2aa8a" width="600"/><br>Fig 8. Time series of biogeochemical state variables (a) in units of mmol N per m3, and bottom DO and bottom-surface-density-difference (b) at Similk Bay for the year 2014.</p><br>

There must be some other way to quantify what is happening. There just seems to be so much information and so much going on in terms of the timing and magnitude of different events. As I thought about this question of timing, I was reminded of Lily Engel's work on time scales. So I reached out to her and she sent me her manuscript and a review paper. My current question: does it seem reasonable to go down the path of timescale analysis and Buckingham Pi to help identify dominant processes or nondimensional numbers that influence hypoxia? This may also help untangle the mess of different inte-related variables like depth and bottom large detritus or NH4 concentration.

---
## More scatter plots

Lastly, I made more scatter plots of bottom DO and calculated more correlation coefficients. This time, I plotted bottom DO vs. the biogeochemical state variables from the bullet-point list in the previous section. 

I have also included inlet labels on these figures.

I know that I was previously focused on physical characteristics of the system, but I also wanted to see if biogeochemical state variables were a better indicator of low DO. 

Figure 9 shows the scatter plots with significant correlation from last week (this time with inlet labels).

<p style="text-align:center;"><img src="https://github.com/user-attachments/assets/2d934b0e-562f-4c25-8ef3-a0647cf1b264" width="900"/><br>Fig 9. Scatterplots and correlation coefficients between bottom DO and physical characteristics for 21 inlets throughout Puget Sound. Bottom DO and delta tho averaged from Aug 1 - Sep 30. Tidal currents average from hourly data over one year.</p><br>

Figure 10 shows new scatter plots of bottom DO vs. biogeochemical state variables-- all of which were averaged from daily lowpass data between Aug 1 - Sep 30.

<p style="text-align:center;"><img src="https://github.com/user-attachments/assets/826fdf69-2526-4c9a-8720-02523a87784e" width="900"/><br>Fig 10. Scatterplots and correlation coefficients between bottom DO and biogeochemical state variables for 21 inlets throughout Puget Sound. Variables averaged from Aug 1 - Sep 30.</p><br>

Bottom DO has significant negative correlation to surface NO3 and bottom large detritus. Bottom DO has significant positive correlation to surface phytoplankton and surface zooplankton. Hammersely and Totten Inlets seem to always be on the extreme ends of these scatter plots. These inlets also have the shallowest depths compared to all other inlets. Thus, I am again seeing some interactions between depth and biogeochemical state variable values. 

### WWTP loading idea: Jan 2013 bottom NH4?

Lastly, I had this silly idea of quanityfing the strength of WWTP loading by measuring the average bottom NH4 concentration in January 2013. Why? Because the model was initialized in late 2012 with zero NH4 in the water. Additionally, between late 2012 through January 2013, phytoplankton growth will have been light-limited so there will be almost no bottom NH4 produced as a result of remineralization. NH4 is also primarily discharged from WWTPs located at the bottom of the water column (it also comes from rivers, but in lower concentrations than WWTPs). Thus, might January 2013 average bottom NH4 concentration be a good proxy for WWTP influence?

Figure 11 shows a map of average bottom NH4 in January 2013, and qualitatively, the hotspots seem to correspond to the WWTP locations. 

<p style="text-align:center;"><img src="https://github.com/user-attachments/assets/35038676-382e-49bc-8bcc-03792c1c0024" width="350"/><br>Fig 11. Average bottom NH4 concentration in January 2013.</p><br>

I didn't yet get a chance to make a scatter plot of 2014 bottom DO vs. Jan 2013 bottom NH4, but I plan to make one soon.

---
## Next steps (timescales and nondimensional numbers??)

**First priority:**
Estimate flushing time by calculating tidal prism from tef2!

**Other steps**
Read papers from Lily and think about timescale analysis. Try out Buckingham Pi and come up with nondimensional numbers for a parameter space plot colored by bottom DO. Depth seems to be an important variable to consider in this analysis!

**In progress**
Write! Write! Write!
Also, I am reviewing Jilian's manuscript which is helping to inform my thesis as well. 