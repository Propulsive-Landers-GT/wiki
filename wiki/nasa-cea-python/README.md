# NASA-CEA Python

NASA CEA / CEARUN Setup for Python

For our Python scripts, we use the Python `cea` package instead of manually compiling NASA CEA from source. This means you do **not** need to install Intel Fortran, CMake, Ninja, or Visual Studio Build Tools for the normal GTPL CEA scripts.

## <mark style="color:red;">Start here only if you don't have python installed on any environment yet, otherwise scroll down:</mark>

### Install Python

First, make sure Python is installed on your computer.

1. Go to [https://www.python.org/downloads/](https://www.python.org/downloads/)
2. Download the latest stable Python version.
3. During installation, check the box that says **Add Python to PATH**.
4. Finish the installation.

To check that Python installed correctly, open Command Prompt or PowerShell and run:

`python --version`

You should see a Python version number.

### Create a Project Folder

Make a folder where you want to keep your CEA scripts.

For example:

`mkdir CEA-Run`

`cd CEA-Run`

### Create a Virtual Environment

A virtual environment keeps the packages for this project separate from the rest of your computer.

Run:

`python -m venv .venv`

Then activate it:

`.venv\Scripts\activate`

If it worked, you should see `(.venv)` at the beginning of your terminal line.

## <mark style="color:red;">Start here if you've already got python installed on VS Code, or PyCharm:</mark>

### Install Required Packages

Install the packages needed for the CEA scripts:

`python -m pip install cea numpy matplotlib`

That is it. You do not need to compile NASA CEA manually.

### Check That CEA Installed Correctly

Run:

`python -c "import cea; print('CEA installed successfully')"`

If it prints:

`CEA installed successfully`

then you are good.

### Running the Script

Put the provided CEA Python script inside your project folder.

Then run:

`python your_script_name.py`

Replace `your_script_name.py` with the actual script filename.

### What This Setup Gives You

With this setup, the script can directly run CEA calculations from Python and output things like:

* C\* vs O/F
* Isp vs O/F
* Ivac vs O/F
* Cf vs O/F
* Chamber temperature
* Exit temperature
* Chamber gamma
* Exit gamma
* Molecular weight
* Exit Mach number
* Exit sonic velocity
* Summary of the peak C\* case
* Summary of the peak Isp case

## <mark style="color:red;">Common Issues</mark>

### `python` is not recognized

Python is either not installed or was not added to PATH.

Reinstall Python from https://www.python.org/downloads/ and make sure **Add Python to PATH** is checked.

### `pip` does not work

Use this instead:

`python -m pip install cea numpy matplotlib`

### The script says `ModuleNotFoundError: No module named 'cea'`

Your virtual environment may not be activated, or `cea` was installed somewhere else.

Activate your virtual environment again:

`.venv\Scripts\activate`

Then reinstall:

`python -m pip install cea numpy matplotlib`

### The plot does not appear

Make sure `matplotlib` is installed:

`python -m pip install matplotlib`

Then run the script again.

### Notes

This setup is the recommended approach for normal GTPL CEA scripts. You only need the full Fortran/CMake build process if you are trying to compile NASA CEA from source, modify the original solver, or do something more advanced than running the provided Python scripts.
