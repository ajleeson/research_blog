
## Tidally-Averaged Velocity and Salinity Profiles

This post is a follow-up to last week's basic idealized estuary run. As a reminder, the forcing and 10-day tide heights are provided below.

|Forcing | Model Input|
|---|---|
|River Flowrate|1000 m3 s-1|
|Tidal Forcing| Includes spring-neap cycle|
|Ocean Forcing| Estuary half fresh at t=0|
|Atmospheric Forcing|Not implemented in this model|

<video src="https://user-images.githubusercontent.com/15829099/174448749-2efaf6de-66e2-48a1-8a7b-69545f0c0bd5.mp4" controls="controls" style="max-width: 700px;">
</video>

This week my focus with these data was to create a daily-averaged salinity and velocity profile. The data were extracted with a moor extract at the location of the pink star in the above video.

### Tidally-Averaged Salinity Profile

To calculate the daily average salinity profile, the salinity data were passed through a 24-hour Hanning window, and then plotted with the average depth of each cell. The results are shown below.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/175992817-8c264beb-4fa7-4400-bede-f579d5c9ba52.png" alt="avg-sal-prof" width="600"/></p>

As time progresses, the salinity profile becomes fresher and initially remains mostly well mixed. During the last few days, stratification begins to set up.

### Tidally-Averaged Velocity Profile

The velocity profiles were calculated by first determining the transport velocity (velocity * cell thickness) for each depth cell at all times. These data were then processed through a 24-hour Hanning window and sub-sampled to one measurement per day (at 12 pm UTC). This average transport velocity was then divided by the average cell thickness (also calculated with a 24-hour Hanning window) to determine the average velocity on each day. Finally, the average velocity was plotted with the average depth of each cell. The resulting velocity profiles are shown below.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/175987072-85866118-f642-4604-aff6-4cfe7680c2d5.png" alt="avg-vel-prof" width="600"/></p>

As the tide shifts from spring to neap tide from early-February to mid-February, there is a corresponding change in the velocity profile. The exchange flow gets stronger during neap tide.

### Comparing to Analytical Model?

I rant into several questions while attempting to compare the tidally-averaged velocity profile to an analytical solution. Most of these questions involve geometry as the analytical solution I derived previously assumed a rectangular estuary. However, the numerical estuary model has a parabolic cross section with an estuary width that tapers towards the river mouth.

- Should I re-derive an analytical solution using a parabolic cross-section rather than a rectangular cross section? Would I even be able to solve this analytically?
- How should I define the estuary length and the estuary width in the analytical solution? I initially thought about defining a rectangular prism with a depth equal to the midpoint depth of the parabolic estuary. From there I thought about defining length and width with by equating the rectangular prism volume to the volume of the numerical model estuary. I'd also try to maintain the same length to width aspect ratio between the two models. However, I'm not sure what to use as the length of the numerical estuary model. The main confusion is that I've extracted data near the river mouth, so I imagine an equivalent analytical estuary should be shorter than the full length of the ideal estuary model.
- How should I really handle the estuary depth? The numerical estuary is sloped, and I've extracted data at only one point along the estuary length. Not to mention that the parabolic cross section means that the estuary has a curved bottom. Should an effective rectangular estuary have a depth equal to the average depth of a single cross section, or equal to the maximum depth of a cross section (i.e. the parabola vertex?). Should the length of the numerical estuary be defined such that the average vertex depth is equal to the depth at which I've extracted data? 
- Do these questions make sense? I've included a sketch of the geometries below to help.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/176041842-48b0ab65-85c3-4ac0-8ff8-c5a8894a1f96.jpg" alt="est-geoms" width="300"/></p>

Hopefully I've documented my confusions in a way that isn't confusing.
