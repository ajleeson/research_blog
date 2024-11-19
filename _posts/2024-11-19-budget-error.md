## Let's talk about error

While I wrapped up thesis revisions this week, I also revisited the error term in my oxygen budgets so I could comment on them in the thesis.
Depending on how I plot or quantify the error, it can either look small or very large.

First, I want to acknowledge that I originally did check error in the 2-layer volume budget and the 2-layer oxygen budget. Figures 1 and 2 show the 2-layer volume and oxygen budget for Lynch Cove, respectively. At the time, I thought that the error term looked reasonably small compared to the exchange flow and vertical transport terms. 

<p style="text-align:center;"><img src="https://github.com/user-attachments/assets/cacc7902-bdd4-432f-aa30-f8ecee88e437" width="900"/><br>Fig 1. Lynch Cove 2-layer VOLUME budget.</p><br>

<p style="text-align:center;"><img src="https://github.com/user-attachments/assets/2d619e02-e29f-46d7-8fbd-6d7407d634c6" width="900"/><br>Fig 2. Lynch Cove 2-layer OXYGEN budget.</p><br>

In the latest thesis revisions, I also quantified the budget error, following Jilian's example. In her budget manuscript, she quanitfied the relative size of the error as a percentage of:

$$\frac{\text{Annual average error}}{\text{Annual average transport via the inflowing branch of the exchange flow}}$$

I used the same method to calculate the size of the error for my DO budgets in the 13 deep (mean depth > 10 m) terminal inlets. Instead of directly calculating the annual average, however, I first calculated the absolute value of the error and exchange flow terms (so that large positive and negative values don't average out to zero). The resulting percent errors are listed in Table 1. Note that Jilian calculated a 0.16% value for her salt budget in the entire Salish Sea.

|Inlet                  |Oxygen budget: % of QinDOin|
|---                    |---                    |
|Crescent Bay           |1.06%                  |
|Penn Cove              |1.28%                  |
|Port Susan             |1.54%                  |
|Holmes Harbor          |1.26%                  |
|Dabob Bay              |1.24%                  |
|Dyes Inlet             |0.58%                  |
|Sinclair Inlet         |0.71%                  |
|Elliot Bay             |0.60%                  |
|Lynch Cove             |**3.91%**              |
|Case Inlet             |0.86%                  |
|Carr Inlet             |0.70%                  |
|Quartermaster Harbor   |3.52%                  |
|Commencement Bay       |0.88%                  |

At 3.9%, Lynch Cove had the largest relative error compared to all other budgets. However, this still seemed reasonably small to me.

That is, until I made a few realizations:
- Ultimately, I am interested in the storage term. The storage term is much smaller than the exchange flow term in the budget. Thus, the relative size of the error term could be important.
- The error term is the error of the entire inlet (not just the deep layer). So, I think it makes sense to compare the error term to budget terms over the whole inlet, and not just a single layer.
- Since the budget rate terms are volume-integrated, I'm pretty sure I can just sum the surface and deep layer terms to reconstruct a single budget for the whole inlet. I can volume-normalize the terms after summing the volume-integrated terms.

With that, Figure 3 shows a single layer oxygen budget for Lynch Cove. The error term is emphasized in red, and the storage term is emphasize in black. Suddenly, it seems that the error term is no longer small.

<p style="text-align:center;"><img src="https://github.com/user-attachments/assets/c78f92c0-dc80-439e-8a76-2953af968dbb" width="800"/><br>Fig 3. Lynch Cove single layer (entire inlet) oxygen budget.</p><br>

Is is still reasonable to make conclusions about the storage term if the error is relatively large? I'm inclined to think that my thesis conclusions are much less conclusive than I originally thought.