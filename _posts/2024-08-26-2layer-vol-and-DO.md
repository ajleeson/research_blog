## Two-layer Volume and DO Budget in 5 Inlets

This week I finished two-layer volume and DO budgets in 5 terminal inlets. They are listed below, along with their average Aug/Sep bottom DO concentrations in parentheses:
- Lynch Cove (0.0 mg/L)
- Penn Cove (1.8 mg/L)
- Case Inlet (1.9 mg/L)
- Carr Inlet (4.0 mg/L)
- Budd Inlet (5.8 mg/L)

I also began constructing bar charts to compare the dominant processes influencing bottom DO in all of the inlets. It seems like the exchange flow and vertical exchange are always the largest terms. Perhaps the differences between inlets are truly the smaller terms, such as respiration. 

More details below.

---
## Interface depths

Following our discussion last week, I selected interface depths that were about 1/3 the total depth. The exceptions to this is Budd Inlet, in which I used 1/2 the total depth instead of 1/3 because this inlet is very shallow.

The resulting interface depths are:

|Inlet     |Depth at moor|Interface Depth|
|----------|-------------|---------------|
|Lynch Cove|20 m         | 6 m           |
|Penn Cove |20 m         | 6 m           |
|Case Inlet|25 m         | 8 m           |
|Carr Inlet|53 m         | 18 m          |
|Budd Inlet|12 m         | 6 m           |


---
## Two-layer volume budgets

Before re-attempting two-layer DO budgets, I started by creating two-layer volume budgets in each of the inlets. In these figures, I have included both TEF and Eulerian results. 

### TEF and Eulerian have same sign

Lynch Cove and Budd Inlet appear to have a persistent exchange flow throughout the year, with inflow always entering the bottom layer (Fig. 1 and Fig. 2)

<p style="text-align:center;"><img src="https://github.com/user-attachments/assets/1ed1fa86-3a41-40b0-a3ee-27e29bbed4ef" width="800"/><br>Fig 1. Lynch Cove two-layer volume budget.</p><br>

<p style="text-align:center;"><img src="https://github.com/user-attachments/assets/824813e9-cd1d-479b-84bf-775fd24e2c99" width="800"/><br>Fig 2. Budd Inlet two-layer volume budget.</p><br>

### TEF and Eulerian sometimes have different sign

However, the other inlets all appear to have exchange flow reversals throughout the year, with more inflow coming in through the surface layer than the bottom layer (Fig. 3, Fig. 4, and Fig. 5).

<p style="text-align:center;"><img src="https://github.com/user-attachments/assets/f58a0a66-f1cb-4341-8401-6a71a6d65cb8" width="800"/><br>Fig 3. Penn Cove two-layer volume budget.</p><br>

<p style="text-align:center;"><img src="https://github.com/user-attachments/assets/93db100c-7e00-40e2-ae87-2adeaab8b463" width="800"/><br>Fig 4. Case Inlet two-layer volume budget.</p><br>

<p style="text-align:center;"><img src="https://github.com/user-attachments/assets/d9aa63af-6dac-4da2-bd3f-684a5ca82f0c" width="800"/><br>Fig 5. Carr Inlet two-layer volume budget.</p><br>

For the rest of this analysis, I am showing results using TEF Qin and Qout. However, I also have results for the Eulerian version as well. In inlets with exchange flow reversals, I wonder whether the Eulerian versions would be more appropriate.

---
## Two-layer DO budgets

These budgets were constructed using the TEF exchange flow method: assuming all of Qin goes into the bottom layer, and all of Qout exits from the top layer. I have also created these same two-layer DO budgets using Eulerian horizontal advection, and can share those figures upon request.

I also refactored Jilian's bio processing code to process all inlets simultaneously. Even though the script still takes 6+ hours to run, I only needed to run it once total (rather than once per inlet, which was my greatest concern).

Figures 6 through 10 show the two-layer DO budget time series in all 5 inlets.

<p style="text-align:center;"><img src="https://github.com/user-attachments/assets/6ade5522-f9b5-4527-836a-8045b51941cc" width="800"/><br>Fig 6. Lynch Cove two-layer DO budget.</p><br>

<p style="text-align:center;"><img src="https://github.com/user-attachments/assets/1382c0fe-335d-4464-835f-f7b261fa2048" width="800"/><br>Fig 7. Penn Cove two-layer DO budget.</p><br>

<p style="text-align:center;"><img src="https://github.com/user-attachments/assets/7ebd406a-963b-4d83-9378-4f84a2d3310a" width="800"/><br>Fig 8. Case Inlet two-layer DO budget.</p><br>

<p style="text-align:center;"><img src="https://github.com/user-attachments/assets/c5b72bb6-4ca4-4175-971d-c6896cb1dc4b" width="800"/><br>Fig 9. Carr Inlet two-layer DO budget.</p><br>

<p style="text-align:center;"><img src="https://github.com/user-attachments/assets/671f3436-1487-4d67-bb3d-c252322251fd" width="800"/><br>Fig 10. Budd Inlet two-layer DO budget.</p><br>

I also note that Penn Cove has a seasonal error term that is more negative during winter. Though the magnitude of this error term is small, it seems almost comparable to some of the smaller budget terms.

The Case Inlet error term also has a strange positive blip near the beginning of August. I haven't looked into this in depth, but I only see this blip in the TEF budget, and not the Eulerian version of the budget.

---
## Budget bar charts

Using the two-layer TEF budgets, I created bar charts of the average DO transports rates over the time intervals of one year (first row of Fig. 11), spring (second row of Fig. 11), and summer (third row of Fig. 11). 

These rate terms are volume-averaged, meaning that I divided the volume-integrated rates by the average volume of each layer over the same time period. 

I have also listed the inlets in ascending order of average Aug/Sep bottom DO. 

<p style="text-align:center;"><img src="https://github.com/user-attachments/assets/56e4a947-896c-45fa-b9cb-9cfa0aa6f328" width="800"/><br>Fig 11. Bar charts of volume-averaged DO transport rates in 5 terminal inlets.</p><br>

I was hoping that these bar charts would show a clear difference between the hypoxic and oxygenated inlets, but alas, that is not the case.

---
## Next steps

- Start writing again!
- Continue analyzing budgets. What makes hypoxic inlets different than oxygenated inlets??
- Investigate sensitivity to depth?
    - I could by remaking volume budgets, which would take less time than the DO budgets.
    - Look at TEF interface depth compared to salinity profile over time at all 5 inlets
- Extend prior scatter plot analysis through 2019 (dots are average of 2014-2019, and error bars to indicate standard deviation)