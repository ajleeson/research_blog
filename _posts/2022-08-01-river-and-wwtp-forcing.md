
## Simultaneous River and WWTP Forcing

This post is a continuation of last week's update, [WWTP Lat/Lon to Point Source Dischargers](https://ajleeson.github.io/research_blog/2022/07/25/WWTP-PointSource-Algorithm.html).

Last week I developed an algorithm that places point source dischargers on the grid, and I ran the model with WWTP forcing for two days. This week I created a forcing generation script that allows us to define both river and WWTP forcing. The results after two days are shown in Figure 1.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/182209987-9a99499f-661f-4548-b302-ffbbf8b2f736.png" width="600"/><br>Fig 1. Surface salinity at last hour of two-day point source and river test run.</p><br><br>

The river and WWTPs are both forced with freshwater. The transport are both 1000 m3 s-1, uniformally distributed between the sigma layers. Even though forcing for the river and WWTPs are the same in this case, the code is written such that they can have different transports, vertical distrubutions, temperature, etc.

The temperature of the river, WWTP discharge, and ocean are all defined to be 10 C in the forcing files. However, I am still observing a temperature anomaly on the coastal plants (Figure 2). Despite attempts at debugging, I am still unsure why this anomaly is occuring and how to prevent it from happening.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/182222359-11818fb5-b3e2-4463-9dff-37730b6b925b.png" width="600"/><br>Fig 2. Surface temperature at last hour of two-day point source and river test run.</p><br><br>

The next sections provide more detail about the forcing script and temperature anomaly troubleshooting.
<br><br>

---
## Simultaneous Forcing Script

It was actually relatively straightforward to create a script that writes forcing information for both rivers and WWTPs.

Parker had already written rivA0, which is a script that writes a ROMS forcing file for rivers using information saved during grid generation (river_info.csv).

Last week I had modified rivaA0 to create a ROMS forcing file for point sources using information saved during grid generation for wwtps (wwtp_info.csv).

To combine these, I added the WWTP forcing information to the end of rivA0, after the river forcing information.

The first half of the script now creates a dataset specific to rivers. The second half of the script now creates a dataset specific to WWTPs. The datasets provide forcing inputs for all time at each of the rivers and WWTPs. At the end of the script, I simply merge the two datasets and save the result as a .nc file for ROMS.

I was worried that ROMS would get confused since I have enabled both LuvSrc and LwSrc (since river transport comes from u- and v-faces, while WWTP transport comes from w-faces). However, ROMS seems to perform just fine as long as the river_direction is provided in the forcing file. For LuvSrc (rivers), the river_direction variable is either 0 or 1. For LwSrc (WWTPs), the river_direction variable is always 2.

Figure 3 shows an excerpt from the ROMS log file. Since this simulation had 1 river and 5 WWTPs, this excerpt convinced me that things are working properly.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/182446387-9a998118-eee3-484f-b88f-990fc9b87b81.png" width="700"/><br>Fig 3. Excerpt from ROMS log file for point source flags.</p><br><br>

---
## Temperature Anomalies

A cross-section movie of the temperature anomalies is shown below.<br><br>

<video src="https://user-images.githubusercontent.com/15829099/182450731-dec81604-70d5-4c9d-ae45-304abbfe9a0a.mp4" controls="controls" style="max-width: 800px;">
</video><br><br>

I do not know why these temperature anomalies are occurring. They appear at the second timestep, as if the coastal point sources are being forced with a different temperature. The warmer source is ~15 C, the colder source is ~4 C. It is strange to me that this phenomenon is most prominent at the two coastal sources. The ocean point source is also slightly cooler than 10 C. The point sources and river within the estuary do not appear to have a temperature anomaly.

I have checked all of the forcing inputs, and they are 10 C everywhere for all time. At the first timestep, the water is also at 10 C everywhere.

It is also interesting that there seems to be a larger anomaly near the surface (ie. warmest and coldest points are at the surface). I checked the analytical surface fluxes, and there is zero surface heat flux.There is zero bottom heat flux too.

I've exhausted all of my hypotheses at this point. The next step is to browse the ROMS forums and see if anyone else has encountered a similar issue.