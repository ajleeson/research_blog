## Continued Oak Harbor Lagoon WWTP Debugging

This week I continued my attempts to debug Oak Harbor Lagoon WWTP. I repeated some experiments from last week using a *perfect restart* initialization. I ran a new experiment with WWTPs discharging to all sigma layers rather than just the bottom or bottom third. So far, removing WWTPs is the only reliable way to ensure that ROMS will run. Changing the vertical profile does not prevent ROMS from crashing. I took these results to the [ROMS forum](https://www.myroms.org/forum/viewtopic.php?t=6250&sid=4bdef9bdea3fb338c5aec5a7064d7847) again, and I am currently trying to understand the feedback I received. More details below.

---
## Repeating Experiments

Over the past two weeks I ran experiments at Oak Harbor Lagoon WWTP using a *continuation* initialization. However, at the end of last week I realized that the blowup issue could not be reproduced using *continuation*. I could only reproduce the blow up when initializing a run with *perfect restart*. Thus, I needed to repeat these past experiments.

### Reproducing Failure

By using a *perfect restart* initialization on 2021.02.18 with WWTPs discharging to just the bottom sigma layer, I could reproduce my intial failure at Oak Harbor Lagoon WWTP. The surface velocities for this run prior to blow up are shown in Figure 1.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/215623063-025a6c9d-d7cd-4cc6-9bb6-ec6034a72bc2.png" width="750"/><br>Fig 1. Surface velocities at hour 20 on 2021.02.19 near Oak Harbor Lagoon WWTP. This run was initialized on 2021.02.18 with WWTPs, and it blew up.</p><br>

### Removing all WWTPs

First, I removed all WWTPs and re-initialized a run on 2021.02.18 (one day prior to blow up). The model ran successfully, which confirms that WWTPs are indeed the source of the issue. Figure 2 shows the surface velocities near what would be Oak Harbor Lagoon WWTP.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/215623065-8375c789-860d-4884-8f47-6bf279b095b3.png" width="750"/><br>Fig 2. Surface velocities at hour 20 on 2021.02.19 near what would be Oak Harbor Lagoon WWTP. This run was initialized on 2021.02.18 without WWTPs, and it ran successfully. This surface velocity snapshot corresponds to the same time as Figure 1.</p><br>

### Discharging to the bottom 1/3 sigma layers

Then, I repeated the experiment in which all WWTPs discharge to the bottom 1/3 sigma layers rather than just the bottom sigma layer. This model run crashed at the same time as the original run (hour 21 on 2021.02.19) which suggests that a vertical CFL violation may not be the problem. Figure 3 shows the surface velocities of this run prior to blow up.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/215623061-52dbcb6f-3377-4a2f-8b64-433d8ef79dab.png" width="750"/><br>Fig 3. Surface velocities at hour 20 on 2021.02.19 near Oak Harbor Lagoon WWTP. This run was initialized on 2021.02.18 with WWTPs discharging to the bottom 1/3 sigma layers, and it blew up. This surface velocity snapshot corresponds to the same time as Figure 1.</p><br>

---
## WWTP discharge equally distributed across sigma layers

As a final test, I equally distributed WWTP discharge amongst all sigma layers. Now, the vertical profile of WWTPs matches that of rivers, with the only difference being whether the sources is implemented vertically (LwSrc) or horizontally (LuvSrc).

This run also blew up. In fact, it blew up one hour earlier than the original run. Figure 4 shows surface velocities for this run prior to blow up. Based on these results, I am confident that too much transport is not the issue with WWTPs.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/215623059-e93c2a7a-b2de-459d-a8e9-d864771d9833.png" width="750"/><br>Fig 4. Surface velocities at hour 19 on 2021.02.19 near Oak Harbor Lagoon WWTP. This run was initialized on 2021.02.18 with WWTPs discharging equally to all sigma layers, and it blew up. Note these surface velocities correspond to a time one hour earlier than Figure 1.</p><br>

The following set of figures shows a comparison of depth profiles between the original run (with WWTPs discharging to just the bottom sigma layer), and this latest run (with WWTPs discharging equally to all sigma layers). These figures begin at the start of 2021.02.19 up until ROMS blows up.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/215623066-80b1ae47-dde6-4b7b-a3fb-c381bccdce25.png" width="850"/><br>Fig 5. Comparison of velocities.</p><br>

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/215623056-196fde53-84df-44cc-aaad-733ed9bca4eb.png" width="850"/><br>Fig 6. Comparison of sigma layers.</p><br>

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/215623051-0784211b-9dd3-492d-9837-d164f42c2e5f.png" width="850"/><br>Fig 7. Comparison of salinity, temperature, and select biogeochemistry variables.</p><br>

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/215623054-a8a7041f-adfc-4f2b-a43f-7e4df6474a8f.png" width="850"/><br>Fig 8. Comparison of CFL numbers.</p><br>

Both model runs generally have similar profiles. Some key differences:
- A dramatic jump in SSH is only observed in the "bottom sigma layer" case. Although, this is might be because the "all sigma layers" case blew up one hour earlier. Perhaps the same jump would have been observed if the run had continued for an additional hour. It is unclear.
- Ammonium and nitrate concentrations are greater near the sea floor in the "bottom sigma layer" case, which is reassuring because 30 times more ammonium and nitrate is discharging to the bottom sigma layer compared to the "all sigma layers" case.
- Both model runs experience a vertical homogenization of all variables prior to blowing up

---
## Feedback from ROMS forum

I took these observations to the ROMS forum and received quite a bit of feedback! I need some help understanding all of the comments I'm receiving. However, I've started taking a look at a few suggested items.

First, I put together some plots of density, vertical eddy viscosity, and vertical eddy diffusivity. These are shown in Figure 9.
Density was not an output of the mooring extraction, so I instead calculated it using the gsw library (TEOS-10). This required me to convert depth to pressure, practical salinity to absolute salinity, and potential temperature to conservative temperature.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/215623055-a21e418d-809c-4c06-8807-fcb2214fad21.png" width="850"/><br>Fig 9. Comparison of density and vertical mixing.</p><br>

There doesn't appear to be an obvious density instability. In general, lighter water sits above heavier water.
I'm not familiar with vertical patterns of eddy diffusivity/viscosity, however, I notice that both values increase throughout the water column around the time of vertical homogenization.

Someone also asked to have a look at the transport timeseries of Oak Harbor Lagoon WWTP. I've plotted it in Figure 10 along with Whidbey East tiny river as a comparison. Note that Whidbey east is a tiny river located near Oak Harbor Lagoon WWTP, and is visible on the surface velocity plots.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/215660462-93d3c852-fbe7-4b1c-affa-0001c07e3157.png" width="500"/><br>Fig 10. Oak Harbor Lagoon WWTP and Whidbey East tiny river transport timeseries.</p><br>

These flowrates are what I would expect to see based on the climatology I generated.

All in all, I'd like to continue to address the comments on the ROMS forum so I can provide more information and receive more feedback. 