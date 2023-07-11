## Summer Planning and Continued Debugging

The following couple of weeks I've been swimming in a growing sea of thoughts and ideas inspired by the GRS/GRC. Per Alex's recommendation, I have put together a tentative action plan for the summer based on some of these thoughts. I have also tried to summarize my new knowledge and ideas pertaining to WWTP implementaton. A lot of these ideas are still in their infancy, so I welcome any suggestions and feedback. Lastly, I took a look at the change in depth of WWTPs implemented as tiny rivers.

More details below.

---
## Summer 2023 Plan

My main goals for the summer are as follows:

- Debug WWTPs
- Conduct preliminary DO analysis using existing results
  - Including mini project with Dakota to look at Penn Cove
- Come up with effluent plume implementation
- Get involved with model validation
- Run new LiveOcean experiment with *working* WWTPs
- Document results

To achieve these goals, my standard weekly milestones are:

- 3-5 papers to read per week
- 1 figure using preliminary DO results (begin asking science questions)
- 1 earnest attempt at fixing WWTPs
- 1 blog post
- 3 hours of note-texing (aim to wrap up by end of July)

---
## New Insight into WWTP Implementation

The last two weeks I have been looking into how a UCLA ROMS team, and how the SSM, implements point sources. These findings have been eye-opening. More details below.

### UCLA ROMS-BEC WWTP Scheme

Last week I met with Minna Ho, a PhD candidate at UCLA studying the effects of wastewater effluent in the Southern California Bight (SCB). It is great to connect with someone else conducting similar research as me. Unlike myself, Minna mentioned that her team is not using the LwSrc option in ROMS. Instead, they are adding point sources using Fortran code that a UCLA postdoc integrated with ROMS. I took a look at where their point source code is located in the ROMS source code, and it looks rather invasive (Fig. 1)

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/051ace64-bc7e-467d-a13c-2b3d87f2b721" width="500"/><br>Fig 1. Every file in which the UCLA ROMS point source code gets used.</p><br>

I don't think that it will be worth making such invasive changes to ROMS for the LiveOcean WWTPs. Thus, I have instead been reading on the theory of how these sources are implemented.

The UCLA ROMS point sources are based on work by Uchiyama et al., 2014. My biggest takeaway from their work is that they have initialized the effluent inputs with some vertical and horizontal distribution. They mention that their grid resolution is not refined enough to resolve the effluent plume, and that ROMS, being a hydrostatic model, would have difficulty dealing with a buoyant plume (sounds familiar). Thus, they opted to add the effluent using a climatology for vertical/horizontal distribution based on their knowledge of currents throughout the year. (They mention the option of using a buoyant plume model instead of the climatology, but their results suggest that the climatology is good enough.)

I have another paper on my "to-read" list about the UCLA ROMS model configuration, which I intend to get to this week. However, Minna already mentioned that her model also includes a horizontal/vertical distribution of the initial effluent inputs. She used WWTP observational data to evaluate where in the water column the effluent plume should be centered. I believe KC has similar information for some of the larger WWTPs, so I could do something similar.

I have spoken with Alex several times about possibily developing some vertical/horizontal effluent initialization structure. I am even more excited about this option after reading and meeting with Minna. Right now, my idea is as follows:

- Implement volume-less WWTPs
- Only include biological tracers, i.e. no temperature/salinity (is this possible in ROMS?)
- Add effluent sources using some initial vertical/horizontal distribution that matches observational record of WWTP plumes
  - Create using some climatology for plume structure

We talked previously about removing temperature/salinity from the effluent so that we don't run the risk of creating a density instability. I am envisioning creating a climatology for the effluent plume structure so we can still capture the correct vertical distribution of nutrients in the water column, despite no longer having a buoyant plume.

One last point that I will mention is that Uchiyama et al. (2014) noted that the SCB is mostly thermally stratified. Thus, their effluent plume still contained freshwater. For our Salish Sea application, I am envisioning removing both temperature and salinity.

### SSM WWTP Implementation

I finally reached out to Ben this week to ask about SSM implementation of WWTPs. He provided great information about what's going on.

The SSM, like ROMS, is also hydrostatic. Similar to ROMS, volume from WWTPs is added by increasing the sea surface height. Just like I have been trying to do in LiveOcean, the effluent plume in SSM is also added at a single node. It does not have an initial vertical/horizontal plume distribution.

Since there appear to be several similarities between the LwSrc implementation of point sources and the SSM implementation of point sources, I am interested in learning and comparing the implementations at a deeper level. Ben shared the FVCOM manual with me, and I might spend more time looking through it in the coming week.

---
## WWTP Debugging

