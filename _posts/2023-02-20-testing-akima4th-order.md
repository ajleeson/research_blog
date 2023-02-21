## Testing Akima 4th Order Advection Scheme

**The good news:**
This past week I gave a presentation to the EFM group, I finished some drafts for the KC quarterly meeting, I was accepted to the GRS and GRC (yay), and I was able to carve out some time for ROMS tests.

**The not quite as good news:** I switched to an Akima 4th order advection scheme, but still didn't having any luck with WWTPs. More details below.

---
## Akima 4

Per a recommendation from the ROMS forum, this week I tested the Akima 4th order advection scheme for salt and temperature in place of using MPDATA. The change was implemented in BLANK.in, and I simply needed to change the "MPDATA" advection options to "A4" (Fig. 1).

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/220171795-b71c69d6-92af-424c-ac61-dd9233117efd.png" width="500"/><br>Fig 1. BLANK.in updated with Akima 4 advection scheme.</p><br>

I can make sure I implemented this correctly by checking the ROMS log file (Fig. 2). The ROMS log file has really helped me keep my test conditions straight-- it serves as a good sanity check.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/220171799-b7a94823-9a24-4312-9336-f0b930837daa.png" width="400"/><br>Fig 2. Snippet from ROMS log file confirming that Akima 4th order advection is being used.</p><br>

Sadly, ROMS still blew up at Oak Harbor Lagoon WWTP and Birch Bay WWTP. Both model runs did make it one hour longer than usual, but the issue has not been resolved. Figure 3 shows the surface velocities at Birch Bay WWTP prior to blowup, and Figure 4 shows the surface velocities at Oak Harbor Lagoon WWTP prior to blowup.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/220171801-21526a95-c79d-4a47-ae16-1880dc9933cb.png" width="750"/><br>Fig 3. Surface velocities near Birch Bay WWTP on hour 21 of 2020.01.01. This run used the Akima 4th order advection scheme, and it did blow up.</p><br>

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/220172164-5b2b9628-18ad-4e43-a810-3cf2242f4388.png" width="750"/><br>Fig 4. Surface velocities near Oak Harbor Lagoon WWTP on hour 21 of 2021.02.19. This run used the Akima 4th order advection scheme, and it did blow up.</p><br>

The ROMS log file attributed both of these failures to density blowing up near the WWTP.

---
## Akima 4 and Kantha-Clayson Combination

A few weeks ago, we found that we could eke out a success at Oak Harbor Lagoon WWTP when running with the Kantha-Clayson closure model. However, the surface velocities still look a bit unrealistic (Fig. 5).

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/217307508-a8216cfc-966d-473b-9118-b063615e308e.png" width="750"/><br>Fig 5. Surface velocities at hour 20 on 2021.02.19 near Oak Harbor Lagoon WWTP. This run was initialized on 2021.02.18 with the Kantha-Clayson closure model, and it did not blow up.</p><br>

ROMS still blew up at Birch Bay WWTP, even when using the Kantha-Clayson closure model. Thus, there is certainly still room for improvement.

Because Kantha-Clayson has so far been the closest success story we've seen, I wanted to try a combination run using Kantha-Clayson and the Akima 4 advection scheme.

However, this combination run also blew up at Oak Harbor Lagoon WWTP. Surface velocities are shown in Figure 6. Alex mentioned that Kantha-Clayson may simply be suppressing the symptoms of blowup without actually helping the cause of blowup. These new results might be more evidence that Kantha-Clayson isn't the right solution.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/220189858-763388be-8e8c-49a1-9ecd-c7ca0b3c9e57.png" width="750"/><br>Fig 6. Surface velocities at hour 20 on 2021.02.19 near Oak Harbor Lagoon WWTP. This run was initialized on 2021.02.18 with a combination of the Kantha-Clayson closure model and Akima 4th order advection, and it blew up.</p><br>

---
## Next Steps: Ramping up WWTP Discharge

Parker had previously recommended that I try to ramp up WWTP discharge (rather than suddenly introducing the full discharge amount on the first day of the year). Perhaps this could help issues at Birch Bay WWTP.

During my EFM talk last week, Morteza also seemed shocked that I haven't implemented a ramping function.

I've finally exhausted most of the suggestions from the ROMS forum, so as a next step I will try to add a ramping function.

I'll also share my latest results with the ROMS discussion forum and see if they have any other ideas.