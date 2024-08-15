## Two-Layer Budget Test in Lynch Cove

This week I completed a two-layer budget test in Lynch Cove!
I would not have accomplished this without Jilian's support.

In this blog post, I summarize my procedure, share results for Lynch Cove, and list open questions and to-dos. Note that these results are preliminary.

---
## Jilian's resources

I reached out to Jilian last Friday. She has been traveling all this week, so we haven't had a chance to meet. However, she mentioned that she had previously conducted a two-layer budget test, and she sent me her code and some procedural notes in a [GitHub repo](https://github.com/Jilian0717/LO_user/tree/main/tracer_budget/two_layer).

Figure 1 shows the example output from her two-layer budget.

<p style="text-align:center;"><img src="https://github.com/user-attachments/assets/ae25e964-4598-47e2-8988-463135cb0710" width="600"/><br>Fig 1. Jilian's two-layer budget example for the whole Salish Sea.</p><br>

Her code includes scripts to calculate biogeochemistry rates: respiration, photosynthesis, nitrification, air-sea gas exchange, and DO*volume.

For the exchange flow, TRAPS, and vertical exchange terms, I wrote my own code following the steps she outlined in her repo.

These resources significantly sped-up my process of completing a two-layer budget prototype--- Thanks Jilian!

---
## What is the interface depth?

My first step was to decide on the interface depth for the two-layer model. For this, I plotted annual average density and oxygen profiles in all 21 inlets for the year 2014 (Fig. 2).

<p style="text-align:center;"><img src="https://github.com/user-attachments/assets/0e4ca868-acbb-4e9b-9934-d685cf183c0a" width="900"/><br>Fig 2. Annual average density and oxygen profile in all 21 inlets throughout 2014. If the difference between bottom and surface density is greater than 1 kg/m3, then we consider the inlet to have two-layers. Dots indicate locations of max d/dz(DO) and d/dz(Density). These depths are labeled in the bottom left corner of each two-layer inlet.</p><br>

Figure 3 shows a close-up of Lynch Cove. Conveniently, the oxycline and pycnocline line-up nicely in this inlet. For Lynch Cove, I have selected an interface depth of -6 m.

<p style="text-align:center;"><img src="https://github.com/user-attachments/assets/d00b76c9-a780-4465-b7f7-d91ab83768eb" width="200"/><br>Fig 3. Close up of Lynch Cove annual average density and oxygen profile in 2014.</p><br>

In the future, I will need to decide how to pick interface depth for some of the trickier inlets (like Carr Inlet).

---
## Process notes

This section summarizes some of the key steps I took to create the Lynch Cove budget.

### Exchange flow term

For the exchange flow, I used the output of tef2 sections to get the velocity and DO concentrations flowing in and out of Lynch Cove. 

Per Jilian's outlined steps, I separated the DO budget terms (DO * velocity * area) by depth into a shallow and a deep layer. Thus, the exchange flow budget terms are based on vertical depth rather than salinity coordinates. 

I asssumed that positive velocities mean flow is entering Lynch Cove. Note that when I drew my section lines, I always drew them so that the terminal inlet fell on the positive side of the line (based on the sign convention described in Parker's [tef2 repo](https://github.com/parkermac/LO/tree/main/extract/tef2)).

My code was able to reproduce the exchange flow term in Jilian's budget (dark blue line in Fig. 4). However, my term has the opposite sign as hers. I can only assume that this difference is due to the way the section line was drawn...but I could not think of a way to verify this. If my assumption is correct, then the exchange flow term I calculated for Lynch Cove is also correct and I do not need to multiply by -1. Otherwise, my exchange flow term may have a flipped sign..

<p style="text-align:center;"><img src="https://github.com/user-attachments/assets/ec2e5a3b-4154-4ff3-a791-fc3a5c03a71c" width="400"/><br>Fig 4. Rough comparison between my exchange flow term (left) and Jilian's exchange flow term (right, dark blue line) for the Salish Sea in 2014.</p><br>


Thus, my exchange flow term magnitude is correct for Lynch Cove, but questions remain about its sign.

### Biogeochemistry terms and air-sea gas exchange

The biogeochemistry terms and air-sea gas exchange rates seemed by far the most complex terms to calculate. Luckily, Jilian shared her code for these. All I needed to change was the interface depth and filepaths in her code.

This code took many hours to run on apogee, but it worked well. 

The only issues I came across:
1. DO consumption rates were positive, so I multiplied them by -1
2. The code saved hourly values of DO * volume. To calculate the storage term d/dt(DO*volume), I took the time derivative of this term.

### Rivers and WWTPs

There are two small rivers and one small WWTP in Lynch Cove. 

I calculated DO transport rates using the river forcing files for the 2014 run, which contains daily discharge rate and daily DO concentrations. 

I added all river discharge to the surface layer, and all WWTP discharge to the bottom layer.

### Vertical exchange

Finally, to calculate vertical exchange, I determined the error between the storage term and all other DO budget terms:

$$Vertical\ exchange = \frac{d}{dt}\big(DO * V\big) - (consumption+exchange+photosynthesis+airsea+TRAPS)$$

Ideally, the vertical exchange terms in the surface and bottom layers should sum to zero.

---
## Results

Figure 5 shows the inlet domain for the Lynch Cove budget analysis.

<p style="text-align:center;"><img src="https://github.com/user-attachments/assets/4cae2cd6-11f5-4001-b4d6-f177e61eed52" width="200"/><br>Fig 5. Lynch Cove budget domain: everything to the right of the section line.</p><br>

Figure 6 shows the resulting budget. My big issue: vertical exchange is a source term for surface DO in winter, and a loss term for bottom DO in winter. This is the opposite of what I would expect.

<p style="text-align:center;"><img src="https://github.com/user-attachments/assets/b6b9f7b4-365b-4e68-b352-9bd758fce445" width="800"/><br>Fig 6. Lynch Cove budget results.</p><br>

Figure 7 shows the error term (sum of surface and bottom vertical exchange). The error is nonzero, but small relative to the largest terms in the DO budget.

<p style="text-align:center;"><img src="https://github.com/user-attachments/assets/7e1d6670-a8ea-4130-91fc-ce288228d0ab" width="600"/><br>Fig 7. Error term.</p><br>

---
## Questions and open items

- What are the interface depths for the other 21 inlets, especially those that may not have as clear of an interface as Lynch Cove?
- Why is the vertical exchange term the opposite of what I would expect? Is this due to the exchange flow term having the incorrect sign?
- Is the sign of the exchange flow term correct?
    - Something else that makes me suspicious: the seasonal cycle of the exchange flow term in Lynch Cove is opposite of the seasonal cycle in the Salish Sea
- Was it correct to multiply consumption terms by -1? Are there any other terms with a flipped sign?
- Did I calculate the storage term correctly?
- I used sed_sum2 instead of sed_sum, because Jilian's code had a comment noting that sed_sum2 was the more rigorous method of calculating sediment oxygen demand.
- How do I scale up Jilian's biogoechemistry code to process 21 inlets?
    - We are meeting on Monday
- Is my error term small enough?
- How will I eventually compare 21 budgets?