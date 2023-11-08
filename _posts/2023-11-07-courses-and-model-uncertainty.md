## PhD Courses and Model Uncertainty

After our last discussion, I spent this past week compiling a list of courses I would like to take during my PhD. I also made a few more model evaluation figures and began exploring model uncertainty literature. More details below.

---
## Courses

Like you recommended last week, I have put together a list of all courses that I am interested in taking over the course of my PhD. Originally, this list was much longer. However, I shorted it in consideration of time away during the Valle exchange program in Scandinavia.

The following courses are ones that I am most eager to take.

<details><summary><strong>OCEAN 587: Fundamentals of Climate Change</strong></summary>
(3) Autumn
<br>
<i>Description:</i> "Examines Earth's climate system; distribution of temperature, precipitation, wind ice, salinity, and ocean currents; fundamental processes determining Earth's climate; energy and constituent transport mechanisms; climate sensitivity; natural climate variability on interannual to decadal time scales; global climate models; predicting future climate."
</details>

<details><summary><strong>CEWA 572: Numerical Modeling of Hydrodynamics</strong></summary>
(3) Spring
<br>
<i>Description:</i> "Develops finite-difference, finite-volume and spectral numerical methods to solve ordinary and partial differential equations relevant to hydrodynamics and hydrology, and ocean sciences. The course framework will provide insight into working in a UNIX system, writing numerical models, hands-on training with numerical models."
</details>

<details><summary><strong>Engage Seminar: The Science Speaker Series</strong></summary>
(3) Winter -- application due in Fall
<br>
<i>Description:</i> "The Engage graduate-level seminar course at the University of Washington teaches emerging scientists to effectively communicate through the development of a seminar on their own research for a general audience. The course themes include storytelling, audience consideration, and public speaking while incorporating lessons and tools from a variety of sources: improvisational games, group discussion and feedback, and, most importantly, practice. After completion of the course, students give their presentations at a public venue (currently hosted by Town Hall Seattle as the “UW Science Now” series.)"
</details>


<details><summary><strong>CSE 412: Introduction to Data Visualization</strong></summary>
(4) Winter
<br>
<i>Description:</i> "Introduction to data visualization design and use for both data exploration and explanation. Methods for creating effective visualizations using principles from graphic design, psychology, and statistics. Topics include data models, visual encoding methods, data preparation, exploratory analysis, uncertainty, cartography, interaction techniques, visual perception, and evaluation methods."
</details>
<br>

The courses below are "if time permits" options.

<details><summary><strong>CEWA 574: Hydraulics of Sediment Transport or OCEAN 541: Marine Sedimentary Processes</strong></summary>
(4 or 3) Spring or ???
<br>
<i>Description for CEWA 574:</i> "CEWA 574: Introduction to sediment transport in steady flows with emphasis on physical principles and their extension to modeling of sediment transport. Topics include sediment characteristics, initiation of particle motion, particle suspension, bedforms, sediment discharge formulae, and modeling of scour and deposition in rivers and channels. HECRAS modeling of transport in channels."
<br>
<i>Description for OCEAN 541:</i> "OCEAN 541: Investigates fundamental process of marine sedimentation, including equations characterizing boundary-shear flows, initiation of grain motion, bedload and suspended-load transport, and sediment accumulation. Applies concepts to sediment dispersal in rivers, deltas, estuaries, beaches, continental shelves, slopes, and rises, with emphasis on the relationships between active processes and resulting deposits."
</details>

<details><summary><strong>OCEAN 535: Biological Oceanography</strong></summary>
(3) Winter
<br>
<i>Description:</i> "Examines major patterns and processes in upper ocean pelagic ecosystems, emphasizing quantitative analysis of mechanisms controlling production and abundances of organisms, from plankton to fish. Introduces interdisciplinary study of effects of anthrogenically induced changes in climate and ocean chemistry on organisms, ecosystem processes, and biogeochemical cycles."
</details>

<details><summary><strong>OCEAN 513: GFD II</strong></summary>
(3) Spring
<br>
<i>Description:</i> "Theories, models of large-scale dynamics of oceans, atmospheres. Potential vorticity, Q principles; Rossby waves, ray tracing, Green's function, setup of general circulation; atmospheric "channels" versus ocean "basins"; wave-mean flow interaction, mountain drag, internal momentum flux; "Lagrangian" motion of particles, tracers; cascades, eddy flux of heat, moisture, Q."
</details>
<br>

