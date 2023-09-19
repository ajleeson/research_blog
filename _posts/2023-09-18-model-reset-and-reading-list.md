## Model reset and new reading list

This week I restarted the model evaluation run with the old initial conditions. At the time of writing, the run has made it to mid-March without issue.

I have also started thinking about literature for this upcoming quarter and my upcoming presentations. Some proposed reading lists are provided below.

---
## Model rest

Last week we realized that we tried to change too many things at once for the model evaluation run. We agreed to take a step back and use the old initial conditions for now, but keep the biogeochemistry changes. However, between the old run and the new run, the model grid was updated from cas6 to cas7 to include Agate Pass and Swinomish Channel (Fig 1).

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/f0e72145-a848-4244-b121-a8e9b1660549" width="300"/><br>Fig 1. Grid changes. Areas in red indicate new water cells added to the cas7 domain.</p><br>

Reverting to the old initial conditions required that I fill in the new grid cells with appropriate state variable values. I achieved this by using a nearest neighbor algorithm with a KD tree. Figures 2 and 3 show the resulting surface and bottom temperature, respectively. I have generated similar figures for every state variable and have decided that the KD tree is working. I would be happy to share these other figures during our meeting if they would be helpful (I just did not want to clutter this  blog post).

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/fb1d547c-2787-4b56-9693-7816e348bfc6" width="700"/><br>Fig 2. Surface temperature initial conditions. The top two panels come from the old grid, the bottom two panels include values populated with nearest neighbor in the new grid.</p><br>

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/4adbbb19-fbe1-437a-8202-3806e57fbf29" width="700"/><br>Fig 3. Bottom temperature initial conditions. The top two panels come from the old grid, the bottom two panels include values populated with nearest neighbor in the new grid.</p><br>

On Friday afternoon I launched the new run, and it is currently in mid-March. This run was initialized with start-type 'continuation' using model output from cas6_v00_uu0mb on 2016.12.31 (which I converted to the new grid).

In the upcoming week I'll continue working with existing model evaluation scripts to check on the new model performance. I would also like to generate my own figures from the model-model-data dataframes that the existing scripts generate.

### Short aside

On Wednesday afternoon, I naively told Parker that I could have the new run ready to go that same day. Then I learned: KD trees are confusing! It took me many attempts (and two extra days) to finally get things right. Figure 4 shows one of my favorite examples of things gone wrong. In this case, I have accidentally flooded the entire state with water. Luckily we fixed this issue!

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/a8712f9c-1206-46f7-939b-312710905ebd" width="650"/><br>Fig 4. Surface temperature initial conditions in a case gone wrong.</p><br>

---
## Reading list

A few weeks ago, we also discussed plans for a weekly journal group. As someone who needs more structure around reading, I am fully in favor of this plan. Below I've provided a short list of potential options for our new reading group.

At the end of this blog post I've also included a list of papers I'd like to read prior to my EFM talk (which I'm using as practice for NTU). For the PO seminar at NTU, I would like to provide sufficient background information on the Salish Sea and Puget Sound. Moreover, I'd like to be able to field questions about our region. Alex recommended that I take a look at some local hydrodynamics literature. I am open to more recommendations as well. 

### Proposed weekly reading for this quarter

Allen, J. I., Somerfield, P. J., & Gilbert, F. J. (2007). Quantifying uncertainty in high-resolution coupled hydrodynamic-ecosystem models. *Journal of Marine Systems*, 64(1–4), 3–14. https://doi.org/10.1016/j.jmarsys.2006.02.010

Hansen, D. V., & Rattray Jr, M. (1966). Gravitational circulation in straits and estuaries.

Mofjeld, H. O., & Larsen, L. H. (1984). Tides and tidal currents of the inland waters of western Washington.

Scully, M. E. (2013). Physical controls on hypoxia in Chesapeake Bay: A numerical modeling study. *Journal of Geophysical Research: Oceans*, 118(3), 1239–1256. https://doi.org/10.1002/jgrc.20138

Sutton, J. N., Johannessen, S. C., & Macdonald, R. W. (2013). A nitrogen budget for the strait of Georgia, British Columbia, with emphasis on particulate nitrogen and dissolved inorganic nitrogen. *Biogeosciences*, 10(11), 7179–7194. https://doi.org/10.5194/bg-10-7179-2013

### Planned reading before EFM talk

Babson, A. L., Kawase, M., & MacCready, P. (2006). Seasonal and interannual variability in the circulation of Puget Sound, Washington: A box model study. *Atmosphere - Ocean*, 44(1), 29–45. https://doi.org/10.3137/ao.440103

Banas, N. S., Conway-Cranos, L., Sutherland, D. A., MacCready, P., Kiffney, P., & Plummer, M. (2015). Patterns of River Influence and Connectivity Among Subbasins of Puget Sound, with Application to Bacterial and Nutrient Loading. *Estuaries and Coasts*, 38(3), 735–753. https://doi.org/10.1007/s12237-014-9853-y

Deppe, R. W., Thomson, J., Polagye, B., & Krembs, C. (2018). Predicting Deep Water Intrusions to Puget Sound, WA (USA), and the Seasonal Modulation of Dissolved Oxygen. *Estuaries and Coasts*, 41(1), 114–127. https://doi.org/10.1007/s12237-017-0274-6