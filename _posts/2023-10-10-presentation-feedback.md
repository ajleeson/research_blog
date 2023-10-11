## Presentation Feedback and Willamette River

This past week was dominated by my EFM talk and traveling to Taiwan. However, I am happy to report that I am now settled into my new office, and I managed to start working on a few tasks.

First, I consolidated feedback for my EFM presentation. Then, I removed Willamette River from TRAPS forcing. More details below.

Admittedly, this isn't as much work as I'd have liked to have accomplished (which is frustrating). Therefore, I am excited to dive back into research this upcoming week (hopefully with a brain that functions during the correct hours of the day).

---
## Presentation Feedback

After my EFM talk last week, I received several good suggestions for both my talk and my research. These ideas are listed below.

**Suggestions for talk**
- Retain some discussion about the WWTP bug in the final NTU presentation
- Before showing model evaluation results, provide information about our expectations. For instance, I could create a slide with goals for "good model skill"
  
**Suggestions for research**
- Check in with Erin for suggestions of where to find ORCA metadata. Maybe some log files could help us investigate the Hoodsport mooring.
- Revisit NH4 multiple linear regression from Ecology, especially in Lynch Cove. NH4 loading should be higher than it is. Take a look at Mike's data for more information.
- Compare model output to velocity data (if available) to help evaluate the model hydrodynamics
- Check whether observations near septic systems have elevated nutrient signals after large storms
- Re-visit Fraser River ammonium concentration. Should I also have changed nitrate?
  - How well is the model performing in the Strait of Georgia?

So far, I have asked Erin about ORCA log files and look forward to hearing back from her.

---
## Willamette River

Parker also reached out to me this week about an issue with Willamette River in Oregon. It seems that this most recent model run accidentally double counted contributions from the Willamette River. Even though the Willamette is not explicitly a pre-existing LiveOcean River, it discharges to the Columbia River which *is* a pre-existing river. The pre-existing Columbia River uses data from a USGS gauge that is downstream of the Willamette river junction. Thus, I revisited the TRAPS code this week and removed the Willamette.

I have pushed this change to the LO_traps Github, and have updated the "Exceptions and Nuances" README section accordingly.

This was a relatively straightforward modification. However, it does mean that we will eventually need to re-run the model. Luckily the Willamette is far enough away from Puget Sound that the current model evaluation results are likely accurate enough.
