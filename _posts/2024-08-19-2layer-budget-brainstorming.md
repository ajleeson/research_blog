## Two-Layer Budget Brainstorming

Countdown to PECS: one month

Countdown to thesis defense: two months

My goal is to share a complete draft of the thesis before I leave for France. 

In the past few days I have mostly been planning and brainstorming. I also met with Jilian on Monday, which helped answer some questions, but raised other questions. I've shared my major thoughts below.

Just as a reminder, Figure 1 shows the Lynch Cove budget from last Friday.

<p style="text-align:center;"><img src="https://github.com/user-attachments/assets/b6b9f7b4-365b-4e68-b352-9bd758fce445" width="800"/><br>Fig 1. Lynch Cove budget results.</p><br>

---
## Main questions for thesis

What are the dominant processes contributing to hypoxia in Puget Sound?

To answer this question, we can break it down into intermediate steps:
- What are the dominant processes influencing the bottom DO budget in Puget Sound terminal inlets?
- Are the dominant processes in hypoxic inlets different than in oxygenated inlets?
- Are the dominant processes mostly the same in hypoxic inlets and oxygenated inlets, but the magnitude of the terms are different?
- Are the dominant processes different amongst different hypoxic inlets?

---
## 5 inlets for budget analysis

Because the budget code takes quite a bit of time to run, I have taken Alex's advice and have narrowed down my selection of inlets.

**1. Lynch Cove:** I already have a first pass budget for Lynch Cove. Also, the lowest DO in Puget Sound is in Lynch Cove, so a study of Puget Sound hypoxia is incomplete without considering this inlet.

**2. Elliot Bay:** This inlet is sandwiched between the two largest dischargers in Puget Sound, but it does not experience hypoxia. It will be informative to understand which processes keep this inlet oxygenated.

**3. Penn Cove:** This inlet also becomes hypoxic, and it seems to be an inlet of concern. Also, it will provide some insight into processes influencing oxygen in Whidbey Basin, which may have unique dynamics since it is a river dominated system (Newton and Van Voorhis, 2002).

**4. Case Inlet:** Another inlet that develops hypoxia. Case Inlet is located in South Sound, which is showing signs of eutrophication (Albertson et al., 2007; Harrison et al., 1994). 

**5. Budd Inlet:** Also located in South Sound, but this inlet does not develop hypoxia. There are also 4 small WWTPs along the perimeter of this inlet. What makes the DO response in Budd Inlet different than in Case Inlet?


Thus, I am considering 3 hypoxic inlets (Lynch Cove, Penn Cove, and Case Inlet), and 2 oxygenated inlets (Elliot Bay and Budd Inlet).
 
Lynch Cove, Elliot Bay, and Budd Inlet also have observational data from Ecology CTD casts (the model underestimates summer bottom DO in all of these inlets).

Note that I have not selected any of the inlets which are supersaturated in oxygen (e.g. Hammersley Inlet), since we are interested in the processes driving *hypoxia*, not the processes driving oxygen supersaturation. Also, I've found that LiveOcean has a hard time reproducing measured oxygen and chlorophyll signals in the supersaturated inlets.

---
## Initial ideas for comparing budgets

5 budgets feels like a manageable number of budgets to compare.
Taking inspiration from Jilian's budget manuscript, I am thinking about comparing the magnitude of the budget terms using a bar chart (like the sketch in Figure 2).

<p style="text-align:center;"><img src="https://github.com/user-attachments/assets/be6d7ef2-f738-4235-a70f-f6f294f50e99" width="600"/><br>Fig 2. Budget comparison idea.</p><br>

I plan to average the budget terms over different time intervals (one bar chart plot for each time interval):

- Annual average
- Spring average (Apr - Jul)
    - My hypothesis is that the magnitue of the budget terms during spring may set up the DO concentrations during the summer
- Summer average (Aug - Sep)
    - To capture the influence of any processes that may still be active during the hypoxic season

I don't have enough inlets to conduct rigorous statistical tests. However, I could still qualitatively assess whether the largest terms in the budget are different in the oxygenated vs. hypoxic inlets.

---
## Takeaways from meeting with Jilian

I met with Jilian on Monday morning. She helped answer some of my questions regarding methodologies, and we also discussed some ideas for adapting the budget moving forward. 

A couple of questions came up that I wanted to discuss in more depth:

1. I'd like to revisit Alex's question about using the error term as the vertical exchange term. Would it be more robust to calculate the vertical exchange term using other model output, such as the vertical velocity and eddy diffusivity. A question for Alex: how have you calculated vertical exchange using observations in the past?
2. Is the TEF exchange flow or the Eulerian exchange flow more appropriate for a two-layer budget? Because we are not calculating any othe terms (e.g. respiration rate) within different salinity classes, does it still make sense to mix TEF exchange flow with a depth-based analysis for all other budget terms? Using the TEF analysis, could we accidentally introduce more error to the vertical exchange term? What if we called the Eulerian exchange flow a "horizontal advection" term rather than an "exchange flow" term?

---
## Eulerian volume budget

Per Parker's advice, I also adapted my DO budget code for Lynch Cove to produce a volume budget. Note that this is still using the Eulerian framework. 

Figure 3 shows the result of the volume budget. Given that the error term is relatively small, I have more confidence that I calculated my two-layer DO budget correctly last week.

<p style="text-align:center;"><img src="https://github.com/user-attachments/assets/6b2c6ff8-6f13-42c1-b754-9e1dfaf722ca" width="800"/><br>Fig 3. Lynch Cove volume budget using Eulerian framework.</p><br>

---
## TEF volume budget

Like we discussed last week, I also plan to create a volume budget using the TEF framework. Jilian just sent me her code today, and I will try to put this together before our meeting.

---
## Monthly density and oxygen profiles

In progress. I'll try to add this before we meet in the morning.

---
## References

Albertson, S. L., Bos, J., Pelletier, G., & Roberts, M. (2007). *Estuarine flow into the south basin of Puget Sound and its effects on near-bottom dissolved oxygen.* Olympia, Washington: Washington Department of Ecology.

Harrison, P. J., Mackas, D. L., Frost, B. W., Macdonald, R. W., & Crecelius, E. A. (1994, January). An assessment of nutrients, plankton, and some pollutants in the water column of Juan de Fuca Strait, Strait of Georgia and Puget Sound, and their transboundary transport. In *Review of the marine environment and biota of Strait of Georgia, Puget Sound, and Juan de Fuca Strait: proceedings of the BC/Washington symposium on the marine environment* (pp. 138-72).

Newton, J., & Van Voorhis, K. (2002). *Seasonal patterns and controlling factors of primary production in Puget Sound’s Central Basin and Possession Sound*. Washington Department of Ecology. Retrieved from https://apps.ecology.wa.gov/publications/documents/0203059.pdf

