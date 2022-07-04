
## Plotting Progress

This week I successfully extracted data from a mooring and created a timeseries of the tides using the idealized estuary model. As a refresher, this model used the following forcing:

|Forcing | Model Input|
|---|---|
|River Flowrate|1000 m3 s-1|
|Tidal Forcing| Includes spring-neap cycle|
|Ocean Forcing| Estuary half fresh at t=0|
|Atmospheric Forcing|Not implemented in this model|

The video below shows section salinity and a sea surface height time series.

<video src="https://user-images.githubusercontent.com/15829099/174448749-2efaf6de-66e2-48a1-8a7b-69545f0c0bd5.mp4" controls="controls" style="max-width: 700px;">
</video>

The salinity depth profile at the mooring is shown below:
<img src="https://user-images.githubusercontent.com/15829099/174460928-71adfcd8-874b-4d5c-a035-a07794576f28.png" alt="CEWA570_estuary" width="700"/>

### Some thoughts and future work

A few thoughts. First, I wonder if my mooring is too far upstream and therefore too heavily influenced by the river. It may be better to move the mooring downstream in future analysis. Second, I wonder whether a simpler salinity plot would be easier to interpret. I am imagining a four-panel figure showing the salinity profile during:

- Flood (during spring tide)
- Ebb (during spring tide)
- Flood (during neap tide)
- Ebb  (during neap tide)

The next plot I'd like to make is a tidally-averaged velocity profile. As discussed previously, it would be interesting to compare results to the analytical velocity profiles we derived in CEWA 570 - Hydrodynamics (Figure below. Note that in this problem, the river was on the left and the ocean was on the right -- opposite of the idealized estuary).

<img src="https://user-images.githubusercontent.com/15829099/174674772-c04f2874-7167-4e95-8f8e-5bbc53288b8b.png" alt="CEWA570_estuary" width="600"/>

I have a few questions about the approach to creating such a figure from the idealized estuary model output. Since the depth of every layer is constantly changing with the tides, is it recommended to interpolate velocity between layers? Using these interpolations, I could create a depth vs. velocity vs. time array with uniformly-spaced depth values for all time. Then I could average the velocity over one full spring-neap cycle to determine the tidally-averaged velocity profile. In this case, what is the recommendation for handling the bottom and surface velocities given that the data are intermittent due to the tides? Should these data be omitted from the plot? Is this overall procedure I described the best approach for creating a tidally-averaged velocity profile to begin with?