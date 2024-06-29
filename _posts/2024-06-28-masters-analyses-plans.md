## Master's Analyses Plans

Per Alex's advice, this week I spent time thinking about the analyses that I want to include in my Master's thesis. My goals from these analyses are to answer:
- Where does hypoxia occur?
- When does hypoxia occur?
- How does the spatial distribution and timing of hypoxia relate to biogeochemical state variables?
- What are the physical characteristics of inlets that develop hypoxia, compared to those that do not

In the following section, I describe my plans to address these questions. Some of the sections include preliminary figures as well. 

One of the end goals of this Master's data exploration is to identify key characteristics that appear to correlate with hypoxia. After my defense, I plan to conduct more detailed hypothesis testing to explore why these key characteristics might be driving hypoxia. 

---
<details><summary><strong>Types of Figures</strong></summary>

Below I list several types of figures that I have generated in the past. I think of this list as my current tool box, as it will be rather straightforward to generate similar figures, but perhaps for different state variables.

### Tools for addressing "when"

**Time series at a single lat/lon coordinate.**
- Pros: Multiple state variables in one plot
- Cons: Lose spatial resolution (i.e. don't resolve depth and don't resolve horizontal)
Example: ocean sciences figure

**Depth vs. time colors by state variable**
- Pros: Resolve vertical structure of state variables, and can see how the entire water column evloes over time
- Cons: Can only look at one point at a time, and can't plot multiple state variables on the same axis
Example: moor depth vs time property plot

### Tools for addressing "where"

Colormaps of the Puget Sound region
- Pros: Visualize spatial distribution of state variables across region
- Cons: No depth resolution, and not temporal resolution (either a snapshot, or averaged over time)
Example: average bottom DO concentration

### Miscellaneous

2D histograms
New figure: state variable along transect

### Overall thoughts

No tool is solely able to answer a single question. The temporal and spatial components are always important to consider. However, we can use a combination of tools to help narrow down what we want to look at.

For instance, I can create colormaps of bottom DO in Puget Sound for every month of the year. Based on these figures, we can begin to identify when hypoxia may be developing in different inlets. This analysis will work even if hypoxia develops at different times in different inlets. Then, I can pick a few inlets, and look at timeseries of biogeochemical state variables (or physical variables, like buoyancy frequency). From these timeseries, we can then explore whether differences in the timing of nutrient availability, or blooms, or stratification, might explain differences in timing of hypoxia. 

</details>

Need to add example figures

---
<details><summary><strong>Where does hypoxia occur?</strong></summary>

I hypothesize that hypoxia is most likely to occur where there is:
- Strong stratification
- Weak turbulence
- Weak circulation
- Lots of detritus

**Stratification**

When waters are strongly stratified, it is difficult to mix oxygenated surface waters to low DO bottom waters. Thus, I expect that stratified inlets are more susceptible to hypoxia.

To explore stratification, I will look at the maximum Brunt-Vaisala frequency in the water column. This data exploration will start with a colormap of average N across Puget Sound during the hypoxia season. Qualitatively, does there appear to be a correlation between bottom hypoxia and average N?

To calculate N, I will first calculate density from salinity and temperature. Then, I plan to pass the vertical density field through a Butterworth filter. Finally, I will approximate the vertical density gradient at every grid cell:

$$ N = \sqrt{-\frac{g}{\rho_0} \frac{\partial \rho(z)}{\partial z}} \ 
 \mathrm{ where } \frac{\partial \rho(z)}{\partial z}
 \Bigg|_{z = z_{rho}^{\sigma}} \approx 
\frac{\rho(z_{rho}^{\sigma}) - \rho(z_{rho}^{\sigma-1})}{z_{rho}^{\sigma} - z_{rho}^{\sigma-1}}$$

After looking at colormaps of N, I will then look at a 2D histogram of bottom DO vs. max buoyancy frequency at every lat/lon grid cell for every day of the year. 

Note that max N tells us the resulting stratification after all turbulent processes have acted on a water body. A location could have strong river input, but could have extermely strong turbulence such that there is a small resulting max N. Thus, I need to be aware that max N does not tell us the strength of turbulence or the ratio of freshwater input. It simply tells us the resulting stratification after all of these other processes have happened.

