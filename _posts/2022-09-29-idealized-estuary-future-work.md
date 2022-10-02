## Proposed Idealized Estuary Work

During the summer quarter, I spent four days per week working on the idealized estuary, and one day working on getting river/WWTP data for LiveOcean.

This quarter I propose the opposite ratio. I will foucs 80% of my research time towards adding small rivers and point sources to LiveOcean, and the remaining 20% towards working with idealized models.

Below I've summarized some issues with the current model that I have yet to resolve. Further down I propose a question that I am interested in answering with a working idealized model.

---

## Issues with current model

Before I can answer any new questions, I need to resolve some issues I was having with my boundary conditions.

I described them in [my previous blog post](https://ajleeson.github.io/research_blog/2022/09/09/nutrients-mini-experiment-1.html), but have summarized the issues again below:

- Applied Gradient boundary condition to biogeochemistry variables (inflowing value equal to nearest interior value)
- concerns:
  - that ocean does not introduce more phytoplankton if interior population drops to zero, thus preventing any new growth
  - that the ocean does not bring dissolved oxygen, so hypoxia at the domain edges will always remain at the domain edges

*Are these issues really issues, or are these somewhat realistic conditions?*

One possible fix is to switch from Gradient boundary conditions to Radiation Nudging. However, this may cause an edge-effect in which the biogeochemistry variables have a large gradient at the boundary.

Perhaps the Gradient boundary conditions are reasonable, but the phytoplankton population grew too uncontrollably. Would different initial conditions be more realistic?

## Proposed Focused Question

I've been thinking more about the spatial distribution of WWTP nitrate loading in Puget Sound and how King County and Vancouver Metro discharge the most nitrate to Puget Sound (Figure 1). I've been curious how much other smaller dischargers make an impact. Even though these dischargers are smaller, there are also more of them.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/188669987-a5d99a57-0748-4768-bf22-7ee8b832e76d.png" width="300"/><br>Fig 1. Average DIN load of each WWTP.</p><br>

My question (pending refinement) for the idealized estuary is:
*How does the spatial distribution of WWTP nitrate loading affect the severity of hypoxia and the total hypoxic volume?*

On the one hand, larger WWTPs may create a bigger bloom, and thus lead to hypoxia. But perhaps this hypoxia is confined locally. In the case of many smaller WWTPs, there may not be as concentrated of a bloom, but perhaps less severe hypoxia is distributed across a larger volume.

To simplify the model and target this question, I plan to switch back to using the flat-shelf grid (used for upwelling experiment). The grid is shown in Figure 2. It has a vertical wall at the coast, uniform depth of 100 m, and no estuary.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/179569924-35eafbb8-f208-48a7-8d8d-0ed5edba089e.png" width="600"/><br>Fig 2. Flat-shelfed grid.</p><br>

I would run at least two different test cases. One in which I have many small dischargers equally spaced from each other, and another in which I have only a couple of large dischargers equally spaced from each other. A quick sketch of these scenarios is depicted in Figure 3.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/192891920-0a2a544a-5b3d-4747-b5d5-d63457e60c62.png" width="500"/><br>Fig 3. Sketch of proposed experimental conditions. Circles indicate locations of WWTPs, and circle size indicates relative loading rate.</p><br>

Constraints:
- Total nitrate loading is the same across all scenarios
- Effluent nitrate concentration is constant. I will only scale flowrate to create "smaller" and "larger" dischargers.

Biogeochem Boundary Conditions:
- Periodic on East and West edges (assuming WWTPs repeat in frequency to the East and West of the domain)
- TBD on the South, pending resolution of issues described earlier

I will keep tides so that there is more mixing and momentum in the system.

## Other questions of interest

Something interesting that happened in my last model run was that the phytoplankton population grew over the first two weeks, and then suddenly collapsed. I hypothesized that this was due to mass zooplankton grazing, though it may also be caused by nutrient depletion. I've included the model spin-up video below as a reminder:

<p style="text-align:center;"><video src="https://user-images.githubusercontent.com/15829099/189448347-d151a717-1e4f-47fa-acc9-3ac10bd75f45.mp4
" controls="controls" style="max-width: 700px;">
</video><br></p>

I don't have a fully formed question at the moment, but I am interested in exploring these population dynamics more. For example, how do these population dynamics change if I changed the initial concentratin of phytoplankton, zooplankton, or nitrate in the ocean? How sensivitve is the growth rate of the phytoplankton population to changes in max daily solar radiation? And how do the fluctuations in phytoplankton populations affect bottom DO? (I'm hypothesizing that there is some sweet spot in which phytoplankton are less variable than my last experiment. Maybe then there would be fewer organisms that die, settle, and get broken down, and thus less hypoxic volume?)