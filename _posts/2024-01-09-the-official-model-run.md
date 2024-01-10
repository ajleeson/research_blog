## Setting up the official model run

This week I set up and launched the official model run for my master's thesis (**cas7_t0noN_x4b**). I generated TRAPS forcing without NO3 and NH4 in the WWTP effluent and initialized the run for the year 2013. Delayed by a forgetful moment, I did not start the run until Tuesday afternoon after the hyak maintenance was completed on klone.

This blog post details the important steps I took to prepare the model run. It is mostly for my own reference, in case I need to check something in the future. However, I also included some notes at the end proposing a method to improve the forcing generation process.

---
## Setting up the model run

Setting up the model run involved a few different steps. I have broken them down into the following sub-steps:

- N-less WWTP forcing
- All other forcing
- Dot_in folder
- Setting up ROMS
- Initial conditions
- Running the model

### N-less WWTP forcing

To generate N-less WWTP forcing, I first copied the most up-to-date version of TRAPS forcing (trapsF00) into my LO_user folder. Then, I renamed the forcing folder as "trapsF01" and modified LO_user/forcing/trapsF01/make_wwtp_forcing.py to make WWTPs discharge zero NO3 and NH4 (Fig 1).

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/171be5c8-3136-4e1b-bdd3-3773b967e043" width="800"/>Fig 1. Change made to make_wwtp_forcing.py to make WWTPs discharge zero NO3 and NH4.
</p><br>

I used this new script to generate river and WWTP forcing for the year 2013. Figures 2 - 4 show the resulting forcing input timeseries for WWTPs, tiny rivers, and pre-existing LO rivers. Based on these figures, the forcing looks as expected.

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/9c5770bf-5705-4541-8cfd-5a9de33c453e" width="800"/>Fig 2. Resulting WWTP forcing timeseries. Note that NO3 and NH4 concentrations are always zero.
</p><br>

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/510e2072-6abb-458f-ae82-d46b72fcdcb0" width="800"/>Fig 3. Resulting tiny river forcing timeseries.
</p><br>

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/f6d65994-89f1-442d-9985-34b46580d80a" width="800"/>Fig 4. Resulting pre-existing LO river forcing timeseries.
</p><br>

### All other forcing

*See the "Streamline forcing generation" section below for more thoughts on forcing generation.*

To generate forcing for the atmosphere, tides, and ocean, I used the same forcing folders that Parker used for the long hindcast run: atm00, tide00, and ocn01.

Each of these folders attempt to generate forcing using data stored in *my* LO_data directory on the remote machine (e.g. hycom data). However, I don't have these data stored in LO_data. They are stored in *Parker's* LO_data directory. Therefore, I copied the forcing folders into LO_user, and hardcoded Parker's LO_data directory into the script (Fig 5). This is not a method that will be robust to future change. However, it is good enough for now.

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/e3e86f44-b2e7-41f0-b29c-03590a2bc65d" width="500"/>Fig 5. Example code in LO_user/forcing/ocn01/Ofun.py that specifies the filepath to Parker's saved hycom data.
</p><br>

After making these changes, I could generate forcing on perigee.

### Dot_in folder

I copied the long hindcast dot_in folder from LO to LO_user.

Parker's long hindcast run is called "cas7_t0_x4b". So I have called this new run "cas7_t0noN_x4b" because it is identical to the long hindcast run, except that there is no N in the WWTP effluent.

In the forcing_list.csv file, I changed the name of the traps forcing from "trapsF00" to "trapsF01" (Fig 6).

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/d6892b81-1c46-4c5a-b701-5f19f20f3f1f" width="350"/>Fig 6. Update made in forcing_list.csv to use the new N-less TRAPS forcing.
</p><br>

Then in make_dot_in.py, I updated the filepath of lo_tools to be accessible from LO_user (Fig 7).

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/9a05dbe4-bc34-46d8-8eaa-fb9b1f6dec71" width="700"/>Fig 7. Change made in make_dot_in.py to allow access of lo_tools from the LO_user repo.
</p><br>

### Setting up ROMS

Next, I set up the new github version of ROMS on klone. Following the same naming convention as Parker, I cloned the ROMS github repo into a directory called "LO_roms_source_git."

