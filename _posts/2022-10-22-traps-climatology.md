## TRAPS Climatology

This week I finished creating TRAPS climatology. There are a few loose ends that I'll need to tie up later. Otherwise, I am at the point where I can now add real flow/temperature/nutrient values to TRAPS in LiveOcean.

For all TRAPS I have climatology for flow, temperature, nitrate, ammonium, total inorganic carbon, total alkalinity, and dissolved oxygen. 

All of the generated climatology files follow the same naming convention and have the same layout as current LiveOcean river climatology files.

In general, creating climatology files took me much longer than I anticipated. For rivers, I had problems with leap years. For WWTPs, I had problems with converting monthly timeseries into daily timeseries. More details below.

---
## River Climatology

Ecology's timeseries provided daily values for each river. Using Pandas' groupby function it was easy to averages based on date to create a one-year average profile.

To handle leap years, the groupby function averages all year for which there is data on February 29th. In other words, the average Feb 29th values are computed from a much smaller sample size than the other days of the year.

The main problem I had with leap years actually came while I was checking my work. I tried to plot the climatology average on the same figure as all individual years' data. The issue is that 1/4 of the years have 366 days, and the other years had 365 days. In the end, I padded non-leap years with nan values on Feb 29th. 

Some examples of the resulting climatology timeseries are shows in Figures 1 - Figure 4 for Union River. Note that Union River is called "Lynch Cove" in Ecology's timeseries data. 

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/197361710-924f56c3-d7a0-4c66-9a38-4a79d310bae0.png" width="600"/><br>Fig 1. Union River flowrate climatology plotted with individual year flowrate values.</p><br>

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/197361732-dc40fed5-6fe2-4255-bb4b-67720c31e54e.png" width="600"/><br>Fig 2. Union River temperature climatology plotted with individual year flowrate values.</p><br>

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/197361780-4fcce141-0167-41fe-91da-25454575a12a.png" width="600"/><br>Fig 3. Union River nitrate climatology plotted with individual year flowrate values.</p><br>

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/197361972-31f007cc-ebbe-4e7c-9408-e18acae67c17.png" width="600"/><br>Fig 4. Union River dissolved oxygen climatology plotted with individual year flowrate values.</p><br>

The purpose of these plots was really to build confidence that I averaged correctly. After this point, it was easy relatively straightforward to save the climatology files. The main nuance was converting from Ecology's units to units expected by LiveOcean. Unit conversions are listed in the table below.

| Variable  | Ecology Units | LiveOcean Units   | Conversion, C <br> [LiveOcean] = C*[Ecology]  |
| ---       | ---           | ---               | ---   |
| flow      | m3 s-1        | m3 s-1            | 1     |
| temp      | degC          | degC              | 1     |
| NO3       | mg L-1        | mmol m-3          | 71.4  |
| NH4       | mg L-1        | mmol m-3          | 71.4  |
| TIC       | mmol m-3      | mmol m-3          | 1     |
| Talk      | mmol m-3      | mEq m-3           | 1     |
| DO        | mg L-1        | mmol m-3          | 31.26 |

I will also note that I created climatology for all rivers in Ecology's dataset, including rivers that overlap with LiveOcean. Since the TRAPS placement algorithm I finished last week already handles duplicates, it wasn't necessary for me to handle them again here. Basically, we have more climatology information than we need, and downstream code will selectively pick which climatology files to use.

---
## WWTP Climatology

Despite having already written climatology scripts for rivers, WWTP climatology generation still proved to be tricky. The main issue is that Ecology's WWTP timeseries provide data on a monthly basis. I wanted to create climatology on a daily basis.

Eventually, I was able to fill in the missing days using a constant value across the entire month. Figure 5 shows an example of the resulting Brightwater flow profile. 

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/197363089-0d62a8d1-63af-4a45-a343-1608c63e2fe0.png" width="600"/><br>Fig 5. Brightwater flowrate climatology plotted with individual year flowrate values.</p><br>

Brightwater is an interesting case because this WWTP did not exist prior to 2011. Ecology's timeseries thus lists zero flow prior to 2011. I padded all of these zero values with nans and calculated means ignoring nans. This way, zero values will not bias the climatology values downwards.

This climatology file works for Brightwater after the year 2011. However, it **only** makes sense to use Brightwater climatology after 2011. I will later need to implement a check for when each WWTP came online, and then decide whether or not to add the WWTP to LiveOcean for a given year.

Another similar issue is how to handle WWTPs that may have upgraded their treatment process. Right now, the climatology generation is blindly averaging values from both before and after the upgrade. What I should do instead is create two different climatologies for before and after the upgrade.

---
## TRAPS Integration Checklist

Last week I put together a list of unresolved issues with TRAPS integration. I was able to check a few things off this week. The list also grew slightly (new items are *italicized*.)

**Priorities before KC meeting**
- [X] (2022.10.22) Create climatology for TRAPS 
- [ ] Verify that my ROMS forcing file generation code does what it should be doing (eg. check my units, timing, etc.)
- [ ] Try to run LiveOcean myself!
- [ ] Debug why ROMS is crashing

**All remaining items**
- [ ] Verify that the UTM to lat/lon conversion was correct
- [ ] Make sure any river mouths located on a narrow strip of land get placed on the correct side of the land (the algorithm is blindly placing rivers on the nearest coastal grid cell without awareness of real geography)
- [ ] Compare Ecology's Hood Canal flow data to Mike Brett's data. Conduct other data checking, if possible.
- [ ] Determine from which sigma layer each marine point source should discharge, and how to translate this information into the ROMS variable, river_Vshape
- [X] (2022.10.22) Think about how to handle forecasting?
  - handled by using climatology files
- [ ] *Figure out how to handle climatology for WWTPs that came online after certain year, or upgraded treatment type after certain year*