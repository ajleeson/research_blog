## Characteristics of Hypoxic Inlets

This week I began taking a look at characteristics of inlets that might correspond to hypoxia.

As a reminder, we are interested in looking at factors that correspond to the spatial distribution of hypoxia in Figures 1 and 2 below.

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/5744cb27-2872-4698-bba7-3ea42f888907" width="900"/><br>Fig 1. Average bottom DO in Puget Sound during hypoxic season.</p><br>

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/b2e7fafb-5801-4ca0-9b31-51130acaf2d8" width="900"/><br>Fig 2. Puget Sound map of days with bottom DO < 2 mg/L.</p><br>

So far, I have only been able to look at one factor: stratification. Figure 3 shows the average stratification (bottom minus surface density) during the 2014 hypoxic season.

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/b8713ff1-c9b8-4d76-812d-2fa4179ccc3c" width="415"/><br>Fig 3. Average bottom minus top density during the 2014 hypoxic season.</p><br>

In general, Hood Canal and Whidbey Basin appear more stratified than South Sound and Main Basin. Some of the more stratified regions, like Hood Canal, tend to have hypoxia. But not all stratified areas seem to develop hypoxia (e.g. Commencement Bay by Puyallup River is stratified, but not hypoxic). 

I also notice that areas with near-zero stratification have the highest oxygen levels. Specifically, inlets in South Sound and the northern section of Port Susan appear to be well-mixed and thus well-ventilated.

I was really eager to make more figures, but I ran into some computer issues. It seems like I am trying to extract too much data from the LiveOcean output in one script. The extraction script takes over one hour to run per year, and it keeps stopping mid-run. My next step will be to re-strategize how to extract the data I want. Perhaps I need to break up my extractions into smaller chunks.

Once I get past this roadblock, I'm hoping to look at:
- tidal currents
- depth-averaged eddy viscosity
- depth-integrated bio variables (e.g. phytoplankton, detritus, etc.)
- flushing time
- size of inlets