I also conducted an automated degree audit through MyUW. It looks like I need to take a seminar course to fulfill my PhD course requirement. Alex, does this sound familiar? If so, I can register for the CEE or PO seminar during any quarter.

---
## Ocean Sciences Update

Yesterday I also received an updated from the Ocean Sciences comittee. My abstract was accepted (yay), and I was selected to give an oral presentation (yikes).

I am the fifth speaker in the session "CP43A - Buoyancy-Driven Flows in Estuaries and Continental Shelves III Oral."

By the time of the conference, I really hope to have quantitative results to share.

---
## Model DIN ICs

We previously noted that modeled DIN tended to overestimate observed DIN (Fig 1). Back then, we wondered whether the issue was due to ICs, due to internal biogeochemical dynamics, or due to a combination of both.

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/6ac0fbed-be24-47ef-9345-9f9c2c75f2f3" width="800"/><br>Fig 1. Slide from NTU presentation showing model overestimate of DIN compared to observations.</p><br>

This week I created some monthly property-property plots of DIN (Figs 2 and 3). Note that observations are more sparse during the winter, and more abundant during the summer. Nonetheless, it appears as though modeled initial conditions are not that far off from observed DIN. Rather, the model overestimation seems to begin when the bloom dynamics take effect in April. These results suggest that the model's internal biogeochemistry rates are having problems reproducing DIN concentrations.

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/b46e9d08-beb5-4c13-b06d-f1e2d20a8c92" width="800"/><br>Fig 2. DIN property-property plot in shallow water (z > -30 m).</p><br>

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/f2e05a4b-aee1-4522-ae6d-bbd0f4e390e0" width="800"/><br>Fig 3. DIN property-property plot in deep water (z < -30 m).</p><br>

What is strange is that the modeled DO in Hood Canal had the opposite performance trends. In particular, model ICs tended to overestimate observed bottom DO, but modeled DO converged towards observed values once bloom dynamics took effect in May (Fig 4).

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/4184e14c-57ff-4ee1-ac2a-9f2f278e5aa8" width="800"/>Fig 4. Slide from NTU presentation showing DO water column profiles.
</p><br>

Therefore, even though the model is having trouble with DIN, it is still doing decently with oxygen.

---
## Density profiles

Last week Parker shared the observed average seasonal density profiles at ORCA buoy station locations.

This week I added to this figure the *modeled* seasonal density profile at ORCA station locations (Fig 5). Note that the model profiles only include results from 2017, whereas the observations are averaged over many years.

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/6dbf9fdf-c976-438d-8d58-4939e4242e18" width="800"/>Fig 5. Comparison of model and observation seasonal density profiles at ORCA buoy locations.
</p><br>

Note that this figure is preliminary for now, as I need to double-check my conversion of practical salinity to absolute salinity, my conversion of potential temperature to conservative temperature, and my final calculation of the density anomaly. I will share another update next week.

---
## Model evaluation and uncertainty

This week I also explored some literature on quantifying model uncertainty in hydrodynamic-biogeochemical models.

**Allen et al., 2007**

First, I took a look "Quantifying uncertainty in high-resolution coupled hydrodynamic-ecosystem models" from Allen et al. (2007). They emphasize the lack of, and thus the need for, exploring and quantifying the uncertainty in increasingly complex models. In their study, they looked at three key statistics:

- Nash-Sutcliffe Model Efficiency (ratio of model error to variability in observations)
- Model Bias (model error normalized by data)
- Root Mean Square Error

Allen et al. (2007) also provided performance criteria for these statisitics (e.g. model efficiency > 0.65 is "Excellent"). Note that model bias and root mean square error are statistics that we have already been looking at for LiveOcean!

Another statistical tool that Allen et al. (2007) proposed was the use of a self-organizing map (SOM). To the best of my understanding, an SOM is a technique that organizes model/observations into self-consistent regions. In simpler terms, geographic regions with similar statistics, spatio-temporally, get grouped into the same "class." Then, bulk statistics, such as mean and standard deviation, can be calculated for each of these classes. They calculated more complicated statistics as well, but I would need to read the paper a few more times before I could understand it.

While the concept of an SOM is intriguing, SOMs are likely too complex for the scope of our project. They are unsupervised neural networks, which is currently far outside of my coding/CS capabilities.

However, the performance criteria for model efficiency, bias, and root mean square error are promising.

**Kessouri et al., 2021**

How did I find the Allen et al., (2007) paper to begin with? I first saw a citation in Kessouri et al., (2021), which is a paper that Minna Ho shared with me at the start of summer. As a reminder, Minna is a graduate student in Jim McWilliams' group using the UCLA ROMS-BEC model to study the effects of wastewater discharge in the Southern California Bight.

