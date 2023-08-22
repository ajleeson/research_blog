## TRAPS Final Touches

This week I focused on adding the final touches to TRAPS. I am so close to having the TRAPS module ready, and I'm optimistic that we can begin our model evaluation run this week. More details below.

## Shifted rivers

After our KC meeting on Thursday, we discussed rivers in Hood Canal that are based on the Big Beef Creek gage. The Big Beef Creek gage went out of service in 2012, meaning the little rivers in Hood Canal needed to get data from elsewhere. We found that 2013 is a copy of 2009 shifted by 3 months, and 2014 is a copy of 2010 shifted by 3 months. Originally, I only removed years 2013 and 2014. However, Alex recently recommended that I removed all flow data after 2012.

Strangely though, it seems like unique data reappears in 2015 in the full 1999-2017 Union River hydrograph (Fig. 1). I am not quite sure where this new unique data is coming from, but 2015 onwards does not appear to be a copy of previous years.

I also discovered that the copied and shifted problem appears to begin midway through 2012. Late 2012 seems to be a copy of early 2009.

With all of these findings under consideration, I have decided to remove data from July 2012 through the end of 2014.

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/bedcb08b-2979-411c-817f-1dcc9c539f4d" width="600"/><br>Fig 1. Union River hydrograph with repeat (and shifted) data removed.</p><br>

## WWTP open and close date

Another task that has been stuck on my to-do list has been to address WWTP open and close dates. This week I finally tackled this issue.

Using Ecology's data, I plotted a complete timeseries of flow for every point source. I'm surprised I haven't done this yet. Looking at these timeseries made it very easy to evaluate whether any WWTPs opened/closed during the 1999-2017 time period. Figure 2 shows an example timeseries of the Brightwater WWTP which opened at the end of 2011. Therefore, I will only include the Brightwarder WWTP for years 2012 and later (which I don't think is relevant because LiveOcean can't be run this far back! Nevertheless, I've included this condition for completeness.)

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/e5c992f2-a6db-416a-99ff-8ed9c20d2807" width="600"/><br>Fig 2. Brightwater WWTP hydrograph. This WWTP opened midway through 2011.</p><br>

The complete list of WWTP open and close conditions are:
- Brightwater (open 2012)
- Kimberly_Clark (close 2005)
- Lake Stevens 001 (close 2012)
- Lake Stevens 002 (open 2012)
- Oak Harbor RBC (close 2011)
- OF100 (open 2005)

## Weird biology in pre-existing LO rivers

Lastly, I revisited the biology in pre-existing LO rivers. I've had a few thoughts about changing the current implementation, which I'd like to discuss. Some relevant background information:

- Ecology's data includes information for several (20+) pre-existing LiveOcean rivers
  - Let's call these "duplicates"
- Parker asked me to force the duplicates using biogeochemistry climatologies based on Ecology's data (but keep temperature and flowrate as is)
  - I added this feature after iplementing tiny rivers, so tiny river and duplicate river climatology generation are handled entirely separately.
- Unfortunately, Ecology's data had weird biogeochemistry (zero DO, zero NO3, zero NH4, negative TIC) for several of the duplicate rivers. I've simply decided to *not* add Ecology data to these weird rivers, and only add Ecology data to the 17 other duplicate rivers.
  - Note, that by default, LiveOcean still adds a best guess biogeochemical profile to all of its rivers. Non-duplicate rivers therefore still have a bioglogy profile (just not based on Ecology data).
- This week I realized that some of the remaining 17 duplicate rivers have one or two weird biocheochemistry parameters
  - For instance, Fraser River has data for DO and NO3, but NH4 is effectively zero (see Figure 3.)

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/bac10a38-168c-43ad-a2ab-fb0dfe4fe6dd" width="600"/><br>Fig 3. Fraser River climatologies.</p><br>

The easiest option is to simply omit my climatologies from duplicate rivers with one or two weird biogeochemistry parameters.

However, I think a better option would be to combine the tiny river and duplicate river climatology generation code. My tiny river code deals with weird biogeochemisty by replacing their profiles with the average climatology of all other rivers. This is okay for tiny rivers since n > 100. For duplicate river, n is small so I did not implement this feature. If I were to instead combine tiny river and duplicate river scripts such that *all* of Ecology's rivers are handled together, then I could implement the same "fill in with average climatology" method. The only issue this presents is that I would need to slightly re-structure and re-test the TRAPS code. This would take some time, but I could probably still finish this task by the end of this week.

Another idea could be to group the rivers by watershed, and replace weird biogeochemistry profiles with the average climatology of other rivers within the same watershed. The sample size will be smaller, but this method could also help differentiate between rivers in more remote or more urban environments. I'm thinking that this task could be a stretch goal implemented in a later run.

## Last TO-DOs before launching the model evaluation run

- Finalize biogeochemistry for duplicate rivers
- Spend more time with Comox/Tsolum/Vancouver Isl N river
  - Maybe reach out to Susan Allen
- Push all new code to github
- Write test script to verify all TRAPS inputs are correct