**Weak turbulence**

If turbulent mixing is strong, then there is higher likelihood that vertical mixing can overcome stratification and ventilate bottom waters.

As a proxy for turbulence strength, I plan to look at tidal currents. Again, I will start with a Puget Sound map of the average tidal current, U, during the hypoxia season. 

$$ U = \sqrt{\bar{u}^2 + \bar{v}^2} $$

LiveOcean also outputs eddy viscosity and eddy diffusivity. I thought of looking at the min eddy viscosity in the water column. However, I am worried that this method could preferentially sample eddy viscosity in the bottom layer, rather than at the pycnocline.

**Weak circulation**

When circulation is weak, I expect that bottom waters will not be renewed often and that an inlet is thus more susceptible to hypoxia. I imagine that the best way to look at circulation would be to use the TEF method. 

Ideally, I want to calculate the flushing time of each of the 21 inlets defined in Figure ??? in the section "Comparisons between inlets," where flushing time is:

$$ T_{flush} = \frac{V}{Q}$$

as defined in MacCready et al. (2021) where V is the volume of the inlet and Q is Qout or Qin calculate from TEF. I could draw a section at the  mouth of each of the 21 inlets and determine the Qin and Qout flowing in and out of the terminal inlet.

Realistically, I do not think I have enough time before August 9 to learn how to do a TEF calculation, and do it well. However, I want to bookmark this thought and revisit it later. 

**Lots of detritus**

**Depth**

This is a bonus section about depth, based on prior results. Figure ??? is an image I shared a few weeks ago. They are 2D histograms of DO minima depth vs. DO minima concentration. 

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/6f18c313-0766-40bc-b1be-d184548faf48" width="800"/><br>Fig ???. 2D histograms of depth vs. concentration of the DO minima for every lat/lon grid cell on every day of the year. Data from Straits omitted. 100 x 100 bins.</p><br>

Based on this figure, it appears that hypoxia does not develop in waters shallower than ~10 m. 

We also observe that hypoxia does not develop in waters deeper than ~180 m. I hypothesize that this is due to the limit of sinking and respiration rate in LiveOcean. Organic matter will be fully respired before they can sink to a depth deeper than ~180 m. However, if this depth limitation were true, then I would have expected a gradual transition in depth from 180 m to deeper water as DO increases. But this is not what we observe. There is a hard cutoff of ~180 m up until ~ 3.75 mg/L where suddenly there are points that extend throughout the entire water column.

My guess is that this low DO water at depth is actually coming from deep water intrusions over Admiralty Sill. In other words, these deep low DO waters are of oceanic origin, and not solely a result of biogeochemical processes within Puget Sound.

