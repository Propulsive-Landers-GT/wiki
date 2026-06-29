# Script for LOX/RP-1

This script runs a NASA CEA O/F sweep for LOX/RP-1 using the Python `cea` package.

It outputs:

* A plot of Cf vs O/F
* A plot of Isp and Ivac vs O/F
* A plot of C\* vs O/F
* A plot of chamber and exit temperature vs O/F
* A plot of chamber/exit gamma and molecular weight vs O/F
* A plot of exit Mach number and exit sonic velocity vs O/F
* A terminal summary of the peak C\* case
* A terminal summary of the peak Isp case

Before running it, install the required Python packages:

`python -m pip install cea numpy matplotlib`

You do **not** need to update `CEA_EXE` or `CEA_DATA_DIR` anymore. The modified version calls the installed Python `cea` package directly instead of running an external NASA CEA executable.

```
import numpy as np
import matplotlib.pyplot as plt
import cea

PRESSURE_BAR = 40.0
PE_BAR = 1.01325
OF_VALUES = np.arange(0.35, 10.01, 0.35)

OX_NAME = "LOX"
FUEL_NAME = "RP-1"

LOX_TEMP_K = 90.17
RP1_TEMP_K = 298.15

G0 = 9.80665
DEBUG = True


def debug_print(msg: str) -> None:
    if DEBUG:
        print(msg)


def run_cea_case(of_ratio: float) -> dict:
    reactant_names = ["RP-1", "O2(L)"]
    reactant_temperatures = np.array([RP1_TEMP_K, LOX_TEMP_K])
    fuel_weights = np.array([1.0, 0.0])
    oxidizer_weights = np.array([0.0, 1.0])
    pressure_ratio = PRESSURE_BAR / PE_BAR

    reactants = cea.Mixture(reactant_names)
    products = cea.Mixture(reactant_names, products_from_reactants=True)
    solver = cea.RocketSolver(products, reactants=reactants)
    solution = cea.RocketSolution(solver)

    weights = reactants.of_ratio_to_weights(oxidizer_weights, fuel_weights, of_ratio)
    chamber_enthalpy = reactants.calc_property(cea.ENTHALPY, weights, reactant_temperatures) / cea.R

    solver.solve(solution, weights, PRESSURE_BAR, [pressure_ratio], hc=chamber_enthalpy, iac=True)

    if not solution.converged:
        raise RuntimeError("CEA did not converge.")

    return {
        "cf": solution.coefficient_of_thrust[-1],
        "isp_s": solution.Isp[-1] / G0,
        "ivac_s": solution.Isp_vacuum[-1] / G0,
        "cstar": solution.c_star[-1],
        "tc": solution.T[0],
        "te": solution.T[-1],
        "gamma_c": solution.gamma_s[0],
        "gamma_e": solution.gamma_s[-1],
        "mw_c": solution.MW[0],
        "mw_e": solution.MW[-1],
        "mach_e": solution.Mach[-1],
        "sonic_e": solution.sonic_velocity[-1],
    }


if __name__ == "__main__":
    results = {
        "of": [],
        "cf": [],
        "isp_s": [],
        "ivac_s": [],
        "cstar": [],
        "tc": [],
        "te": [],
        "gamma_c": [],
        "gamma_e": [],
        "mw_c": [],
        "mw_e": [],
        "mach_e": [],
        "sonic_e": [],
    }

    for of_ratio in OF_VALUES:
        try:
            parsed = run_cea_case(of_ratio)

            results["of"].append(of_ratio)
            results["cf"].append(parsed["cf"])
            results["isp_s"].append(parsed["isp_s"])
            results["ivac_s"].append(parsed["ivac_s"])
            results["cstar"].append(parsed["cstar"])
            results["tc"].append(parsed["tc"])
            results["te"].append(parsed["te"])
            results["gamma_c"].append(parsed["gamma_c"])
            results["gamma_e"].append(parsed["gamma_e"])
            results["mw_c"].append(parsed["mw_c"])
            results["mw_e"].append(parsed["mw_e"])
            results["mach_e"].append(parsed["mach_e"])
            results["sonic_e"].append(parsed["sonic_e"])

            debug_print(
                f"O/F={of_ratio:.3f} | "
                f"Cf={parsed['cf']:.6f}, "
                f"Isp={parsed['isp_s']:.3f} s, "
                f"Ivac={parsed['ivac_s']:.3f} s, "
                f"C*={parsed['cstar']:.3f} m/s, "
                f"Tc={parsed['tc']:.3f} K, "
                f"Te={parsed['te']:.3f} K, "
                f"gamma_c={parsed['gamma_c']:.6f}, "
                f"MW_c={parsed['mw_c']:.6f}"
            )

        except Exception as e:
            debug_print(f"FAILED at O/F = {of_ratio:.3f}: {e}")

            results["of"].append(of_ratio)
            results["cf"].append(np.nan)
            results["isp_s"].append(np.nan)
            results["ivac_s"].append(np.nan)
            results["cstar"].append(np.nan)
            results["tc"].append(np.nan)
            results["te"].append(np.nan)
            results["gamma_c"].append(np.nan)
            results["gamma_e"].append(np.nan)
            results["mw_c"].append(np.nan)
            results["mw_e"].append(np.nan)
            results["mach_e"].append(np.nan)
            results["sonic_e"].append(np.nan)

    of = np.array(results["of"])
    cf = np.array(results["cf"])
    isp_s = np.array(results["isp_s"])
    ivac_s = np.array(results["ivac_s"])
    cstar = np.array(results["cstar"])
    tc = np.array(results["tc"])
    te = np.array(results["te"])
    gamma_c = np.array(results["gamma_c"])
    gamma_e = np.array(results["gamma_e"])
    mw_c = np.array(results["mw_c"])
    mw_e = np.array(results["mw_e"])
    mach_e = np.array(results["mach_e"])
    sonic_e = np.array(results["sonic_e"])

    fig, axs = plt.subplots(3, 2, figsize=(16, 9))
    fig.suptitle(
        f"{OX_NAME} / {FUEL_NAME} O/F Sweep at Pc = {PRESSURE_BAR:.1f} bar, Pe = {PE_BAR:.5f} bar",
        fontsize=16,
    )

    ax = axs[0, 0]
    ax.plot(of, cf, linewidth=2)
    ax.set_title("Cf vs O/F")
    ax.set_xlabel("O/F Ratio")
    ax.set_ylabel("Exit Thrust Coefficient, Cf")
    ax.grid(True)

    ax = axs[0, 1]
    ax.plot(of, isp_s, linewidth=2, label="Isp")
    ax.plot(of, ivac_s, "--", linewidth=2, label="Ivac")
    ax.set_title("Specific Impulse vs O/F")
    ax.set_xlabel("O/F Ratio")
    ax.set_ylabel("Specific Impulse (s)")
    ax.legend()
    ax.grid(True)

    ax = axs[1, 0]
    ax.plot(of, cstar, linewidth=2)
    ax.set_title("C* vs O/F")
    ax.set_xlabel("O/F Ratio")
    ax.set_ylabel("Characteristic Velocity, C* (m/s)")
    ax.grid(True)

    ax = axs[1, 1]
    ax.plot(of, tc, linewidth=2, label="Tc")
    ax.plot(of, te, linewidth=2, label="Te")
    ax.set_title("Temperature vs O/F")
    ax.set_xlabel("O/F Ratio")
    ax.set_ylabel("Temperature (K)")
    ax.legend()
    ax.grid(True)

    ax = axs[2, 0]
    ax.plot(of, gamma_c, linewidth=2, label="Gamma chamber")
    ax.plot(of, gamma_e, linewidth=2, label="Gamma exit")
    ax.set_title("Gamma and Molecular Weight vs O/F")
    ax.set_xlabel("O/F Ratio")
    ax.set_ylabel("Gamma")
    ax.grid(True)

    ax2 = ax.twinx()
    ax2.plot(of, mw_c, ":", linewidth=2, label="MW chamber")
    ax2.plot(of, mw_e, "--", linewidth=2, label="MW exit")
    ax2.set_ylabel("Molecular Weight")

    lines1, labels1 = ax.get_legend_handles_labels()
    lines2, labels2 = ax2.get_legend_handles_labels()
    ax.legend(lines1 + lines2, labels1 + labels2, loc="best")

    ax = axs[2, 1]
    ax.plot(of, mach_e, linewidth=2, label="Mach exit")
    ax.set_title("Exit Mach and Sonic Velocity vs O/F")
    ax.set_xlabel("O/F Ratio")
    ax.set_ylabel("Exit Mach Number")
    ax.grid(True)

    ax2 = ax.twinx()
    ax2.plot(of, sonic_e, "--", linewidth=2, label="Sonic velocity exit")
    ax2.set_ylabel("Exit Sonic Velocity (m/s)")

    lines1, labels1 = ax.get_legend_handles_labels()
    lines2, labels2 = ax2.get_legend_handles_labels()
    ax.legend(lines1 + lines2, labels1 + labels2, loc="best")

    plt.tight_layout()
    plt.show()

    print("\nSummary")

    valid_cstar = ~np.isnan(cstar)
    if np.any(valid_cstar):
        idx = np.nanargmax(cstar)
        print("\nPeak C* Case")
        print(f"Peak C*      : {cstar[idx]:.2f} m/s at O/F = {of[idx]:.2f}")
        print(f"Cf           : {cf[idx]:.4f}")
        print(f"Isp          : {isp_s[idx]:.2f} s")
        print(f"Ivac         : {ivac_s[idx]:.2f} s")
        print(f"Tc           : {tc[idx]:.2f} K")
        print(f"gamma_c      : {gamma_c[idx]:.4f}")
        print(f"MW_c         : {mw_c[idx]:.4f}")
    else:
        print("\nNo valid C* points were computed.")

    valid_isp = ~np.isnan(isp_s)
    if np.any(valid_isp):
        idx = np.nanargmax(isp_s)
        print("\nPeak Isp Case")
        print(f"Peak Isp     : {isp_s[idx]:.2f} s at O/F = {of[idx]:.2f}")
        print(f"Ivac         : {ivac_s[idx]:.2f} s")
        print(f"C*           : {cstar[idx]:.2f} m/s")
        print(f"Cf           : {cf[idx]:.4f}")
        print(f"Tc           : {tc[idx]:.2f} K")
        print(f"gamma_c      : {gamma_c[idx]:.4f}")
        print(f"MW_c         : {mw_c[idx]:.4f}")
    else:
        print("\nNo valid Isp points were computed.")
```
