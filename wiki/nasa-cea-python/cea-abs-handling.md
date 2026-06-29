# CEA ABS Handling

<figure><img src="../../.gitbook/assets/image (40).png" alt=""><figcaption></figcaption></figure>

ABS is not included in our CEA library as a reactant so instead we define ABS manually in CEA using a the molecular formula and heat of formation from Whitmore's work. This lets us use ABS in the same way we use paraffin (as a named fuel with explicitly provided thermochemical properties).

* For our scripts, ABS is modeled as:
  * `C3.85 H4.85 N0.43`
* with heat of formation:
  * `ΔH_f = 62.63 kJ/mol`
* CEA wants enthalpy in `cal/mol`, so we convert:
  * `62.63 kJ/mol = 62630 J/mol`
  * `62630 / 4.184 = 14969 cal/mol`
* So the CEA fuel line becomes:
  * `fuel=ABS wt%=10 t(k)=298.15 h,cal/mol=14969 C 3.85 H 4.85 N 0.43`

### What This Means

This line tells CEA:

* The fuel name is `ABS`
* ABS makes up `10 wt%` of the fuel blend in this specific script
* The ABS starts at `298.15 K`
* The heat of formation is `14969 cal/mol`
* The approximate elemental formula is `C3.85 H4.85 N0.43`

In the N2O/paraffin + ABS script, our fuel blend is:

* `90 wt%` paraffin wax
* `10 wt%` ABS

<mark style="color:red;">**Note that:**</mark> This will have to be modified based on the actual mass fractions of paraffin and abs for our current fuel grain.&#x20;



So the reactants section looks like:

`fuel=Paraffin wt%=90 t(k)=298.15 h,cal/mol=-444600 C 73 H 124`

`fuel=ABS wt%=10 t(k)=298.15 h,cal/mol=14969 C 3.85 H 4.85 N 0.43`

`oxid=N2O wt%=100 t(k)=298.15`



## ABS Thermochemical Properties for CEA

| Parameter          | Value to Input                  | Notes                                                      |
| ------------------ | ------------------------------- | ---------------------------------------------------------- |
| Fuel name          | ABS (user-defined)              | Not directly available as `fuel=ABS` in the CEA library    |
| Chemical formula   | `C3.85 H4.85 N0.43`             | Based on 50/43/7 monomer ratio \[1]                        |
| ΔH\_f (molar)      | `62.63 kJ/mol` or `62630 J/mol` | Group addition method \[1]                                 |
| ΔH\_f (CEA input)  | `14969 cal/mol`                 | CEA wants `h,cal/mol`, so `62630 / 4.184 = 14969`          |
| ΔH\_f (mass-based) | `1097.4 kJ/kg`                  | Around 140% higher than HTPB \[1]                          |
| Molecular weight   | `57.07 g/mol`                   | Used for converting molar values to mass-based values \[1] |
| Density (FDM)      | `~975 kg/m^3`                   | Used in ballistic / fuel grain modeling \[1]               |
| Density (extruded) | `~1025 kg/m^3`                  | Representative value for extruded ABS \[2]                 |
| h\_v (extruded)    | `~3.21 MJ/kg`                   | Used in regression / vaporization models \[2]              |
| h\_v (FDM)         | `~3.07 MJ/kg`                   | Lower than extruded ABS in the cited comparison \[2]       |
| Oxidizer           | `N2O`                           | Standard nitrous oxide, available in CEA                   |
| Useful O/F range   | `4.0-5.5`                       | Approximate range for peak C\* \[1]                        |

### Important Note

This is an approximate ABS model, not a universal property for every ABS filament or printed part. Different ABS grades can have different monomer ratios, additives, colorants, and print properties.

* For our purposes, this is a useful first-pass CEA model because it lets us estimate performance trends for N2O/paraffin/ABS fuel blends while keeping the ABS definition traceable to Whitmore's hybrid rocket work.

### References

\[1] Whitmore et al., AIAA Journal of Propulsion and Power, DOI: [https://arc.aiaa.org/doi/10.2514/1.B34382](https://arc.aiaa.org/doi/10.2514/1.B34382)

\[2] Whitmore et al., AIAA Journal of Propulsion and Power, DOI: [https://arc.aiaa.org/doi/10.2514/1.B35621](https://arc.aiaa.org/doi/10.2514/1.B35621)
