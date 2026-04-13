## Proposed mCDR Sites

During our call on Friday April 10, we discussed considerations for site selection. This blog post summarizes key points from our discussion and proposes 10 sites for mCDR deployment in the LiveOcean model. I welcome any feedback or suggestions.

Key considerations in the site selection decision process include:
- 5 "testbed locations", i.e., sites spanning pilot, marine lab, or industrial facility locations
- 5 "natural locations" spanning locations hypothesized to have the highest and lowest surface residence times
- Try to co-locate natural locations with nearbly marine labs or facilities, if possible

## Proposed sites

Figure 1 shows the proposed 10 mCDR sites.

<p style="text-align:center;"><img src="/research_blog/figures/2026.04.12/proposed_site_selection.png" width="425"/><br>Fig 1. Proposed mCDR site locations. Testbed locations are colored in pink and natural locations are colored in blue. Open circles indicate a site selection, and solid dots indicate that there is a nearby monitoring station, research center, or WWTP facility. The black x-marks indicate the locations of marine stations and WWTPs that we discussed in our Friday meeting, but are not being proposed in the list of 10 selected sites.</p><br>


Of these 10, I hypothesize that the worst mCDR candidates are the San Juan Islands and the Strait of Juan de Fuca. The hypothesized best sites are the Columbia River plume and Hood Canal. All other sites have contradicting predictions based on the dye tests and surfaced mixed layer calculations (Table 1). The contradictions likely arose because the dye tests only show us where dye ends up; it does not tell us the source of the high concentration of dye. Additionally, river discharge may dilute the nearby dye field, causing river plumes to look like poorer candidates than reality.

I also listed Whidbey Basin in Table 1, because our results suggest that it is also a promising mCDR site. However, we already have two river plume locations (Columbia and Fraser in the Souther SoG), so I did not include it in the recommended 10 sites. Though, I am open to other opinions.


| # | Site    | Nearby Facility | lowest/highest dye residence time| deepest/shallowest surface mixed layer depth |
| - | ------- | --------------- | ------------------------------------- | ------------------------- |
| 1 | San Juan Islands   | Friday Harbor Labs | very poor | deep/~5m|
| 2 | Port Angeles | Ebb Carbon outfall | very poor/poor | ~10m/shallow |
| 3 | Sequim Bay   | PNNL | very poor/poor | ~10m/shallow |
| 4 | Main Basin (WWTP)   | South King WWTP | very poor/poor | ~5m/shallow |
| 5 | Main Basin (Port of Seattle)   | Seattle Aquarium | very poor/poor | ~5m/shallow|
| 6 | Northern SoG   | Hakai observing station | poor/very good | ~15m/~5m |
| 7 | Southern SoG   | Iona WWTP | very poor | shallow |
| 8 | SJdF   | N/A | very poor/poor | ~10m|
| 9 | Hood Canal   | Alderbrook WWTP | very poor/good | shallow |
| 10| Columbia River   | Point Adams Research Station | poor/very good | shallow |
| *Extra?*| *Whidbey Basin*   | *N/A* | *poor/good* | *shallow* |

Note that at 9 sites, I plan to discharge alkalinity/decarbonized water at the surface, even if there is a WWTP nearby. The South King WWTP (site 4) is the only location in which I plan to discharge from the bottom. South King WWTP is a large WWTP that discharges at the same scale as West Point, but its location is closer to Port of Seattle. The South King WWTP and Port of Seattle locations thus serve a dual purpose of comparing surface and deep discharges from the (nearly) same location in Puget Sound.

The proposed discharge location for the Columbia River plume is located near the Point Adams Research Station near the mouth of the river. We had previously discussed using a location upstream where the Snake River tributary enters the Columbia. However, LiveOcean does not extend far enough inland to make this feasible.

Several of the testbeds are located on a land cell in LiveOcean. I will place the alkalinity/decarbonized water outfall at the nearest water cell.

## Future considerations

As a first pass, we will test OAE at each of these ten sites. The first step in this process is determining an appropriate alkalinity discharge rate such that nearby pH does not spike to unsafe levels.

Later, we will select one or two sites to test DOR. We will also consider varying the size of the discharge plume.