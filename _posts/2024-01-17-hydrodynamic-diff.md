## Why are there hydrodynamic differences?

### Recap

Yesterday, I shared that the N-less run's hydrodynamics are different than the long hindcast run's.

As we discussed in our meeting, I have run a few experiments in an attempt to identify the cause of the hydrodynamic deviations.

First, I checked the hydrodynamic deviations at the end of day 1 of the N-less run and verified that I could observe some differences.

Then, I ran the N-less model for one day (2013.01.01) using three different initialization combinations:

1. perfect restart (on 5 nodes)
2. 10 nodes (using continuation)
3. perfect restart and 10 nodes (match long hindcast)

Recall that I initialized the  OG N-less run using **continuation and 5 nodes** and the long hindcast used **perfect restart and 10 nodes**.

### Results

At the end of day 1 in the OG N-less run, it was difficult to see any hydrodynamic differences on a scale of -1 to 1 m/s. However, a scale from -0.1 to 0.1 m/s made the differences very apparent. Thus, -0.1 to 0.1 m/s is the scale I use for all figures on this post.

Also, since u and v tend to have deviations of the same magnitude in the same region, I am only showing u.

Furthermore, the hydrodynamic deviations are most apparent in the surface layer, so I am only showing results from the surface.

Thus, these figures are showing differences in surface u compared to the long hindcast run on a scale from -0.1 to 0.1 m/s.

Figure 1 shows the hydrodynamic difference in Puget Sound, and Figure 2 shows the hydrodynamic difference in the Salish Sea.

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/01de2f43-bc8c-4c04-8ea6-679db0b32ec5" width="800"/><br>Fig 1. Hydrodynamic difference between long hindcast and N-less runs in Puget Sound.
</p><br>

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/c682f75e-877a-49e7-b51c-8ab53fbbe0c0" width="800"/><br>Fig 2. Hydrodynamic difference between long hindcast and N-less runs in Salish Sea.
</p><br>

### Summary

While the perfect restart does not fully eliminate hydrodynamic differences, it does significantly reduce them compared to the OG N-less run. However, it seems that both a perfect restart **and** the same number of nodes (i.e. 10 nodes) are required to reproduce the hydrodynamics of the long hindcast run.