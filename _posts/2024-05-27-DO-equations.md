## DO equation

This week, Dakota and I had a good working session together thinking about our governing DO equation. 

Afterwards, I spent some more time with the math and came up with the following equation:

$$
\frac{\partial}{\partial t}\int_{CV} C \mathrm{d}V = 
\mathrm{vertical\  mixing} + 
\mathrm{advection\ in} - 
\mathrm{advection\ out} + \\
\mathrm{primary\ production} - 
\mathrm{water\ column\ respiration} -
\mathrm{benthic\ respiration}
$$

Where C is the concentration of DO in an arbitrary Puget Sound inlet. Details of the math are listed below.

Using this equation, it should be easier to work on hypothesis development. Each hypothesis could alter one or more of the terms on the RHS in the above equation. The net change on the RHS will tell us whether DO is likely to increase or decrease in an inlet. We can also compare the magnitude of the terms on the RHS between different inlets. (i.e. Inlet X has a lot of benthic respiration, but also much more vertical mixing compared to Inlet Y, so Inlet X has more DO than Inlet Y.)

However, I am already realizing that DO dynamics are much more complicated than this equation. For example, stronger currents can advect both oxygen *and* organic matter, which would alter the amount of respiration. I will need to think up a way of capturing this phenomenon. 

I am also looking forward to really diving into research and hypothesis development/testing in the coming weeks. My classes are finally complete, and in two weeks I will be done with grading and travel. I'm hoping that my research progress will really pick up accordingly.

---
## Derivation

*Apologies, in-line latex isn't working in the blog post :(*

Consider the Puget Sound inlet depicted in Figure 1 

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/c56ce20c-2197-416d-88fc-3da6634ad5c8" width="400"/><br>Fig 1. Idealized Puget Sound inlet.</p><br>

Figure 2 shows the main fluxes and sinks of oxygen in a control volume in this estuary. I have defined the control volume to extend laterally from channel edge to channel edge, and to extend vertically from the channel bottom to the interface (assuming 2-layer model).

<p style="text-align:center;"><img src="https://github.com/ajleeson/LO_user/assets/15829099/d5dcba8b-1deb-4d09-adab-4af27d335020" width="400"/><br>Fig 2. Control volume with arrows indicating sources and sinks of oxygen.</p><br>

Let C represent DO concentration.

### Volume-integrated advection diffusion

We will begin by considering the advection-diffusion equation ("Convection–diffusion equation",2024)

$$
\frac{\partial C}{\partial t} = (\nabla \cdot \kappa \nabla C) - (\nabla \cdot \vec{u} C) + Q
$$

where kappa is eddy diffusivity, u = (u,v,w) is the velocity vector, and Q represents our source and sink terms. 

Note that the secont term on the RHS is the divergence of flux, where uC is the flux of DO. 

Now let's volume integrate our advection diffusion equation over our control volume:

$$
\int_{CV} \frac{\partial C}{\partial t} \mathrm{d}V = 
\int_{CV} (\nabla \cdot \kappa \nabla C) \mathrm{d}V - 
\int_{CV} (\nabla \cdot \vec{u} C) \mathrm{d}V + 
\int_{CV} Q \mathrm{d}V
$$

where the first term on the RHS is the volume-integrated diffusion, and the second term is the volume-integrated flux divergence.

Because we have a fixed control volume, we can simplify the LHS (Kundu and Cohen, 2004):

$$
\int_{CV} \frac{\partial C}{\partial t} \mathrm{d}V = 
\frac{\partial}{\partial t}\int_{CV} C \mathrm{d}V
$$

Thus, we have:

$$
\frac{\partial}{\partial t}\int_{CV} C \mathrm{d}V = 
\int_{CV} (\nabla \cdot \kappa \nabla C) \mathrm{d}V - 
\int_{CV} (\nabla \cdot \vec{u} C) \mathrm{d}V + 
\int_{CV} Q \mathrm{d}V
$$

Now we can invoke Gauss' theorem to convert the diffusion and advection volume integrals into surface integrals (Kundu and Cohen, 2004):

