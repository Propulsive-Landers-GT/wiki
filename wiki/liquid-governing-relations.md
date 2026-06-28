# Liquid Governing Relations

### Characteristic Length

Characteristic length relates chamber volume and throat area:

$$
L^*=\frac{V_c}{A_t}
$$

or:

$$
V_c=L^*A_t
$$

L\* is mostly chosen based on propellant combination and combustion residence time. It is a sizing rule for making sure the chamber has enough volume for combustion to complete. Typical workflow is to finalize your propellant combination and then lookup Huzel & Huang's book and use the corresponding L\*.&#x20;

### Contraction Ratio

Contraction ratio relates chamber area to throat area:

$$
\epsilon_c=\frac{A_c}{A_t}
$$

or:

$$
A_c=\epsilon_c A_t
$$

For small liquid rocket engines, contraction ratio is often around:

$$
\epsilon_c \approx 3 \text{ to } 8
$$

### Chamber Diameter

For a circular chamber:

$$
D_c=\sqrt{\frac{4A_c}{\pi}}
$$

### Chamber Length

Once chamber volume and chamber area are known, chamber length can be estimated as:

$$
L_c=\frac{V_c}{A_c}
$$

If the converging section volume is included separately:

$$
L_c=\frac{L^*A_t - V_{conv}}{A_c}
$$

### Liquid Engine Sizing Flow

A simple liquid engine sizing order is:

$$
A_t=\frac{\dot{m}_{tot}c^*}{P_c}
$$

$$
V_c=L^*A_t
$$

$$
A_c=\epsilon_cA_t
$$

$$
L_c=\frac{L^*A_t - V_{conv}}{A_c}
$$
