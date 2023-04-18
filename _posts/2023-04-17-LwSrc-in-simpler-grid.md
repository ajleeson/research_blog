## LwSrc Tests in Even Simpler Grid

This week I have been running LwSrc tests in an even simpler grid, courtesy of John Wilkin. I am able to reproduce strange w values in my runs. However, I am unable to reproduce John's w results even when I run his model without any modification. Maybe something is uniquely problematic with the way I am running ROMS. More details below.

## Note on Volume Conservation

Before we dive into the details of the new grid, I'd like to bring closure to the issue of volume conservation in the upwelling test case. (Spoiler: volume is conserved! Whew!)

Last week I shared preliminary results in which I compared the point source discharge rate to the surface integrated rate of sea level rise:
$$\int_A \frac{\partial\zeta}{\partial t} dA$$

As you may recall, volume was not conserved between the 10 m3/s point source and the 10.75 m3/s sea level rise.

This week I identified and resolved the issue. As I had originally thought, the horizontal area of each grid cell in the upwelling model is indeed 1000 m2. The domain is 80,000 m long, and 41,000 m wide, and each cell has dimensions of 1000 m x 1000 m. What I missed in my analysis last week is that the full model is defined by 82 x 43 horizontal grid cells. Essentially, the horizontal perimeter of the domain is surrounded by an extra set of grid cells, which is described in detail on the [Wiki ROMS](https://www.myroms.org/wiki/Numerical_Solution_Technique).

Once I removed the edge cells from my area integral, I obtained a 10 m3/s sea level rise rate. Thus, volume is conserved when using LwSrc (at least in this case).

---
## Running John Wilkin's Model As-Is

Last week John Wilkin shared a very simple ROMS test case to work with. The model set-up is user friendly, and it is easy to adjust the inputs. However, I wanted to start by running the model on klone as-is. John shared some of his preliminary results with me, so comparing my results to his will give me confidence that I've set things up proprerly on klone. The as-is set up had the following characteristics:

- 5 km x 5 km domain
- 20 m deep flat-bottom
- 30 sigma layers
- Periodic BCs on all edges
- No Coriolis
- No wind
- Initial thermal stratification of 0.3 degC/m
- 10 m3/s freshwater source discharging to bottom sigma layer
  - Source located at the very center of the domain

Initially, I ran the model for four minutes of model time with a 6-second timestep (which is how it was set up when I received the test case). Figure 1 shows the resulting surface parameters at each timestep.

<p style="text-align:center;"><video src="https://user-images.githubusercontent.com/15829099/232638906-e021dcef-9d57-42e0-93c1-b8083e82f107.mp4" controls="controls" style="max-width: 550px;"></video><br>Fig. 1 Surface velocity and SSH in test case as-is.</p><br>

In this simple model, I am still observing a constant, negative, surface vertical velocity at the location of the vertical source.

For completion, the next few videos show the section evolution of salinity, u, v, and w.

<p style="text-align:center;"><video src="https://user-images.githubusercontent.com/15829099/232668713-4375ff16-85d9-4628-96ea-1314c568918e.mp4" controls="controls" style="max-width: 800px;"></video><br>Fig. 2 Section salinity in test case as-is.</p><br>

<p style="text-align:center;"><video src="https://user-images.githubusercontent.com/15829099/232668775-8c332ba7-96a8-4b7a-bf55-ba24dd89bbbc.mp4" controls="controls" style="max-width: 800px;"></video><br>Fig. 3 Section u in test case as-is.</p><br>

<p style="text-align:center;"><video src="https://user-images.githubusercontent.com/15829099/232668839-7dbfa523-ee8e-4074-8524-8beedd50eb6e.mp4" controls="controls" style="max-width: 800px;"></video><br>Fig. 4 Section v in test case as-is.</p><br>

<p style="text-align:center;"><video src="https://user-images.githubusercontent.com/15829099/232668874-b7db716b-9285-43ac-931f-e2afd728a262.mp4" controls="controls" style="max-width: 800px;"></video><br>Fig. 5 Section w in test case as-is.</p><br>

Salinity, u, and v generally seem to match John's results. However, w looks different. Early in his run, he observed a mostly downward flow towards the source with greatest magnitude near the bottom. In the beginning of my run, I initially observe an upward flow away from the source. Later on in the run, John observed an upward plume directly over the source, with downward flow in surrounding cells. In my run, the plume has upward velocity in the lower-half of the water column before switching to downward flow in the upper-half of the water column.

I ran this exact same setup for 15 minutes of model time to see if the vertical velocity reaches a steady-state that more closely resembles John's results. I also lowered the max and min values of the colorbar. Many of the values are now oversaturated, but we can at least better resolve the smaller velocities. The resulting section plot of w is shown in Figure 6.

<p style="text-align:center;"><video src="https://user-images.githubusercontent.com/15829099/232669670-4f33ccd6-27d3-45c5-83a9-23474048b41c.mp4" controls="controls" style="max-width: 800px;"></video><br>Fig. 6 Section w in test case as-is (15 min run).</p><br>

I am still unable to replicate John's results. For one, his run does not appear to have a negative vertical velocity at the surface.

Perhaps the results he shared do not exactly correspond to the "as-is" conditions in the files he shared. However, I'm now wondering whether some strange offset is being added to my code. Specifically, I've been curious whether my values for w are all shifted by the surface downwards velocity (-6e-4 m/s in this run), but they are only shifted in the water column that contains the WWTP. It's a big mystery, and I'll need to probe more.

On a side note, I checked volume conservation for this run based on sea level rise, and it is indeed conserved.

---
## Testing a Neutrally Buoyant Source

To remove more complexity, I set LtracerSrc = F F in John's model so that inflowing water is now neutrally buoyant. The source is no longer discharging freshwater. No other changes were made.

It is worth noting that John generated his vertical source by modifying the ROMS analytical code. In contrast, I have always been generating forcing files with Python, and telling ROMS to look for these forcing files. Because John's model was easy to manipulate, I stuck with his analytical code. In theory, both methods should produce the same results, but I haven't tested the comparison yet. Perhaps that could be an exercise for later this week.

In any case, I did take a look at his code and confirmed that there is a single vertical source discharging at 10 m3/s to solely the bottom sigma layer (as far as I can tell from my limited Fortran knowledge). These conditions match what I had previously been doing in the upwelling test case.

Then I ran the model for 15 minutes of model time. The surface velocities and SSH are shown in Figure 7.

<p style="text-align:center;"><video src="https://user-images.githubusercontent.com/15829099/232672455-bdf32243-7485-46d9-b2ef-5a4d8e8bffb4.mp4" controls="controls" style="max-width: 550px;"></video><br>Fig. 7 Surface velocity and SSH in test case with neutrally buoyant source.</p><br>

A section movie of w is provided in Figure 8.

<p style="text-align:center;"><video src="https://user-images.githubusercontent.com/15829099/232672488-d546d054-1352-4fbc-b362-9b6f181a2955.mp4" controls="controls" style="max-width: 800px;"></video><br>Fig. 8 Section w in test case with neutrally buoyant source.</p><br>

The major difference between this run and the prior "as-is" case is that w does not seem to entrain as much fluid in the surrounding cells. To me, this makes some physical sense because I imagine a positively buoyant source to rise upwards and entrain surrounding fluid in the process. However, as Alex mentioned last week, it is weird that the large magnitude velocites in the neutrally buoyant case are not entraining surrounding fluid. (Perhaps this observation corroborates my theory that just the WWTP-containing water column has an offset in w).

Another observation of note is that in both the neutrally buoyant test case and the as-is test case, the vertical velocity at the surface is -6e-4 m/s.

---
## The Offset Theory

I have a few more thoughts on the "offset theory" that I put forward. To be explicit, the offset theory is my hypothesis that w has a constant negative offset in only the vertical column that contains the vertical source.

First, my results for salinity, u, v, and w generally seem in agreement with John's results, *except* for w in the WWTP-containing column. Most notably, his vertical velocities appear to be closer to zero at the surface, while mine *always* have a constant negative value at the surface. Because all other parameters seem similar, and w for the most part seems similar, I am quite confused why a single vertical column could differ. To the best of my knowledge, all of the data are being handled in the same way.

Second, if this offset is indeed occuring, I am guessing the offset is happening sometime before my plotting code (though I could be very wrong on this). Something is causing the WWTPs in LiveOcean to blow up, and the strange vertical velocities have been the biggest clue so far. Could a strange offset be the culprit? However, a counterargument might be that the strange vertical velocities do not seem to entrain surrounding fluid. So maybe the offset is actually occuring at the plotting step.

Third, this offset does not appear to be related to the difference between the "Fortran analytical method" versus the "Python forcing file" method of adding sources given that I have seen strange w values from both methods.

So, how do I test this offset theory? Part of the answer may be to make sense of the surface w value. As Parker wondered last week, what is the significance of the constant negative velocity at the surface? Does it equal anything of note? Could this be our clue?

When I take a step back, I am also quite confused that John's results and my results differ. It may help to make an explicit list of the differences between our runs. As a start, these are the minor changes I made simply to get the model running on klone:

- Added all files to a new folder in LO_roms_user
- Copied over build_roms.sh from the upwelling test case so I could compile John's test case in the new folder
- Copied over, and modified, a version of Parker's klone_batch.sh script, which instructs klone to run the new model
- In the dot-in file, updated the path to the varinfo.yaml file to point to ORIG_varinfo.yaml (rather than the modified version we use in LiveOcean)
- Changed the tiling scheme from 1x1 to 2x2 to be compatible with 4 cores