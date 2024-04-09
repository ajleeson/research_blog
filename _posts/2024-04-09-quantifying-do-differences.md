## Quantifying DO differences

Since last week, I have shifted my focus towards quantifying DO differences between the two model runs. First, I re-created figures from Khangaonkar et al. (2018), but using our LiveOcean results. Then, I developed an additional figure to tease apart the change in DO in the anthropogenic run relative to the starting DO in the natural run. Finally, I reached out to a colleague for more inspiration. 

This week I have also finally updated driver_roms3 to use forcing files in Parker's account (so I don't need to re-generate forcing, which can take over 24 hours). 

More details below.

---
## Re-creating figures in Khangaonkar et al. (2018)

Following Alex's advice, I re-visisted Khangaonkar et al. (2018) to kick-off our efforts to quantify DO differences.

### Original figures from Khangaonkar et al. (2018)

Figure 1 shows the main comparison of hypoxic differences in Khangaonkar et al. (2018). The top panel shows bottom hypoxic area timeseries (DO < 2 mg/L) in Puget Sound, and the bottom panels show the number of days in which bottom DO is < 2mg/L. 

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/ee181d77-8522-4b95-8258-0c478882a1ed" width="450"/><br>Fig 1. Figure 19 from Khangaonkar et al., (2018). Puget Sound bottom hypoxic area timeseries, and maps showing days in which bottom DO < 2mg/L.</p><br>

### Figures from our model runs

First, I tried to re-create similar figures using our LiveOcean results. Note that Khangaonkar et al. simulated 2014, while we simulated 2013, so we can't yet make a direct comparison between the two models.

Figures 2 and 3 show LiveOcean results. Figure 2-a shows the numbers of days in which bottom DO < 2 mg/L in the natural run, while Figure 2-b shows how many more days the anthropgenic run had bottom DO < 2mg/L relative to the natural run. Figure 3 shows timeseries of bottom area with DO < 2mg/L.

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/48746906-f8d2-450e-a211-9ebf3eefd2b9" width="550"/><br>Fig 2. (a) Puget Sound map of natural run showing days with bottom DO < 2 mg/L. (b) Anthropogenic minus natural run. Blue indicates that the anthropogenic run had more days with bottom DO < 2 mg/L.</p><br>

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/e905a7bd-de9e-4663-9986-47dcea5c3263" width="550"/><br>Fig 3. Puget Sound bottom hypoxic area timeseries (DO < 2 mg/L).</p><br>

In general, our results suggest that WWTP loading slightly increase the occurence of bottom hypoxia, but not by much. Of course, these analyses include the Hood Canal region which we alreay know has not adjusted to changes in WWTP loading, so I am eager to see what results for 2014 will look like.

---
## Thoughts for additional analysis

Although these figures are a decent first look at differences in hypoxia between our two test conditions, the figures are limited in the information that they can convey. 

Figure 4 illustrates these limitations. These maps are similar to the ones shown in Figure 2, except using a 4 mg/L and 5 mg/L threshold instead.

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/b32cc55b-bdf8-497a-a52b-a8e1e33d4820" width="900"/><br>Fig 4. Puget Sound maps with days in which bottom DO < 4 mg/L (left), and < 5 mg/L (right).</p><br>

In the 4 mg/L threshold map, it looks like the anthropgenic run has many more days with DO < 4 mg/L east of Vashon/Maury Island (circled). But using a 5 mg/L threshold, that dark blue spot is almost gone. This implies that DO was already < 5 mg/L in this region, but not < 4 mg/L. Perhaps WWTP loading only changed DO by a small amount, but we do not see the magnitude of this change. All we see is whether bottom DO was above/below an arbitrary threshold. So this type of map (with a boolean value based on some cutoff), can sometimes exaggerate how much the WWTP influences DO. It would be better to come up with an idea for also visualizing the magnitude of the change caused by WWTPs (because maybe DO is only changing by 0.1 mg/L, but that change is enough to push it over the threshold in some locations).

