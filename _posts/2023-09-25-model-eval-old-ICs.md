## Early look at model evaluation with old ICs

This week I focused on three main tasks:
1. Preliminary model evaluation
2. Working on slides for EFM presentation
3. Reading and processing Scully (2013)

More details below.

---
## Model evaluation

At the time of writing, the new model evlaluation run has made it into early August. So far, the new model evaluation run appears to have comparable or better skill versus the GRC run. Bottom DO predictions in South Sound and Main Basin seem to have improved significantly. However, the model is still overpredicting bottom DO in the far reaches of Hood Canal.

In the coming week, I am interested in further exploring the spatial distribution of skill. Are there certain regions that the model is consistently performing worse in, and why?

### Orca Buoy Bottom DO

Figure 1 shows a comparison of bottom DO versus our three model runs and ORCA buoy data. The three model runs are:

1. GRC run (WWTPs as tiny rivers, old biogeochem, old ICs)
2. Prior run (functioning TRAPS, new biogeochem, new ICs)
3. New run (functioning TRAPS, new biogeochem, old ICs)

For the remainder of this blog post I will refer to these different runs as "GRC run," "Prior run," and "New run."

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/8bf31956-4f4e-4327-8246-65d6bb59ed7b" width="800"/><br>Fig 1. Model-model-ORCA buoy comparison of bottom DO.</p><br>

Compared to the GRC run and prior run, the new model run appears to improve bottom DO predictions in Main Basin and South Sound-- specifically at Carr Inlet and Point Wells. However, the model is still having trouble reproducing the hypoxic conditions in Hood Canal at Hoodsport and Twanoh stations. In fact, at these stations, bottom DO of the new run is very similar to that of the GRC run. It seems as though the updated biogeochemistry parameters improved DO predictions in South Sound and Main Basin, but did not influence Hood Canal predictions by much.

Closer to the mouth of Hood Canal, at Dabob Bay and North Buoy, the new model results predict lower DO concentrations than the prior GRC results. At North Buoy, the new run and prior run have quite similar early summer DO predictions. Unfortunately, there are no ORCA observations at these stations for comparison.

Overall, it seems that Hood Canal is still being tricky.

### Comparison to Bottle Data

Next, I compared the three model runs to all available bottle data in the region. These comparisons were made using scripts in Parker's obsmod repo, which I have updated to use the self-imposed <span style="color:#399299">**blue**</span>-<span style="color:#A2D426">**green**</span>-<span style="color:#9778CE">**purple**</span> color theme of my PhD.

**Full Grid**

Figures 2 and 3 show the property-property plots of the full grid for both shallow and deep depths, respectively.

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/8fe840c7-61e6-4a09-a4b0-e5de15a16d59" width="800"/><br>Fig 2. Model-model-data comparison over full grid (shallow). New run (TRAPS+old ICs) compared to prior run (TRAPS+new ICs) compared to GRC run compared to shallow bottle data.</p><br>

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/f6fc1e5b-7b33-4ee7-aa90-ed6c095033d1" width="800"/><br>Fig 3. Model-model-data comparison over full grid (deep). New run (TRAPS+old ICs) compared to prior run (TRAPS+new ICs) compared to GRC run compared to deep bottle data.</p><br>

**Puget Sound**

Figures 4 and 5 show the property-property plots of Puget Sound for both shallow and deep depths, respectively.

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/8ac44d1c-6ea2-432c-9396-86ef77f30de5" width="800"/><br>Fig 4. Model-model-data comparison over Puget Sound (shallow). New run (TRAPS+old ICs) compared to prior run (TRAPS+new ICs) compared to GRC run compared to shallow bottle data.</p><br>

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/78a2b236-1f79-4d85-8c38-7fef8198d5b9" width="800"/><br>Fig 5. Model-model-data comparison over Puget Sound (deep). New run (TRAPS+old ICs) compared to prior run (TRAPS+new ICs) compared to GRC run compared to deep bottle data.</p><br>

**Hood Canal**

Figures 6 and 7 show the property-property plots of Hood Canal for both shallow and deep depths, respectively.

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/3c367d45-1a54-4990-abd2-a2324b8c24cf" width="800"/><br>Fig 6. Model-model-data comparison over Hood Canal (shallow). New run (TRAPS+old ICs) compared to prior run (TRAPS+new ICs) compared to GRC run compared to shallow bottle data.</p><br>

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/1ed0bafc-fe97-49eb-8b6e-d32978ffaade" width="800"/><br>Fig 7. Model-model-data comparison over Hood Canal (deep). New run (TRAPS+old ICs) compared to prior run (TRAPS+new ICs) compared to GRC run compared to deep bottle data.</p><br>

**Strait of Georgia**

Figures 8 and 9 show the property-property plots of Hood Canal for both shallow and deep depths, respectively.

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/040db1ee-4145-4a2b-8e6d-10b1b9b99849" width="800"/><br>Fig 8. Model-model-data comparison over Strait of Georgia (shallow). New run (TRAPS+old ICs) compared to prior run (TRAPS+new ICs) compared to GRC run compared to shallow bottle data.</p><br>

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/9caae746-42b8-4f2a-a2a6-653358a89650" width="800"/><br>Fig 9. Model-model-data comparison over Strait of Georgia (deep). New run (TRAPS+old ICs) compared to prior run (TRAPS+new ICs) compared to GRC run compared to deep bottle data.</p><br>

**Comments**

In general, the new run and the GRC run have mostly similar performance with the new run have slightly better or slightly worse skill for different state variables. The exceptions are **DO** and **total alkalinity**, which appear to be significantly improved in the new model run.

What I'd like to do next is to make similar plots to these, but instead of comparing different model runs to each other, I'd like to compare the performance of different regions in the new run. For example, how does the model perform in Hood Canal vs. Main Basin? My curiousity is driven by the results shown in the ORCA buoy comparison (Fig 1.)

---
## Next steps and EFM presentation

In my mind, my next research steps are directly intertwined with the EFM presentation. During the presentation next week, I would love to share some preliminary results with the group. However, I first need to have some preliminary results. In the next week, what are some of the most efficient uses of my time that could produce good figures? I've already provided one of my ideas above: to evaluate the model skill based on region.

I'm also thinking further ahead to the NTU PO seminar on October 25th. It would be great to set some milestones for this date as well. Perhaps, after the current evaluation run finishes, I could start a new run without WWTP DIN and make a similar "money plot" that I did for GRC. However, does this preliminary experiment depend on the current model run having "good enough" skill? What is "good enough" skill?
