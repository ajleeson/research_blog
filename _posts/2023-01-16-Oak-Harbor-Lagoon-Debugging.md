## Oak Harbor Lagoon WWTP Debugging

This week I tested a run without point sources, and ROMS did not blow up. I shared the point source blow up issue on the ROMS forum and received a good amount of feedback and some ideas for future experiments. I also investigated WWTP placement on the rho grid, resolving some misunderstandings and identifying another new bug.

---
## Comparison With and Without Point Sources

In last week's update, I shared that LiveOcean crashed on 2021.02.19 near Oak Harbor Lagoon WWTP. This week, I ran a continuation run starting on 2021.02.17, except in this continuation I turned off all point sources such that there were only pre-existing rivers and tiny rivers. This new run did not crash.

Figures 1 through 4 below show comparisons of the "with point sources" and "without point sources" runs. 

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/212557264-68abb019-947a-4294-a586-ab7b2f810313.png" width="750"/><br>Fig 1. Comparison of surface velocity profiles near Oak Harbor Lagoon WWTP just before the "with point sources" run blew up.</p><br>

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/212557269-e2e8a213-1a8a-4098-8403-f76b5ea7e69d.png" width="850"/><br>Fig 2. Comparison of sigma layers at location of Oak Harbor Lagoon WWTP on 2021.02.19. The dashed line on the "without point sources" plots indicate time of blowup in the "with point sources" case.</p><br>

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/212557270-87c2fd6a-1d94-4a0a-9334-671e52537b8e.png" width="850"/><br>Fig 3. Comparison of depth vs. velocity at location of Oak Harbor Lagoon WWTP on 2021.02.19. The dashed line on the "without point sources" plots indicate time of blowup in the "with point sources" case.</p><br>

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/212557272-1737b895-b865-424b-930a-72ca9635f95c.png" width="850"/><br>Fig 4. Comparison of depth vs. other parameters at location of Oak Harbor Lagoon WWTP on 2021.02.19. The dashed line on the "without point sources" plots indicate time of blowup in the "with point sources" case.</p><br>

Not only did the velocities spike right before the "with point sources" case blew up, but the sea surface height also made a dramatic jump.

I still do not understand why WWTPs are causing problems, but this experiment confirms that WWTPs are indeed the sources that cause problems.

---
## Feedback from ROMS Forum

One of the main concerns that the ROMS community shared is that the point source transport may cause a violation in the CFL condition.

Transport for point sources is relatively small compared to rivers, but point sources discharge from only the bottom sigma layer while rivers are spread between all sigma layers.

In the upcoming week, I will experiment with point sources having vertically distributed transport, just like rivers.

---
## Point Source Placement on the Rho-Grid

This week I revisited the TRAPS placement algorithm. Simply by luck, it seems like I based the algorithm on the rho-grid, meaning that WWTPs were placed at the centerpoint of the rho-grid. In other words, this algorithm was defining the correct row/col indices to place a WWTP, based on the rho-grid. [Side note: I'll try not to rely on luck from now on]

The problem, however, is that this placement algorithm is written in Python which indexes from (0,0). To be consistent with ROMS indexing conventions, I may have needed to add 1 to the row/col indices when I generated forcing files(still a bit more investigation needed).

The effect of this Python/ROMS indexing inconsistency would mean that WWTPS are offset diagonally by one grid cell from where the placement algorithm intended to put them. Unless a WWTP was accidentally mapped to a land cell, I don't expect this offset to be causing ROMS to blow up. In the upcoming week I will resolve and test the solution to this issue.