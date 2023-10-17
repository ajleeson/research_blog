##  In-depth Hood Canal evaluation

## Re-cap

When I last shared model evaluation results, I was focusing mostly on comparing model output to ORCA mooring observations. The corresponding bottom DO comparison plot is shown in Figure 1.

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/2ff1c349-a220-44e6-b678-c2eeeefdf7b0" width="800"/><br>Fig 1. Model-model-ORCA buoy comparison of bottom DO. "Current LiveOcean" is the cas6_x2b_traps2 model version being used in the current forecast. "Updated model" is the latest cas7_trapsV00_meV00 run.</p><br>

Model DO at Point Wells and Carr Inlet match the observations well. Performance at Twanoh is also decent, thought there is a significant chunk of observations missing in spring/early summer. Most significantly, we identified Hoodsport as a location to investigate further. At this location, the model predicts much higher bottom DO concentrations than observed in early spring/early summer.

This week, I explored model performance in Hood Canal in more depth. First, I compared model performance to CTD casts in Hood Canal. Then, I re-visited the ORCA data to investigate whether the data collected from Hoodsport seem anomalous in any way.

More details below.

---
## CTD comparison in Hood Canal

As a new model evaluation step, I began comparing model performance to CTD casts in Hood Canal. The figures below were created with code based on Parker's cast extraction and plot_cast scripts.

Figure 2 shows monthly DO profiles at 3 different Ecology monitoring stations in Hood Canal.

- HCB003: Near Hoodsport (ish)
- HCB004: Near Alderbrook
- HCB007: Near the head of Lynch Cove

Note that the Hoodsport ORCA mooring is located between HCB003 and HCB004, and the Twanoh ORCA mooring is located between HCB004 and HCB007.

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/b1b55903-5354-4774-b9f4-0c291e8bd40f" width="800"/><br>Fig 2. Model-model-CTD comparison of monthly DO profiles. "Current LiveOcean" is the cas6_x2b_traps2 model version being used in the current forecast. "Updated model" is the latest cas7_trapsV00_meV00 run.</p><br>

**Bottom DO comparison to ORCA measurements**

First, let's look at the top panel of Figure 2: Station HCB003 near Hoodsport. In July and August, observed bottom DO concentrations drop to ~60 uM (~2 mg/L). This bottom DO concentration is much higher than the 0 mg/L reported in the ORCA observations (Fig 1). Similar "not-high-but-not-zero" DO concentrations were also observed at the HCB004 and HCB007 stations near Alderbrook and Lynch Cove, respectively.

**Initial DO profile**

The current LiveOcean model and the updated model have similar profiles at the beginning of the year, which is what we would expect since both model runs are using the same initial conditions. At all three location, the model initial conditions appear to be overestimating DO throughout the water column. What is interesting is that the model's bottom DO concentration matches the ORCA buoy measurements quite well (Fig 1). Thus, there appears to be some disagreement between the ORCA measurements and the CTD casts.

While this is finding is scary, it is also a good lesson: observational data have uncertainty.

So, how can we determine what is more correct? One idea is to look at similar water column profiles, but using bottle data. The bottle data water column DO profile is shown in Figure 3 for station HCB003 (near Hoodsport).

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/0d6a9d0a-c16a-4599-a328-8adaaa569b94" width="600"/><br>Fig 3. Model-model-bottle comparison of monthly DO profiles near Hoodsport (HCB003). "Current LiveOcean" is the cas6_x2b_traps2 model version being used in the current forecast. "Updated model" is the latest cas7_trapsV00_meV00 run.</p><br>

Figure 3 suggests that the model initial conditions are indeed overestimating bottom DO. (Note that bottle data at HCB004 and HCB007 tell a similar story, I simply have not uploaded figures from those stations.) Again, this finding is in conflict with the ORCA buoy comparison that suggests the initial bottom DO is on target.

**General findings**

As mentioned above, both model runs appear to initially overestimate DO concentrations throughout the water column.

Around May, the updated model begins to deviate from the current model performance-- predicting lower DO concentrations. In the updated model, bottom DO decreases throughout most of the water column, but increases at the surface (coincident with what appears to be a spring bloom, as indicated by a large peak in the observed DO profile at the surface).

