## Even more thoughts on d/dt(DO)

Because I have been studying for quals, I didn't have as much time to carefully go through the DO budget terms like we discussed doing last week. However, I still did a little bit of thinking. Please see below for more updates.

---
## Full water column, all inlet DO budgets

Last week, I found it helpful to plot a time series of d/dt(DO) of all inlets on the same figure.
I thought it would be instructive to make a similar figure, but for all the budget terms. 

Figure 1 shows a 2017 time series of all budget rate terms (except d/dt(DO)) for all 13 terminal inlets. Therefore, there are 13 photosynthesis curves, 13 exchange flow curves, etc. These budgets are for the entire inlet (i.e., not just the deep layer). 

<p style="text-align:center;"><img src="https://github.com/user-attachments/assets/3fc7529a-0eef-456e-bbe6-685a9d0298dd" width="650"/><br>Fig 1. 2017 DO budget time series for all 13 terminal inlets. These budget terms are volume-normalized, and correspond to the entire volume of the inlet (not just the deep layer). A 10-day Hanning window is applied to the data.</p><br>

We previously discussed how the exchange flow may bring in more nutrients, which fuels more photosynthesis, and thus more consumption. The hypothesis is thus: a larger exchange flow will result in a proportionally larger photosynthesis and consumption term. I tested this hypothesis by normalizing the curves in Figure 1 by the scale magnitude of the exchange flow term, where the scale magnitude is the max annual value of the filtered exchange flow curve. If all other terms scale proportionally to the exchange flow, I expected all of the budget terms to collapse. For instance, all of the photosynthesis curves should collapse into one photosynthesis curve. However, normalizing by the exchange flow did not cause all of the curves to collapse. The terms also do not collapse if I normalize by just Qin.

Interestingly, normalizing by the max annual photosynthesis rate actually resulted in clearer results (Figure 2). In particular, the consumption term collapsed, which is consistent with my prior finding that consumption rate scales proportionally with the photosynthesis rate. I also observe that the exchange flow and TRAPS transport seem to be correlated during the winter in inlets with large river flow. This correlation makese sense because greater TRAPS discharge into the inlet will results in greater DO export out of the inlet via the exchange flow.

I also notice that the air-sea gas exchange term also appears to have collapsed somewhat. The relationship between air-sea gas exchange and photosynthesis also makes sense because stronger photosynthesis will result in a larger DO delta between the surface waters and atmosphere. A larger DO delta means a higher air-sea gas exchange rate.

<p style="text-align:center;"><img src="https://github.com/user-attachments/assets/f2734f92-75dd-4684-9549-b7f3f912d6d4" width="650"/><br>Fig 2. 2017 DO budget time series for all 13 terminal inlets normalized by max annual photosynthesis. These budget terms are volume-normalized, and correspond to the entire volume of the inlet (not just the deep layer). A 10-day Hanning window is applied to the data.</p><br>

Based on this exercise, I confirmed that consumption scales proportionally with photosynthesis. I also learned that river inflow can sometimes play a role in the exchange flow export of oxygen, and that air-sea gas exchange is also influenced by photosynthesis.

---
## Putting the pieces together

To view the budget one other way, Figure 3 shows boxplots of all budget terms during the drawdown period. Each individual box corresponds to an individual term in the budget. There are 13 points in each individual box, where each point is calculated as the average value of the budget term between mid-June through mid-August for a single inlet. Thus, these boxplots show the spread in the budget terms between inlets.

As we have seen previously, the d/dt(DO) storage term has minimal variability between all inlets-- d/dt(DO) is essentially the same in all inlets. TRAPS inflow is generally low, but nonzero in a few inlets. The exchange flow, photosynthesis, and consumption terms have the most inter-inlet variability. We already know that consumption rate scales proportionally with photosynthesis rate. There is slightly less inter-inlet variability in the air-sea gas exchange term. Perhaps it is a coincidence, but the mean air-sea gas exchange value of all inlets is almost identical to the mean d/dt(DO) term of all inlets.

<p style="text-align:center;"><img src="https://github.com/user-attachments/assets/7cbed8a8-8d66-45ff-a278-e6f27eee9f07" width="650"/><br>Fig 3. Boxplots of volume-normalize DO budget terms in all 13 terminal inlets between mid-June through mid-August 2017. Each boxplot is comprised of 13 points-- one point per inlet, where the value is the average budget term in that inlet between mid-June through mid-August.</p><br>

Our simplified DO budget is:

$$d/dt(DO) = TRAPS + exchange flow + photosynthesis + consumption + airsea$$

Where the exchange flow is:
$$Q_{in}DO_{in}+Q_{out}DO_{out}$$

Qualitatively, there are several terms in the DO budget that interact with each other.

First, the exchange flow is related to TRAPS and the biological processes. The only reason Qin and Qout would be different is if there were TRAPS inflow. For many of these inlets, especially during the summer, TRAPS discharge is small, so Qin and Qout will be very similar. Thus, the primary reason why the exchange flow term is a net loss term is because DOout > DOin. This is the case because photosynthesis produces oxygen in the inlets which then get exported out by the exchange flow. Of course, DOout is also influenced by consumption and air-sea gas exchange, but photosynthesis is a larger source term of oxygen than both of these loss terms. Thus, the exchange flow term has some dependence on the rate of photosynthesis in the inlet.

As mentioned earlier, air-sea gas exchange also depends on the DO difference between surface waters and the atmosphere. For simplicity, I assume that the atmospheric DO concentration and the wind speed is similar across all of the inlets. Thus, differences in air-sea gas exchange between inlets is primarily due to differences in surface DO concentration, which is related to photosynthesis. More photosynthesis likely results in more air-sea gas exchange. 

Despite these relationships, it does not appear that the exchange flow, TRAPS inflow, air-sea gas exchange, or photosynthesis terms have clear linear relationships with each other. In other words, there are interactions between these terms, but they appear to be nonlinear. Perhaps it is because of these nonlinearities that we have a constant d/dt(DO) rate. If all terms scaled proportionally with one another (like consumption and photosynthesis), then I would expect d/dt(DO) to also scale in the same proportion. But it does not.