$$
\frac{\partial}{\partial t}\int_{CV} C \mathrm{d}V = 
\oint (\kappa \nabla C \cdot \vec{n}) \mathrm{d}S - 
\oint (\vec{u}C \cdot \vec{n}) \mathrm{d}S + 
\int_{CV} Q \mathrm{d}V
$$

The time rate of change of DO in the control volume is equal to the net diffusion of DO entering the volume, plus the net advection of DO into the volume, plus the volume-integrated sources and sinks.

### Diffusion term

$$
\int_{CV} (\nabla \cdot \kappa \nabla C) \mathrm{d}V = 
\kappa_H \bigg( \frac{\partial C}{\partial x} \Big|_{x = x_0 + \Delta x} - \frac{\partial C}{\partial x} \Big|_{x = x_0}\bigg) (\Delta y \Delta z) +
\kappa_V \bigg( \frac{\partial C}{\partial z} \Big|_{z = - \xi} \bigg) (\Delta x \Delta y)
$$

where we have assumed there is no diffusion in y, since the edges of the domain extend to the channel edges. We also assume no diffusion at the bottom z=-H. 

I have also assumed two different eddy diffusivities. K_H is the horizontal eddy diffusivity and K_V is the vertical eddy diffusivity.

Assuming that the water column is vertically stratified, then I expect K_H >> K_V. 

However, I also assume that there is a strong oxygen gradient with the oxycline roughly aligned with the pycnocline. In which case, I expect the horizontal gradient of C to be much smaller than the vertical gradient of C.

Furthermore, I anticipate that the horizontal advection will usually be much larger than the horizontal diffusion. Thus, the most significant diffusion term is the vertical term. This results in:

$$
\int_{CV} (\nabla \cdot \kappa \nabla C) \mathrm{d}V \approx
\kappa_V \bigg( \frac{\partial C}{\partial z} \Big|_{z = - \xi} \bigg) (\Delta x \Delta y)
$$

However, this thought process feels rather hand-wavy to me, and I'll revisit some notes to see if I can come up with better justification.

### Advection term

Our y-boundaries are closed boundaries, so there is no advection entering the control-volume laterally. 

Let's also assume H/L << 1, so that w << u. Then our advection term becomes:

$$
\oint (\vec{u}C \cdot \vec{n}) \mathrm{d}S = 
\big(
u(x_0)C(x_0) - u(x_0+\Delta x)C(x_0+\Delta x) 
\big) (\Delta y \Delta z) = 
(u_{out}C_{out} - u_{in}C_{in})(\Delta y \Delta z)
$$

### Source and sink term

Our main source terms come from photosynthesis, and is only applicable if the water column is shallow enough such that light reaches the deep water.

We have two sink terms. First, we have DO lost to water column respiration. Second, we have DO lost to benthic respiration (which is respiration of any remaining organic matter that reaches the bottom of the channel).

Combining these source/sinks together, we obtain:

$$
\int_{CV} Q \mathrm{d}V = 
\int_{CV} \mathrm{PP} \  \mathrm{d}V - 
\int_{CV} \mathrm{Resp}_{wc} \  \mathrm{d}V -
\int_{CV} \mathrm{Resp}_{b} \  \mathrm{d}V
$$

Where PP indicates the rate of DO created from primary productivity, and Resp_wc and Resp_b indicate the rate of respiration in the water column and benthos, respectively.

### Putting it all together

Combining everything together, we have:

$$
\frac{\partial}{\partial t}\int_{CV} C \mathrm{d}V = 
\kappa_V \bigg( \frac{\partial C}{\partial z} \Big|_{z = - \xi} \bigg) (\Delta x \Delta y) + 
(u_{in}C_{in} - u_{out}C_{out})(\Delta y \Delta z) + \\
\int_{CV} \mathrm{PP} \  \mathrm{d}V - 
\int_{CV} \mathrm{Resp}_{wc} \  \mathrm{d}V -
\int_{CV} \mathrm{Resp}_{b} \  \mathrm{d}V
$$

---
# References

Kundu, P.K. and Cohen, I.M. (2004) Fluid Mechanics. Elsevier Academic Press, Cambridge.

Convection–diffusion equation. (2024, March 13). In *Wikipedia*. https://en.wikipedia.org/wiki/Convection%E2%80%93diffusion_equation


