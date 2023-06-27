## GRS/GRC 2023 Reflection and Summer Planning

The GRS/GRC were really great experiences. My brain was turning into jelly by the end of the week, but I think that's a symptom of a lot of learning and a lot of networking.

The student talks were truly excellent, and I enjoyed connecting with other early-career scientists. Even though most of us study vastly different topics, we all share the common language of environmental fluid mechanics. I suppose that this similarity is the point of the conference, but in the moment those conversations still felt special.

I learned an incredible amount from the GRC talks as well. There were a diverse set of topics ranging from internal tsunami waves, to underground estuaries. I was also pleased that the established scientists at the GRC were all very pleasant people. It was great to finally meet Susan Allen and John Wilkin in person, both of whom gave fantastic talks too. I also connected with Daniel Dauhajre from the UCLA ROMS group. He helped e-introduce me to a PhD student at UCLA who is working with WWTP effluent in ROMS and has dealt with similar stability issues as I have been. I am looking forward to what I learn from her.

The poster sessions were my favorite part of the conference. I liked the casual atmosphere, which made it easy to ask questions and learn. People at all stages of their career also stopped by my poster and seemed interested in and supportive of my work thus far.

The most common questions I was asked were:
1. What biogeochemical model am I using?
2. How are the ocean nutrients added to the model?

I'll be sure to provide this information on the next poster that I make. Several people also seemed to want a bathymetry diagram, so I will add a bathymetry map next time as well.

### Specific Feedback from Poster Session

- Read Scully (2013) about simple oxygen model in Chesapeake that actually works rather well.
- Look at transects at a higher temporal resolution to see whether we can tell when the low DO water gets over the sills.
- Is there some numerical mixing artifacts in the spread of the WWTP tracers?
- Test without rivers too. Maybe there is some river-WWTP interactions.
- Turn off certain regions or individual WWTPs to see if there are specific ones that Puget sound DO is most sensitive to.
- Look at residence times in different regions and how they might effect biology (short residence times might not have enough time to grow phytoplankton).
- What is the typical depth of discharge of WWTPs (in reality and in the "WWTP-as-river" implementation)?
- Can we resolve the effluent plume?

---
## Summer Planning

Now that classes and conferences are both wrapped up, I can finally return my focus to research. I have broken my goals up into "long-term" and "short-term" tasks. Long-term goals are those I wish to complete by the end of the summer. Short-term goals are those that I'd like to complete in the next two weeks.

Short-term goals
- Move all code from klone to mox
- Debug weird forcing conditions in idealized model
  - *Where  I left off:* Removing volume-adding code from vertical sources improved vertical velocities. However, the salinity and temperature forcing used in the model run did not match the salinity and temperature values that I prescribed. I need to figure out why this happens. A first step will be to test with LtracerSrc = T for both salt and temp, and LtracerSrc = F for both salt and temp. Right now, I have one true and one false, and perhaps that is causing some strange behavior..
- Begin debugging volume-less vertical sources in LiveOcean
  - After fixing the forcing conditions in the idealized model, I will try to run the volume-less sources in LiveOcean again. The last time I tried to run volume-less sources in LiveOcean, ROMS still blew up. My next step is to make the vertical sources neutrally buoyant to test whether density instabilities are causing the blow-up.
  
Long-term goals
- Finish debugging volume-less vertical sources
- Finalize WWTP and river implementation in LiveOcean
  - I need to upgrade to Ecology's updated dataset
  - I'd also like to review my code one more time. There are small improvements to the river/WWTP forcing that I have wanted to make for some time now.
- Run LiveOcean for one year with and without WWTP nutrients
- Model validation
  - I will need to connect with Jilian and Parker on this
- Read!
  - My goal is 3-5 papers per week
- Develop hypothesis
  - Based on preliminary results and my literature review, I'd like to come up with an interesting science question to explore.
- Document results
  - Document preliminary results and research direction in a report & presentation. Maybe I can give another EFM presentation during Fall quarter.