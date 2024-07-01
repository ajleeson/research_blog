## Master's Analyses Plans

Per Alex's advice, this week I spent time thinking about the analyses that I want to include in my Master's thesis. My goals from these analyses are to answer:
- Where does hypoxia occur?
- When does hypoxia occur?
- How does the spatial distribution and timing of hypoxia relate to biogeochemical state variables?
- What are the physical characteristics of inlets that develop hypoxia, compared to those that do not
- How do our findings compare with observations?

The list below summarizes the figures I plan to generate:
- Maps of state variables averaged over hypoxia season (one figure per state variable. First subplot is average of all years, remaining subplots are deviation of individual years from average)
    - Max water column Brunt-Vaisala frequency 
    - Depth-averaged tidal currents
    - Depth-integrated detritus
- Six year flame plot time series (one large figure with 8 subplots)
    - photic-zone integrated NO3
    - photic-zone integrated NH4
    - depth-integrated phytoplankton
    - depth-integrated zooplankton
    - depth-integrated small detritus
    - depth-integrated large detritus
    - max buoyancy frequency
    - bottom DO
- Six year time series of state variables at a single extraction location from the center of four different inlets: Lynch Cove, Elliot Bay, Northern Port Susan, Southern Port Susan, something in South Sound
    - photic-zone integrated NO3
    - photic-zone integrated NH4
    - depth-integrated phytoplankton
    - depth-integrated zooplankton
    - depth-integrated small detritus
    - depth-integrated large detritus
    - maximum buoyancy frequency
    - bottom DO
- Scatter plot of 21 points corresponding to 21 terminal inlets in Puget Sound. Calculate correlation coefficient. (Average data over six years)
    - average hypoxia season bottom DO vs. average depth
    - average hypoxia season bottom DO vs. average max N during hypoxic season
    - average hypoxia season bottom DO vs. average tidal current
    - average hypoxia season bottom DO vs. residence time (if time permitting; need TEF)
    - average hypoxia season bottom DO vs. WWTP loading into inlet (still brainstorming...)
- Parameter space scatter plot with 21 points
    - Mixing number vs. gradient Richardson number, colored by average bottom DO
- Comparison to obervations
    - Property-property plots using 6 years of data throughout Puget Sound
    - Compare time series at a few select inlets (e.g. near Lynch Cove)
- Extra math
    - Calculate max depth to which detritus can sink in LiveOcean, using sinking rate and respiration rate

In the following sections, I describe my plans to address these questions in more detail.

One of the end goals of this Master's data exploration is to identify key characteristics that appear to correlate with hypoxia. After my defense, I plan to conduct more detailed hypothesis testing to explore why these key characteristics might be driving hypoxia. 

---
## Types of Figures

Below I list several types of figures that I have generated in the past. I think of this list as my current tool box, as it will be rather straightforward to generate similar figures, but perhaps for different state variables.

### Tools for addressing "when"

**Time series at a single lat/lon coordinate.**
- Pros: Multiple state variables in one plot
- Cons: Lose spatial resolution (i.e. don't resolve depth and don't resolve horizontal)
- Example in Figure 1 below

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/ab7badb6-62a4-40d1-b1b4-d03a66c37789" width="500"/><br>Fig 1. Example time series of bottom DO and surface chlorophyll at point in Main Basin in 2013 (data from mooring extraction).</p><br>

**Depth vs. time property plot**
- Pros: Resolve vertical structure of state variables, and can see how the entire water column evolves over time
- Cons: Can only look at one lat/lon point at a time, and can't plot multiple state variables on the same axis
- Example in Figure 2 below

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/8b147a72-ab70-498b-9e0d-0d47faf3248c" width="800"/><br>Fig 2. Example depth vs. time property plot from mooring extraction in Main Basin in 2013.</p><br>

### Tools for addressing "where"

