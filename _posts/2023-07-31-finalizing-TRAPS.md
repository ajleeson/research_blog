## Finalizing TRAPS and starting model evaluation

This week I made several improvements to the TRAPS code. I also began conversations with Parker and Jilian about model evaluation efforts. More details below.

## 1. Improvements to TRAPS

Since we have finally debugged WWTPs, my next goal is to clean up and finalize TRAPS integration. This week I completed about half of the improvements on my to-do list. The following subsections describe this progress in more detail.

I'd like to spend one more week wrapping up TRAPS integration prior to setting up a LiveOcean+TRAPS evaluation run.

### 1.1. Upgraded to Ecology's latest loading files

Now that WWTPs no longer blow up, I finally got around to updating TRAPS with Ecology's latest loading files. I have not compared the old and new data sources in detail, but I do know that the updated files did not introduce new sources.

### 1.2. Grid and TRAPS mapping improvements

This week I spent some time looking at every TRAPS in the LO grid. Through this exercise, I have identified a few potential improvements.

**Tiny Rivers**

First, I noticed that a few tiny rivers aren't discharging in the proper location. Figures 1-3 show the rivers I have identified. Pink diamonds indicate the original lat/lon coordinate of the river mouth, and purple dots indicate the resulting river mouth location in the LO grid (arrows indicate discharge direction).

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/9340815a-e9e9-4677-9293-9a2aec8e9799" width="600"/><br>Fig 1. Port Gamble tiny river mouth mapping. White cells indicate land, blue cells indicate water. A snapshot of the region from Google maps is shown for reference.</p><br>

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/65100108-2572-446d-bede-8fd65898ba48" width="500"/><br>Fig 2. Skookum Creek tiny river mouth mapping. White cells indicate land, blue cells indicate water.</p><br>

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/d4b5a030-b850-4b42-a9f9-5057102b69f4" width="300"/><br>Fig 3. McAllister Creek tiny river mouth mapping. White cells indicate land, blue cells indicate water.</p><br>

There are three options for what to do with these rivers:
1. Leave them as-is
2. Hard-code "if"-statements that manually map these river mouths to a better location
3. Extend the LO grid into the inlets from which the rivers discharge

**Point Sources**

Generally, the TRAPS algorithm seems to place the point sources in reasonable locations. It helps that most point sources are located off-shore to begin with, so the TRAPS algorithm didn't need to move them.

However, I have identified one pair of sources that we could potentially improve. The Swinomish and La Conner WWTPs both discharge to Swinomish Channel. Since the LO grid does not include the Swinomish Channel, the TRAPS algorithm ends up mapping both WWTPs to the same gridcell on the southern end of the channel (Fig. 4). The pink dots indicate the original locations of both WWTPs, while the green dot indicates the mapped location of the WWTPs in the LO grid.

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/3d7f84cf-d63f-443b-9b84-fd162508e66f" width="180"/><br>Fig 4. Swinomish WWWTP and La Conner WWTP mapping. White cells indicate land, blue cells indicate water.</p><br>

This mapping seems reasonable to me. My only concern is whether water in Swinomish Channel has an average northward direction. If so, then perhaps these WWTPs should have been mapped to the north side of the channel rather than the south side of the channel. I plan to read some literature to inform further decisions.

**Grid Extension**

Lastly, I would like to propose that we extend the LO grid to include Agate Pass. From the preliminary DO results for GRC, we saw that water just south of Agate Pass was disproportionately affected by WWTP DIN. I have an inkling that we would not have seen this hot spot if Agate Pass were present in the model. Figure 5 shows a copy of the GRC money plot. Notice the hotspot of decreased bottom DO near the northwest corner of Bainbridge Island. Figure 6 shows the current LO grid in comparison to a Google maps reference snapshot.

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/0f8e3ca0-753a-48a7-a97e-81d635c14822" width="800"/><br>Fig 5. GRC money plot.</p><br>

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/83455a77-79a0-4a8c-bd53-bce2dda4040a" width="800"/><br>Fig 6. LO grid structure near Agate Pass. A snapshot of the region from Google maps is shown for reference.</p><br>

### 1.3. Revisiting pre-existing LO rivers and SSM rivers that overlap

I also spent some time this week double-checking the overlapping LO and SSM rivers. If you may recall, some pre-existing rivers in LO were also present in SSM, but just had a different name. The goal was to remove the duplicate SSM river so that we do not double count river flow into the model domain. I previously checked for duplicates by looking at the placement of LO rivers and SSM rivers on a map. However, there were a few ambiguous duplicates (four, to be exact). For the last several months, I have opted to remove all ambiguous duplicates. This week, I took a closer look at these ambiguous duplicates by comparing their hydrographs.