---
## Delta change vs. Natural bottom DO

To address the limitation discussed above, I have also generated a 2D histogram plotting the change in bottom DO (anthropogenic minus natural) vs. the original bottom DO concentration in the natural run (Fig 5). Note that points shown on this 2D histogram come from every single grid point in Puget Sound (see Fig 6.) on every single day of the year. This amounts to a lot of points, and we are unable to resolve spatial and temporal variability in DO. However, we can now visualize how much WWTP loading is causing DO to change relative to where DO concentrations naturally start. In general, changes are < 0.2 mg/L, and the majority of the changes occur for water above 3 mg/L. 

The area under "slope = -1"  is omitted because it is impossible for the anthropogenic run to have a decrease in DO that is larger than the DO concentration of the natural run (i.e. no negative DO concentrations). This line thus represents the maximum possible change that the WWTP loading could make to bottom DO concentrations.

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/fbd302e9-ff29-448e-ad88-0fa9e62f3e41" width="500"/><br>Fig 5. 2D histogram of change in bottom DO (anthropogenic minus natural) vs. natural bottom DO. The total points on this figure correspond to points from every single grid point on every single day of the year (i.e. number of gridpoints in Puget Sound times 365 days = number of points in this histogram plot). Colorbar indicates the number of points that occupies a location on this figure, with counts reported in log scale. 100 x 100 bins.</p><br>

The left panel in Figure 6 shows the grid cells included in the analysis above. I selected a 6 mg/L cutoff because nearly all bottom water in Puget Sound has days with < 6 mg/L (encompasses all area), and we are not concerned with higher DO concentrations. Note that I have omitted points just outside of Puget Sound to limit bias from bottom water in the Straits.

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/da776886-3a9b-439b-9c49-46178d27feaf" width="500"/><br>Fig 6. Puget Sound maps with days in which bottom DO < 6 mg/L, with regions outside of Puget Sound cropped out.</p><br>

For completion, Figure 7 shows a timeseries of the bottom water area with DO < 6 mg/L, corresponding to the region shown in Figure 6.

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/7b825102-2a8f-4609-9bf9-36da8e648f35" width="500"/><br>Fig 7. Puget Sound timeseries or area with DO < 6 mg/L. Area corresponds to colored region in Figure 5.</p><br>

---
## Next steps

I am open to feedback for next steps, otherwise my plans for the upcoming weeks are as follows:

- **This week**
    - Review papers from Kyle Hinson
        - Last week I reached out to Kyle Hinson, a postdoc at PNNL who worked on modeling Chesapeake Bay hypoxia for his PhD. We met at Ocean Sciences, and he told me a bit about his past work. After contactig him again, he shared his published work with me which nicely summarizes many of the modeling experiments on Chesapeake Bay hypoxia. I'm planning to review his paper and these reference papers for more inspiration on DO analysis/quantification. Hopefully this reading will also help me continue to build an understanding of the mechanisms driving hypoxia in the Cheapeake.
    - Compare SUNA data to Ecology data
        - Parker previously shared some SUNA datasets with me, which contain nutrient measurements. This week I'm planning to compare these measurements to Ecology data to see if we can reliably use SUNA data for model evaluation in the future.
    - Generate TRAPS forcing for 2014 (be prepared to run when Parker gets back and resources become available on hyak)
- **Upcoming weeks**
    - Identify statistics that can summarize DO differences in a neat number (or numbers)
    - Return back to mechanisms
        - where do nutrients go
        - timing and influence of deep water intrusions
        - explain the early spring bloom
        - and more...

---
## References

Khangaonkar, T., Nugraha, A., Xu, W., Long, W., Bianucci, L., Ahmed, A., ... & Pelletier, G. (2018). Analysis of hypoxia and sensitivity to nutrient pollution in Salish Sea. *Journal of Geophysical Research: Oceans*, 123(7), 4735-4761.



