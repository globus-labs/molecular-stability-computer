# Molecular Stability Computer
[![CI](https://github.com/HydrogenStorage/molecular-stability-computer/actions/workflows/python-app.yml/badge.svg)](https://github.com/HydrogenStorage/molecular-stability-computer/actions/workflows/python-app.yml)

Estimate the synthesizability of a molecule by assessing its stability relative to other molecules.
Computes the E<sub>min</sub> metric proposed by Lee et al., which is the difference between the energy of a molecule 
and its lowest-energy constitutional isomer.

## Installation

> The installation requirements are only compatible with Linux.

Create a virtual environment with the required Python packages using Anaconda:

```commandline
conda env create --file environment.yml
```

Then download [Surge](https://github.com/StructureGenerator/surge), which is used to generate candidate molecules.

```commandline
cd bin
. get-surge.sh
```

## Quickstart

The [`compute_emin.py`](./compute_emin.py) script is CLI application 
which computes E<sub>min</sub> for arbitrary molecules.

At minimum, it takes the SMILES string for your target molecule as input.

```commandline
time python compute_emin.py CC=C
```

The application will then compute the energy of the molecule using xTB,
then compare that energy to molecules from PubChem and those generated by Surge.

```log
2023-11-01 08:44:36,316 - main - INFO - Starting E_min run for CC=C (InChI Key: QQONPFPTGQHPMA-UHFFFAOYSA-N) in runs/C3H6
2023-11-01 08:44:36,371 - main - INFO - Started Parsl and created the app to be run
2023-11-01 08:44:36,372 - main - INFO - Loaded 0 energies from previous runs
2023-11-01 08:44:38,604 - main - INFO - Target molecule has an energy of -9.442 Ha
2023-11-01 08:44:39,651 - main - INFO - Pulled 6 molecules for C3H6 from PubChem
2023-11-01 08:44:39,654 - main - INFO - Waiting for 5 computations to finish
2023-11-01 08:44:40,299 - main - INFO - Successfully ran 6/6 molecules from PubChem
2023-11-01 08:44:40,299 - main - INFO - Emin of molecule compared to PubChem: 0.0 mHa
2023-11-01 08:44:40,299 - main - INFO - Comparing against all molecules generated by Surge
2023-11-01 08:44:40,302 - main - INFO - Generated 2 molecules and submitted 0 to run
2023-11-01 08:44:40,302 - main - INFO - Ran 0 molecules from Surge. 0 failed.
2023-11-01 08:44:40,302 - main - INFO - Final E_min compared against 6 molecules:  0.0 mHa
```

The results of energy calculations will be stored in the `runs` directory so that you can 
compute the energies of other molecules more quickly.

### Options

`compute_emin.py` provides a few options for 

- `--surge-amount`: How many molecules to generate with Surge. Set to either a fraction 
  of all possible molecules or a total amount.
- `--level`: What level of quantum chemistry to run. Options include 'xtb'
- `--no-relax`: Skip relaxing the model

A full listing of options for `compute_emin.py` is available by calling.