Then, I copied Parker's LO_roms_user/x4b folder in the LO_roms_user/x4b folder on my local pc. I made one small change in build_roms.sh to use my use my directory instead of Parker's on klone (Fig 8).

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/1d61b9da-13c8-4b63-9da8-1baf65a579b8" width="400"/>Fig 8. Modification in LO_roms_user/x4b/build_roms.sh to update root directory.
</p><br>

Then, I compiled ROMS. This process went rather smoothly.

### Initial conditions

The last step prior to running the model was to create an initial condition file. For this, I simply copied over the last history file of 2012 from the long hindcast run.

More specifically, I copied /dat1/parker/LO_roms/cas7_t0_x4b/f2012.12.31/ocean_his_0025.nc from apogee into klone in /mmfs1/gscratch/macc/auroral/LO_roms/cas7_t0noN_x4b/f2012.12.31.

This was more or less a simple copy and rename task.

### Running the model

Finally, I started the model run. Since we already have a history file from 2012.12.31, I initialized my model run as a continuation using the following command on klone in LO/driver:

```
python3 driver_roms3.py -g cas7 -t t0noN -x x4b -r backfill -s continuation -0 2013.01.01 -1 2013.12.31 -np 200 -N 40 < /dev/null > mt_noN_2013_run0.log &
```

So far, it has been running okay. Once I have a month or so of data, I will compare this run to the long hindcast to verify that they are different.

---
## Streamline forcing generation

On monday, I tried to launch the model run. It immediately failed. As it turns out, I had forgotten to generate forcing for the atmosphere, ocean, and tides (doh!).

Have any of you ever watched Food Network late at night? I always found it so painful to see delicious food on the TV but with no means of eating any of it. This is how I felt when I realized that Parker's apogee account contains all of the forcing I need for my model run, but I had no easy way to access it. Thus, I opted to re-generate the forcing. Unfortunately, the forcing generation process takes multiple hours, which delayed the model run until Tuesday afternoon. In fact, I am still generating ocean forcing at the time of writing (but forcing generation should out-pace the model run).

This process is working for now, but I'd like to propose an alternative for future runs. Since Parker has already generated the atmospheric, tidal, and oceanic forcing for the long hindcast run, it makes more sense to use the forcing files he has already created. Therefore, I would like to modify the driver_roms3 script to search for forcing files in Parker's apogee account rather than my perigee account. The exception, of course, is that the script should look in my perigee account for the new TRAPS forcing. I could call the new script "driver_roms3_AL." I am open to alternative solutions as well.

---
### To-Do's for this week

### Technical work

**Modify driver_roms3 to use long hindcast forcing**
- *Notes:* Having a script that uses the forcing files Parker generated for the long hindcast run will speed up my modeling workflow for furture model runs
- *Anticipated obstacles:* It will be difficult to test this script since the nodes on klone are busy being used for active model runs. I will need to think through the best way to verify that the new script is working.

**Compare N-less run to long hindcast**
- *Notes:* After a month (of model time) or so, I will compare my N-less run to the long hindcast to verify that my TRAPS changes are indeed present.
- *Anticipated obstacles:* I'll need to dig up some old plotting code (which is, admittedly, a mess). If I have time, I may clean some of this plotting code while I adapt it for use in this new application.

### Learning and collaborating

**Lit review of hypoxia studies**
- *Notes:* As Alex suggested, I will read literature on different hypoxic estuaries. The goal of this exercise is to create a brief summary of the drivers and processes leading to hypoxia in the different systems. Hopefully this work will inform a hypothesis for my own thesis.
- *Anticipated obstacles:* I have a tendency to fall asleep every time I read. Last week, I came up with a plan to take active notes to hopefully stay awake and focused.

### Housekeeping

**Submit traps pull request**
- *Notes:* While generating new traps forcing, I identified a few places of improvement in the traps code. I will submit a pull request to integrate these new changes into the main traps code base in LO.
- *Anticipated obstacles:* Pull requests are still very new to me. Do I need to create a new branch to submit a new pull request? Can I use the same branch as before? What is best practice? I'll need to do some github research.

**Look into new laptop**
- *Notes:* My current laptop is still functional, but it throws a blue screen error often enough that I am concerened about its longevity. This week I will research some alternative laptops that would work well for research.
- *Anticipated obstacles:* I don't understand technology very well (what is a RAM?). I'll need to spend some time understanding what specs I should be looking for in a laptop.