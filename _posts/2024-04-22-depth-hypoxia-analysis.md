## Hypoxia and depth analysis continued...

This week I continued exploring relationships between low DO and depth. 
In most cases, the DO minima in the water column are located in the bottom sigma layer. Additionally, hypoxic DO minima tend to only occur in waters shallower than 50 m. The hypoxic season begins in May and ends at the end of November. 
More details below.

---
## Summary of findings from last week

Last week I shared Figure 1 below, showing the relationship between water depth and hypoxic occurence in the bottom sigma layer. My main result was that **bottom** hypoxia tends to only occur when the water column depth is between 10 m - 50 m. Anything shallower or deeper than this range does not usually develop bottom hypoxia

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/e3ad6f7b-e648-46ea-b4d8-7e6f37dad102" width="800"/><br>Fig 1. Categorical maps showing the co-occruence of hypoxia and shallow water depth.</p><br>

However, everyone in the group meeting brought up some great questions about **mid- water column DO minima**. If the DO minima are located somewhere in the water column other than the bottom, do we still see this 10 - 50 m depth dependence?

---
## Extended analysis on low DO and depth 

To prevent data in the straits from biasing our investigation, I very coarsely cut out LiveOcean data from the Straits. Figure 2-a shows the Puget Sound region I investigated in the following analyses, in which regions in the Straits are cropped out.

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/da776886-3a9b-439b-9c49-46178d27feaf" width="500"/><br>Fig 2. Puget Sound maps with days in which bottom DO < 6 mg/L, with regions outside of Puget Sound cropped out.</p><br>

## 2D histogram analyses

I apologize that you are about to see many figures that look exactly the same. But I assure you that there is something we can learn from each of them. 

Figures 3 - 7 are all 2D histograms meant to explore relationships between depth, DO minima, and time. Every point on these 2D histograms shows the depth, or s-level, or DO concentration of the DO minima in the water column for every lat/lon grid cell in the model. Thus, the vertical coordinate that we are looking at necessarily corresponds to the water column DO minima (rather than looking at just the bottom layer). Panel (a) in each figure shows the histogram for the natural run. Panel (b) shows the difference between the anthropogenic and natural histograms. In other words, panel (b) shows where the anthropogenic histogram had higher/lower counts relative to the natural run.

In these analyses, I focus only on panel (a)-- results from the natural run. However,I'm also happy to discuss any patterns you might see in the anthropogenic results as well!

### Depth and DO minima

Figure 3 shows 2D histograms of the depths of the DO minima vs. the concentration of the DO minima at every lat/lon grid cell for every day of the year. Here I use 100 x 100 bins.

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/f72071f1-8d40-481b-97cb-054b4033a0c7" width="800"/><br>Fig 3. 2D histogram of depth of the DO minima vs. DO minima concentration at every lat/lon grid cell on every day of the year. 100x100 bins.</p><br>

Based on these results, we observe that **hypoxic (< 2 mg/L) DO minima generally only develop at depths shallower than 50 m** (and maybe deeper than 10 m? more investigation needed). This finding is consistent with what we observed last week!

### Sigma layer and DO minima

Figure 4 shows 2D histograms of the s-levels of the DO minima vs. the concentration of the DO minima at every lat/lon grid cell for every day of the year. Here I use 100 x 30 bins, since there are only 30 s-levels.

Note: s-level = sigma layer. s-level = 0 means bottom; s-level = 29 means surface

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/c09dae65-1fdd-4246-80fa-c81dc9532556" width="800"/><br>Fig 4. 2D histogram of s-level of the DO minima vs. DO minima concentration at every lat/lon grid cell on every day of the year. 100x30 bins.</p><br>

From these results, we observe that **DO minima are mostly located in the bottom sigma layer (s-level = 0)**. This explains why our findings from last week are mostly consistent with this week's.

### Sigma layer of DO minima through time