In general, it seems like the updated model is getting the max DO concentration correct, which indicates that the model is capturing DO oversaturation from photosynthesis. One quirk, however, is that the DO maximum is always at the surface in the model, but is generally sub-surface in the observations. The DO profiles in May at HCB004 are a good example of this quirk (Fig 2). Perhaps these results suggest that the model sinking rates deviate from reality.

Aside from this quirk, the updated model appears to do a decent job of predicting water column DO concentrations. I am especially impressed with performance at HCB003 from September - December. It seems as though once the Spring bloom dynamics begin, the updated model DO profile begins to converge towards the observed DO profile, despite overestimating initial concentrations.

At first glance, the updated model appears to perform worst at station HCB007 near the head of Lynch Cove. Though, it is worth keeping in mind that the depth at HCB007 is ~25 m, whereas the depth at HCB003 (Hoodsport) is > 150 m. Therefore, model performance at the head of Lynch Cove may truly be more inaccurate, or we are observing the same level of inaccuracy at both station locations and the inaccuracies are simply magnified at the shallow depths of Lynch Cove.

---
## Hoodsport ORCA data

We previously identified a large discrepancy between the observed  bottom DO from the ORCA moorings and the modeled bottom DO at Hoodsport (Fig 1). In September, the observed DO suddenly jumps up in concentration, and is then in agreement with the modeled concentrations. We had a few suspicions that the mooring may have temporarily had some issues, such as biofouling. I reached out to Erin for ORCA metadata. She shared several great resources, but mentioned that there isn't much metadata available.

Another proposed method of investigation was to overlay several years of bottom DO ORCA timeseries, to see whether the 2017 Hoodsport timeseries jumps out. Figure 4 shows the timeseries comparison at four ORCA buoy locations (buoy locations on map in Fig 1) for the years 2017-2021. Based on this figure, 2017 does not appear to be an anomalous year. In fact, all years appear to develop anoxia at Hoodsport during the summer, and 2017 happens to have higher DO for the remainder of the year.

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/5298df60-62e3-451b-b5bc-ab0245e42c52" width="650"/><br>Fig 4. Bottom DO timeseries comparison at 4 different ORCA buoy stations. The year 2017 is highlighted in pink because 2017 is the year we are using for model evaluation.</p><br>

Mike recently sent me some of the ORCA data that he downloaded, and I am struck by how different those DO timeseries look compared to mine. Figure 5 shows an 18 year DO climatology at the Hoodsport station.

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/291debef-5530-40bb-8088-bc769c235524" width="700"/><br>Fig 5. DO climatology at 80 m depth at Hoodsport, from ORCA data. (https://nvs.nanoos.org/Climatology?action=oiw:site:siteset1:ORCA_Hoodsport_site:plots:2:oxygen_conc)</p><br>

Figure 5 suggests that Hoodsport DO is generally much higher in concentration than the values seen in my figures. I did notice that the downloaded data reports DO concentrations at a depth of 80 m, rather than a depth of 120 m (which is what I had been plotting). Thus, I took a look at Hoodsport bottom DO at a depth of ~80 m (Fig 6).

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/53c126a4-ba3e-4937-9048-f749220cc208" width="350"/><br>Fig 6. ORCA DO concentration and Hoodsport at a depth of ~80 m.</p><br>

At 80 m, DO concentrations at Hoodsport are still anoxic during the summer, and 2017 still appears to have generally high DO concentrations than later years.

Given all of this investigative work, I have a few hypotheses:

1.  DO concentrations were significantly lower than average from 2017 - 2021 (note that Mike's downloaded data extends back to 2005)
2.  The data I am working with is compromised in some way
3.  These data do not come from the same source (i.e. one or both datasets are not from Hoodsport ORCA buoy)

---
## Takeaways

- Model initial conditions seem to overestimate DO in Hood Canal (based on CTD casts and bottle data)
- Model initial conditions match DO reported by ORCA moorings
- Modeled DO maximum tends to sit higher in the water column than the observed DO maximum
- The updated model DO profile converges towards values reported in CTD casts after the Spring bloom, despite overestimating DO initial conditions
- Updated model performance near Hoodsport looks better than performance at the head of Lynch Cove
- ORCA mooring data that I have processed are suspicous and the data don't appear to match other published sources of (supposedly) the same data
- Model evaluation is difficult and confusing!

As always, I'd love to get your feedback, suggestion, and guidance on this work and next steps.