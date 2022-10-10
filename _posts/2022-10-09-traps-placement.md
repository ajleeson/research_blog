## TRAPS Placement

This week I started implementing code to add tiny rivers and point sources (TRAPS) to LiveOcean. Currently, I have working code that identifies the LiveOcean grid cells (cas6) into which to place TRAPS. However, this code is currently stand-alone and does not yet modify anything on LiveOcean runs. I have yet to implement code that incorporates Ecology's data to create forcing for ROMS.

In this blog post I will disucss some details of the progress I have made so far, and list some of the issues that need to be resolved.

I've also included a brief timeline at the end of this post. My goal is to get TRAPS running in LiveOcean by the end of the month.

---
## Marine Point Sources

Marine point sources include mostly WWTPs. There are also other sources such as factories.

These were fairly straightforward to plot on the LiveOcean grid because I could rely heavily upon the WWTP placement algorithm I had written over the summer. As a reminder, this algorithm takes the lat/lon coordinates of point sources and decided where on the grid to place a point source. I don't change the location of point sources that are located in water. However, I calculate the nearest coastal grid cell for point sources that are originally located on land.

Figure 1 shows a map of the resulting marine point source locations on the LiveOcean grid after running my algorithm.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/194786272-64afccfb-8a4e-42fc-896d-87c4639c2edd.png" width="400"/><br>Fig 1. Location of marine point sources on LiveOcean grid. Pink circles denote the original lat/lon coordinates of the point source (from Ecology's dataset). The pale pink crosses indicate that the point source was originally in the water and my algorithm did not change its location. The dark purple crosses indicate that the point source was originally on land and my algorithm placed it at the nearest coastal grid cell.</p><br>

Figure 2 shows a zoom near the US-Canada border with point source labels. I have not extensively checked my work, but it's encouraging to see some familiar names where I would expect to see them (eg. Blaine).

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/194787407-201ec63b-7950-4ab7-87d0-db4ba872a734.png" width="300"/><br>Fig 2. Marine point sources near US-Canada border.</p><br>

---
## Rivers

Rivers ended up being quite a bit tricker than marine point sources. One minor issue was that large rivers in SSM are given two lat/lon coordinates corresponding to the river mouth. I consolidated these pair of coordinates down to one by simply averaging the coordinate locations. There were also several larger issues with rivers, which I discuss below.

### LiveOcean and SSM Common Rivers
First of all, LiveOcean already includes some rivers. I needed to filter out rivers that are already included in the model and add only the rives that do not already exist. The problem is that LiveOcean and SSM do not have the same naming convention. For instance, the river that flows into Puget Sound from Lake Washington is called "cedar" in LiveOcean, and "Lake Washington" in SSM (Figure 3). To help deal with this issue, I plotted all LiveOcean river names and SSM river names on the same grid. After zooming in, I could distinguish which rivers are overlapping, and which are unique to their respective models.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/194789429-5f417e7f-dd1a-4749-bbe0-5e965612701e.png" width="300"/><br>Fig 3. River flowing from Lake Washington that is present in both LiveOcean and SSM. In LiveOcean, it is called "cedar," and in SSM it is called "Lake Washington."</p><br>

For the most part, I have accounted for all of the rivers that are present in both models. However, there are five rivers that are still giving me problems. I disucss them in more detail in the "Known Issues" section below. For now, I am simply assuming that these questionable overlap rivers are indeed overlapping rivers, and I have excluded them from the LiveOcean grid.

The names of overlapping rivers are saved in an excel file in LO_data (LiveOcean_SSM_rivers.xlsx), and read by the TRAPS algorithm.

### River Directionality
Another issue with rivers is that they will be added as horizontal momentum sources. Unlike marine point sources which will be added as vertical momentum sources, rivers have directionality. This means that my algorithm needs to know where land and water are located, relative to the river mouth. ROMS also requires that horizontal momentum sources be located on a coastal cell.

 I mentioned that I left marine point sources where they were if they were already located in water. For rivers, however, I needed to move the river mouth to the nearest coastal cell regardless of whether the river mouth was located in water or on land. From there, I could determine whether the North, East, South, or West side of the coastal cell had land. The adjacent land-cell defines the direction from which the horizontal momentum will be introduced into LiveOcean.

 River mouths that were already located in coastal grid cells were left as is.

