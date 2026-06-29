# Cooling Nusselt Relations

Coolant-side Nusselt correlations are used to estimate the convective heat transfer coefficient between the cooling channel wall and the coolant. In regen cooling, this is what tells us how effectively the coolant can pull heat out of the chamber wall.

<figure><img src="../.gitbook/assets/image (23).png" alt=""><figcaption><p>Credits: <a href="https://commons.wikimedia.org/wiki/File:Physics_of_a_regeneratively_cooled_combustion_chamber.png">https://commons.wikimedia.org/wiki/File:Physics_of_a_regeneratively_cooled_combustion_chamber.png</a></p></figcaption></figure>

The basic flow is:

$$
Nu \rightarrow h_c
$$

where:

$$
h_c=\frac{Nu k_c}{D_h}
$$

Once we know the Nusselt number, coolant thermal conductivity, and hydraulic diameter, we can then estimate the coolant-side heat transfer coefficient.

where:

* Nu: Nusselt number
* hc: coolant-side convective heat transfer coefficient
* kc: coolant thermal conductivity
* Dh: hydraulic diameter of the coolant channel

### Important Dimensionless Numbers

#### Reynolds Number

Reynolds number tells us whether the coolant flow is laminar, transitional, or turbulent.

$$
Re=\frac{\rho v D_h}{\mu}
$$

or equivalently:

$$
Re=\frac{\dot{m}D_h}{A\mu}
$$

where:

* rho: coolant density
* v: coolant velocity
* Dh: hydraulic diameter
* mu: coolant dynamic viscosity
* mdot: coolant mass flow rate
* A: total coolant flow area



For regen cooling, we usually want turbulent flow because it gives much better heat transfer. A common rough cutoff is:

* Re < 2300: laminar
* Re around 2300 to 3000: transitional
* Re > 3000: turbulent

#### Prandtl Number

Prandtl number compares momentum diffusion to thermal diffusion.

$$
Pr=\frac{c_p\mu}{k}
$$

where:

* cp: coolant specific heat
* mu: coolant dynamic viscosity
* k: coolant thermal conductivity

Pr matters because different coolants move heat differently, even at the same Reynolds number.

### Haaland Friction Factor

Later for Gnielinski correlation/dP calcs, we need a Darcy friction factor. One explicit approximation is the Haaland relation:

$$
\frac{1}{\sqrt{f}}=
-1.8\log_{10}
\left[
\left(\frac{\epsilon/D_h}{3.7}\right)^{1.11}
+\frac{6.9}{Re}
\right]
$$

where:

* f: Darcy friction factor
* epsilon: channel roughness
* Dh: hydraulic diameter
* Re: Reynolds number

For laminar flow, a simple Darcy friction factor is:

$$
f=\frac{64}{Re}
$$

## Common Nusselt Relations:

### Dittus-Boelter Correlation

A simple and common turbulent internal-flow relation is:

$$
Nu=0.023Re^{0.8}Pr^{0.4}
$$

This is a good first-pass relation when the coolant flow is turbulent and the geometry is not doing anything too weird.

Use this when:

* You want a quick coolant-side heat transfer estimate
* Flow is turbulent
* Properties are not changing too aggressively
* You do not need wall-temperature correction yet

### Gnielinski Correlation

The Gnielinski correlation is usually a better general-purpose turbulent pipe/channel relation because it includes friction factor:

$$
Nu=
\frac{(f/8)(Re-1000)Pr}
{1+12.7(f/8)^{1/2}(Pr^{2/3}-1)}
$$

where:

* f: Darcy friction factor
* Re: Reynolds number
* Pr: Prandtl number

This is useful because coolant-side heat transfer and pressure drop are connected. Higher turbulence improves heat transfer but it also usually increases pressure drop.

Use this when:

* You want a more detailed turbulent estimate
* You are already calculating dP (for your channel walls, for example)
* You have a friction factor model (refer to **CRANE Technical Paper 410**)
* You want something more reliable than plain Dittus-Boelter

### Kerosene (RPA) Coolant Correlation

For kerosene/RP1 coolant work, there's a slightly more refined wall-temperature correction that is provided by the Rocket Propulsion Analysis (RPA) manual.&#x20;

$$
Nu=0.021Re^{0.8}Pr^{0.4}
\left(0.64+0.36\frac{T_c}{T_{wc}}\right)
$$

where:

* Tc: bulk coolant temperature
* Twc: coolant-side wall temperature

This is useful because kerosene properties change a lot with temperature. The wall correction helps account for the fact that the coolant near the hot wall is not behaving exactly like the bulk coolant.

Use this when:

* Coolant is kerosene/RP-1
* Wall temperature is much higher than bulk coolant temperature
* You want a regen-specific estimate instead of a generic pipe-flow estimate

### Notes on developing a regen/film cooling solver:

* Do not treat any single Nusselt correlation as truth. Use multiple correlations as a sensitivity check then compare against test data when available.
* If resources permit, run CFD with a combustion model to account for the effects of injector and engine geometry more comprehensively.&#x20;
* After that, continue refining model based on experimental data.&#x20;
