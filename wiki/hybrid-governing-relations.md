---
description: Governing equations needed in the general design of a hybrid rocket engine.
---

# Hybrid Governing Relations

{% embed url="https://www.youtube.com/watch?v=LJj69JuZ7og" %}

Team's favorite reference for these are: [https://web.stanford.edu/\~cantwell/AA284A\_Course\_Material/Karabeyoglu%20AA%20284A%20Lectures/AA284a\_Lecture10.pdf](https://web.stanford.edu/~cantwell/AA284A_Course_Material/Karabeyoglu%20AA%20284A%20Lectures/AA284a_Lecture10.pdf)&#x20;

* Highly recommended for those who wish to understand in-depth how design choices affect hybrid performance and how changing one variable lead to change in another.&#x20;
* Contains all the equations up to the point which you will need to design some sort of solver to "optimize" for the length of your engine. In air quotes because there are different levels of optimization!&#x20;

### Port Diameter Growth

For constant oxidizer mass flow rate, the port diameter as a function of time is:

$$
D_p(t)=\left[D_{pi}^{2n+1}+\frac{(2n+1)2^{2n+1}a}{\pi^n}\dot{m}_{ox}^{\,n}t\right]^{\frac{1}{2n+1}}
$$

### Oxidizer Mass Flux

The instantaneous oxidizer mass flux through the port is:

$$
G_{ox}(t)=\frac{4\dot{m}_{ox}}{\pi D_p^2}
$$

### Regression Rate

The fuel regression rate is modeled as:

$$
\dot{r}(t)=aG_{ox}^{n}
$$

### Fuel Mass Flow Rate

The instantaneous fuel mass flow rate is:

$$
\dot{m}_f(t)=\rho_f \pi D_p L \dot{r}
$$

### Oxidizer-to-Fuel Ratio

The instantaneous mixture ratio is:

$$
\frac{O}{F}=\frac{\dot{m}_{ox}}{\dot{m}_f}
$$

### Chamber Pressure

Using total mass flow rate, nozzle throat area, discharge coefficient, and characteristic velocity:

$$
P(t)=\frac{(\dot{m}_{ox}+\dot{m}_f)c^*_{theo}\eta_c}{A_{nt}C_d}
$$

### Thrust

The thrust can be estimated from total mass flow rate, characteristic velocity, combustion efficiency, thrust coefficient, and nozzle efficiency:

$$
T(t)=(\dot{m}_{ox}+\dot{m}_f)c^*_{theo}\eta_c C_{F,theo}\eta_n
$$

### Initial and Final Port Diameter Relation

For constant oxidizer flow rate, the initial and final port diameters are related by:

$$
\left(\frac{D_{pf}}{D_{pi}}\right)^{2n+1}-1=
\frac{(2n+1)2^{2n+1}a}{D_{pi}^{2n+1}\pi^n}\dot{m}_{ox}^{\,n}t_b
$$

### Final Port Diameter From Total Oxidizer Mass

The final port diameter can also be related to total oxidizer mass:

$$
D_{pf}=
\left[
\left(
\frac{(2n+1)2^{2n+1}a}{\pi^n}
\right)
\frac{M_{ox}^{n}t_b^{1-n}}
{1-\left(D_{pi}/D_{pf}\right)^{2n+1}}
\right]^{\frac{1}{2n+1}}
$$

### Fuel Grain Length

The fuel grain length can be related to total oxidizer mass, target O/F, fuel density, and port diameter change:

$$
L=
\frac{4M_{ox}}
{\pi \rho_f (O/F)\left(D_{pf}^2-D_{pi}^2\right)}
$$
