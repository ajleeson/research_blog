## First look at model evaluation results

Early in the week I wrapped up [TRAPS readmes](https://github.com/ajleeson/LO_traps). Afterwards, I began looking at the preliminary model results.

 I first compared the modeled bottom DO to ORCA buoy data. Then I practiced running the model-model-data comparison scripts in Parker's obsmod folder. 

I used the model-model-data comparison scripts to compare the new model run to observational data as well as the GRC run (WWTPs as tiny river implementation).

Preliminary results suggest that the new model run appears to outperform the GRC run in some cases, but underperform in other. More details below.

---
## Model evaluation

This week the model evaluation run has continued chugging along on klone, and so far it has made it to the middle of August.

### ORCA buoy DO comparison

When I was first looking at preliminary results from our "WWTP as tiny river" tests for GRC, I compared modeled DO to ORCA buoy DO. In the timeseries, we observed that the model tended to overestimate bottom DO.

Now that we have new results with fully functional TRAPS and improved biogeochemistry, I have decided to re-make the same ORCA buoy comparison figure. Figure 1 shows a timeseries of bottom DO from the GRC run, the new run, and the ORCA buoy observations.

Note that the depths of comparison are not the same. ORCA buoy bottom DO data refers to the deepest DO measurement available. Model bottom DO is taken from the bottom sigma layer, and the average depth of the bottom sigma layer is the "model depth."

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/29d64977-77d5-406c-a612-a84c9673bc48" width="800"/><br>Fig ????. Model-model-ORCA buoy comparison of bottom DO.</p><br>

In general, the new results have a higher initial bottom DO compared to prior observations and the ORCA measurements. At Twanoh, Dabob Bay, and Hoodsport, the bottom DO in the new model run remains higher than observations and the GRC run. However, bottom DO at Point Wells, North Buoy, and Carr Inlet decreases more significantly in the new model run compared to the GRC run. At Point Wells and Carr Inlet, specifically, the new model run appears to do a better job capturing the obersved summer DO depletion than the GRC run.

This preliminary analysis suggests that the new model run has mixed performance depending on the location-- either improving DO skill in Main Basin and South Sound, or overestimating DO in Hood Canal.

We can also conclude that the new results are influenced by a combination of both initial conditions and model dynamics (which is good, since we changed both).

### obsmod

This week I also began exploring some of Parker's model skill assessment scripts in his obsmod folder. In addition to creating modeled-observed plots, the script also calculates the following statistics:

$\mathrm{Bias} = \mathrm{mean}\big( \mathrm{modeled} - \mathrm{observed}\big)$

$\mathrm{RMSE} = \sqrt{\mathrm{mean}\big[ (\mathrm{modeled} - \mathrm{observed})^2\big]}$

The figures below show model-model-data comparison plots. Every plot compares the new model run and the GRC run to one of the following set of observations:
- NCEI Salish
  - shallow bottle data
  - deep bottle data
- NCEI Coastal (no data for new run yet)
  - shallow bottle data
  - deep bottle data
- Ecology
  - shallow bottle data
  - deep bottle data
- DFO
  - shallow bottle data
  - deep bottle data

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/05b05a39-b4b0-4116-a69d-17dd9bda44b6" width="800"/><br>Fig 2. Model-model-data comparison. New run compared to GRC run compared to nceiSalish shallow bottle data.</p><br>

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/cb441d7a-7dfd-4da5-930b-f92c370c5ad4" width="800"/><br>Fig 3. Model-model-data comparison. New run compared to GRC run compared to nceiSalish deep bottle data.</p><br>

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/3f56abdb-0ad1-4cf4-9a64-32a2b65e48ce" width="800"/><br>Fig 4. Model-model-data comparison. New run compared to GRC run compared to nceiCoastal shallow bottle data.</p><br>

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/3f2fdecf-1977-47bf-a2c5-8c1c96898c60" width="800"/><br>Fig 5. Model-model-data comparison. New run compared to GRC run compared to nceiCoastal deep bottle data.</p><br>

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/84e7fe34-0f68-4d77-a460-04a0a73a41d4" width="800"/><br>Fig 6. Model-model-data comparison. New run compared to GRC run compared to Ecology shallow bottle data.</p><br>

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/a6a38228-635b-4c44-b408-db1a99d09954" width="800"/><br>Fig 7. Model-model-data comparison. New run compared to GRC run compared to Ecology deep bottle data.</p><br>

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/eaacb928-580c-4360-8cf2-2748ee78f8ff" width="800"/><br>Fig 8. Model-model-data comparison. New run compared to GRC run compared to DFO shallow bottle data.</p><br>

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/407d3060-06d5-4b0b-9c29-3c7351fdefcd" width="800"/><br>Fig 9. Model-model-data comparison. New run compared to GRC run compared to DFO deep bottle data.</p><br>

The table below provides a summary of whether the new model run has improved or worsened bias/RMSE compared to the GRC run.
"Improved" implies that the new run has lower bias and RMSE compared to the GRC run.
"Worsened" implies that the new run has higher bias and RMSE compared to the GRC run.

| Variable              |Salinity| Temp   | DO     | NO3    | NH4    | DIN    | DIC    | TA     | Chl    |
| :------------         |:------:|:------:|:------:|:------:|:------:|:------:|:------:|:------:|:------:|
| NCEI Salish (shallow) |worsened|worsened RMSE<br>same bias|improved|worsened|improved RMSE<br>same bias|worsened|improved|improved|N/A|
| NCEI Salish (deep)    |worsened|worsened RMSE<br>same bias|worsened RMSE<br>improved bias|worsened|improved|worsened|improved|improved|N/A|
| NCEI Coastal (shallow)|N/A     |N/A     |N/A     |N/A     |N/A     |N/A     |N/A     |N/A     |N/A     |
| NCEI Coastal (deep)   |N/A     |N/A     |N/A     |N/A     |N/A     |N/A     |N/A     |N/A     |N/A     |
| Ecology (shallow)     |worsened|worsened|worsened RMSE<br>improved bias|worsened|immproved RMSE<br>worsened bias|worsened|N/A|N/A|N/A|
| Ecology (deep)        |worsened|worsened|worsened|worsened|improved|worsened|N/A     |N/A     |N/A     |
| DFO (shallow)         |worsened|improved RMSE<br>worsened bias|improved||worsened|N/A|N/A|N/A|improved|
| DFO (deep)            |worsened|worsened|improved|worsened|N/A     |N/A     |N/A     |N/A     |improved|


In general, the new model appears to have less skill in capturing salinity, temperature, NO3, and DIN. The new model tends to improve DIC, TA, and Chl. There are mixed results for DO and NH4.