Before I can implement any plume structure, I first need the WWTP vertical source forcing to work. Prior to GRS/GRC, I was testing volume-less WWTPs in an idealized domain. I had discovered that the hydrodynamics of the volume-less sources look reasonable. However, the salinity and temperature coming from the WWTP did not match what I had specified in the forcing file.

After a series of follow-up tests last week, I discovered that the forcing issue is related to the "volume-less" switch. When the WWTPs add volume, the forcing behaves as expected. When the WWTPs are "volume-less," the forcing misbehaves. A quick look at the "volume-less" code did not tell me much-- there is no mention of temperature or salinity in this code.

Even though I don't have a resolution yet, I at least know to focus on the "volume-less" code for further debugging.

Recently I also met with Jilian and Dongxiao to discuss my issues with WWTPs. Dongxiao mentioned that he is able to succesfully use LwSrc in his research. That's promising! He also mentioned that the [Wiki ROMS](https://www.myroms.org/wiki/River_Runoff) states that only one of LuvSrc and LwSrc can be used at a time. However, a [2022 update](https://www.myroms.org/projects/src/ticket/905) stats that LuvSrc and LwSrc can both be used. I have tested LiveOcean with only LuvSrc (works), and with LuvSrc + LwSrc (doesn't work). But I have not tested only LwSrc. Jilian said she'd be willing to help test only LwSrc in her Hood Canal domain. I'll also give it a shot for one day in LiveOcean.

If it's true that only one of LuvSrc or LwSrc can be used at a time, then I may need to evaluate the "WWTP as tiny river" implementation more closely.

---
## "WWTP as tiny river" analysis

As a first step in evaluating the "WWTP as tiny river" implementation, I've taken a look at how much the discharge depth of WWTPs changed when I pushed them to the coast. Figure 2 shows new depth vs. original depth for all WWTPs, where circle area is proportional to DIN discharge.

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/7e7b1b53-d6a0-415e-bf00-2fad086d6966" width="430"/><br>Fig 2. New WWTP depth vs original WWTP depth. Circle area is proportional to average DIN discharge.</p><br>

Per Ben and Parker's recommendation, I also took some time to read Khangaonkar and Sun's 2023 paper about moving WWTP outfalls to different depths. The colors in my figure correspond to changes in depth regime, as defined by Khangaonkar and Sun (2023). Above 25 m is the photic zone, and above 60 m is the surface outflow layer. In general, the majority of the WWTPs stayed in the same depth regime.

West Point is one of the largest WWTPs, and it happened to shift from a deeper inflowing layer to a surface outflowing layer. Based on what I have learned from Khangaonkar and Sun (2023), I would anticipate that this shift will not result in a large change in DO. WWTP nutrients that are flowing outward in the surface layer may reflux at the sill and recirculate in Main Basin.

---
## What have I been reading?
*(accountability metric)*

Kessouri, F., McWilliams, J. C., Bianchi, D., Sutula, M., Renault, L., Deutsch, C., Feely, R. A., McLaughlin, K., Ho, M., Howard, E. M., Bednaršek, N., Damien, P., Molemaker, J., &#38; Weisberg, S. B. (2021). Coastal eutrophication drives acidification, oxygen loss, and ecosystem change in a major oceanic upwelling system. <i>Proceedings of the National Academy of Sciences</i>, <i>118</i>(21). https://doi.org/10.1073/pnas.2018856118

Khangaonkar, T., &#38; Yun, S. K. (2023). Estuarine nutrient pollution impact reduction assessment through euphotic zone avoidance/bypass considerations. <i>Frontiers in Marine Science</i>, <i>10</i>. https://doi.org/10.3389/fmars.2023.1192111

Sutherland, D. A., Maccready, P., Banas, N. S., &#38; Smedstad, L. F. (2011). A Model study of the salish sea estuarine circulation. <i>Journal of Physical Oceanography</i>, <i>41</i>(6), 1125–1143. https://doi.org/10.1175/2011JPO4540.1

Uchiyama, Y., Idica, E. Y., McWilliams, J. C., &#38; Stolzenbach, K. D. (2014). Wastewater effluent dispersal in Southern California Bays. <i>Continental Shelf Research</i>, <i>76</i>, 36–52. https://doi.org/10.1016/j.csr.2014.01.002

**UCLA ROMS-BEC code**

Kessouri, Faycal, McWilliams, C James, Deutsch, Curtis, Renault, Lionel, Frenzel, Hartmut, Bianchi, Daniele, & Molemaker, Jeroen. (2020). ROMS-BEC oceanic physical and biogeochemical model code for the Southern California Current System V2020. Zenodo. https://doi.org/10.5281/zenodo.6886319
