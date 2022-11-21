## TRAPS Debugging Part III

This week I found a few issues with the biogeochemistry inputs which could have been respondible for nans in the output file. For example, I had a line of code that replaced zeros with nans, and I found some rivers that introduced negative TIC values to the system. After fixing these issues, the biogeochemistry output files looked complete.

Unfortunately, fixing the biogeochemisty did not correct the Birch Bay WWTP blow up issue. I tested moving Birch Bay WWTP seaward by one grid cell. This model run did not crash, however the velocities near the WWTP still look unstable.

---
## Nans

Ecology padded state variables with zeros for WWTPs that opened midway through the the timeseries. For example, a WWTP that opened in 2005 would have 0 as the value of each state variable prior to 2005. I thought that I would be clever and replace these zeros with nans before taking averages for climatology. Nans are ignored in pandas mean calculations, and I did not want zeros biasing the data downwards. However, what happens if the timeseries has a state variable with a value of 0 for all time? These zeros all get converted to nan, and the mean of nans is nan. Several rivers (eg. Willamette) had 0 alkalinity across the entire timeseries which got turned into an average value of nan. As a result, my code was basically injecting nans into the alkalinity field of the model (Fig 1). I later discovered that the BP Cherry Point source had zero nitrate for all time, so I was also injecting nans into the nitrate field of the model. I wouldn't be surprised if there are other instances of nan injection occurring as well.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/202876032-8dca559a-f6c1-41ec-988b-45726cd4c46d.png" width="500"/><br>Fig 1. "Nan-injection" code: Lines of code injecting nans into climatology. Big oops.</p><br>

I'll note that BP Cherry Point is quite close to Birch Bay WWTP. Perhaps this is why the nitrate values from last week were having issues (Fig 2)

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/200236388-98fbdc90-644a-477f-abc2-d0b0d555c514.png" width="500"/><br>Fig 2. Point sources near Birch Bay. BP Cherry Point is the nearest point source to Birch Bay WWTP.</p><br>

I took out the "nan-injection" code and re-ran the model for one day. Even with this fix, however, LiveOcean still crashed on hour 21 of the run at the Birch Bay WWTP with similar velocity issues (Figure 3)

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/202876036-13dfbe15-8166-4794-8b68-64394763a94a.png" width="700"/><br>Fig 3. Surface velocity near the Birch Bay WWTP just before model blowup. Forcing for this run was created without the "nan-injection" code.</p><br>

I have decided to add the nan-injection code back into my script, so all zeros are replaced with nans. This time, I also added a line of code that replaced any lingering nans with zeros after averages have been taken. Thus, state variables that always have 0 as its value will have an overall average of zero instad of nan (Fig 4). Additonally, WWTPs that open sometime in the middle of the period don't have state variable values that get biased downwards due to zeros.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/202876033-44a8b9ea-07c1-40d5-80f9-6e52828590c7.png" width="600"/><br>Fig 4. Updated code that removes nan-injection problem.</p><br>

---
## Negative TIC

After digging some more into the inputs, I discovered that some rivers have negative TIC values in Ecology's dataset.

I added a line of code that converts negative TIC values to 0, then re-ran LiveOcean for one day. The model still crashed at hour 21 at Birch Bay WWTP. It crashed so badly that I'm unable to open any history files beyond hour 18. In addition to the nan-injection code and negative TIC values, there *must* be something else going on.

However, the biogeochemisty values now appear to be working (Fig 5).

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/203094170-d59f3fe0-3c44-4ef7-a7df-50a787ee7a41.png" width="600"/><br>Fig 5. State variables vs. depth and time at the location of the Birch Bay WWTP. This model was run with fixed nan-injection code and with negative TIC values forced to zero.</p><br>

At this point, I decided to generate and save climatology plots for all TRAPS so I could investigate further.

---
## Climatology Timeseries

A while back, Alex recommended that I add error bars to my climatology timeseries. I finally got around to doing this. The added bonus is that I saved these plots as well, so I now have climatology timeseries plots for all sources. This makes it much easier to check if there is something strange in the climatology inputs (before I was checking sources and state variables one-by-one based on suspicion only...and not even saving the plots).

Figure 6 shows an example of the Union River (Lynch Cove) climatology plot.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/202876034-04f06ef2-f2c4-4dfc-9e13-25f3fce7edf0.png" width="750"/><br>Fig 6. Union River climatology results. Note that Union River is called "Lynch Cove" in the Ecology dataset.</p><br>

I noticed that the zero alkalinity values and negative TIC values always occur together. In fact, there is large list of rivers that contain zero alkalinity, negative DIC, and extremely low DO. These rivers have different flow profiles, but the exact same (strange) biogeochemistry profiles. An example of one such river, the Tsitika River, is shown in Figure 7.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/202876037-097a10d9-31e9-4532-a24a-a92e466c7e84.png" width="650"/><br>Fig 7. Tsitika River climatology results with strange alkalinity, DIC, and DO values.</p><br>

