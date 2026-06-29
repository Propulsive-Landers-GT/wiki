# Bartz Correlation

The Bartz correlation is a commonly used empirical relation for estimating the convective heat transfer coefficient on the hot-gas side of a rocket engine chamber and nozzle. In early engine design, it's  a first pass way to estimate how much heat the combustion gases transfer into the wall before we couple that with the coolant channel wall calcs.

This is especially useful for regen cooling, film cooling, ablative liner sizing, and general wall temperature checks! It is not perfect, and known to overestimate heat flux especially at throat but one of the more accurate and relied upon relation out there without CFD and further analysis.

### General Bartz Equation

One common form of the Bartz correlation is:

$$
h_g =
C \mu^{0.2} c_p Pr^{-0.6}
\rho_0^{0.8}
(c^*)^{-0.8}
D_t^{-0.2}
\left(\frac{A}{A_t}\right)^{-0.9}
\sigma
$$

where:

* hg: hot-gas-side convective heat transfer coefficient
* C: empirical constant that depends on the unit system being used
* mu: gas dynamic viscosity
* cp: gas specific heat at constant pressure
* Pr: gas Prandtl number
* rho0: stagnation gas density
* c\*: characteristic velocity
* Dt: throat diameter
* A: local cross-sectional area at the point being evaluated
* At: throat area
* A/At: local area ratio
* sigma: correction factor for property variation through the boundary layer

In practice, hg is calculated at different axial locations along the chamber and nozzle. Since the area ratio changes along the contour, Bartz gives us a way to estimate how the heat transfer coefficient changes from chamber to throat to nozzle exit. Being able to estimate the convective heat flux is the first step in all of regen design.&#x20;
