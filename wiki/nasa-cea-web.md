---
description: >-
  NASA's CEA program models chemical equilibrium and thermodynamic properties to
  support rocket performance, detonation, and shock wave analysis.
---

# NASA-CEA Web

In this guide, you will learn how to use the NASA CEARun web version to obtain equilibrium combustion properties, theoretical rocket performance, chamber temperature, characteristic velocity, specific impulse, and exhaust composition for a selected propellant combination. For anything rockets, this is a site you will be visiting frequently for a variety of reasons. In GTPL, you will need it for purposes including but not limited to:

* Obtaining Isp, c\*, expansion ratio, Cf, for engine design
* Obtaining combustion properties and product mole fractions to design things for regen & film cooling

This goes over how to obtain those values given a fixed set of conditions. It is the foundation of the foundation of the foundation to rocket design, so please make sure you are familiar with what the solver is doing on the high level such that you can incorporate it into your design effectively.&#x20;

The limitations of the web version is, of course, that you can't directly integrate it into your script. Hence, we will be using the python (our main script language) / Matlab with the downloadable version of MATLAB which we are able to obtain from NASA @ [https://github.com/nasa/cea](https://github.com/nasa/cea) and run locally for full design workflow, batch cases, and integration with our own design tools. For that, please check out [nasa-cea-python.md](nasa-cea-python.md "mention"). The download process is quite complicated so make sure to reach out to Baldwin or Michael if you have any troubles.&#x20;



Official link to NASA-CEA: [https://cearun.grc.nasa.gov/](https://cearun.grc.nasa.gov/)

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

