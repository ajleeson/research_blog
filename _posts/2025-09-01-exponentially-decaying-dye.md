## Exponentially Decaying Dye

This week I successfully converted an inert passive dye into a nonconservative dye that decays exponentially! 
However, the dye decayed at a rate that was 8-times smaller than what I defined. Clearly, I still need to do a bit more debugging.

The good news is that there isn't any apparent noise in the model output, but I need to confirm this with more rigorous testing in the coming week.

More details below.

---
## The experiment and the results

In this experiment, I added two inert passive tracer dyes to LiveOcean:
- dye_01
- dye_02

Both of these dyes have zero concentration everywhere, except for a single point source at West Point WWTP, which discharges both dyes at the same concentration.

After verifying that both inert passive dyes worked, I then forced dye_02 to decay exponentially. The movie below shows the concentration differences between dye_01 and dye_02 over the course of one day. 

Because dye_01 and dye_02 discharge from West Point at the same rate and concentration, then anywhere that dye_01 > dye_02 suggests that dye_02 must have decayed.

<p style="text-align:center;"><video src="/research_blog/figures/2025.09.02/dye01_d02_diff.mp4" controls="controls" style="max-width: 800px;"></video><br>Fig. 1 Comparison of dye_01 and dye_02 concentration after one day of run time. The left panel shows surface differences. The right panel shows bottom differences. The pink circle denotes the location of West Point WWTP.</p>
<br>

Below I have also included a binary difference plot to show how much the signal propagates. It seems as though the difference in dye concentrations propagates rather far-- but the propagation doesn't appear noisy. For instance, I'm not seeing any spurious differences at Sinclair Inlet.

<p style="text-align:center;"><video src="/research_blog/figures/2025.09.02/binary_diff.mp4" controls="controls" style="max-width: 800px;"></video><br>Fig. 2 Binary comparison of dye_01 and dye_02 after one day of run time. Black indicates that there are concentration differences between dye_01 and dye_02 in at least one sigma level within a horizontal grid cell.</p>
<br>

However, I still need to investigate noise more rigorously in future experiments to verify that the tracer implementation is truly noise-free.

---
## Interventions in the code

**fennel.h interventions**

<p style="text-align:center;"><img src="/research_blog/figures/2025.09.02/fennel01.png" width="500"/><br>Initialize idye index.</p><br>

<p style="text-align:center;"><img src="/research_blog/figures/2025.09.02/fennel02.png" width="500"/><br>Initialize scratch arrays for dye.</p><br>

<p style="text-align:center;"><img src="/research_blog/figures/2025.09.02/fennel03.png" width="500"/><br>Add dye concentrations to scratch arrays.</p><br>

<p style="text-align:center;"><img src="/research_blog/figures/2025.09.02/fennel04.png" width="500"/><br>Use backward-implicit exponential decay scheme on dye_02, which corresponds to inert(2).</p><br>

<p style="text-align:center;"><img src="/research_blog/figures/2025.09.02/fennel05.png" width="500"/><br>Update global tracer array with dye concentrations from scratch array.</p><br>

**fennel_def_pio.h**

<p style="text-align:center;"><img src="/research_blog/figures/2025.09.02/fennel_def_pio.png" width="500"/><br>Print the input exponential decay rate, decay_dye2, into output netCDF file.</p><br>

**fennel_def.h**

<p style="text-align:center;"><img src="/research_blog/figures/2025.09.02/fennel_def.png" width="500"/><br>Print the input exponential decay rate, decay_dye2, into output netCDF file.</p><br>

**fennel_inp.h**

<p style="text-align:center;"><img src="/research_blog/figures/2025.09.02/fennel_inp01.png" width="500"/><br>Read in the specified exponential decay rate, decay_dye02.</p><br>

<p style="text-align:center;"><img src="/research_blog/figures/2025.09.02/fennel_inp02.png" width="500"/><br>Print the specified exponential decay rate, decay_dye02, to log.txt file.</p><br>

**fennel_mod.h**

<p style="text-align:center;"><img src="/research_blog/figures/2025.09.02/fennel_mod01.png" width="500"/><br>Declare the specified exponential decay rate, decay_dye02.</p><br>

<p style="text-align:center;"><img src="/research_blog/figures/2025.09.02/fennel_mod02.png" width="500"/><br>Allocate the specified exponential decay rate values to the variable decay_dye02.</p><br>

**fennel_var.h**

<p style="text-align:center;"><img src="/research_blog/figures/2025.09.02/fennel_var01.png" width="500"/><br></p>

<p style="text-align:center;"><img src="/research_blog/figures/2025.09.02/fennel_var02.png" width="500"/><br></p>

<p style="text-align:center;"><img src="/research_blog/figures/2025.09.02/fennel_var03.png" width="500"/><br>Assign metadata indices for the decay_dye02 variable.</p><br>

**fennel_wrt_pio.h**

<p style="text-align:center;"><img src="/research_blog/figures/2025.09.02/fennel_wrt_pio.png" width="500"/><br>Prints exponential decay rate, decay_dye02, to output file.</p><br>

**fennel_wrt.h**

<p style="text-align:center;"><img src="/research_blog/figures/2025.09.02/fennel_wrt.png" width="500"/><br>Prints exponential decay rate, decay_dye02, to output file.</p><br>

**bio_Fennel_BLANK.in**

<p style="text-align:center;"><img src="/research_blog/figures/2025.09.02/bio_fennel_blank.png" width="500"/><br>Specify the value of the exponential decay rate, decay_dye02.</p><br>


---
## Bug in the decay rate

I specified an exponential decay rate of 8E-6 1/s in my input file (see above code snippet), which corresponds to a half-life of 1 day. However, ROMS used a value of 1E-6 1/s. In both the log.txt file and the output history files, ROMS prints:

<p style="text-align:center;"><img src="/research_blog/figures/2025.09.02/decay_val.png" width="500"/><br></p>

I am not sure why I am seeing this bug and I need to spend some time understanding why this problem popped up.



