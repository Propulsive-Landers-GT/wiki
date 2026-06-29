# CEA Paraffin Handling

For most CEA scripts, we can use the Python `cea` package directly, similar to how we use packages like CoolProp. This works cleanly for common propellant combinations like LOX/RP-1, but Paraffin is a little different.

For our paraffin cases, we use the classic CEA input-file method instead of the direct Python solver API. This is because our paraffin model is passed into CEA using an explicit reactant line:

`fuel=Paraffin wt%=100 t(k)=298.15 h,cal/mol=-444600 C 73 H 124`

This line tells CEA to model paraffin using:

* Fuel name: `Paraffin`
* Fuel temperature: `298.15 K`
* Enthalpy: `h,cal/mol=-444600`
* Approximate elemental composition: `C 73 H 124`

Because of this, the paraffin scripts generate normal CEA `.inp` files and run the CEA executable, instead of using the direct `cea.Mixture(...)` Python API.

The important part is that we are **not** using a manually compiled CEA installation anymore. The script still gets CEA from the Python package installed with:

`python -m pip install cea`

The script automatically finds the CEA executable and data folder inside the installed package:

`CEA_ROOT = Path(cea.__file__).resolve().parent`

`CEA_EXE = CEA_ROOT / "bin" / "cea.exe"`

`CEA_DATA_DIR = CEA_ROOT / "data"`

So for team members, the setup is still simple. They only need to install the Python package, and the script will use the packaged CEA executable internally.

### Why We Do This

The direct Python API works well when the propellants are already handled cleanly by `cea.Mixture(...)`.

For paraffin, we want to keep the exact reactant definition that has worked in our legacy CEA scripts. The `.inp` method lets us pass that definition directly to CEA and keeps the run reproducible across different computers.

In short:

* LOX/paraffin and N2O/paraffin use the CEA input-file method.
* Propellants like LOX/RP-1 (or ones without paraffin) uses the direct Python `cea` API.
* All scripts still only require `pip install cea`.
* No one needs to compile NASA CEA manually for these scripts.

**TLDR:** Paraffin was not defined in the cea github version but defined in the legacy database. To deal with that, we import the data for paraffin manually into the cea API.&#x20;

```
reactants
  fuel=Paraffin wt%=100 t(k)={PW_TEMP_K} h,cal/mol=-444600 C 73 H 124
```

