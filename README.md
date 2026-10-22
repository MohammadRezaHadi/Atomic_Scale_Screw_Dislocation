# Atomic Scale Screw Dislocation

This repository contains input scripts, structure-generation commands, and simulation outputs for studying **1/2⟨110⟩ screw dislocations in FCC Aluminum** at the atomic scale.

The workflow combines **Atomsk** for building dislocation structures and **LAMMPS** for energy minimization and force evaluation using different interatomic potentials (EAM/MEAM and Machine-Learning MTP).

---

## Repository Structure

```
Atomic_Scale_Screw_Dislocation/
│
├── Atomsk Commands/               # Atomsk scripts to generate dislocation structures (Also the .lmp files for LAMMPS codes)
│   ├── Single_Screw_Dislocation.txt
│   ├── Dipole_Screw_Dislocation.txt
│   └── Quadrupole_Screw_Dislocation.txt
│
├── Codes/                        # LAMMPS input scripts
│   ├── EAM_MEAM.in               # Energy minimization with EAM/MEAM potentials
│   └── MTP.in                    # Energy minimization with MTP potential
│
└── Dump Files/                    # LAMMPS dump outputs for visualization/analysis
    ├── Dipole Screw Annihilation/
    └── Quadrupole Screw Annihilation/
```

---

## Requirements

| Tool | Purpose |
|------|---------|
| [Atomsk](https://atomsk.univ-lille.fr/) | Generate FCC supercells and insert screw dislocations |
| [LAMMPS](https://www.lammps.org/) | Energy minimization and force calculation |
| [OVITO](https://www.ovito.org/) | Visualize dump files and compute structural properties |

### Interatomic Potentials

- **EAM/MEAM files** (e.g., `Al99.eam.alloy`, `library.meam`, `Al.meam`) are required for `EAM_MEAM.in`.
- **MTP potential files** (`mlip.ini`) are required for `MTP.in`.
- Place the potential files in the working directory before running LAMMPS, or update the `pair_coeff` paths in the input scripts accordingly.

---

## Usage

### 1. Generate a Dislocation Structure (Atomsk)

Use the command files in `Atomsk Commands/` to create the initial atomic configuration. For example, to build a **single screw dislocation**:

```bash
atomsk --create fcc 4.05 Al orient [1-12] [-111] [110] -duplicate 40 20 20 Al_supercell.xsf
atomsk Al_supercell.xsf -dislocation 0.5*box 0.5*box screw Z Y 2.863782464 Al_screw_single.xsf lmp
```

The output `.lmp` file is ready to be read by LAMMPS.

### 2. Run LAMMPS

Navigate to the `Codes/` directory and run the desired input script:

```bash
# EAM/MEAM potential
lmp -in EAM_MEAM.in

# MTP + ZBL potential
lmp -in MTP.in
```

> **Note:** The input scripts contain commented lines for single, dipole, and quadrupole configurations. Uncomment the relevant `read_data` and `write_data`/`dump` lines for your case.

### 3. Analyze Results

The dump files in `Dump Files/` can be opened in **OVITO** to visualize:

- Atomic positions and types
- Centrosymmetry parameter (`c_csym`)
- Potential energy per atom (`c_peatom`)
- Stress components (`c_stress[1]`–`c_stress[6]`)
- Dislocation analysis (`DXA`)

---

## Output Files

The LAMMPS scripts generate:

- Relaxed atomic configurations (`*_relaxed.lmp`)
- Force and stress dump files (`dump.screw_*_force`)
- Movie-style dump files for minimization trajectories (`dump.screw_*_movie_*`)

---

## References

If you use this repository in your work, please cite the relevant tools:

- **Atomsk**: P. Hirel, *Atomsk: A tool for manipulating and converting atomic data files*, Comput. Phys. Commun. **197**, 212 (2015).
- **LAMMPS**: A. P. Thompson et al., *LAMMPS – a flexible simulation tool for particle-based materials modeling at the atomic, meso, and continuum scales*, Comput. Phys. Commun. **271**, 108171 (2022).

---

## Contact

**Mohammad Reza Hadi**  
GitHub: [@MohammadRezaHadi](https://github.com/MohammadRezaHadi)
