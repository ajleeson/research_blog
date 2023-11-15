## The story of DIN

This week I finalized the density profile figure from before, and I took a closer look at model DIN performance. More details below.

---
## Improved density profile figure

Last week I learned that I was accidentally plotting *in-situ* density instead of potential density. Parker helped me fix this issue last week, and I have since cleaned up the plots. See figure 1 below.

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/f74ce4dd-88c4-41d8-8f9f-95d907596aee" width="800"/>Fig 1. Comparison of model and observation seasonal density profiles at ORCA buoy locations.
</p><br>

LiveOcean tends to overesimate density in deeper water at all orca buoy stations. In surface waters, LiveOcean tends to underestimate density, especially in the spring at Point Wells.

Figures 2 and 3 show depth profiles for temperature and salinity, respectively. The models tends to underestimate temperature in every season, but it gets the general profile shape correct. However, LiveOcean does overestimate temperature in surface waters at Point Wells during the Spring and Summer.

Additionally, LiveOcean is understimating salinity in surface water at Point Wells during the Spring.

Perhaps there is too much frewhwater discharge entering Hood Canal during the Spring, which is leading to our density error. 

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/2e8bf2c1-2541-461b-9201-7f69a46ca57b" width="800"/>Fig 2. Comparison of model and observation seasonal temperature profiles at ORCA buoy locations.
</p><br>

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/af5ed63f-7da8-4c20-a4ca-9e48238c9566" width="800"/>Fig 3. Comparison of model and observation seasonal salinity profiles at ORCA buoy locations.
</p><br>

---
## DIN bullseye maps

### Where we left off...

Last week, monthly property-property plots of DIN seemed to suggest that DIN ICs are not the problem. Rather, there is not enough uptake of DIN in surface waters during the spring (Figs 4 and 5).

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/b46e9d08-beb5-4c13-b06d-f1e2d20a8c92" width="800"/><br>Fig 4. DIN property-property plot in shallow water (z > -30 m).</p><br>

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/f2e05a4b-aee1-4522-ae6d-bbd0f4e390e0" width="800"/><br>Fig 5. DIN property-property plot in deep water (z < -30 m).</p><br>

This week, I re-generated the same figures using depth cut-offs of 25 m and 30 m, and mostly observed the same trends.

Some of our lingering unkowns from last week included:
- Bottles samples from which regions are contributing most to the discrepancy?
- What are the depths of these samples? (want more resultion than >30m or <30m.)
- What's going on in March in the deep layer where the model strongly overestimates DIN at three locations?

### New updates

This week I sought to address our unkowns using what I am calling "bullseye maps." The following sets of figures show maps of the Salish Sea, where every circle indicates a bottle sample location. The area of the circle is proportional to depth, with the smallest circles representing the shallowest depths. Note that I sorted data by depth prior to plotting, so smaller circles are always on top of larger circles (and thus visible to us). The color of the circle denotes the difference between the modeled and observed DIN concentrations. Red means the model is over-estimating DIN, and blue means the model is under-estimating DIN. Every bullseye map displays one month of data.

In winter (Fig. 6) we observe that there is litte difference between the modeled and observed DIN concentrations. The January and February maps are good evidence that the model ICs are doing a decent job of representing DIN (at least in Puget Sound). In March, we again see evidence of the model strongly overestimating DIN at depth, with the added information that the overestimation is occuring in Hood Canal. However, this darkest spot in the middle of Hood Canal near Dewatto is odd-- either the model is missing a deep sink of DIN, or the observations have error. I am not yet sure which is the case. 

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/aa7457cc-6087-4104-8140-1105190ca822" width="800"/><br>Fig 6. Winter DIN bullseye map. Circle area is proportional to depth (deeper is larger). Circle color indicates difference between modeled and observed value.</p><br>

In spring (Fig. 7), we begin to see more regionally consistent results. In April, throughout Hood Canal and Main Basin, the model is overestimating DIN in surface waters. Like we hypothesized last week, it appears as though the model is underestimating the amount of nitrogen being taken up by the spring bloom. The model error is less significant in deeper waters and in South Sound (at least in April).

