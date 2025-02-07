## Exploring d/dt(DO)

One of the last remaining questions we have for my manuscript is: Why does DO decrease at the same rate in all inlets between mid-June through mid-August?

Figure 1 shows a boxplot of the net DO decrease rate in all inlets. Note that these decrease rates correspond to the entire inlet (and not just the deep layer). All of the analysis in this blog post focuses on the entire inlet as well, rather than one of two layers.

<p style="text-align:center;"><img src="https://github.com/user-attachments/assets/c8cda5ae-8b01-4df2-b4ed-55e32bd838cb" width="700"/><br>Fig 1. Boxplot of d/dt(DO) in all inlets between the months of mid-June through mid-August. Data are daily d/dt(DO) rates. Inlets are sorted from shallowest to deepest. These values correspond to the entire inlet.</p><br>

This week I made a little bit more progress in understanding how all of the oxygen budget terms are related to each other. However, I still need to do more work to fully describe how the terms all interact with each other to result in a constant d/dt(DO) rate.

My progress so far is summarized below. Note that all of the scatter plots are average values between mid-June through mid-August. Each dot represents one inlet.

---
## Nutrient fluxes and phytoplankton growth

First, I confirmed that a stronger exchange flow does indeed bring more nutrients into the inlets. Figure 2 shows a scatter plot of DIN fluxes vs. exchange flow strength. I have volume normalized both of these terms by the volume of the inlet. The resulting terms are a bit tricky to understand, so I have written them out explicitly in the axis labels. DIN fluxes into the inlet are QinDINin/Vinlet, and the exchange flow strength is Qin/Vinlet, which is really just 1/Tflush. Note that DINin is calculated as NO3in + NH4in

<p style="text-align:center;"><img src="https://github.com/user-attachments/assets/f5e02501-de14-46d5-9bfd-e85d5c17037b" width="350"/><br>Fig 2. Scatter plot of DIN fluxes into the inlet vs. strength of the exchange flow. Values are normalized by inlet volume. Data are average between mid-June through mid-August for each inlet. Every dot represents one inlet.</p><br>

The slope of this scatterplot tells us that on average, across the 13 inlets, the exchange flow has a DIN concentration of 0.0265 mmol/L, which is equivalent to 26.5 uM. This is a reasonable value.

Because the R2 value is so high, we have nearly a linear fit. Therefore, compared to the scale of change in the exchange flow strength across all of the inlets, the variability in nutrient concentrations entering the inlets is very very small. Thus, nutrient fluxes into the inlets scale almost perfectly linearly with increasing exchange flow strength. 

This result does not imply that photosynthesis will also scale nicely with increasing exchange flow strength, even though there are more nutrients to fuel photosynthesis. This is because photosynthesis scales nonlinearly with nutrient concentration, phytoplankton concentration, and light availability. Although I find that photosynthesis indeed increases with increasing exchange flow strength, it does so nonlinearly (Figure 3).

<p style="text-align:center;"><img src="https://github.com/user-attachments/assets/307cdfce-fcc0-4b20-8a09-633737707cbd" width="350"/><br>Fig 3. Scatter plot of photosynthesis production of DO vs. strength of the exchange flow. Values are normalized by inlet volume. Data are average between mid-June through mid-August for each inlet. Every dot represents one inlet.</p><br>

---
## Photosynthesis and consumption

I have learned that inlets with stronger exchange flow tend to have more photosynthesis, but that the relationship between photosynthesis and exchange flow strength is nuanced and complex. The biogeochemistry equations related to photosynthesis suggest that we cannot predict photosynthesis strength unless we also take into account nutrient, phytoplankton, and light availability within the inlet. This is a complicated task which I bypassed for now.

Instead, I took a look at the relationship between consumption rate and photosynthesis. Incredibly, consumption strength in the inlet scales nearly perfectly linearly with photosynthesis strength, by a factor of -0.559 (Figure 4). Thus, for every 1 mg L-1 day-1 of oxygen that photosynthesis produces, we can expect biological processes to consumme 0.559 mg L-1 day-1 of oxygen.

<p style="text-align:center;"><img src="https://github.com/user-attachments/assets/a1139b05-04f4-40d5-9a16-f15a18616450" width="350"/><br>Fig 4. Scatter plot of consumption of DO vs. photosynthesis production of DO. Values are normalized by inlet volume. Data are average between mid-June through mid-August for each inlet. Every dot represents one inlet.</p><br>

This result is somewhat surprising because the consumption rate of oxygen is a sum of water column respiration and sediment oxygen demand, neither of which are linearly related to photosynthesis rate in the BGC equations. Perhaps the nonlinearities explain the variability in the scatter plot (i.e. why R2 is not perfectly 1), but in general, more photosynthesis means stronger consumption.

---
## Exchange flow

Finally, I took a look at the exchange flow contribution to the oxygen budget. Figure 5 shows QoutDOout vs. QinDOin. Amazingly the exchange flow is a loss term of -0.062 mg L-1 day-1 of oxygen in every single inlet.

<p style="text-align:center;"><img src="https://github.com/user-attachments/assets/199f7985-93ad-41f9-93d5-f922de358408" width="350"/><br>Fig 4. Scatter plot of QoutDOout vs. QinDOin. Values are normalized by inlet volume. Data are average between mid-June through mid-August for each inlet. Every dot represents one inlet.</p><br>

This means that no matter how strong the exchange flow is in an inlet, the net effect of the exchange flow's inflow and outflow is a loss of -0.062 mg L-1 day-1 of oxygen.

---
## Putting things together

For the entire inlet, we can write a simplified DO budget equation as:

$$\frac{\partial}{\partial t}(DO) = exchange + photosynthesis + consumption + airsea + rivers$$

We can modify this equation based on our findings so far. First, we know that the exchange flow is a constant loss term of oxygen that is the same amongst all inlet. So, we can replace exchange with -0.062 mg/L day-1. Second, we have found that in all inlets, consumption scales linearly with photosynthesis by a factor of -0.559. Thus:

$$\frac{\partial}{\partial t}(DO) = -0.062 \ \mathrm{mg\ L^{-1}day^{-1}} + 0.441(photosynthesis) + airsea + rivers$$

---
## Next steps

I will continue going through the biogeochemistry equations to see if I can understand the interactions between the remaining terms in the budget equation. Hopefully I will be able to come to a more concrete understanding of why d/dt(DO) is the same in all inlets. 

I have already tried to look at the dependence of air-sea gas exchange on photosynthesis and QinDOin, but have not found anything insightful as of yet. 

I'm happy to entertain more ideas for testing as well! Otherwise I'll keep chugging along.