# Pressurant System Design

<figure><img src="../../.gitbook/assets/image (41).png" alt=""><figcaption><p>Credit: <a href="https://www.ihi.co.jp/ia/en/products/space/tanks/index.html">https://www.ihi.co.jp/ia/en/products/space/tanks/index.html</a></p></figcaption></figure>

A pressurant system is the part of the feed system that uses a high-pressure gas to push propellant out of a tank. For GTPL systems, we use GN2 to maintain nitrous tank pressure during fill, coldflow, and hotfire. For LREs, pressurant is often needed for both fuel and oxidizer ends. The pressurant itself can also come in other common forms such as helium, though at a higher cost.&#x20;

At a high level, the pressurant system has one job:

> Keep the propellant tank at the pressure needed to deliver the required flow rate.

## What a Pressurant System Does

A pressurant system can be used to:

* Pressurize a propellant tank before a test
* Maintain tank pressure during a burn
* Push liquid propellant through the feed system
* Purge lines with inert gas
* Vent or depressurize parts of the system after a test
* Support leak checks and pressure checks



The pressurant system is usually sized around the propellant tank pressure we need. That tank pressure needs to be high enough to overcome:

* injector pressure drop
* line losses (major losses), for calculation see [component-dp-equations.md](component-dp-equations.md "mention")
* valve and fitting losses (minor losses), for calculation see [component-dp-equations.md](component-dp-equations.md "mention")
* regulator losses
* any elevation or hydrostatic effects (typically negligible)
* required chamber pressure margin

## Basic Pressurant System Layout

Starting from its most basic form, below shows 3 different configurations for what the pressurant  system could look like for hybrids and liquids.&#x20;

<figure><img src="../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

1. **The first configuration (hybrids)** is the simplest case. There, you have one pressurant source, one regulator, and one propellant tank. This is common for hybrids where we only need to pressurize one oxidizer tank, such as a hydrogen peroxide tank. The regulator takes the high pressure pressurant and drops it down to the tank pressure we want.
2. **The second configuration (liquids)** uses one regulator to pressurize two propellant tanks. This can work only if both tanks are supposed to operate at the same pressure. For example, if the LOX and RP1 tanks both need to maintain the same tank pressure, then one regulator is sufficient for both branches.
3. **The third configuration (liquids)** is similar to the second, except there are now two separate regulators for each of the tank. In this example, one regulator feeds the LOX tank and the other regulator feeds the RP1. This is usually the more flexible layout for liquids because the fuel and oxidizer tanks often need different upstream pressures due to a difference in requirements.

## Regulator Behavior

Major Reference: [https://www.swagelok.com/downloads/webcatalogs/en/ms-06-114.pdf](https://www.swagelok.com/downloads/webcatalogs/en/ms-06-114.pdf)

A regulator is not a perfect constant pressure device. It is a mechanical component that responds to upstream pressure, downstream pressure, flow demand, spring force, seat behavior, and internal geometry. As a result, the pressure downstream of a regulator is not always exactly equal to the number we think we set.

For regs, the important behaviors to understand are droop, creep, lockup, and flow capacity (flow capacity/reg sizing is covered specifically in [regulator-sizing.md](regulator-sizing.md "mention")).

<figure><img src="../../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

### Droop

**Droop is when the regulator outlet pressure decreases as flow demand increases.**

* For example, say we set a regulator to `500 psi` with no flow. Once the system starts flowing pressurant gas into a tank, the outlet pressure may drop below `500 psi`. The more flow we demand, the more the outlet pressure can droop.
* This is important for many reasons with the first being that there is often a minimum required upstream pressure to achieve the desired flow rate.&#x20;
  * **In the context of a VTVL vehicle**, that could hinder the control authority of our throttle valve. Say if the tank pressure falls significantly lower than expected, the engine would not be able to reach the commanded thrust level due to insufficient flow rate requirement.&#x20;
  * **In the context of a vehicle designed for a fixed altitude**, regulator droop can cause the engine to underperform compared to what it was originally designed for. This can reduce the total impulse, change the thrust curve, etc.&#x20;
  * **In the context of a static fire**, say that your goal is to character your engine at 60 barA upstream, but because of droop you might end up with a hotfire at 55 barA instead. In that case, you may have to re-do your hotfire if the performance does not meet your mission requirements.

#### Compensating for Droop:

* Do not treat the regulator set pressure as the actual operating pressure.&#x20;
* Check the regulator flow curve from the datasheet. The curve should tell you how much outlet pressure drops at different flow rates.
  * Put a pressure transducer downstream of the regulator and close to the tank or branch being regulated so you can verify.&#x20;
* If we know the regulator will droop by a certain amount, we may intentionally set the regulator slightly higher so that the flowing pressure lands near the target pressure.
* Run a cold flow before hotfire. This lets us measure the actual downstream pressure at realistic flow conditions.
* Leave enough margin in the pressure ladder for a few bars of droop if necessary to still meet the required flow rate demanded by the engine.&#x20;
* If droop is too large, use a regulator with higher flow capacity, reduce restrictions in the pressurant line, or worst case scenario, consider a two-stage regulator setup.

The main idea here is that we should design around the regulator's behavior using manufacturer's datasheet whenever possible, verify it, and account for it before we get to the hotfire.&#x20;

### Creep

Creep is when the regulator outlet pressure slowly rises even when there is little or no flow.

* This usually happens because the regulator seat does not seal perfectly. A tiny amount of gas leaks through the regulator and slowly increases pressure downstream.
* This matters a lot for pressurant systems because a downstream volume can become overpressurized if it is isolated and the regulator keeps creeping.

In practice:

* Creep is most dangerous when the downstream side is isolated.
* A pressure transducer downstream of the regulator can help catch creep.
* A relief valve or vent path downstream of the regulator can protect the system.
* If the regulator outlet pressure keeps climbing after flow stops, that is a warning sign that should prompt you to investigate your current PFS and reg setup.&#x20;

### Lockup

A regulator’s **lockup** pressure refers to the pressure increase in the regulator’s outlet pressure above setpoint required to shut the regulator off tight.

* Essentially, the static no flow pressure (when fluid's not moving) downstream of the regulator may settle slightly higher than the pressure we set. This happens as the downstream pressure had to rise enough to fully close the regulator seat.

So if we set a regulator to `500 psi` while flowing, it could settle at something like `510-520 psi` once flow stops. That small stable offset is usually lockup.

* This is different from creep. If the pressure rises slightly and then stops, that is lockup. If the pressure keeps slowly climbing over time, that is creep, and that is a much bigger concern.
* Lockup pressure is not normally indicated as a specific numerical value on a standard regulator datasheet. However, most regulator design selection guides estimate that standard lockup pressures will be less than 5% of the regulator's total control range so keep that in mind.&#x20;

<figure><img src="../../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