The team of Kessouri et al., including Minna, published three related papers at around the same time. The first explains the methodology used to integrate wastewater outfalls into the model and also discusses the model evaluation results (Kessouri et al., 2021a). The second dives into the sources of nutrients and freshwater entering the Southern California Bight (Sutula et al., 2021). The third paper provides the results of modeling experiments that investigate the impact of wastewater nutrients on oxygen and acidification in the region (Kessouri et al., 2021b).

The evaluation methods and performance criteria used by Kessouri et al. (2021a) were based on Allen et al. (2007). However, Kessouri et al. (2021a) did not use the SOM neural network magic. Instead, they leaned more heavily on performance criteria such as model efficiency (where > 0.65 is "Excellent").

Kessouri et al. (2021a) used the Nash-Sutcliffe model efficiency as well as model bias, but not root mean square error. They also introduced other performance criteria (along with ranges for "Excellence"):

- Pearson Correlation Coefficient (linear correlation between model and observations + p-value)
- Cost Function (quantifies difference between model & observations with "goodness of fit")
- Ratio of Standard Deviations (between observations and model)
- Two-sample t-test or Welch's t-test

**My thoughts so far**

Model uncertainty is a huge issue to consider when we try to investigate the impact of WWTP N on DO. This is especially true since the regional DO criterion is a human-induced difference of a mere < 0.2 mg/L, and also because the amount of N from WWTPs is such a small fraction of the total DIN going into the Salish Sea. For these reasons, I think it is really worth being meticulous and thoughtful when conducting model evaluation, assessing model uncertainty, and even running experiments to test different model spin-up schemes.

I'm also now thinking in terms of timelines, in particular for my first main body of work. Before, I wanted to focus on a modeling experiment to compare the effects of WWTP N on DO in Puget Sound. But maybe that experiment (or sensitivity study, as we discussed last week) should wait. Perhaps the first important piece of work should focus on the methodology of model evaluation, on quantifying model uncertainty, and on spin-up experiments, and later work will focus on a formal experiment. (I'm influenced by Kessouri et al.,  who published a suite of three papers that thoroughly goes through their experimental set-up, evaluation, and results)

Does this level of detail seem appropriate?

I also plan to explore more model evaluation literature this week.

Another thought: I'm thinking that we may need to conduct spin-up experiments before formally quantifying uncertainty model uncertainty. I found some ostensibly relevant literature assessing the variability between different models in the CMIP5 attributed to different spin-up procedures (Séférian et al, 2016). I'll look into this more in the upcoming week as well. Parker and Jilian also shared some literature related to residence times and adjustment times that could be relevant to spin-up timescales.

---
## References

Allen, J. I., Somerfield, P. J., & Gilbert, F. J. (2007). Quantifying uncertainty in high-resolution coupled hydrodynamic-ecosystem models. *Journal of Marine Systems*, 64(1–4), 3–14.

Kessouri, F., McLaughlin, K., Sutula, M., Bianchi, D., Ho, M., McWilliams, J. C., Renault, L., Molemaker, J., Deutsch, C., & Leinweber, A. (2021a). Configuration and Validation of an Oceanic Physical and Biogeochemical Model to Investigate Coastal Eutrophication in the Southern California Bight. *Journal of Advances in Modeling Earth Systems*, 13(12).

Kessouri, F., McWilliams, J. C., Bianchi, D., Sutula, M., Renault, L., Deutsch, C., Feely, R. A., McLaughlin, K., Ho, M., Howard, E. M., Bednaršek, N., Damien, P., Molemaker, J., & Weisberg, S. B. (2021b). Coastal eutrophication drives acidification, oxygen loss, and ecosystem change in a major oceanic upwelling system. *Proceedings of the National Academy of Sciences*, 118(21).

Séférian, R., Gehlen, M., Bopp, L., Resplandy, L., Orr, J. C., Marti, O., ... & Romanou, A. (2016). Inconsistent strategies to spin up models in CMIP5: Implications for ocean biogeochemical model performance assessment. *Geoscientific Model Development*, 9(5), 1827-1851.

Sutula, M., Ho, M., Sengupta, A., Kessouri, F., McLaughlin, K., McCune, K., & Bianchi, D. (2021). A baseline of terrestrial freshwater and nitrogen fluxes to the Southern California Bight, *USA. Marine Pollution Bulletin*, 170.