As the season progresses into May and June, the model overestimation of DIN is also present at deeper depths. In May, we also start to see signs of DIN overestimation in surface waters in South Sound.

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/265795c1-15d7-4162-935d-0b1f3be518e2" width="800"/><br>Fig 7. Spring DIN bullseye map. Circle area is proportional to depth (deeper is larger). Circle color indicates difference between modeled and observed value.</p><br>

During summer (Fig 8.), the bullseye maps begin to show strange behavior. In June and July, the most pronounced DIN overestimation appears to occur at deeper depths, while the overestimation is now less severe at the surface. One hypothesis as to what might be happening: in the spring the model is underestimating the DIN uptake rate by phytoplankton at the surface, and during the summer the model is overestimating the respiration rate at depth. Another hypothesis is that the model introduced a deep flux of DIN-rich water from the ocean, which was perhaps too rich in DIN.

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/041c81ce-3b38-4d36-971b-a9ab386a554e" width="800"/><br>Fig 8. Summer DIN bullseye maps. Circle area is proportional to depth (deeper is larger). Circle color indicates difference between modeled and observed value.</p><br>

In fall (Fig. 9), LiveOcean still slightly overestimates DIN, but not as severely as during the summer.

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/774050ca-4ab9-456e-8295-89a063b8d457" width="800"/><br>Fig 9. Fall DIN bullseye map. Circle area is proportional to depth (deeper is larger). Circle color indicates difference between modeled and observed value.</p><br>

### Other observations

I also took a very quick look at DO bullseye maps. Here I've listed some observations of note. Please see the appendix for all DO bullseye maps.

Figure 10 shows DO bullseye maps for January and February. We previously had observed that model ICs tend to overestimate DO concentrations, and these maps are more evidence that this is indeed the case. Note that the deep overestimation of DO in Strait of Georgia persists through June (Fig. 11). Residence times in Strait of Georgia are long, so it makes sens that the initial overestimation of DIN would stick around for so long.

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/7e89de0e-32fd-4559-9d57-5d5ea351de1c" width="350"/><br>Fig 10. Jan/Feb DO bullseye map. Circle area is proportional to depth (deeper is larger). Circle color indicates difference between modeled and observed value.</p><br>

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/8e2ab245-8478-4eb1-8c78-b8c7c5399da8" width="175"/><br>Fig 11. June DO bullseye map. Circle area is proportional to depth (deeper is larger). Circle color indicates difference between modeled and observed value.</p><br>

In September (Fig. 12), both the DIN and DO bullseye maps are suggesting to me that there is a small bloom in Hood Canal that the model is not capturing. In September the model is overestimating DIN and underestimating DO in surface waters near the head of Hood Canal.

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/4acce4ae-ceb7-4353-a842-e51865cfa878" width="350"/><br>Fig 12. September DIN and DO bullseye maps. Circle area is proportional to depth (deeper is larger). Circle color indicates difference between modeled and observed value.</p><br>

## Appendix: DO bullseye maps

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/5824acd1-e415-4b59-bfd8-2719309a8511" width="800"/><br>Fig 13. Winter DO bullseye map. Circle area is proportional to depth (deeper is larger). Circle color indicates difference between modeled and observed value.</p><br>

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/b6cab195-f363-4879-8f9a-5b89aa03ff46" width="800"/><br>Fig 14. Spring DO bullseye map. Circle area is proportional to depth (deeper is larger). Circle color indicates difference between modeled and observed value.</p><br>

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/7a465701-f865-4460-8d35-ec3033027548" width="800"/><br>Fig 15. Summer DO bullseye map. Circle area is proportional to depth (deeper is larger). Circle color indicates difference between modeled and observed value.</p><br>

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/ef012a1e-54b9-42c1-b4af-f3f10a50075e" width="800"/><br>Fig 16. Fall DO bullseye map. Circle area is proportional to depth (deeper is larger). Circle color indicates difference between modeled and observed value.</p><br>