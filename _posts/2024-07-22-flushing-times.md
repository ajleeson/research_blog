## Bottom DO negatively correlates with flushing time

This week I finally estimated flushing times of the 21 inlets using tidal prism and inlet volume calculated from tef2. August and September bottom DO negatively correlates with flushing time. 

I also began thinking more critically about the different timescales at play. 

I feel like I am getting close to wrapping up results for the thesis, but I need some guidance on tying up the loose ends and constructing a logical narrative.

More details below.

---
## Flushing time

I calculated flushing time using the expression:

$$ T_{flush} = \frac{V}{Q_{prism}} $$

To get volume and tidal prism, I went through most of the steps in the tef2 README. 
The inlet volume came from vol_df_cas7_c21.p (c21 being the ctag I gave my sections). The tidal prism came from the .nc files in the generated bulk_2014.01.01_2014.12.31. I used tef_fun to get a tef dataframe with tidal prism, following the example in bulk_plot.py. Then I simply averaged the daily tidal prism values over the year 2014. 

Figure 1 shows the resulting scatterplot of average Aug/Sep bottom DO vs. average annual flushing time. There is a significant negative correlation between bottom DO and flushing time.

<p style="text-align:center;"><img src="https://github.com/user-attachments/assets/fe9caaf7-522a-4988-94aa-34caa6ea9aed" width="400"/><br>Fig 1. Scatterplots and correlation coefficients between bottom DO and flushing time for 21 inlets throughout Puget Sound. Bottom DO averaged from Aug 1 - Sep 30, and flushing time averaged over the entire year.</p><br>

---
## Relevant timescales

This week I also re-visited the NPZD equations in Davis et al. (2014) and Siedlecki et al. (2015). Per Parker's advice, I tried calculated relevant timescales that will be good fore me to keep in mind.

The average depth of my 21 inlets is roughly 40 m. In 40 m of water, it will take 2.5 days for small detritus to sink to the bottom, but only 1/4 of a day for large detritus to sink to the bottom. 
The respiration timescale is much longer than the sinking timescale. The e-folding timescale of remineralization/respiration is 10 days. Thus, in shallow waters, I would expect that a large fraction of small detritus will sink to the bottom before it is fully respired in the water column. An even greater fraction of large detritus will reach the bottom without being respired.

The oxygen production rate via photosynthesis has an "e-growth" timescale (time for concentration to *increase* by a factor of e) of roughly 0.6 days (this is a minimum time, using the maximum phytoplankton growth rate). Photosynthesis thus produces oxygen faster than it is respired. 

---
## Where to go from here

At this point, I have many different results. I have six-years of maps that show where we observe hypoxia and low DO in Puget Sound. I have time series of state variables at all 21 inlets, and comparisons to observations in 7 of these inlets. I also have scatterplots with correlation coefficients between physical characteristics and biogeochemical state variable values at these 21 inlets. Many show signficant correlation to Aug/Sep bottom DO. 

The majority of my time is currently going towards writing my thesis. However, I still feel like there are some loose ends in my analysis, and I'd like to constuct a more complete narrative. I also still have a dream of creating a parameter space plot colored by bottom DO concentration.

It would be great to spend some time in this week's meeting discussing how to wrap up analysis for the thesis. 