### Final River Mouth Map

The resulting river mouth locations on the LiveOcean grid (excluding all overlapping rivers) are shown in Figure 4.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/194804847-96a453b0-23e6-42f1-8a58-76982298c10d.png" width="400"/><br>Fig 4. Location of river mouth locations on LiveOcean grid.</p><br>

---
## Known Issues

**Questionable overlap rivers**

As discussed earlier, there are a few rivers in LiveOcean and SSM seem like they could be the same river, but also seem like different rivers. All ambiguous rivers are shown in Figure 5. Sometimes I wonder if SSM is representing a tiny creek in the same area as a LiveOcean river. Conversely, I wonder if LiveOcean has a river that has been gridded further upstream, while SSM is summarizing the same river further downstream (and using the name of the inlet rather than the name of the river).

1. SSM's "Sunshine Coast" vs LiveOcean's "clowhom"
2. Is LiveOcean's "tsolum" represented by SSM's "Comox" or "Vancouver Isl N," or neither? It seems like SSM may have two river mouths located at the same point.
3. SSM's "Alberni Inlet" vs LiveOcean's "sarita"
4. I hesitated here because I don't think Hoko River discharges to Clallam Bay (based on Google maps). This might be a symptom of a poor UTM to lat/lon conversion.
5. So many things are going on in SSM that I'm not sure which river mouth (if any) correspond to LiveOcean's "deschutes"

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/194802363-715e735a-9ca8-467a-9ec1-de746c01ce99.png" width="700"/><br>Fig 5. All river locations in which it is ambiguous whether they are included in both SSM and LiveOcean, or whether they are unique, neighboring rivers.</p><br>

**Seemingly land-based river mouths**

This isn't actually an issue, just a nuance in the code. I think it's worth discussing here so I can remember it later on (I had a bit of a scare this week).

Essentially, there is an offset for setting the direction of the river flow in ROMS. For rivers flowing from the South or West, I thus needed to subtract 1 from the col or row index, respectively. When I plot these rivers, the Northward-flowing and Eastward-flowing rivers thus appear to be placed on a land cell rather than a water cell (Figure 6).

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/194793645-2c5c3d33-3eab-4d80-ba38-b30784a7da91.png" width="400"/><br>Fig 6. Seemingly land-based river mouths.</p><br>

**Lat/lon coordinates**

I mentioned this in a previous blog post, but I am suspicious of the lat/lon coordinates I have been using. Ecology has UTM coordinates for all of the TRAPS in the SSM, and I converted these UTM coordinates to lat/lon. I'm not confident that I did this conversion correctly and will need to verify the coordinate locations later on.

**Checking river location and directionality**

I have this unsettling feeling that the algorithm is struggling with two particular issues. First, it's possible that a land-based river mouth may be discharging to a different cell than the nearest coastal cell. The most egregious possibility is if the river mouth is located on a narrow strip of land, and the algorithm places the river mouth on the wrong side of the land. This would be the completely wrong location to discharge from. Resolving this issue might require me to figure out which direction the rivers are discharging from in SSM.

Another concern is the direction of the river discharge. Currently, the direction of the river is determined based on the nearest land cell to the river mouth (after it has been placed on a coastal cell). But it's possible that the river might be discharging from a coastal cell that is further away. I also think that my algorithm may be biasing towards Western land-cells. What I probably should be doing is calculating the angle from the original SSM location to the placed LiveOcean location. Based on this angle, the algorithm can decide on the proper direction for the river (and verify that there is indeed a land cell from that direction).

---
## Plan Moving Forward

**This week:**

In the upcoming week, I plan to write the forcing-generation script with TRAPS included. I will need to draw from Ecology's timeseries to create the forcing files. To start, I will only pull flowrate, salinity, and temperature data. I will worry about nutrients once I get this simpler case running.

Later during the week I hope to run the model and debug anything that goes wrong. It will be my first time running LiveOcean, so I'm excited.

**Next week:**

The following week I'm hoping to add nutrients to the new rivers and point sources. Once I get this working, I'll go back and fix the issues I listed earlier on. I will also spend some time checking my code to make sure it is doing what I think it is doing.