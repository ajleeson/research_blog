## Flat Shelf Upwelling Experiment (Part 3)

This week I finished running the flat shelf upwelling experiment.

A month-long video is shown below:
<p style="text-align:center;"><video src="https://user-images.githubusercontent.com/15829099/185848374-ba52f309-2ebe-4446-b4b4-a48736c254b0.mp4
" controls="controls" style="max-width: 700px;">
</video><br></p>

The forcing and set up used for this model run are documented in [Flat Shelf Upwelling Experiment (Part 2)](https://ajleeson.github.io/research_blog/2022/08/08/flat-shelf-upwelling-2.html).

I made one change to the boundary conditions prior to an extended model run. It turns out that the gradient boundary conditions were still influencing the model domain. Thus, I changed all east and west boundary conditions to be periodic.

After running this model, I then compared the simulation results to theory. In particular, I compared the interface depth, the upper layer cross-shore velocity, and the upper layer along-shore velocity.

All topics are covered in more depth below.

---
## Boundary Conditions

In the last blog post, I discussed using gradient boundary conditions on the East and West edges to reduce the boundary influence on the domain. All of these open boundary conditions are listed in Figure 1.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/185814367-b235ea41-8958-4921-8da3-2e6030a1f35b.png" width="500"/><br>Fig 1. Gradient boundary conditions</p><br>

However, an extended model run using the gradient boundary conditions still proved problematic. Later into the model run, I still observed that the boundaries were causing some circulation within the domain in the form of southward flow along the western edge, and northward flow along the eastern edge (Figure 2).

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/185814394-ed03b9e9-a815-437d-9697-f2c97feed91c.png" width="560"/><br>Fig 2. Model domain influenced by boundary conditions</p><br>

Using the ROMS upwelling model as an example, I changed all of the eastern and western boundary conditions to be Periodic (Figure 3). This seems to have resolved the issue.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/185814429-b77d74fe-8ee5-46fc-bb0e-9ef931d5c12d.png" width="500"/><br>Fig 3. Periodic boundary conditions</p><br>

---
## Comparison to Analytical Solutions

Figure 4 shows a schematic of the problem.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/185846546-8b8a6ef9-4845-473c-af26-598309a811a0.png" width="600"/><br>Fig 4. Upwelling schematic</p><br>

Using the derivation that Parker shared and following along with Gill, analytical solutions for a two-layer upwelling problem are:

*Interface Vertical Displacement*
$$\boxed{\zeta = \frac{FH}{f\lambda}\ t\ e^{-y/\lambda}}$$

*Upper Layer Cross-shore Velocity*
$$\boxed{v=\frac{F}{f}\Big(1-e^{-y/\lambda}\Big)}$$

*Upper Layer Along-shore Velocity*
$$\boxed{u=F\ t\ e^{-y/\lambda}}$$

where
$$y = \text{distance from shore}$$
$$F = \frac{\tau_{\text{wind}}}{\rho H}$$
$$g' = g \frac{\Delta\rho}{\rho}$$
$$\lambda = \frac{\sqrt{g'H}}{f}$$
---
### Interface Vertical Displacement

In theory, it sounded simple to compare the analytical and numerical upwelling results. Then I realized that at any given time, I do not know the depth of the interface in the numerical model. All I have is a cross-section plot of the salinity which I can use to tease out a reasonable approximation for the interface depth.

I tested several different methods to determine the interface depth. Some examples include:

- Fitting a cubic spline to the vertical salinity profile at each distance, y, from the shore. Then determining the depth of the steepest slope
- Finding the gradient of the entire salinity cross-section meshgrid, then determining the depth of max gradient at each distance, y, from the shore

In the end I plotted a contour line along an isohaline of 31.5 g/kg (which is exactly halfway between top and bottom layer initial salinity conditions). Then I just extracted the corresponding depth data from this isohaline. It turns out that using a contour line produced the smoothest looking interface, and it was the easiest to implement.

The video below shows an hourly comparison of the interface depth over one week.

Despite some instabilities in the simulation, the simulation interface does somewhat look like an exponential decay function, which is the same form as the analytical solution. However, it appears that the analytical solution underestimates the rate of upwelling relative to the simulation.

<p style="text-align:center;"><video src="https://user-images.githubusercontent.com/15829099/185506887-c1c08f69-6957-416b-80d7-53720f28b0c1.mp4" controls="controls" style="max-width: 500px;">
</video><br></p>

---
### Velocity

To analyze cross-shore and along-shore velocity results, I created section profiles of the velocity. However, this did require me to make some changes to plotting_functions in lo_tools. (I made a copy of plotting_functions and modified only the copy).

The functions that helps create section plots is written only for state variables defined at rho-points. I added an if case to handle u and v velocity, defined at the u and v points, respectively.

These changes are shows in Figure 5.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/185849486-c57b589e-84a8-4073-93da-5bc32b128c50.png" width="600"/><br>Fig 5. Modifications to plotting_functions.py</p><br>

**Cross-shore Velocity**

*Note that the positive cross-shore velocities are defined to be northward. This is opposite of how y is drawn in Figure 4. Positive y corresponds to moving away from the coast, but positive v corresponds to moving towards the coast (sorry).*

Figure 6 shows a snapshot of the section cross-shore velocity profile.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/185850770-b6ddaf0b-076e-441f-ae51-1f0782989bc3.png" width="600"/><br>Fig 6. Cross-shore velocity snapshot</p><br>

In the analytical solution, we assumed that u and v are independent of depth. However, there does some to be some variation in Figure 6. 

In the interest of time, I compared the analytically derived velocities to *surface* velocities. Though I note that the surface velocities are an overestimate of the average cross-shore velocity in the upper layer.

The video below shows a comparison between the numerical and analytical cross-shore velocity. On average, the analytical model seems to overestimate the cross-shore velocity in the upper layer, relative to the numerical model. This is interesting because I am using the numerical model's surface velocity for comparison, which is already known to be of greater magnitude than the average upper-layer cross-shore velocity. The analytical model also does not capture oscillations, likely because we omitted the unsteady term from the v-momentum equation.

<p style="text-align:center;"><video src="https://user-images.githubusercontent.com/15829099/185810869-a266c434-c6e1-458e-984c-c68c9d70d10b.mp4" controls="controls" style="max-width: 500px;">
</video><br></p><br>

**Along-shore Velocity**

Figure 7 shows a snapshot of the section along-shore velocity profile.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/185851732-6db255d5-767a-47bc-836b-9c6d24dbbd63.png" width="600"/><br>Fig 6. Along-shore velocity snapshot</p><br>

Again, the velocity appears to vary with depth. I used the surface along-shore velocity for analysis, but I am aware that this is an overestimate of the average along-shore velocity in the upper layer.

The video below whos a comparison between the analytical and numerical along-shore velocities. The analytical model consistently underestimates the along-shore velocity relative to the numerical model. This is the opposite of the cross-shore velocity analysis.

<p style="text-align:center;"><video src="https://user-images.githubusercontent.com/15829099/185852667-0f29869d-92f1-4377-aaef-91a048efdf88.mp4
" controls="controls" style="max-width: 500px;">
</video><br></p>

---
## Some Thoughts

Why the discrepancy?

Here's the salinity and density profile again:
<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/183492005-97108683-1aa6-4b30-8b9d-c0e5f72ab7ff.png" width="400"/><br>Fig 7. Initial ocean salinity and density.</p>

The density difference between the very surface and very bottom of the ocean is only about 2.5 kg m-3 (0.24%). Due to hydrostatic pressure, the density also slightly increases with depth within a layer. Although this change is small, it might still be sigificant given that the density difference between the layers is also small.

In the analytical equation, I used
$$\Delta\rho=2.5\ kg\ m^{-3}$$

Although, this value may be a slight overestimate, given the reasons above. In the analytical solution, an overestimate of the density change between the layers corresponds to a larger reduced gravity, which then corresponds to a larger internal Rossby radius (lambda). This would lead to the analytical solution underestimating the rate of upwelling relative to the simulation (which is what we observe).

Furthermore, the analytical solution assumes that the two layers are immiscible. However, the density difference between the layers is small, so in the simulation there could be some mixing at the interface over time. This can cause the density difference between the layers in the simulation to become even smaller. This can lead to the simulation overpredicting the rate of upwelling relative to the analytical solution.

Overall, the analytical solution underpredicts the upwelling rate relative to the simulation. Differences in the density profile may be one contributing factor of this.