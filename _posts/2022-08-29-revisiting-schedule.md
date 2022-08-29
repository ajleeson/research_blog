## Revisiting Goals and Scheduling

At the beginning of the summer we had set a few goals. I thought it might be good to revisit them and evaluate the best use of the remaining time before the start of Autumn quarter.

Throughout this post, I consider "summer" to be any time prior to the start of Autumn quarter (September 28).

---
## Original Summer Goals

Figure 1 shows the original summer goals that we drafted together.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/187276235-13a92e5f-12c6-47f6-a2c3-d6a260753b4e.png" width="1000"/><br>Fig 1. Original summer goals.</p><br>

Throughout the summer, I have become much more familar and comfortable navigating the LO system and manipulating different options in ROMS.

I was able to complete several mini experiments using the idealized estuary, but I did not complete all of the experiments on our list.

The first notable unfinished experiment is changing nitrate concentrations in WWTP effluent. There is still time to squeeze this experiment in before the end of summer. The next section has more discussion on this topic.

The second notable unfinished experiment is changing the bottom boundary conditions. There likely will not be enough time to run this experiment this summer. However, I think it will still be a good idea to read more literature on this topic throughout the Autumn quarter. (I also told the UW Modeling Group that I would eventually present on this topic, so it might be good to keep that promise in the future.)

This summer I had also planned to add WWTP outfalls to LiveOcean. So far, I have found timeseries data from [Ecology's website](https://fortress.wa.gov/ecy/ezshare/EAP/SalishSea/SalishSeaModelBoundingScenarios.html) that is used in the SSM. I'll write up blog post within the next week that summarizes these data.

I have written a script that can generate timeseries plots for all of the variables from all of the point sources and nonpoint sources in Ecology's dataset. There are many, dischargers and variables so I have not yet run the script to completion. However, I can generate/save these plots in the next week and find a way to share them.

Prior to our quarterly update, I also wrote an algorithm that places inland point sources at the nearest coastal grid cell. More work is needed in the future to transfer this algorithm from an idealized estuary to LiveOcean. 

In the next section I propose a schedule of work for the remainder of summer.

---
## Proposed Schedule for Remainder of Summer

Figure 2 shows my proposed schedule for the remainder of summer. The main goal is to add WWTPs to LiveOcean. 

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/187275781-be302c1b-035b-4edc-adfe-5076f5c44154.png" width="1400"/><br>Fig 2. Proposed schedule.</p><br>

The proposed schedule is ambitious, but not out of the realm of possibility. It's certainly something to view as a goal.

### Test npzd and wwtps on idealized model

Prior to adding WWTPs and nutrient timeseries data to LiveOcean, it will be good to test the procedure on the idealized model.

- **Add npzd to idealized estuary** This past week I have incorporated the npzd module into the idealized estuary (using bio_Fennel_BLANK.in). The model runs without crashing. However, phytoplankton aren't growing. I need to follow-up on this issue.
- **Decide on nutrients and initial conditions** Pick initial biogeochemistry values to use in the ocean, river, and point sources.
- **Create nutrient file of identical format to SSM nutrient data** After deciding on point source and river nutrient concentrations, I will create an excel file with nutrient data that exactly matches the format of Ecology's nutrient timeseries.
- **Add some vertical distribution of effluent** This step will depend on how I decide to represent a plume of effluent across the sigma layers. 
- **Run the model** My goal is to run two test cases. One case with no nitrate in the point source effluent, and one case with high nitrate in the point source effluent.
- **Debug and write up results** I'll likely be debugging at every step of this process. At the end I'll write up my results and findings in another blog post.

### Add WWTPs to LiveOcean

This was the main goal we had set for the summer. Even if this task is not completed by the end of summer, it should be close to completed. 

- **Find lat/lon coordinates of rivers and point sources in SSM** These data are supposedly publically available through Ecology's website. I'm having an issue with the downloadable file right now, but I have a contact at Ecology I am communicating with. Hopefully I will have this information within the next few days.
- **Decide on vertical distribution of effluent** I read quite a bit of *Mixing in Inland and Coastal Waters* last week. This week I'm planning on coming up with a proposed vertical distribution of effluent discharges. 
- **Share SSM data and proposed vertical distribution** Next week I'll dedicate a blog post to the available nutrient data and the proposed effluent distribution
- **Write algorithm that adds points sources and small rivers to LiveOcean** This will be a similar algorithm to the one I have written for the idealized estuary. In addition to placing the point sources, I will also need an algorithm that reads the nutrient data from Ecology's excel timeseries. 
- **Documentation and writing READMES** I will make personal notes throughout this entire process. Once things are in a working state, I will write a more formal README and blog post explaining how to use the point source algorithm (and a high-level of what the algorithm does).