## More on thoughts d/dt(DO)

This week I spent a little bit more time thinking about d/dt(DO), but have not made any breakthrough conclusions. Some of my thoughts are below.

---
## Spurious Correlation

Last week, Alex brought up a good point about spurious correlation. I investigated further this week, comparing Consumption/Vinlet vs. Photosynthesis/Vinlet (Figure 1a) to Random Noise/Vinlet vs. Photosynthesis/Vinlet (Figure 1b).

Previously, I thought that consumption rate was highly correlated to photosynthesis rate. However, I now see that random noise also has moderate correlation to photosynthesis, likely because both terms are normalized by Vinlet. Thus, **we do see spurious correlation**. 

<p style="text-align:center;"><img src="https://github.com/user-attachments/assets/421b072d-18d4-420f-9d48-ab79f4cbe9a0" width="650"/><br>Fig 1. Investigating spurious correlation. Left panel (a): mean inlet consumption vs. photosynthesis during the drawdown period normalized by inlet volume. Right panel (b): random noise scaled to range of consumption vs. photosynthesis noramlized by inlet volume.</p><br>

Moving forwad, I will need to think of other ways to evaluate ther terms influencing d/dt(DO) in each inlet.

---
## d/dt(DO) time-series

Figure 2 shows a 2017 time-series of d/dt(DO) in all terminal inlets in Puget Sound. Although there is a lot of short-term variability, all inlets have similar d/dt(DO) profiles throughout the year. The inlets are all gaining oxygen during the fall and winter, and losing oxygen during the spring and summer. During the mid-June through mid-August drawdown period, the d/dt(DO) time-series is flat, relative to the timescales of seasonal variability. I find it interesting that the d/dt(DO) profiles are similar for all inlets throughout the year, and not just during the drawdown period.

Note that the large changes in d/dt(DO) during the Fall in Penn Cove and Crescent Bay coincide with spikes in river flow (not shown here, but happy to show a figure during our meeting).

<p style="text-align:center;"><img src="https://github.com/user-attachments/assets/22c07a30-3543-481b-933c-63da7f33d652" width="650"/><br>Fig 2. Time-series of d/dt(DO) in (a) the deep layer of all terminal inlets, and (b) the entire inlet. A 30-day Hanning window filter was applied to all data. The region between the vertical lines denotes the mid-June through mid-August drawdown period.</p><br>

I'm curious to explore Dakota's question from last week: does the whole of Puget Sound lose oxygen at the same rate as the terminal inlets? In other words, if I were to make a time-series of d/dt(DO) over Puget Sound, would it overlap with the time-series in Figure 2?

---
## Whidbey Basin model eval

To add on to Jilian's analysis, I compared the model performance in Port Susan to model performance in the rest of Whidbey Basin. Thank you Dakota for sharing the formatted KC Whidbey data! Note that these data are only for the years 2022 (Figure 3) and 2023 (Figure 4). The model version is the same as what I used for my thesis (i.e., with the old biogeochemistry module that tended to underestimate DO).

Although there is still negative DO bias, it seems that it is not as large as what we saw previously across Puget Sound during 2017.
Perhaps this is because 2022 and 2023 had higher DO concentrations, or perhaps the CTD casts simply did not go down to the depths where the model predicts hypoxia.

<p style="text-align:center;"><img src="https://github.com/user-attachments/assets/db4e5ca7-dbb7-4794-acb6-400dea065a8e" width="700"/><br>Fig 3. Model-data comparison in Whidbey Basin during 2022. Observations are King County CTD casts.</p><br>

<p style="text-align:center;"><img src="https://github.com/user-attachments/assets/8f7027f9-91d2-48fb-bf47-d6555032cd36" width="700"/><br>Fig 4. Model-data comparison in Whidbey Basin during 2023. Observations are King County CTD casts.</p><br>