**Colormaps of the Puget Sound region**
- Pros: Visualize spatial distribution of state variables across region
- Cons: No depth resolution and no temporal resolution (either a snapshot, or averaged over time)
- Example in Figure 3 below

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/9a736c57-a26e-4a91-b487-0dc3344c39b7" width="600"/><br>Fig 3. Example maps of average bottom DO in Puget Sound during hypoxic season, and average number of hypoxic days (average of 2014 - 2019 hypoxic seasons).</p><br>

### Miscellaneous

**2D histograms**
- Pros: See relationship between two state variables at all times and all lat/lon cooridnates
- Cons: Cannot resolve how state variables vary in space or time.
- Example in Figure 4 below

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/90b95422-07af-4fd7-ab73-dcc8a5dc9bcf" width="600"/><br>Fig 4. Example 2D histogram of S-level of DO minima vs. DO minima concentration for all lat/lon grid cells on every day of the year. 100 x 30 bins.</p><br>

**State variable along a transect**

This is a new type of figure that I have not made yet. Hopefully it can help us understand how state variables vary in space.
- Pros: See how state variable varies in the vertical and along a transect.
- Cons: No temporal resolution

### Overall thoughts

No tool is solely able to answer a single question. The temporal and spatial components are always important to consider. However, we can use a combination of tools to help narrow down what we want to look at.

For instance, I can create colormaps of bottom DO in Puget Sound for every month of the year. Based on these figures, we can begin to identify when hypoxia may be developing in different inlets. This analysis will work even if hypoxia develops at different times in different inlets. Then, I can pick a few inlets, and look at timeseries of biogeochemical state variables (or physical variables, like buoyancy frequency). From these timeseries, we can then explore whether differences in the timing of nutrient availability, or blooms, or stratification, might explain differences in timing of hypoxia. 

---
## Where does hypoxia occur?

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

Note that max N tells us the resulting stratification after all turbulent processes have acted on a water body. A location could have strong river input, but could have extermely strong turbulence such that there is a small resulting max N. Thus, I need to be aware that max N does not tell us the strength of turbulence or the ratio of freshwater input. It simply tells us the resulting stratification after all of these other processes have happened.

**Weak turbulence**

If turbulent mixing is strong, I predict that there is higher likelihood that vertical mixing can overcome stratification and ventilate bottom waters.

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

I also hypothesize that locations that have accumulated a lot of organic matter or more susceptible to hypoxia because there is more stuff that microbes can break down.

I plan to make a map of depth-integrated detritus averaged over the hypoxia season. I want to depth integrate because subsurface respiration draws down oxygen throughout the water column (since there is no air-sea gas exchange below the surface).

From here, I hope to qualitatively see if there seems to be a correlation between detritus and low DO. 

Note that detritus does not tell us where a bloom occurred, it just tells us where the organic matter ended up.

**Depth**

This is a bonus section about depth, based on prior results. Figure 5 is an image I shared a few weeks ago. They are 2D histograms of DO minima depth vs. DO minima concentration. 

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/6f18c313-0766-40bc-b1be-d184548faf48" width="800"/><br>Fig 5. 2D histograms of depth vs. concentration of the DO minima for every lat/lon grid cell on every day of the year. Data from Straits omitted. 100 x 100 bins.</p><br>

Based on this figure, it appears that hypoxia does not develop in waters shallower than ~10 m. 

We also observe that hypoxia does not develop in waters deeper than ~180 m. I hypothesize that this is due to the limit of sinking and respiration rate in LiveOcean. Organic matter will be fully respired before they can sink to a depth deeper than ~180 m. However, if this depth limitation were true, then I would have expected a gradual transition in depth from 180 m to deeper water as DO increases. But this is not what we observe. There is a hard cutoff of ~180 m up until ~ 3.75 mg/L where suddenly there are points that extend throughout the entire water column.

My guess is that this low DO water at depth is actually coming from deep water intrusions over Admiralty Sill. In other words, these deep low DO waters are of oceanic origin, and not solely a result of biogeochemical processes within Puget Sound.

