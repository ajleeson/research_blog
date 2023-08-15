## AttSW changes in fennel.h

This post describes the changes I made to increase AttSW in the Salish Sea. Note that these changes are in LO_roms_source_alt/npzd_banas/fennel.h. The previous version of fennel.h without my edits is called "fennel_LO_ORIG.h." You can find both of these files in perigee in /data1/auroral/LO_roms_source_alt/npzd_banas.

Figure 1 shows a mp of the intended changes. Outside of the Salish Sea region, AttSW = 0.05 m-1. Inside of the Salish Sea (ish) region, AttSW = 0.15 m-1.

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/de1d5031-c8ec-4ee8-9bde-8b28ae03e041" width="300"/><br>Fig 1. Map of AttSW values.</p><br>

The following section enumerates all of the changes I made to fennel.h.

---
## Changes to fennel.h

1. Add the grid lon and lat as inputs to the function call of fennel_tile
<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/f60cbf57-8e2c-4360-9f35-7cc2927666e6" width="600"/></p><br>
   
2. Add lonr and latr as expected inputs to the fennel_tile subroutine
<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/32d08769-93e4-42dc-bb46-956e6439e73e" width="600"/></p><br>
   
3. Declare lonr and latr as an array of real numbers that spans the i and j indices of the tile
<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/64e47b57-a068-4e5f-a2f0-63b67f29ddb8" width="500"/></p><br>
   
4. Declare a new local variable called 'AttSW_region'
<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/368b7273-d974-4658-811e-6bb3123ae9e0" width="500"/></p><br>
   
5. If lonr and latr are within the Salish Sea, AttSW_region = AttSW * 3. Otherwise, AttSW_region = AttSW. I also added some print statements for testing, but have since commented them out. Note that AttSW is still defined by the user in bio_Fennel_BLANK.in. The initial value is still 0.05 m-1, and simply gets multiplied by a factor of 3 inside of the Salish Sea.
<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/82d50f29-6306-4089-9f3f-1dba0328a841" width="800"/></p><br>
   
6. Update calculations of light attenuation to use the new regional AttSW_region rather than AttSW
<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/257584fa-1a71-45c2-8502-72a4600dfbf6" width="600"/></p><br>
   

### Results

The resulting print statement in the log file demonstrated success:
<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/c336c624-c1d4-4d92-9d98-6c26b64ecf7e" width="300"/><br>Fig 2. Print statement output in the log file listing the different AttSW_region values.</p><br>

Figure 3 shows the corresponding location of these test points on a map.
<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/b4054ffd-8a6c-4772-ba17-b6a0a0b4b027" width="300"/><br>Fig 3. Location of AttSW test points.</p><br>
