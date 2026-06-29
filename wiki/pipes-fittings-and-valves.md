# Pipes, Fittings, and Valves

This is a quick reference for common feed system components you will be working with: tubing, fittings, valves, regulators, and related components. The goal is to help members understand what each part does, when to use it, and what to check before putting it into a pressurized system.

### <mark style="color:red;">PLEASE</mark>, read through the following thoroughly

* Always check pressure rating before using a component.
  * This is huge and is something that people tend to forget.
  * We must keep track of our pressure rating of all components on the system.
* Check material compatibility with the working fluid.
* Avoid mixing fitting standards unless you know exactly what adapter is being used
  * **For example:** don't ever try to put JIC 37 and JIC 45 together because the difference in their taper angles mean they will never seal properly).
* Minimize unnecessary fittings since every fitting adds pressure drop and leak risk.
  * When possible, we want to use as little connectors as we can.&#x20;
* Make sure flow direction is correct for valves, regulators, filters, and check valves.
  * Huge, especially for check valves. We have, unfortunately, installed our check valve in the wrong direction and thankfully we caught it during cold flows.&#x20;
* Do not assume two fittings seal the same way just because the threads look similar.
  * Always look for labelling. Many of these components have a part number labelled so verify when necessary.&#x20;

### Pipes and Tubing

Tubing carries fluid between tanks, valves, injectors, vents, and to our engine.

Common things to check:

* Outer diameter
* Wall thickness
* Pressure rating
* Bend radius
* Material
* Compatibility with oxidizer, fuel, pressurant, or coolant
* Whether the tube is hardline or flexible hose



**Differences between pipes vs tubing:**&#x20;

* Pipe is specified by nominal size and schedule
* Tubing is specified by actual OD and wall thickness.



**General considerations when selecting size and material of tubing:**&#x20;

* Fluid velocity: You want to make sure that your fluid operating velocity is below what is recommended by Compressed Gas Association (CGA) and/or European Industrial Gases Association (EIGA).
  * For oxygen, refer to: EIGA Doc 13/20: Oxygen Pipeline and Piping Systems [https://www.eiga.eu/uploads/documents/DOC013.pdf](https://www.eiga.eu/uploads/documents/DOC013.pdf).&#x20;
  * As there is no universal value like oxygen for N2O, we follow the guide similar guidelines set for oxygen.&#x20;
* Material: You want to make sure that your material is compatible with the working fluid. You can easily find their compatibility by searching it up online.&#x20;

### Fittings

Fittings connect tubes, valves, sensors, tanks, and other components.

Common fitting types on our system:

* AN fittings (JIC 37, JIC 45)
* Compression fittings (Swagelok, Yorlock)
* NPT fittings
* Tube adapters
* Bulkhead fittings

Note: fittings seal in different ways. Some seal on threads, some seal on ferrules, some seal on flares, and some seal on O-rings. Do not mix these up!&#x20;

* The team adopts JIC 37 as the standard fitting for high pressure systems because swagelok is incredibly expensive. Downside of flared fittings like JIC 37 is that it takes more time to prepare/flare and is more likely to have an imperfect seal which means we may have to redo them sometimes.
* NPT fittings are used on our system when we want to connect it to a sensor. Otherwise, our system is mainly JIC 37 and Yorlock is only used on the main lander, 200 bar Nitrogen segment.

<figure><img src="../.gitbook/assets/image (21).png" alt=""><figcaption></figcaption></figure>

### Valves

Valves control whether flow is allowed, blocked, vented, throttled, or isolated.

**Common valve types:**

* Ball valve: simple on/off isolation
  * On and off unless paired with a servo to control valve angle. Characterized by simplicity and a large Cv so little dP across the part.
* Needle valve: fine manual flow adjustment
  * Finer control, but lower Cv so you get higher dP across the component.
* Check valve: allows flow in only one direction
  * The direction of flow is indicated by an arrow on the physical part.
* Relief valve: opens at a set pressure for safety
  * Direct-Acting Relief Valves: The most common and cost-effective type! They operate against a mechanical force, usually a calibrated spring that holds the valve closed until system pressure overcomes the spring's tension.&#x20;
  * If it comes at an adjustable pressure, you would have to calibrate the relief valve on your own.&#x20;
* Regulator: Takes a higher upstream pressure to a lower pressure required by the system. It is most commonly seen right after our nitrogen tanks.



**Note:** On feed systems, the most important thing is to design it in such a way that things fail safe. A separate thread will cover how to design a PFS such that in different contingencies the system relieves all pressures automatically without human intervention thus allowing people to approach safely after.

<figure><img src="../.gitbook/assets/image (20).png" alt=""><figcaption></figcaption></figure>

### Common Failure Modes

* Thread mismatch
* Over-tightening fittings
* Using the wrong sealant
* Forgetting flow direction
* Choosing a valve with too low Cv (if dP is a concern)
* Using a component below required pressure rating
* Assuming oxygen/nitrous compatibility without checking
* Adding too many fittings and creating avoidable pressure drop

### Useful Rules of Thumb

* Fewer fittings usually means fewer leaks.