Note, this is just a hypothesis based on the results. I can verify this hypothesis by calculating the deepest depth that organic matter can sink based on sinking and respiration rates in LiveOcean (or I can see what number Dakota comes up with when she's done with her homework).

---

## When does hypoxia occur?


We lose spatial resolution when we look at time series data. Here I propose two different ways of analyzing temporal data. First, a system-wide approach across all of Puget Sound. Then, a more focused analysis on just a few inlets.

### System-wide temporal analysis

First, I will start by looking at system-wide timing of nutrient availability, nutrient uptake, blooms, conversion to organic matter, and oxygen depletion. 

To do this, I want to make flame plot time series of relevant state variable. 
Recall the oxygen flame plot (Fig. 6)

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/61609ace-d5bf-4e66-baef-ead0eb03e24e" width="800"/><br>Fig 6. Flame plot. 2D histograms of DO minima concentration vs. yearday for every lat/lon grid cell. Data from Straits omitted. 365 x 100 bins.</p><br>

I want to unfurl this figure so that all years extend across one single row, rather than two rows. Then, I will plot a 2D histogram of a different state variable vs. time on every row. Figure 7 shows a quick sketch of my idea.

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/f5359410-d2cb-40eb-837b-1eefdcdefcbe" width="500"/><br>Fig 7. Sketch of flame plot time series for multiple state variables.</p><br>

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

---

## Comparisons between inlets

I am interested in comparing the characteristics of different terminal inlets throughout Puget Sound (Fig. 8)

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/9142edc5-e52b-4c32-a45b-6035dfa07614" width="800"/><br>Fig 8. Inlets in Puget Sound that I will compare.</p><br>

Some of these inlets experience seasonal hypoxia, while others generally have high DO concentrations. Like Alex suggested, we could learn something by plotting these 21 inlets on some relevant nondimensional parameter space, and coloring inlets by average bottom DO.

Even though this parameter space plot is the final goal, I can start by making some exploratory scatter plots, where each point corresponds to one of the 21 inlets. I can plot average bottom DO during hpoxic season vs:
- average depth
- average max N during hypoxic season
- average tidal current
- residence time (may not be able to calculate unless I have time to learn TEF)
- WWTP loading into inlet (my brain is still trying to figure out how to quantify this)

Then I can calculate any statistically significant correlation coefficients (Fig. 9). I plan to average values over all 6 years.

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/2c87de4a-e3da-4c56-8187-7e183b75a30c" width="270"/><br>Fig 9. Sketch of average bottom DO and max N in each terminal inlet during the hypoxic season. I will plot statistically significant slopes and correlation coefficients.</p><br>

Why do I want to make scatter plots and conduct correlation tests using only 21 points, rather than using all of the data in the 2D histograms? Looking at these 21 inlets is a way to subsample the data. Otherwise, I am worried that by looking at every grid cell on every day of the year, my points would not all be independent (spatially adjacent and temporally adjacent points would not be independent!). Additionally, large inlets would inherently have more data points than smaller inlets.

Hopefully these 21 terminal inlets in Puget Sound are reasonably distinct from one another. However, I still do not think think that these 21 points are completely independent. I imagine that inlets in within the same basin (e.g. Whidbey) are more likely to have the similar amounts of organic matter or nutrients to each other than to other basins.

Note that I am still trying to think of ways to *quantify* WWTP contributions into different inlets.

### Potential nondimensional numbers

Based on Geyer and MacCready (2014), I came up with a list of some potential nondimensional numbers for the parameter space plot.

**Gradient Richardson Number**

We can calculate the gradient Richardson number as

$$ \mathrm{Ri} = \frac{N^2}{\big( \frac{\partial u}{\partial z} \big)^2} $$

This is the ratio of buoyancy to shear. When Ri < 1/4, there is not enough buoyant restoring force to suppress mixing. For large Ri, stratification suppresses turbulent mixing.

The biggest challenge to calculate Ri will be to determine the vertical shear. 

After calculate the max N in the water column, I can find the corresponding vertical location-- this is the vertical position of the pycnocline.

Then, I can determine vertical shear at this depth by approximating:

$$ \frac{\partial u}{\partial z} \approx
\frac{u(z_{rho}^{\sigma}) - u(z_{rho}^{\sigma-1})}{z_{rho}^{\sigma} - z_{rho}^{\sigma-1}}$$

My issue with this method is that we have both u and v velocities. How do I approximate shear in two dimensions? Do I calculate du/dz and dv/dz, then add their magnitude in quadrature? Such that:

$$ \frac{\partial \mathbf{u}}{\partial z} =
\sqrt{\Bigg[\frac{\partial u}{\partial z}\Bigg]^2 + 
\Bigg[\frac{\partial v}{\partial z}\Bigg]^2}\approx
\sqrt{
    \Bigg[ \frac{u(z_{rho}^{\sigma}) - u(z_{rho}^{\sigma-1})}{z_{rho}^{\sigma} - z_{rho}^{\sigma-1}}\Bigg]^2
    +
    \Bigg[ \frac{v(z_{rho}^{\sigma}) - v(z_{rho}^{\sigma-1})}{z_{rho}^{\sigma} - z_{rho}^{\sigma-1}}\Bigg]^2
}$$

I think this method gets us the appropriate magnitude of shear, taking into account shear of both u and v.

**Mixing number**

The mixing number was used in the final estuarine parameter space figure in Geyer and MacCready (2014). To me, it seems like a relevant nondimensional number for our purposes as well. The mixing number tells us whether the tides act long enough to mix the entire water column. Small mixing numbers mean that the tidal timescale is shorter than the mixing timescale. Large mixing numbers mean that that the tidal timescale is longer than the mixing timescale. Thus, this number should give us an idea of whether the tides provide enough mixing.

$$ M = \sqrt{\frac{C_D U_T^2}{\omega N_o H^2}}$$

where:
$$ C_D = (1-2.5) \times 10^{-3} $$
$$ U_T^2 = \bar{u}^2 + \bar{v}^2 $$
$$ \omega = \mathrm{tidal\ frequncy} $$
$$ N_o = \sqrt{\frac{\beta g s_{ocean}}{H}} \ \mathrm{and} \ \beta = 7.7 \times 10^ {-4}$$

I already plan to calculate average tidal current, U_T, and average depth, H. The biggest challenge for me will be to estimate the tidal frequency and ocean salinity. I need to dig into the LiveOcean inputs find a way to estimate these values.

**Sketch figure**

Figure 10 shows a sketch of what I am hoping to achieve with a parameter space plot.

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/794379ba-7427-4446-8cb7-fcfff65dd0a0" width="400"/><br>Fig 10. Sketch of 21 inlets in parameter space, colored by average bottom DO.</p><br>

### Other last-minute thoughts

Would it be valuable to look at time series of hypoxic volume normalized by inlet volume, for each of the 21 inlets. To compare the timing of if and when hypoxia develops in each of the inlets? This will be relatively easy to do.

I can also make maps of average monthly state variable values. This will give us both spatial and temporal insight. We would be able to see whether certain areas become more stratified earlier than others, or if some basins have blooms later than others.

---
## Comparison to observations...

I will also continue efforts to compare model output to observations. 
For the model data comparison, I will focus solely in the Puget Sound region. 

I will start by making property-property plots for all six years of available data. 
If we have observations in locations in Lynch Cove, Elliot Bay, Port Susan, or an inlet in South Sound, then I will plan to extract time series from the same lat/lon coordinate as the observations were taken. 

I already know we have an orca buoy near Lynch Cove. Perhaps I can find a inlet in South Sound that will work. I need to check our observational records again, but I do not think we have data in Port Susan, or much in the Whidbey Inlets (the Penn Cove data are too recent to compare.)
