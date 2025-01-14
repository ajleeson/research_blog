## Boxplots, reproducibility, and more!

Based on suggestions from our last meeting, I edited boxplots of net DO decrease rate. 

I have also been working on refactoring code. So far, I have finished cleaning up all of the scripts related to the DO budgets. 

I also revisitied bit-reproducibility issues, as Jilian recommended. It seems that LiveOcean has issues with bit-reproducibility in the biogeochemistry fields, but not in the hydrodynamic fields.

More details below.

---
## Net decrease boxplots

Last week, I shared boxplots of the net decrease rate of all thirteen deep (> 10 m) terminal inlets. I mentioned that based on Welch's ANOVA analysis, all inlets have the same mean decrease rate *except* for Dabob Bay.

Figure 1 is the updated boxplot. The inlets are now organized from shallowest to deepest. I have also added a line representing the mean net decrease rate of all inlets, which makes it graphically clear that all inlets lose oxygen at the same rate except for Dabob Bay. I have added this updated figure to my manuscript draft.

<p style="text-align:center;"><img src="https://github.com/user-attachments/assets/7b310a08-f79a-49b2-a74b-7ee572d4455d" width="700"/><br>Fig 1. Boxplots of mid-Jul through mid-Aug net DO decrease rate of all 13 deep inlets. Data are daily d/dt(DO) rates. Each box contains the middle 50% of all data. The navy dot is the mean of an individual inlet. The blue line is the mean of all inlets. Inlets are ordered from shallowest to deepest.</p><br>

---
## Code refactor, and updated drawdown period?

This week, I also finished refactoring all code related to the DO budgets. I still need to update code related to hypoxic volume and Puget Sound DO maps. However, the budget code needed the largest overhaul, so I feel accomplished. 

As I reviewed my plotting code, I had a thought to extend the drawdown period. The current drawdown period extends from mid-July through mid-August (Figure 2b). I chose this range based on my original 21 terminal inlets (some of the oxygenated inlets did not start their seasonal decline in DO until mid-July). However, I would like to extend this drawdown period from **mid-June** through mid-August instead, because a two-month drawdown period seems appropriate for the remaining thirteen inlets.

<p style="text-align:center;"><img src="https://github.com/user-attachments/assets/b7641d1f-4f76-4d51-90c6-1f693347a52c" width="700"/><br>Fig 2. (a) 2017 monthly mean percent hypoxic volume vs. monthly mean deep layer DO (DOdeep) of all thirteen deep ( > 10 m) terminal inlets. Percent hypoxic volume is the percentage of the inlet that is hypoxic relative to its total volume. (b) 2017 daily time series of mean deep layer DO in all thirteen deep ( > 10 m) terminal inlets, with a 30-day Hanning Window applied. The period between the two vertical lines indicates the mid-July through mid-August oxygen drawdown period in which DOdeep is decreasing in all inlets.</p><br>

Because I just finished refactoring my budget code, this change will be *very easy* to implement. I already tested the new two-month drawdown period to see how it would influence results. Although some values change here and there, all of the conclusions of the statistical analyses remain the same. 

---
## Biogeochemistry bit-reproducibility

Lastly, I have been helping Jilian chase down an issue with bit-reproducibility in the biogeochemistry fields of the model.

For this analysis, I looked at my old loading and no-loading experimental runs. Previously, I had only verified that the hydrodynamics were bit-reproducibile in Puget Sound. However, we are finding that the biogeochemistry fields have some noise on the coast.

All figures below are snapshots taken two weeks after model start time.

Figure 3 shows the difference in u between the loading and no-loading runs. It seems that the hydrodynamics are bit-reproducible across the entire domain. I also verified that temperature and salinity are bit-reproducible as well (not shown).

However, we see some noise in biogeochemistry fields such as DIN (not shown) and oxygen (Figure 4). Note that I expect to see large differences inside of Puget Sound because one run has WWTP loading and the other does not. What is surprising is the noise on the coast.

I am happy to collaborate and help debug this issue in the coming weeks.

<p style="text-align:center;"><img src="https://github.com/user-attachments/assets/8190b05d-f626-44d8-8df8-92846bc2c9c6" width="700"/><br>Fig 3. Bit-reproducibility of u in surface and deep layer of LiveOcean after 2 weeks of run time.</p><br>

<p style="text-align:center;"><img src="https://github.com/user-attachments/assets/49401c74-8ad8-4964-bb72-0d47d076e1ba" width="700"/><br>Fig 4. Bit-reproducibility of oxygen in surface and deep layer of LiveOcean after 2 weeks of run time.</p><br>