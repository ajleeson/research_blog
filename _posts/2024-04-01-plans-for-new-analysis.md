## Plans for new analysis

Hello from the MacBook Air! Despite some ongoing confusion about which keyboard shortcuts do what, I have fully moved into the new computer and we are getting along alright. 

This week it dawned on me that I don't have any upcoming presentations. I am excited to finally have the freedom to explore model results without these immediate deadlines. In this blog post, I explore some next steps for analysis. Hopefully this work will help shape an outline for my master's thesis. More details below.

---
## Depth vs. time property plot in Main Basin

Figure 1 shows depth vs. time property plots at a point in Main Basin. (I have similar figures for Holmes Harbor, Hood Canal, and Admiralty Sill, but I find it's easiest to focus on just Main Basin for now).

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/e4598d95-7c86-4eb6-b2f4-5bca31fb9049" width="800"/><br>Fig 1. Main Basin depth vs. time property plots.</p><br>

The panels in the left-column shows state variable values from the natural run. The right-column shows the anthropogenic minus the natural values, where blue means that anthropogenic is higher, and red means that natural is higher.

Like we observed previously, the natural run has an earlier spring bloom compared to the anthropogenic run, with subsequently strong signals in surface oxygen, zooplankton, small detritus, and large detritus. This observation remains a mystery that I need to explore. During the summer, however, the anthropogenic run tends to have more intense blooms compared to the natural run.

Previously, we also observed a step-like signal in DO that appeared to correspond to deep-water intrusions over Admiralty Sill. In the panels on the right, I am also observing a pattern of high and low NO3, NH4, and DO that appears to vary with some regular frequency. I have yet to determine whether this is related to deep water intrusion events, tides pushing effluent plumes to-and-fro, or some other phenomenon.

Furthermore, in January and February, the anthropogenic run tends to have periods of higher NO3 and NH4 annd lower DO compared to the natural run. I fully expect the anthropogenic run to have more nutrients, but it is odd to me that higher nutrient concentrations appear to also correspond to lower DO concentrations (especially since phytoplankton don't begin blooming until early April). These signals seem similar to respiration (increase in nutrients and drawdown of oxygen), except that there is no detritus to be respired in January and February. It almost looks to me like the effluent plume contains higher nutrient concentrations and lower DO (as if we accidentally also decreased DO concentration in the anthropogenic effluent)--  but I checked the oxygen inputs and they appear to be the same between both runs. More investigation pending.

---
## Ideas for thesis outline

For the Master's thesis, I am envisioning a two-part sequence of analysis.

After the intro and methods sections, I will share results. Thes results will answer "What happened? How did DO (and nutrients/phyto/zoo/detritus) change?"

Then I will dive into the discussion, which will attempt to answer "Why did these changes occur?" Part of answering this question will be to understand the physical processes that control where the nutrients end up, how well-flushed the hypoxic or low-DO regions are, the different mechanisms that drive spatial variability in DO response, etc.

This rough "outline" is still in development, and I will continue to refine it as my analysis matures in the coming weeks.

### Results

**Questions to answer:**
- What are the differences in DO between the two runs? How does DO vary spatially and temporally?
- How do nutrients, phytoplankton, zooplankton, and detritus also compare between these two runs? 

**What I'm thinking about now:**
- What are the best visuals I can make to identify the key differences between the two runs? We have a lot of spatial and temporal variables to work with. How can I simplify these dimensions to show the best summary of what happens?

### Discussion (i.e., why did we observe these results?)

**Questions to answer:**
- Why do we observe an increase in DO in some locations?
- Where do nutrients end up?
- Why does the spring bloom begin earlier in the natural run? And how does timing of blooms influence late-summer/early-fall DO depletion? 
- How do deep water intrusions influence and/or constrain DO depletion in Puget Sound?
- TBD: more questions as they come up in my analysis or literature review...

**What I'm thinking about now:**
- Increases in DO
    - Need to confirm hypothesis that those locations are very shallow, well-mixed, and photosynthesizing throughout the water column. I can explore this by taking a mooring extraction at these locations (e.g. Northern Port Susan) and looking at density/salinity profiles.
- Where nutrients end up
    - This week, I read Ho et al., 2023, a paper that explores the effect of nutrients reductions and wastewater recylcing to primary productive, respiration, oxygen, and pH in the Southern California Bight (SCB). In this paper, the authors calculate ammonium transport from the treatment plants. I'm curious whether I can translate their methods into Puget Sound where we have the added complexity of exchange flow dynamics. I'm also planning to reach out to the authors to ask hhow/whether they corrected for NH4 created from respiration (rather than NH4 solely sourced from outfalls)
    - Another part of this investigation is to answer whether we see an impact in Hood Canal, or other locations further away from the large dischargers. If so, how do the nutrients end up there? These analyses are pending longer model runs.
- Bloom timing
    - Why did the bloom begin earlier in the natural run-- I'm still struggling to untangle this puzzle. I was hoping that the depth vs. time property plots above would help shed some light, but I am still confused. It may be worth diving into the NPZD+O equations to understand what the phytoplankton growth rate depends on.
    - The natural run has earlier and more intense spring blooms, but generally higher bottom DO compared to anthropogenic run. In contrast, the anthropogenic run has more intense summer blooms. How much is late-summer bottom DO influenced by summer blooms vs. spring blooms? Maybe I can come up with a measure for stratification- perhaps the timing of the blooms is really relevant with respect to the timing of stratification. 
- Deep water intrusions
    - How often do DO intrusion events occur, and how can I correlate them with other observations? Do deep water intrusions constrain how low DO can get (since deep water gets replenished ~monthly)? How do intrusions influence where the DO minima ends up in Hood Canal, and is the response different between the anthropogenic and natural run? The first step to answering these questions is to quantify how often and when the deep water intrusions are happening.

---
## Planned work this week

My todo's for this week include:
- Get times of deep water intrusion events (following methods from Deppe et al., 2018) using a mooring extraction at Admiralty  Sill. 
- Look at a transect through West Point. Where is the effluent plume in the water column? Does it move with the tides? Where does it go?
- Update driver_roms3 so I can be ready to extend the model run when Parker gets back. 
- Keep reading.

---
## Modeling project

For Christie's numerical modeling class, I need to make a simple numerical model for a final project. I think there is some potential to make this experiment also relevant and useful for research.

My first idea is probably way too complex and not quite the goal of the "develop your own model" aspect of the project, but I am still really excited about it. It would be fun to use Jilian's Hood Canal model with a simplified biogeochemistry module to understand temperature, river discharge, and wind controls on hypoxia in Hood Canal (I'm thinking about the Scully, 2013 paper on the Chesapeake.)

Another idea is less relevant to research, but much simpler. I could build an idealized Hood Canal estuary. Then I could vary wind direction and river discharge and see how these changes influence stratification and exchange flow strenght in Hood Canal. Maybe I could also finally run some TEF code in this experiment.

I'm also open to other ideas and would love to hear any recommendations you may have! I am still very much in the brainstorming phase of this project. 