Note, this is just a hypothesis based on the results. I can verify this hypothesis by calculating the deepest depth that organic matter can sink based on sinking and respiration rates in LiveOcean (or I can see what number Dakota comes up with when she's done with her homework).

</details>

Don't forget comments on detritus hypothesis

---

<details><summary><strong>When does hypoxia occur?</strong></summary>


We lose spatial resolution when we look at time series data. Here I propose two different ways of analyzing temporal data. First, a system-wide approach across all of Puget Sound. Then, a more focused analysis on just a few inlets.

### System-wide temporal analysis

First, I will start by looking at system-wide timing of nutrient availability, nutrient uptake, blooms, conversion to organic matter, and oxygen depletion. 

To do this, I want to make flame plot time series of relevant state variable. 
Recall the oxygen flame plot (Fig. ???).

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/61609ace-d5bf-4e66-baef-ead0eb03e24e" width="800"/><br>Fig ???. Flame plot. 2D histograms of DO minima concentration vs. yearday for every lat/lon grid cell. Data from Straits omitted. 365 x 100 bins.</p><br>

I want to unfurl this figure so that all years extend across one single row, rather than two rows. Then, I will plot a 2D histogram of a different state variable vs. time on every row. Figure ??? shows a quick sketch of my idea.

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/f5359410-d2cb-40eb-837b-1eefdcdefcbe" width="500"/><br>Fig ???. Sketch of flame plot time series for multiple state variables.</p><br>

The state variables I want to include are:
- photic-zone integrated NO3
- photic-zone integrated NH4
- depth-integrated phytoplankton
- depth-integrated zooplankton
- depth-integrated small detritus
- depth-integrated large detritus
- max N (to see if there is seasonality to when stratification sets up)
- bottom DO

How to calculate depth of photic zone? Look at photic zone because these are the nutrients that are available for uptake.

### Select inlet temporal anslysis

The inlets I am interested in analyzing more deeply are:

- Lynch Cove (because it is the most hypoxic inlet)
- Elliot Bay (because it does not tend to develop hypoxia, but it sits between the two largest WWTPs in Puget Sound)
- Port Susan (to give us some insight into Whidbey Basin, and because the northern and southern half of Port Susan have wildly different oxygen concentrations despite being part of the same water body. I'll probably split this analysis into "Northern Port Susan" and "Southern Port Susan")
- Something in South Sound...

This gives me 5 locations to look at, which feels manageable.

First, I will conduct a mooring extraction from the rough center of each of these inlets/bays. Then, I will look at time series of the same variables listed in the above section:
- photic-zone integrated NO3 
- photic-zone integrated NH4
- depth-integrated phytoplankton
- depth-integrated zooplankton
- depth-integrated small detritus
- depth-integrated large detritus
- maximum buoyancy frequency
- bottom DO

Note that these time series are now just a single line, rather than a 2D histogram of points.

From these time series, I can hopefully resolve the cycle of nutrients, phytoplankton, zooplankton, organic matter, stratification, and bottom DO in these select inlets. 

These time series will hopefully give me an idea of whether the timing or magnitude of different events varies between the selected inlets.

</details>

Would it be valuable to look at time series of hypoxic volume normalized by inlet volume, for each of the 21 inlets. To compare the timing of if and when hypoxia develops in each of the inlets?

---

<details><summary><strong>Comparisons between inlets</strong></summary>

I am interested in comparing the characteristics of different terminal inlets throughout Puget Sound (Fig. ???).

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/9142edc5-e52b-4c32-a45b-6035dfa07614" width="800"/><br>Fig ???. Inlets in Puget Sound that I will compare.</p><br>

Some of these inlets experience seasonal hypoxia, while others generally have high DO concentrations. Like Alex suggested, we could learn something by plotting these 21 inlets on some relevant nondimensional parameter space, and coloring inlets by average bottom DO (Fig. ???)

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/c27d8e3a-2ae9-4cdd-8673-f3b3a1e11ff8" width="400"/><br>Fig ???. Sketch of 21 inlets in parameter space, colored by average bottom DO.</p><br>

Even though this parameter space plot is the final goal, I can start by making some exploratory scatter plots, where each point corresponds to one of the 21 inlets. I can plot average bottom DO during hpoxic season vs:
- average depth
- average max N during hypoxic season
- average tidal current
- residence time (may not be able to calculate unless I have time to learn TEF)
- WWTP loading into inlet (my brain is still trying to figure out how to quantify this)

Then I can calculate any statistically significant correlation coefficients (Fig. ???). I plan to average values over all 6 years.

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/2c87de4a-e3da-4c56-8187-7e183b75a30c" width="270"/><br>Fig ???. Sketch of average bottom DO and max N in each terminal inlet during the hypoxic season. I will plot statistically significant slopes and correlation coefficients.</p><br>

Why do I want to make scatter plots and conduct correlation tests using only 21 points, rather than using all of the data in the 2D histograms? Looking at these 21 inlets is a way to subsample the data. Otherwise, I am worried that by looking at every grid cell on every day of the year, my points would not all be independent (spatially adjacent and temporally adjacent points would not be independent!). Additionally, large inlets would inherently have more data points than smaller inlets.

 Hopefully these 21 terminal indlets in Puget Sound are reasonably distinct from one another. However, I will caveat by saying that I still do not think think that these 21 points are completely independent. Realistically, inlets in Whidbey Basin might be more similar to each other than to inlets in other basins, and so on and so forth.

</details>

