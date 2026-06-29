# Component dP Equations

## <mark style="color:blue;">Main Reference:</mark> [Crane Technical Paper 410](https://drive.google.com/file/d/1vBPWerZ9gr5oE8z6nHaL11AzitJ6nr42/view?usp=sharing)

This page collects common pressure drop equations used for feed system components. No explanations for workflow here, it is simple a page full of equations for pipes, fittings, valves, bends, contractions, expansions, Cv parts, and CdA parts.

<figure><img src="../../.gitbook/assets/image (37).png" alt=""><figcaption></figcaption></figure>

### Shared Terms

Flow area:

$$
A=\frac{\pi D^2}{4}
$$

Velocity:

$$
v=\frac{\dot{m}}{\rho A}
$$

Dynamic pressure:

$$
q=\frac{\rho v^2}{2}
$$

Reynolds number:

$$
Re=\frac{\rho vD}{\mu}
$$

where:

* A: flow area
* D: inner diameter
* mdot: mass flow rate
* rho: density
* mu: dynamic viscosity
* v: velocity
* q: dynamic pressure

### Straight Pipe

Straight pipe or tubing pressure drop is calculated using Darcy-Weisbach:

$$
\Delta P=f\frac{L}{D}\frac{\rho v^2}{2}
$$

where:

* f: Darcy friction factor
* L: pipe or tube length
* D: inner diameter

For laminar flow:

$$
f=\frac{64}{Re}
$$

For turbulent flow, one common explicit approximation is the Haaland equation:

$$
f=
\left[
-1.8\log_{10}
\left(
\left(\frac{\epsilon/D}{3.7}\right)^{1.11}
+\frac{6.9}{Re}
\right)
\right]^{-2}
$$

where:

* epsilon: absolute roughness

### Fitting (Minor) Losses

Most fittings can be represented with:

$$
\Delta P=K\frac{\rho v^2}{2}
$$

This applies to elbows, tees, bends, valves, entrances, exits, contractions, expansions, and other components when K is known.

### Crane K Multiplier Form

Crane 410 often gives fitting losses as:

$$
K=Nf_T
$$

where:

* N: fitting multiplier from Crane
* fT: fully turbulent friction factor from Crane tables
* K: final loss coefficient

Examples:

* Full-port ball valve, open: K = 3 fT
* Gate valve, open: K = 8 fT
* Standard 45 degree elbow: K = 16 fT
* Standard 90 degree elbow: K = 30 fT
* Globe valve, open: K = 340 fT

## Finding Crane Friction Factor

Reference: Crane 410, A-27 Appendix, “Pipe Friction Data for Schedule 40 Clean Commercial Steel Pipe with Flow in Zone of Complete Turbulence.”

For clarification, the Crane friction factor is not the same as the Darcy friction factor we calculate from Reynolds number and roughness. It is a Crane table value for clean commercial steel pipe in the fully turbulent region based on nominal pipe size.&#x20;

| Nominal Pipe Size | fT    |
| ----------------- | ----- |
| 1/2 in            | 0.026 |
| 3/4 in            | 0.024 |
| 1 in              | 0.022 |
| 1 1/4 in          | 0.021 |
| 1 1/2 in          | 0.020 |
| 2 in              | 0.019 |
| 2 1/2 in          | 0.018 |
| 3 in              | 0.017 |
| 4 in              | 0.016 |
| 6 in              | 0.015 |
| 8 in              | 0.014 |
| 12 in             | 0.013 |
| 18 in             | 0.012 |
| 30 in             | 0.011 |

### Pipe Entrance

Reference: Crane 410, A-30 Appendix, “Pipe Entrance.”

For a sharp-edged entrance, use a fixed K value:

$$
\Delta P=K\frac{\rho v^2}{2}
$$

A common first-pass value is:

$$
K \approx 0.5
$$

### Pipe Exit

Reference: Crane 410, A-30 Appendix, “Pipe Exit.”

For a pipe discharging into a large volume, a common value is:

$$
K \approx 1.0
$$

So:

$$
\Delta P=\frac{\rho v^2}{2}
$$

### Custom K Component

If a vendor, test, or reference gives K directly:

$$
\Delta P=K\frac{\rho v^2}{2}
$$

Use this when the component does not fit neatly into a standard fitting category.

### Cv Valve Relation

Many valve datasheets provide Cv instead of K.

For liquids:

$$
\Delta P_{psi}=SG\left(\frac{Q_{gpm}}{C_v}\right)^2
$$

where:

* SG: specific gravity relative to water
* Qgpm: volumetric flow rate in gallons per minute
* Cv: valve flow coefficient

Specific gravity is:

$$
SG=\frac{\rho}{\rho_{water}}
$$

### CdA / Orifice Relation

If a component is represented by effective flow area:

$$
\Delta P=
\frac{\dot{m}^2}
{2\rho(C_dA)^2}
$$

where:

* CdA: effective flow area
* mdot: mass flow rate
* rho: fluid density

Use this for orifices, injector elements, restrictions, and components where effective area is known.

### Contractions

Reference: Crane 410, A-27 Appendix, “Sudden and Gradual Contraction.”

For a contraction, define:

$$
\beta=\frac{D_{small}}{D_{large}}
$$

Crane-style contraction losses depend on beta and contraction angle.

For gradual contractions with angle <= 45 degrees:

$$
K=
0.8\sin\left(\frac{\theta}{2}\right)
\frac{1-\beta^2}{\beta^4}
$$

For sharper contractions with angle > 45 degrees:

$$
K=
0.5(1-\beta^2)
\frac{\sqrt{\sin(\theta/2)}}{\beta^4}
$$

where:

* beta: small diameter / large diameter
* theta: included contraction angle

### Expansions

Reference: Crane 410, A-27 Appendix, “Sudden and Gradual Enlargement.”

For an enlargement or expansion:

$$
\beta=\frac{D_{small}}{D_{large}}
$$

For gradual expansions with angle <= 45 degrees:

$$
K=
2.6\sin\left(\frac{\theta}{2}\right)
\frac{(1-\beta^2)^2}{\beta^4}
$$

For sharper expansions with angle > 45 degrees:

$$
K=
\frac{(1-\beta^2)^2}{\beta^4}
$$

### Reduced-Port Valves and Fittings

Reference: Crane 410, A-27 Appendix, “Formulas For Calculating K Factors for Valves and Fittings with Reduced Port.”

Reduced-port components behave like a valve plus an internal contraction/expansion.

The general correction looks like:

$$
K_{total}=\frac{K_1}{\beta^4}+K_{geometry}
$$

where:

* K1: base component loss coefficient
* beta: reduced port diameter / line diameter
* Kgeometry: added loss from internal contraction/expansion

Important takeaway: A valve can have the same connection size as the line but still have a smaller internal port. That smaller port can drive pressure drop way up.

### Bends

Reference: Crane 410, A-30 Appendix, “90 deg Pipe Bends and Flanged or Butt-Welding 90 deg Elbows.”

For a single bend, Crane-style losses often use:

$$
K=Nf_T
$$

### Tees and Wyes

Reference: Crane 410, Chapter 2, pp. 2-14 through 2-16, “Hydraulic Resistance of Tees and Wyes.”

Tees and wyes are more complicated because the loss depends on flow direction.

You would need to know:

* Is the flow converging or diverging?
* Are we calculating loss through the branch or the straight run?
* What is the branch angle?
* What fraction of flow goes through the branch?
* What is the branch diameter ratio?

For calculations involving Tees and Wyes, check their corresponding section in Crane 410.&#x20;