Other rivers with these same profiles include: Alberni Inlet, Brooks Peninsula, Campbell River, Chehalis  R, Clayoquot, Gold River, Holberg, Homathco River, Klinaklini River, Knight Inlet, Neil Creek, Nimpkish River, North East Vancouver Island, Owikeno Lake, Salmon River, Seymour Inlet, Tahsis, Toba Inlet, Tsitika River, Willamette R, and Willapa R

Ben helped investigate how SSM is handling these rivers in their input files. As it turns out, none of these rivers are included in SSM.

I will also note that unlike rivers, none of the WWTPs had climatology profiles that raised red flags.

I removed the rivers with weird biogeochemistry and tried to run LiveOcean one more time. The model still blew up at hour 21, seemingly at the Birch Bay treatment plant.

---
## Birch Bay WWTP Tests

At this point, I seem to have resolved issues with missing biogeochemsitry variables. Yet Birch Bay WWTP is still causing LiveOcean to blow up. Thus, I've returned my attention to Birch Bay WWTP.

**Fixed Biogeochemisty, Birch Bay WWTP Omitted**

Using the fixed biogeochemisty conditions, I ran the model one more time but excluded Birch Bay WWTP. This model run ran to completion, and the surface velocities at hour 20 appear normal (Fig 8). Not shown are the biogeochemistry outputs, but I can confirm that the output file is complete for these variables.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/203094808-cb162772-c673-47ba-a566-a024d294c9e9.png" width="700"/><br>Fig 8. Surface velocity near the would-be location of Birch Bay WWTP at hour 20. Note that this model run omits the Birch Bay WWTP, and it uses the updated climatology and forcing with working biogeochemistry.</p><br>

**Fixed Biogeochemistry, Birch Bay WWTP Shifted Seawards by One Cell**

As suggested in last week's meeting, I also tested the scenario with Birch Bay WWTP shifted seawards by one grid cell. Again, LiveOcean did not crash! However, the surface velocities near Birch Bay WWTP at hour 20 look a unstable (Fig 9).

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/203095091-748f3671-936d-490b-8553-a9cb613d3f8e.png" width="700"/><br>Fig 9. Surface velocity near the Birch Bay WWTP at hour 20. Note that Birch Bay WWTP is shifted seawards by one gridcell, and this model run uses updated climatology and forcing with working biogeochemistry.</p><br>

Maybe shifting Birch Bay WWTP by only one grid cell is not sufficient to prevent blow up. In fact, it seems like ROMS did blow up at hour 19 during the run, but was able to correct itself after autmatically decreasing the time-step and re-running that hour.

Velocity and other state variables vs. depth and time at the new shifted Birch Bay WWTP are shown in Figures 10 and 11.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/203097986-cfe32f2f-a182-4195-a4e4-6897d4783324.png" width="750"/><br>Fig 10. Velocity vs. depth and time at the location of the shifted Birch Bay WWTP.</p><br>

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/203098124-5980842d-af8c-43cb-b648-fca38ed4c964.png" width="750"/><br>Fig 11. State variables vs. depth and time at the location of the shifted Birch Bay WWTP.</p><br>

---
## TRAPS Integration Checklist

New items are *italicized*.

- [X] (2022.10.22) Create climatology for TRAPS
- [ ] Verify that my ROMS forcing file generation code does what it should be doing (eg. check my units, timing, etc.)
- [X] (2022.10.30) Try to run LiveOcean myself!
- [ ] Debug why ROMS is crashing
- [ ] Verify that the UTM to lat/lon conversion was correct
- [ ] Make sure any river mouths located on a narrow strip of land get placed on the correct side of the land (the algorithm is blindly placing rivers on the nearest coastal grid cell without awareness of real geography)
- [ ] Compare Ecology's Hood Canal flow data to Mike Brett's data. Conduct other data checking, if possible.
- [ ] Determine from which sigma layer each marine point source should discharge, and how to translate this information into the ROMS variable, river_Vshape
- [X] (2022.10.22) Think about how to handle forecasting?
  - handled by using climatology files
- [ ] Figure out how to handle climatology for WWTPs that opened after certain year, shut down after certain year, or upgraded treatment type after certain year
- [ ] Check SSM flow data for ambiguous duplicate rivers, and decide whether they are or are not actually duplicate rivers
- [ ] Handle river mouths that get placed at the same coastal grid cell. Maybe combine the flows?
- [X] (2022.11.16) Add error bars to climatology plots, and save all plots
- [ ] Average Feb 28/Mar 1 data to get a Feb 29 value on non-leap years, so leap years have a larger n
- [ ] Potentially remove 2013/2014 data from Tahuya and Union rivers, so shift doesn't bias climatology
- [X] (2022.11.12) Add logical switches so user can choose to add only tiny rivers or only WWTPs.
- [ ] Have additional logical switch that either does or does not discharge nutrients.
- [X] (2022.11.15) Decide what to do if there are missing values in climatology. Also answer "why are there missing values in climatology?"
- [ ] Why does the Birch Bay treatment plant cause ROMS to blow up??
- [ ] *Clean up TRAPS integration code*