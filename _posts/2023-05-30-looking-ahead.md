## Looking Ahead to the Next Steps

Over the weekend I've had some time to think about my current research status and where to go from here. I've come up with the following list of items:

1. Finish GRC poster
2. Literature deep dive
3. Continue debugging vertical sources

More details below.

---
## GRC Poster

My goal is to have a completed poster draft by next week. I am leaving early for Boston, so my poster needs to be finished by June 13, which is only two weeks from now!

Figure 1 is the same preliminary DO results figure that I shared last week. By now I should have results from both model conditions, so I can expand the timeseries and colormap time period. Before I do that, I'd like to discuss what could be done better for the GRC poster. Mike had mentioned looking at bottom DO over a larger area in Lynch Cove. Or perhaps I could extract the bottom DO timeseries at the location of an ORCA buoy.

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/26a79b64-1ec0-403a-a5cb-dd2757e5af23" width="800"/><br>Fig 1. DO comparison plot. Panel (a) presents a bottom DO timeseries in Penn Cove from the Baseline. Panel (b) shows the difference in bottom DO at Penn Cove between the No WWTP DIN condition and the Baseline run. Panels (c) and (d) are analagous to (a) and (b), respectively, except for Lynch Cove. Panel (e) shows the average bottom DO in Puget Sound between September 1 - September 15. The locations of the Penn Cove and Lynch Cove timeseries are highlighted. Panel (f) shows the difference in average bottom DO between the No WWTP DIN condition and the Baseline condition.</p><br>

There is also one new issue that came up over the weekend. Parker discovered that the forcing file we used in these preliminary DO tests have rivers that discharge into land (Fig. 2).

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/d83e87cb-d826-4033-aef0-cf8a50c29c50" width="350"/><br>Fig 2. Example of two rivers correctly discharging into water (blue), but one river incorrectly discharging into land (white). Borrowed from Parker's email to me.</p><br>

What I have discovered is that the tiny river implementation is correct, but the "WWTP as tiny river" implementation has a bug. Tiny rivers are horizontal sources which have a signed flowrate indicating whether the river flows (+) northwards/eastwards or (-) southwards/westwards. WWTPs were originally vertical sources which always have a positive sign. When I converted WWTPs to tiny rivers, I forgot to account for the horizontal source sign convention. Thus, a decent percentage of WWTPs are not discharging into the water.

Out of 99 WWTPs, 44 were not discharging into water including West Point.

---
## Literature Deep Dive

Aside from this new bug, I've also become somewhat overwhelmed by the amount of information stored in the model output. There are data at every hour at every gridcell for every state variable. This shear amount of information means that there are a lot of possibilities for different analysis methods, and I'm not quite sure what is best. In the coming weeks I'd like to spend time reading literature to learn about what others have done with their model output. After classes end next week, I should have more time to dedicate to this effort.

It's not often that I am aware of my own growth as a student. However, this week I realized that reading literature is much easier than it used to be. I'm understanding the content, and I have a better intuition for which details are the important details. Figure 3 shows a note I left in a paper I read over a year ago. I've learned a lot since then.

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/f9b330d0-fd0b-4846-b2ac-a7355dd04991" width="250"/><br>Fig 3. Comment left in paper.</p><br>

---
## Vertical Source Debugging

Lastly, I want to finish debugging vertical sources in LiveOcean. We've made good progress towards a functioning solution this quarter, and I'm excited to see this effort to the end. Perhaps this work picks up after finals week and GRC.

My next step is to debug the incorrect forcing in the idealized domain. I will test LtracerSrc = T T, or LtracerSrc = F F.