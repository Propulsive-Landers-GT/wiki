# Designing a Fail-Safe System

A feed system, no matter for hybrids or liquids, should not just work when everything goes right. It should also fail in a predictable and safe way when something goes wrong. In propulsion, this matters because stored pressure, and the presence of fuel and oxidizer can pose a significant risk and create dangerous failure modes very quickly.&#x20;

To address that, we design our system in a way that we know will fail-safe. A fail-safe system is designed so that loss of power, loss of signal, overpressure, or in other cases of contingencies will fully depressurize our system and allow us to approach it safely. But how?&#x20;

> <mark style="color:$primary;">To do that, we take advantage of our valve's normal states.</mark>

This is often the step right after an initial version of the PFS has been designed. We'll explore multiple examples using Visio. If you would like to explore on your own, feel free to download Visio. If you are a student, follow the instructions on [installing-visio.md](../../onboarding/software-setup/installing-visio.md "mention").

### Recognizing Trapped Volume

Before the part where we design a system that is meant to be fail-safe, we first need to be able to recognize the things that make it dangerous in the first place. For that, the first question we always want to ask ourselves is:

> <mark style="color:$primary;">Where are all the trapped volumes in this current system?</mark>

Trapped volumes, by definition, is any section of fluid that can become sealed off with no open path to vent, relieve, or drain. In a feed system, this usually happens when fluid is isolated between things like:

* closed valves
* check valves
* regulators
* caps or plugs

To visualize what a trapped volume is, let us consider the following system:&#x20;

<figure><img src="../../.gitbook/assets/image (34).png" alt=""><figcaption></figcaption></figure>

In this guide, we will treat the valve as closed when colored in red, and open when colored in green. Here, we're looking at a simple GN2 system where we're venting to ambient through an orifice. Each of the valves represent the following:&#x20;

* PU-VNT is purge vent, PMV is purge main valve
* PU-REG is purge regulator, PU-PT is purge pressure transducer&#x20;

**Consider this hypothetical situation:** Say that the regulator was originally off and now we turn on our regulator to introduce whatever fluid into our system to prepare for a quick 3 second purge (by turning on PMV) to see if the segment we had built was working. Before we had the chance to turn on PMV, however, we suddenly lose control of the entire system and both valves remain shut with us having no means of actuating them.&#x20;

What we end up with is a situation below. We have a pressurized section of line that has no way to vent. The regulator has allowed pressure into the downstream side, but PU-VNT and PMV are both closed, so the gas is now "boxed in". The section that are boxed in with no way to escape is what we call a trapped volume as highlighted.

<figure><img src="../../.gitbook/assets/image (35).png" alt=""><figcaption></figcaption></figure>

* At this point, the system may look “off” because no valve is actively flowing and nothing is venting. But that does not mean the system is safe. In fact, there is&#x20;
* This is the exact kind of case we are trying to catch during a fail-safe review. Designing a system to be fail-safe is magnitudes more important than designing just a system that meets the flow rate requirement.

The important thing to notice is that the trapped volume may not be obvious at first. The tank side may be isolated by the regulator, and the outlet side may look like it eventually goes to atmosphere, but if the valves around that section are closed, the gas between them is still trapped. This, depending on the fluid that you're working with, can become extremely dangerous real quick.&#x20;

For example, let's take a look at what happens if it were LOX or nitrous:

* **LOX:** If liquid oxygen is trapped between two closed valves, it can warm up and boil. Since liquid oxygen expands dramatically as it becomes gas, the pressure in that trapped section can rise very quickly. This can overpressurize tubing, fittings, valves, or sensors if there is no relief path.&#x20;
  * Fun but not so safe fact: LOX expands by roughly **860x** when it vaporizes to gaseous oxygen at room-temperature, atmospheric-pressure conditions.&#x20;
* **Nitrous oxide:** If nitrous is trapped as a liquid or two-phase mixture, heating can also cause rapid pressure rise. Nitrous is self-pressurizing so a small temperature increase can significantly increase vapor pressure. In a sealed section, this can create can easily create overpressure if there are no relief or active venting paths.&#x20;

## Different System Failure Modes

Before deciding how to fail-safe the system, we first need to define what we are failing against. “Something went wrong” is too vague to design around so to be prepared we need to think through the actual ways we can lose control of the system.

Based on our previous experiences at GTPL, I have rated their likelihoods on a scale of 1-5 with 5 being the most "common" failure mode.&#x20;

**Some common scenarios are:**

* **Loss of electrical power (5):** The system loses power to valves, sensors, controllers, or other electrical hardware.
* **Loss of command signal (3):** The valve or controller still has power, but it no longer receives the signal telling it what to do.
* **A valve fails to move when commanded (2):** The control system tells a valve to open or close, but the valve stays in its previous position.
* **Loss of pneumatic pressure (1):** Pneumatically actuated valves lose the air or gas pressure needed to hold or change position.
* **A regulator creeps or leaks downstream (1):** The regulator continues allowing pressure to rise downstream even when it should be holding a set pressure.

## How to Fail-Safe

Now that we know what can go wrong, we design the system so the safest action happens by default. The easiest way to do this is by using <mark style="color:green;">**valve normal states.**</mark>&#x20;

A valve’s normal state is what the valve does when it is not being actuated. In other words, if we lose power or pneumatic pressure (depending on type of valve), this is the state the valve naturally returns to. A valve can either be normally open or normally closed:&#x20;

* **Normally closed:** valve closes when not actuated
* **Normally open:** valve opens when not actuated

For fail-safe design, we usually want to:

* Stop feeding propellant or pressurant into the system
* Open a safe vent path wherever possible
* Avoid creating trapped volumes
* Make sure the system can depressurize without manual intervention.



Let us return to our previous scenario and ask ourselves the question:&#x20;

> <mark style="color:$primary;">How do we design our valve states such that the system will fail-safe?</mark>

Using this example, give it a shot before scrolling down!&#x20;

<figure><img src="../../.gitbook/assets/image (34).png" alt=""><figcaption></figcaption></figure>

To preface this, there is usually more than one way to make a system fail-safe. The goal is not to find the only possible answer, but to understand what each option does and what failure mode it protects against.

For this example, there are two obvious ways to make the trapped section safer:

1. **The first option** is to add a relief valve to the trapped section. If pressure rises above the relief setting, the relief valve opens and gives the gas somewhere to go. This protects the hardware from overpressure, and the line can remain pressurized. Though, it adds another component that has to be sized, installed, and maintained correctly. For&#x20;
2. **The second option** (and the option the team adopts) is to choose the valve normal states so the trapped volume vents by default. In this case, the safest choice is to make PU-VNT normally open. That way, if we lose power, signal, or pneumatic pressure, the vent path opens automatically and the section between PU-REG and PMV can depressurize.

For this simple system, PU-VNT being normally open is the cleaner fail-safe choice. It means the system does not need software or an operator to save it during a loss-of-control case. The valve naturally moves toward the safer state.

So for this example:

* PU-VNT should be normally open so the trapped section vents by default.
* PMV should likely be normally closed so flow to the outlet stops by default.
* PU-REG should not be trusted as the only thing preventing downstream pressure rise.
* A relief valve can still be added as another safety layer, but it should not replace good valve-state design.

The main takeaway is that relief devices are useful, but they are not a substitute for thinking through valve normal states. The best design is usually one where the system naturally vents or isolates safely when control is lost.

