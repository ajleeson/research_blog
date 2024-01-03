## New Year Planning

Happy 2024!

Thank you, again, for all of your support and flexibility in my being very far away for a very long time.
I am finally back in Seattle, and I am eager to dive back into research.

As I mentioned before our holiday break, my goal is to have a completed thesis draft by mid-summer, with the intention of completing my Master's defense by the end of summer.

In this blog post, I begin with an overview of my proposed 8-month long Master's timeline. Then, I zoom in to the current week and discuss the progress I made on TRAPS over break as well as my TO-DO list for the week.

More details below.

---
## Updated Gantt Chart

During our last meeting, I had proposed a Master's timeline that involved two different experiments:
1. Spin-up experiment
2. Sensitivity study (to WWTP DIN)

After discussing, I learned that these two experiments may take too long. Instead, we decided to focus on a sensitivity study now, and investigate model spin-up procedure later. Specifically, that the primary focus could be to compare a 2017 run with and without DIN in WWTP effluent. Later, I could start a run in 2016 (or 2013), and see if the 2017 results change given the different spin-up condition.

Figure 1 shows a snapshot of my updated Gantt chart, reflecting our shortened experimental timeline.

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/19309f74-e548-46d7-9232-8c8fefeb2b73" width="800"/>Fig 1. Proposed Gantt chart for Master's thesis.
</p><br>

Although this timeline is still ambitious, I hope that by visualizing my path to the end goal, I will research with more determination.

---
## TRAPS Restructure

Before beginning a new experiment, I first needed to wait for the long LO hindcast to finish running. In the meantime, I have been working on improving TRAPS based on some user feedback from Parker. Over break, I sent Parker a Github pull request that introduces a slight restructuring of TRAPS code.

In essence, my proposed changes make the TRAPS code more modular. This means that different versions of Ecology data, climatology scripts, and forcing scripts can be "mix-and-match"ed. The benefit of this change is that users can have multiple, functional, versions of TRAPS on their machine without worrying that they will interfere with one another.

Parker and I are meeting to discuss these changes on Thursday. Once the pull request is approved, then I will begin generating forcing for the next model run.

---
## This week's goals

I'm going to try something new this quarter and organize my goals into different categories. Hopefully, having a diverse set of goals will help me stay motivated. I will breaks my tasks into three categories:
1. Technical work (e.g. coding, plotting, setting up model runs, etc.)
2. Learning and collaborating (e.g. reading literature, using team members' code, reaching out for information, etc.)
3. Housekeeping (e.g. updating READMEs, cleaning code, blog post, etc.)

Given these three categories, my goals for this week are listed below.

### Technical work

Generate forcing for experimental run
- **Description:** Generate WWTP forcing for the year 2017 with no DIN in the effluent. I will compare results from this run to the 2017 results of the long LO hindcast.
- **Notes:** Keep discharge rate the same (no change to hydrodynamics), but set outflowing NO3 and NH4 concentration to 0. I am planning to only alter the WWTP nutrients, and to *not* change the river nutrients. My rationale for leaving the rivers as-is is because I want to study the influence of WWTP DIN on DO in isolation (no confounding variables).
- **Anticipated obstacles:** Before generating forcing, I will need to verify which version of TRAPS was used for the long LO hindcast. It is important that the only difference between our test condition and the LO hindcast is the WWTP nutrient concentrations.

Start "no-WWTP nutrient" run on klone
- **Description:** Begin a 2017 run on klone with the newly generated forcing.
- **Notes:** This run will (hopefully) generate formal results that I can compare to the long LO hindcast. The run will take ~2 weeks, so I'd like to get it started as soon as possible.
- **Anticipated obstacles:** I have note yet run the latest version of LiveOcean, which includes several updates since I last ran the model (such as using the Github version of ROMS). Previously, Parker gave me a list of the new updates. I will need to dig out this list and ask some (TBD) questions before I can start the new model run. Before starting the model, I will also ned to check in with the rest of the lab group to make sure we have enough resources to run.

### Learning and collaborating

Jilian's budget code
- **Description:** Take a peek at Jilian's budget machinery, and try to run the code myself.
- **Notes:** I will start by looking at nitrate, since I will be interested in this nutrient for my model results as well.
- **Anticipated obstacles:** I do not know where Jilian's code is located, nor how to run it. Before starting this task, I'll need to reach out for more information.

Read something, anything!
- **Description:** Read relevant literature.
- **Anticipated obstacles:** Getting distracted. Maybe I should find a good location for reading where I am more likely to stay focused.

### Housekeeping

Migrate notes from Evernote to ???:
- **Description:** Need to move my research notes to a new notetaking platform because I have hit the limit of max allowable notes on the free version of Evernote.
- **Notes:** Evernote recently set a new limit for the maximum allowable notes on the free version of the application. As of this change, I have been unable to create new notes. To fix the problem, I could choose to opt-in to a monthly Evernote subscription. However, I would rather move everything to a free platform. I still need to do more research, but my current idea is to migrate my Evernote notes to a Markdown text editor. If you have other suggestions, please let me know!
- **Anticipated obstacles:** I can envision the migration and adjustment (to a new note-taking platform) to take longer than I hope it will.

Ocean Sciences Prep
- **Description:** Register for Ocean Sciences Meeting and begin reimbursement process.
- **Notes:** Early-bird registration ends on 1/10. I have already purchased flights and will submit a reimbursement request for both the registration fee and the flights.
- **Anticipated obstacles:** Need to learn how to reimburse flights. The PurchasePath link I have is for "non-travel" reimbursements.
