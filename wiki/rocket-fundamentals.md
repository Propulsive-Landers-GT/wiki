---
description: >-
  This equation covers the basic terms that allow us to characterize a rocket
  engine and in some form define their efficiency/compare them against each
  other.
---

# Rocket Fundamentals

### Full Thrust Equation&#x20;

The full thrust equation includes both momentum thrust and pressure thrust.

$$
F=\dot{m}_{tot}v_e+(P_e-P_a)A_e
$$

### Total Mass Flow Rate

Total mass flow rate is the amount of propellant flowing through the engine per second.

$$
\dot{m}_{tot}=\dot{m}_{ox}+\dot{m}_f
$$

### Oxidizer-to-Fuel Ratio

$$
\frac{O}{F}=\frac{\dot{m}_{ox}}{\dot{m}_f}
$$

Simple term, but you gotta engrave it to your head!! Engine performance is heavily related to our O/F ratio as it changes various properties including c\* and Isp.&#x20;

### Characteristic Velocity

Characteristic velocity, or c\*/c-star, is one of the main ways we describe combustion performance.

$$
c^*=\frac{P_cA_t}{\dot{m}_{tot}}
$$

A higher c\* generally means the propellant combination and chamber are producing energy more effectively. It is useful because it mostly describes the chamber/combustion side, not the nozzle expansion side. It is a value we obtain on NASA-CEA. The equation for c\* you see above is what we would use to calculate c\* post-hotfire. In that case, we would compare it against our theoretical c\* to obtain our c\* efficiency.

### Thrust Coefficient

Thrust coefficient tells us how well the nozzle turns chamber pressure into thrust.

$$
C_F=\frac{F}{P_cA_t}
$$

This is mostly a nozzle performance number. It depends on expansion ratio, chamber pressure, exit pressure, ambient pressure, and gas properties. Again, it is a value we obtain on NASA-CEA.

### Specific Impulse

Specific impulse tells us how efficiently the engine uses propellant. Formal definition is thrust produced per unit weight propellant.&#x20;

$$
I_{sp}=\frac{F}{\dot{m}_{tot}g_0}
$$

Higher (I\_{sp}) means more thrust for the same propellant flow rate. This is the number people usually use to compare engines and propellant combinations. It is also the first step in sizing the combined mass flow rate of an engine we're designing for. In that case, we'd rearrange the equation in terms of mdot and calculate it based on required thrust and the Isp we obtain on [nasa-cea-web.md](nasa-cea-web.md "mention")/ [nasa-cea-python.md](nasa-cea-python.md "mention"). The equation for Isp you see above is what we would use to calculate Isp post-hotfire. In that case, we would compare it against our theoretical Isp to obtain our Isp efficiency.

### Thrust

Thrust can be written using chamber pressure and throat area:

$$
F=C_FP_cA_t
$$

or using mass flow and c\*:

$$
F=C_F\dot{m}_{tot}c^*
$$

### Throat Area

Throat area we solve for directly after we've set our desired chamber pressure. Remember at this point, we should already have our total mass flow rate from the Isp equation, and c\* from CEA. So that would leave us with chamber pressure as an unknown variable which we would set to our chamber pressure.&#x20;

$$
A_t=\frac{\dot{m}_{tot}c^*}{P_c}
$$

### Expansion Ratio

Expansion ratio tells us how large the nozzle exit is compared to the throat. It is a value we obtain directly from CEA to calculate the exit area. It can also be calculated using standard isentropic relations.&#x20;

$$
\epsilon=\frac{A_e}{A_t}
$$
