## The ROMS and twinning grind

This week I have been debugging our gradient BC experiment and beginning twin variables implementation. Neither of these battles are complete, but I am steadily making progress. More details below.

---
## Gradient BC experiment

This past week, I set up an OAE experiment to run using Gradient BCs for TIC and alkalinity, and the standard Radiation + Nudging for everything else. This experiments involves re-running both the baseline and perturbation experiment using the Gradient BCs. 

To modify the TIC and alkalinity BCs I made two edits to the LO code. First, I added a new "oae" option in our make_dot_in file, which populates the ROMS input files (Fig 1). Then, I modified the biology input file so that TIC and alkalinity use the "oae" option BC (Fig 2).

<p style="text-align:center;"><img src="/research_blog/figures/2026.10.26/make_dot_in_edits.png" width="300"/><br>Fig 1. Modified make_dot_in.py code that populates the BCs for LiveOcean.</p><br>

<p style="text-align:center;"><img src="/research_blog/figures/2026.10.26/bio_Fennel_BLANK_dot_in_edit.png" width="600"/><br>Fig 2. Modified bio_Fennel_BLANK.in code that lists the BC options for the bgc state variables.</p><br>

Presumably, these two pieces of code should generate a biology input file with Radiation + Nudging BCs for all state variables, except TIC and alkalinity, which should have Gradient BCs. However, that did not happen.

I verified that the generated bio_Banas.in file (our biology input file) has the correct BCs. I also verified that the ROMS log.txt file is reading the correct bio_Banas.in file. However, for whatever reason, the ROMS log.txt file is reporting Radiation + Nudging BCs for all bgc state variables, including TIC and alkalinity.

Now, I have been in debugging mode. I tried:
- Changing the salt/temp BCs to something else, and verified that they correctly change without impacting the bgc state variables. This tells us that the physical BCs are correctly NOT overriding the bgc BCs.
- Changing the NO3 BC to use the "oae" option, and verified that the NO3 BCs correctly changed to Gradient. I also verified that the other bgc state variables remained Radiation + Nudging. This tells us that the first line of the bio input file gets read correctly (i.e., the line in Figure 2 that begins "LBC(isTvar) == ...")

Thus, the problems seems to be reading beyond the first line of the bgc BCs in Figure 2. 

Note that my BC formatting (i.e., Figure 2) also matches the style and format used by the Fennel group, where they list "LBC(isTvar) == ..." once, then separate all of the bgc state variables with a backslash line break.

I'm still puzzling through what to do now and will continue trying to debug. It is important to debug this issue because I eventually plan to use different BCs for the delta TIC and delta alkalinity state variables. Thus, I need to make sure we can properly implement different BCs for different bgc state variables. Any thoughts are welcome.

---
## Twin variables

Thankfully, Laurent et al of the Fennel group added a new cpp flag called "TALK_ADDITION" and "TALK_FILE" associated with the twin variables and alkalinity addition forcing file. Thus, I simply need to search for these two cpp defs to identify all of the changes they made to the ROMS source code.

In total, Laurent et al modified 27 files:
- /Include/cppdefs.h
- /Modules/mod_ncparam.F
- /Modules/mod_ocean.F
- /Modules/mod_forces.F
- /Modules/mod_average.F
- /Utility/wrt_diags.F
- /Utility/set_diags.F
- /Utility/def_avg.F
- /Utility/wrt_his.F
- /Utility/def_quick.F (I don't think I need this because I'm not doing quicksaves)
- /Utility/wrt_quick.F (I don't think I need this because I'm not doing quicksaves)
- /Utility/def_his.F
- /Utility/wrt_avg.F
- /Utility/checkdefs.F
- /Utility/read_phypar.F
- /Nonlinear/get_data.F
- /Nonlinear/set_data.F
- /Nonlinear/set_avg.F
- /Nonlinear/Biology/reduced_bgc_inp.h
- /Nonlinear/Biology/reduced_bgc.h (where the main twinning occurs, equivalent to our fennel.h)
- /Nonlinear/Biology/reduced_bgc_var.h
- /Nonlinear/Biology/reduced_bgc_wrt.h
- /Nonlinear/Biology/reduced_bgc_mod.h
- /Nonlinear/Biology/reduced_bgc_def.h
- /User/Include/oae.h
- /User/External/reduced_bgc.in
- /User/External/varinfo_oae.dat (see line 2215)

Many of these modifications are related to reading the alkalinity forcing inputs. Fewer of them are related to the twin variables implementation. 

My current plan is to first implement the twin variables, run some tests, then implement the alkalinity addition files. 

Right now, I have been slowly making my way through these files and copying the relevant changes from Laurent et al. Figure 3 shows an example change, where I have modified the file that writes diagnostic outputs. Instead of writing a single pCO2 value, it will now write two different pCO2 values-- one for the control condition and one for the addition condition.

<p style="text-align:center;"><img src="/research_blog/figures/2026.10.26/wrt_diags_edit.png" width="600"/><br>Fig 3. Example twin-variable edit in wrt_diags.F.</p><br>