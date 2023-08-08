## A shiny, new version of TRAPS

I spent this week swimming in my TRAPS code. Last week I mentioned that I had converted Ecology's excel loading files into netCDF files. This week, I focused on updating the TRAPS code to work compatibly with the new netCDF files. I also took the opportunity to clean and restructure a lot of code.

The new structure of TRAPS code is shown in Figure 1.

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/505fafff-ed6e-4b78-9b42-cf708503d7f7" width="1000"/><br>Fig 1. (new) Structure of TRAPS code.</p><br>

Refactoring the TRAPS code has taken a lot of time and focus (it was a shocking mess). However, it should be worth the effort, allowing new users to implement TRAPS more easily.

Other added features to the TRAPS code include:
- Optional plotting to speed up climatology scripts
- Removal of 2013/2014 data from rivers based on Big Beef Creek gage
- Identifying and correcting rivers with one or two weird biogeochemistry profiles
- Making TRAPS placement a standalone script that the user only needs to run once

There are still a few loose ends that I need to tie up before I release the final version to GitHub. These include:
- Re-visiting Comox/Vancouver Isl N river confusion
- Propogating climatology updates to pre-existing LO river biology
- Deciding how to handle WWTPs that opened/closed on a given year
- Separting make_forcing_main.py script into one short, main script with three helper scripts (as shown in Figure 1)
- Adding argument parsing
- Writing READMEs