As it turns out, one pair of rivers appears to *not* be a duplicate, and I have since added the corresponding SSM river back into the river forcing. Two pairs of ambiguous rivers are indeed duplicates and require no change. The last pair of rivers gave me quite the headache.

So what is this headache pair of rivers? It's in fact not pair of rivers, but a trio of rivers. These rivers are:

- tsolum (LiveOcean)
- Vancouver Isl N (Salish Sea Model)
- Comox (Salish Sea Model)

First and foremost, the LO and SSM rivers are located very close to each other which is why they are considered ambiguous duplicates. Figures 7-9 show the hydrographs for each of these rivers. Note that the Comox hydrograph is called "Comox/Quadra Islands" because the SSM data used two different names to refer to (I think) the same river.

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/7d77203b-5ae0-49c0-93be-e1def590dc58" width="400"/><br>Fig 7. Tsolum hydrograph, courtesy of Parker's river climatology scripts.</p><br>

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/2acd62cd-0c7b-4783-9b1d-f1a92b15455f" width="450"/><br>Fig 8. Vancouver Isl N hydrograph.</p><br>

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/c147480b-d42c-4a88-8a43-286cd2389a82" width="450"/><br>Fig 9. Comox hydrograph.</p><br>

Based on these profiles, I'm inclined to think that these are all different rivers.

However, I have another issue: Vancouver Isl N and Comox are located at the same lat/lon coordinates. They don't simply map to the same gridcell, they have the exact same starting coordinates (Fig. 10).

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/4641031c-9a7c-4dd7-93e9-3a8d4f2717b9" width="450"/><br>Fig 10. Mapping of Tsolum, Vancouver Isl N, and Comox rivers.</p><br>

What I find puzzling is that these rivers have unique flow profiles, which makes me think that they are unique rivers. If I let my scripts run as is, their discharge rates will simply be consolidated into one larger river. Perhaps that is fine. However, I also do not want to double count any rivers. I have reached out to Ben to see if he is at all familiar with these pair of rivers.

### 1.4. Refactoring climatology scripts

My climatology scripts work perfectly fine, but I always dread using them. The code is terribly ugly. As part of TRAPS finalization, I have decided to refactor the climatology code.

Ecology's loading files are stored in excel sheets, with each source having its own excel file. My previous climatology scripts accessed each individual excel file, calculated mean profiles, generated plots, and saved the climatology profiles-- all in one python script. This means that any time I wanted to change a plot, the code would still access evergy single excel file one-by-one. It was quite inefficient.

As an improvement, I have written a new script that consolidates the excel files into two netCDF files-- one for point sources and one for nonpoint sources (Fig. 11). Now, instead of accessing all of the excel files, I can simply access the netCDF file, which is much easier.

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/0f3257d4-20db-4a06-9da6-332e52e92e35" width="450"/><br>Fig 11. Point source netCDF data.</p><br>

So far, I've reworked the point source climatology script to generate profiles based on the new netCDF. This week I will finish up the nonpoint source climatology script. Additionally, I will address WWTPs that have opened/closed on a certain year, and I will also revisit tiny rivers that have non-realistic biogeochemistry. Lastly, I'll improve the TRAPS climatology README to reflect these updates.

---
## 2. Start of validation efforts

Last Friday I met with Parker and Jilian to discuss validation efforts. In the upcoming week, I plan to dive into initial validation efforts. Specifically, my goals for the upcoming week are:

1. Practice making model vs. observation plots using available observational data and GRC model results
2. Create my own script that makes validation plots and calculates statistics (e.g. RMSE)
3. Write script that increases shortwave radiation attentuation in the Salish Sea
4. Coordinate with Dakota on initial ocean conditions

---
## 3. What have I been reading?

On the theme of improved biological rate parameters, I have been reading more literature that David Shull shared with us:

Sutton, J. N., Johannessen, S. C., &#38; Macdonald, R. W. (2013). A nitrogen budget for the strait of Georgia, British Columbia, with emphasis on particulate nitrogen and dissolved inorganic nitrogen. <i>Biogeosciences</i>, <i>10</i>(11), 7179–7194. https://doi.org/10.5194/bg-10-7179-2013

and a report from Jude Apple on "Spatial and seasonal variability in Salish Sea bottom-water microbial respiration"