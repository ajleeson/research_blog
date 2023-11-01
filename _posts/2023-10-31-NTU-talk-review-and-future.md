## NTU visit and next research steps

This past week I finally gave my talk at NTU. Thank you all for your support up to this point-- I had a great experience. Shih-Nan also says hello to you.

Now that I have finally wrapped the "Spooky Month of Presentations," I am ready to dive back into research. In this blog post, I dive into my current thoughts on research. More details below.

Warning, this post is text-heavy. To help get through it, here is a video of my uncle's baby goat eating leaves from an "off-limits" tree (which is protected by electrical wire...under which the baby goats can crawl).

<video src="https://github.com/ajleeson/LO_user/assets/15829099/7582bf21-8a40-4e74-8a63-9fb44e7a99e8" controls="controls" style="max-width: 250px;"></video>


---
## Visit to IONTU

On Thursday, I visited Shih-Nan Chen at the Institute of Oceanography at National Taiwan University (IONTU). Overall, my visit was a very positive experience.

Shih-Nan was a fantastic host. He gave me a campus tour by bike, and we had a great conversation during lunch. After my talk, he also showed me some of his latest simulations of coherent structures. His research looks quite exciting, though he was rather humble about it all.

Before my talk, Shih-Nan also introduced me to the other PO faculty at NTU. So many IONTU faculty have connections to UW! I did not realize that there had been so much collaboration between these two groups in the past, and I am excited to see more of these collaborations in the future. If I were not staying so far away, then I would be eager to spend more time with the IONTU community.

The talk itself went well. The audience was ~20 strong, but the lecture hall was large and I needed a microphone (which caught me off guard!). I am glad that I practiced my talk with the EFM group in advance because I felt comfortable enough with my slides to step away from the podium, microphone in hand. The students were shy, but the faculty asked great questions.

I am also already seeing the benefits of our journal club. At some point during my conversation with Shih-Nan I brought up Scully (2013), and he was quite excited that I mentioned this work.

### Suggestions from Shih-Nan

Shih-Nan asked several great questions about my research. I am summarizing them below for future reference.

- How long is sufficient for model spin-up? Since the model appears to converge towards observations after the spring bloom, would it be adequate to spin-up the model starting at the spring bloom one year before our year of interest?
- When we model "natural" conditions, how do we ensure that we have allowed the model to spin-up for long enough that the "human-influence" has gone away.
  - *AL thought*: I think this same question applies to modeling the N-less WWTPs. We will need to let the model run for some time so that the DIN sourced from WWTPs has a chance to disappear.
- What is the mechanism for mixing in Hood Canal? Does wind cause lateral mixing effects? (re: Scully, 2013)

---
## Diving back into research

This week I addressed some open action items, and I have begun brainstorming more formal model evaluation criteria and brainstorming a formal modeling experiment set-up. More details below.

### Some loose ends

**Fraser nutrients**

As part of the TRAPS refactor and biogeochemistry improvement, I reached out to Susan Allen for a good estimate of Fraser River NH4 concentrations. She provided a value of 4.4 uM, which was a near 100x increase compared to the 0.072 uM concentration in Ecology's dataset.

We have been using this updated NH4 concentration in the latest model runs. However, I have not compared the Salish Sea Cast NO3 concentrations to Ecology's concentrations. Our concern was that Ecology's NO3 data may have had a higher NO3 concentration than Salish Sea Cast's, which would mean that LiveOcean's Fraser River now has too much DIN.

This week I finally got around to comparing NO3 in LiveOcean's Fraser and Salish Sea Cast's Fraser. Figure 1 shows a snapshot of NO3 concentrations in Salish Sea Cast's Fraser. Figure 2 shows NO3 concentrations in LiveOcean's Fraser.

While it is difficult to compare the exact values given the scale of the figures, in general LiveOcean and Salish Sea Cast appear to have roughly the same magnitude of NO3 in the Fraser. Both timeseries also tend to share the same seasonal profile.

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/b832bd8c-2544-4eb1-b4e7-c6616e26d0e4" width="300"/><br>Fig 1. Salish Sea Cast Fraser River NO3 profile (adapted from Olson et al., 2020).</p><br>

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/ab86bc9a-a529-48eb-a3bf-d81c4b2c4acd" width="450"/><br>Fig 2. LiveOcean Fraser River NO3 profile.</p><br>

**Rainfall**

Last week we talked about potential missing sources of freshwater in LiveOcean. In particular, we wondered whether Ecology's data included rainfall scaled by watershed.

I revisited Ecology's nutrient loading summary, and can confirm that they indeed scaled streamflow by both watershed area and average annual rainfall (Mohamedali et al., 2017).

Ecology also incorporated estimates of groundwater flow and groundwater nutrient loads in their river discharge data (Mohamedali et al., 2017).

### Thoughts on evaluation criteria

Last week Parker mentioned that he anticipates no model can have a RMSE of < 0.2 mg/L for DO. Is this okay? Do we actually need the RMSE to be < 0.2 mg/L to answer our research question, "How much does N from WWTP discharge impact DO in Puget Sound?"