Figure 5 shows 2D histograms of the s-levels of the DO minima vs. day of year at every lat/lon grid cell. Here I use 365 x 30 bins (365 days in the year, 30 s-levels).

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/5a32d667-07fc-422b-ae24-cd8a462d3dfc" width="800"/><br>Fig 5. 2D histogram of s-level of the DO minima vs. day of year at every lat/lon grid cell. 365x30 bins.</p><br>

**Throughout the year, the DO minima generally stay in the bottom sigma layer**.
We see some DO minima in the surface during spring/summer. I suspect that this is for a similar reason that we see higher bottom DO in super shallow regions (or at least this results is consitent with my hypothesis): These regions are so shallow that the entire water column is photosynthesizing-- driving DO concentrations up-- except for at the surface where they converge to an equilibrium with the atmosphere, which in this case may end up being the lowest DO concentration. This result is also consistent with what we see in Figure 4, in which DO minima in the surface sigma layer tends to have a concentration between 8-12 mg/L. 

### Depth of DO minima through time

Figure 6 shows 2D histograms of the depth of the DO minima vs. day of year at every lat/lon grid cell. Here I use 365 x 100 bins.

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/814d94f0-4feb-4516-9364-cbcf33e5555a" width="800"/><br>Fig 6. 2D histogram of depth of the DO minima vs. day of year at every lat/lon grid cell. 365x100 bins.</p><br>

In deeper waters starting after day 200, we observe vertical "stripes" in the 2D histogram that suggest that some DO minima movesup in the water column. These events occur about every 25-30 days, which are reminiscent of the deep water intrusions that we had observed in earlier results. So, deep water intrusions are potentially pushing some deep DO minima higher up in the water column. This hypothesis is consistent with dynamics I have read about in Hood Canal in Deppe et al. (2018).

We also see a very bright yellow line around 10-15 m. Is this a real signal? Does this result imply that the DO minima is most common at depths just below the surface? I am somewhat surprised by this result, and don't quite know what to make of it. 

There is also a strange step-like signal in deep depths around year day 20. What is going on there?

### DO minima through time

Finally, Figure 7 shows 2D histograms of the concentration of the DO minima vs. day of year at every lat/lon grid cell. Here I use 365 x 100 bins.

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/c8c547e2-0fb1-4284-a0a8-72ce7296b0b8" width="800"/><br>Fig 7. 2D histogram of DO minima concentration vs. day of year at every lat/lon grid cell. 365x100 bins.</p><br>

This figure shows a nice seasonal pattern with DO generally higher during the winter, dropping during the spring and summer, and climbing back up in late fall. 

Based on this figure, we can also identify the **hypoxic period in Puget Sound**: roughly from year day 120 - 330, corresponding to **May 1 - November 30**. 

This is informative: In future analysis, we can use May - November to represent the hypoxic season. 

---
## Next steps

My Master's defense is tentatively scheduled for August 9, and I am starting to panic.
With classes, grading, and a bunch of unexpected extra travel, I'm starting to feel worried beause I am not making as much progress on research as I would like to be making. For instance, there was a ton more analysis that I hoped to get to this week, but never found the time to do so. I can't help but feel somewhat stressed, but I'll keep chugging along. The silver lining to all of these feelings is that I seem to be excited to analyze results, and it's hard to not want to keep studying hypoxia during my free time. So I guess I love the research and wish I had more time for it.

In any case, there's a lot of next steps that I want to get to:

Rough outline of thesis
- Will send draft on google docs this week

Start the 2014 run
- Can continue lit review and 2013 analyses while we wait

Plot average bottom DO during hypoxia season
- Similar to figures I have made in the past, except now we know that hypoxia season is May - November in this model

Test hypotheses for higher DO in shallow water (< 10 m)
- Calculate PAR (is there primary productivity throughout the water column?)
- Calculate H2/d (is the depth shallow enough that the entire water column is mixed and can ventilate?)

Reach out to Tall
- Bottom parameterizations may be important to our DO results