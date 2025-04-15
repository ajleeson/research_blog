## Chapter 2 Thoughts

Now that I have finally passed quals (yay!), I turned my attention back towards my PhD research. This blog post summarizes my current thoughts on Chapter 2 of my PhD 

There are five main considerations that we need to think through before setting up a loading/no-loading experiment:

1. What version of LiveOcean to use? The old biogeochemistry module which overestimated DO? A newer version?
2. Bit-reproducibility issues manifesting as noise in the biogeochemistry fields
3. Shortening model run time by outputting daily averages rather than hourly data
4. New loading data from Ecology and USGS for point sources
5. What are our test conditions?

---
## 1. Version of LiveOcean

Depending on how we decide to handle issues 2 - 4 below, we may need to run both a loading and a no-loading run for the Chapter 2 experiment.

---
## 2. Bit-reproducibility


---
## 3. Daily average outputs

Describe idea!!!!!!

I was originally very excited for this idea. However, I would like to compare the fluxes of nutrients, detritus and DO into the terminal inlets between the loading/no-loading runs. The way I calculated DO fluxes in Chapter 1 was using the TEF code, which requires hourly model output. Therefore, I think we necessarily need to output hourly data rather than daily average data.  

I'm happy to chat more about this, though. It would be great to know if we could still make daily averages work for us!

---
## 4. New WWTP loading data

**New WWTP loading data**

As I mentioned previously, Ecology and the USGS have published new monthly loading data for WWTPs from 2005 - 2020 (links below). These WWTP loading data are used as inputs to a SPARROW model-- a joint effort between Ecology and the USGS to estimate the seasonal nutrient loads discharging from 19 different basins across Puget Sound. The SPARROW model takes into account an assortment of upstream nutrient sources such as animal feed operations, septic tanks, Red Alder nitrogen fixation, etc.

- [New point source loading data (includes WWTPs)](https://www.sciencebase.gov/catalog/item/64762b37d34e4e58932d9d81)
- [Documentation of SPARROW model (Schmadel et al., 2025)](https://essopenarchive.org/users/883361/articles/1264697-preprint-simulated-seasonal-loads-of-total-nitrogen-and-total-phosphorus-by-major-source-from-watersheds-draining-to-washington-waters-of-the-salish-sea-2005-through-2020)
- [Watershed data used as inputs to SPARROW model](https://www.sciencebase.gov/catalog/item/6511dfbdd34e823a0275dd71)
- [Calibration data for SPARROW model](https://www.sciencebase.gov/catalog/item/65860a21d34eff134d437203)

For our purposes, I am only considering incorporating the new WWTP loading data into LiveOcean. I intend to leave the tiny river climatologies as-is because the outputs of the SPARROW model are resolved at a seasonal time scale rather than a daily time scale. However, I'm happy to chat more about all of our options in more detail.

**Why upgrade the LiveOcean WWTP loading?**

The old TRAPS climatologies were generated from an older dataset that spanned from 1999 through July of 2017. One reason why I generated climatologies was to expand the dataset beyond July 2017, so we could use it as model input for these later years.

However, Stefano has previously mentioned to me that some WWTPs have been upgraded over the years, and their loads may have changed abruptly. Thus, climatologies may not be the most appropriate input to use for our loading/no-loading experiment. I'm curious to look into the WWTPs with changing loads in more detail and compare our climatologies to the new data for these specific plants. Stefano is out of office currently, so we'll wait to learn more once he's back (I don't remember which WWTPs had changing loads).

As a first look, Figure 1 shows a comparison of the West Point nutrient load climatology compared to the new nutrient load dataset. 

<p style="text-align:center;"><img src="https://github.com/user-attachments/assets/93a1ac64-851c-43ad-9701-acf92fbc9aed" width="650"/><br>Fig 1. West Point WWTP discharge. New Ecology/USGS loading data compared to the climatologies currently being used in LiveOcean.</p><br>


**Implementation thoughts**

It's going to take me at least one week of full-time focus to integrate the new data into the LO system. Some challenges I anticipate include:

- Restructuring my TRAPS code to handle and process the new data format and metadata
- Identifying which new WWTPs correspond to which old WWTPs (because the names aren't the same)
- Deciding how to deal with loading inputs after 2020
- Differentiating between WWTPs and industrial facilities and fish hatcheries
    - Do industrial facilities and fish hatcheries also discharge from the sea floor?

---
## 5. Test conditions

Are we interested in only running a loading/no-loading experiment for one year? There is interannual variability in the loading. How do we want to deal with that?

Original plan: run with identical hydrodynamics, but reduce nutrient concentrations to zero in the no-loading run.

Mike had more thoughts about this. 



