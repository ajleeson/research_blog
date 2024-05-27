## DO hypotheses and equations

summary

[main equation]

---
## Hypotheses

---
## Derivation

Consider the Puget Sound inlet depicted in Figure ???. 

<p style="text-align:center;"><img src="" width="600"/><br>Fig ???. Idealized Puget Sound inlet.</p><br>

Figure ??? shows the main fluxes and sinks of oxygen in a control volume in this estuary. I have defined the control volume to extend laterally from channel edge to channel edge, and to extend vertically from the channel bottom to the interface (assuming 2-layer model).

<p style="text-align:center;"><img src="" width="600"/><br>Fig ???. Control volume with arrows indicating sources and sinks of oxygen.</p><br>

Let $C$ represent DO concentration.

### Volume-integrated advection diffusion

We will begin by considering the advection-diffusion equation (Wikipedia????)

$$
\frac{\partial C}{\partial t} = (\nabla \cdot \Kappa \nabla C) - (\nabla \cdot \vec{u} C) + Q
$$

where $\Kappa$ is eddy diffusivity, $\vec{u} = (u,v,w)$ is the velocity vector, and $Q$ are our source and sink terms. 

Note that $(\nabla \cdot \vec{u} C)$ is the divergence of flux, where $\vec{u} C$ is the flux of DO. 

Now let's volume integrate our advection diffusion equation over our control volume:

$$
\int_{CV} \frac{\partial C}{\partial t} \mathrm{d}V = 
\int_{CV} (\nabla \cdot \Kappa \nabla C) \mathrm{d}V - 
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
\int_{CV} (\nabla \cdot \Kappa \nabla C) \mathrm{d}V - 
\int_{CV} (\nabla \cdot \vec{u} C) \mathrm{d}V + 
\int_{CV} Q \mathrm{d}V
$$

Now we can invoke Gauss' theorem to convert the diffusion and advection volume integrals into surface integrals (citation??????):

$$
\frac{\partial}{\partial t}\int_{CV} C \mathrm{d}V = 
\oint (\Kappa \nabla C \cdot \vec{n}) \mathrm{d}S - 
\oint (\vec{u}C \cdot \vec{n}) \mathrm{d}S + 
\int_{CV} Q \mathrm{d}V
$$

The time rate of change of DO in the control volume is equal to the net diffusion of DO entering the volume, plus the net advection of DO into the volume, plus the volume-integrated sources and sinks.

### Diffusion term

$$
\int_{CV} (\nabla \cdot \Kappa \nabla C) \mathrm{d}V = 
\Kappa_H \bigg( \frac{\partial C}{\partial x} \Big|_{x = x_0 + \Delta x} - \frac{\partial C}{\partial x} \Big|_{x = x_0}\bigg) (\Delta y \Delta z) +
\Kappa_V \bigg( \frac{\partial C}{\partial z} \Big|_{z = - \xi} \bigg) (\Delta x \Delta y)
$$

where we have assumed there is no diffusion in $y$, since the edges of the domain extend to the channel edges. We also assume no diffusion at the bottom ($z=-H$). 

I have also assumed two different eddy diffusivities. $\Kappa_H$ is the horizontal eddy diffusivity and $\Kappa_V$ is the vertical eddy diffusivity.

Assuming that the water column is vertucally stratified, then I expect $\Kappa_H \gg \Kappa_V$. 

However, I also assume that there is a strong oxygen gradient with the oxycline roughly aligned with the pycnocline. In which case, I expect $\frac{\partial C}{\partial x} \ll \frac{\partial C}{\partial z}$.

Furthermore, I anticipate that the horizontal advection will usually be much larger than the horizontal diffusion. Thus, the most significant diffusion term is the vertical term. This results in:

$$
\int_{CV} (\nabla \cdot \Kappa \nabla C) \mathrm{d}V \approx
\Kappa_V \bigg( \frac{\partial C}{\partial z} \Big|_{z = - \xi} \bigg) (\Delta x \Delta y)
$$

However, this thought process feels rather hand-wavy to me, and I'll revisit some notes to see if I can come up with better justification.

### Advection term

### Source and sink term

### Putting it all together

---
# References

Kundu and Cohen, 2004

Wikipedia for advection diffusion equation!!!

Gauss' theorem!! From Kundu too??