Perhaps it is okay if the model deviates from reality by more than 0.2 mg/L. We are interested in the difference in DO between *two model runs*: One with N in WWTP discharge, and one without N. Therefore, it is the difference in DO concentration between these two model runs that must have an uncertainty < 0.2 mg/L.

How do we determine model uncertainty, if not by using the RMSE? If the model ran quickly and had some stochasticity, we could run our two scenarios 30+ times, then use a student's t-test to assess whether WWTP DIN caused a significant decrease in DO >0.2 mg/L. But LiveOcean runs slowly, and the model does not have any stochasticity.

Then, what can we do with only two model runs? Perhaps we can consider the locations of "impaired waters" in Puget Sound. We can sample points at 30+ different locations within impaired waters (being sure that the locations are far enough away from each other that their DO concentrations are independent). Then we could conduct a student's t-test to compare DO concentrations at these different locations in the two model runs. Maybe each location could be a separate basin, and what we compare is the average DO concentration within the basin (rather than DO concentration at a single point). We could even conduct this t-test at different temporal resolutions. For instance, we could compare DO differences at every month of the year. Unfortunately, this test would collapse our spatial understanding of DO.

Thinking about future tests still doesn't address my current dilemma of "what is an acceptable model evaluation criterion?" If we can't expect to have DO RMSE < 0.2 mg/L, then perhaps we can instead evaluate the the likely drivers of hypoxia, such as:

- Stratification
- Nutrient concentrations
  - One TO-DO of mine: compare DIN initial conditions to observations
- Circulation and mixing
  - currents
  - wind

### Thoughts on formal experiment

Once the model evaluation is complete, we can finally move on to a formal experiment. To me, it makes sense to repeat the same experiment we ran for GRC (except using the updated version of LiveOcean):

- Baseline: No N being discharged from WWTPs (but still discharging the same amount of freshwater)
- Test condition: WWTPs discharge N (existing conditions)

With this test, we can analyze the influence of N sourced from WWTPs on DO.

Of course, Shih-Nan's questions about model spin-up are quite relevant. How early should we start the model to allow adequate spin-up time? Six months? One year?

Some other questions worth considering:
- What year does it make sense to run the model? Khangaonkar et al. (2018) ran the SSM for the year 2014. Should we also run in 2014? Would a different year be better?
- Should we conduct experiments in the future that forecast the difference in DO caused by WWTP N, assuming a population increase over the next decade or so? One reason why we may consider to do this is because it will take many years to upgrade WWTPs.

Another potential pathway that addresses the second bullet point above, is to conduct a "sensitivity study" type of experiment. Instead of two test conditions, we could run three:

- Baseline: No N being discharged from WWTPs
- Test condition 1: WWTPs discharge N at existing levels
- Test condition 2: WWTPs discharge N at 2x existing levels (or whatever factor we deem reasonable)

Conducting an experiment like this would help us understand how existing levels of N-loading from WWTPs influence hypoxia, and it would also help answer how future loads may influence hypoxia.

---
## Winter course registration

Registration for Winter quarter opens on Friday. I need 12 more credits to satisfy the CEE PhD course requirement. During the Winter, I would like to take one course. Some potential candidates are listed below:

- **CEWA 582 (3): Wastewater Reuse and Resource Recovery**
  - This is a course on WWTPs that Mike recommended to me last year.
  - *Description:* "Designed to provide students with fundamentals of wastewater and biosolids treatment. Focuses on technologies utilized to recover resources from waste products. Covers experience with real-world applications both in the United States and abroad. Topics include: bioplastic and biogas production, recovery of water, fertilizer, sulfur, and metals"
- **CEWA 577 (4): Open Channel Flow with Modeling**
  - *Description:* "Water flow in natural and engineered channels, rivers, and streams. Analysis and design of channels (lined, vegetated), flow controls (weirs, spillways), and structures affecting fish passage (culverts). Prediction of water surface profiles. Introduction to river mechanics. Design-oriented problems. Introduction to the HEC-RAS modeling software"
- **AMATH 582 (5): Computational Methods for Data Analysis**
  - *Description:* "Exploratory and objective data analysis methods applied to the physical, engineering, and biological sciences. Brief review of statistical methods and their computational implementation for studying time series analysis, spectral analysis, filtering methods, principal component analysis, orthogonal mode decomposition, and image processing and compression"
- **OCEAN 531 (3): Marine Phytoplankton and Biogeochemistry**
  - *Description:* "Covers phytoplankton in the marine environment: evolution, ecology, primary productivity, and physiology, emphasizing their role in the global carbon cycle; spatial and temporal distributions of phytoplankton and how these patterns may change as ocean conditions change; and methods for determining distributions and rates in different ocean ecosystems."
- **OCEAN 535 (3): Biological Oceanography**
  - *Description:* "Examines major patterns and processes in upper ocean pelagic ecosystems, emphasizing quantitative analysis of mechanisms controlling production and abundances of organisms, from plankton to fish. Introduces interdisciplinary study of effects of anthrogenically induced changes in climate and ocean chemistry on organisms, ecosystem processes, and biogeochemical cycles."

Note that during Spring quarter I plan to take Christie's numerical modeling class (CEWA 572).