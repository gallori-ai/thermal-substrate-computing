# Thermal-Substrate Computing

Simulation code and reproducibility data for the paper:

> **Decentralized Thermal-State Load Routing and an ENAQT-Inspired Circuit
> Design Principle for Energy-Efficient Manycore Architectures**
> Cleber Barcelos Costa (2026).
> [![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.19857070.svg)](https://doi.org/10.5281/zenodo.19857070)
> [![License: CC BY 4.0](https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by/4.0/)
> [![Code License: MIT](https://img.shields.io/badge/Code-MIT-blue.svg)](LICENSE)

ORCID: [0009-0000-5172-9019](https://orcid.org/0009-0000-5172-9019)

---

## Overview

This repository contains the code and result files needed to reproduce the
two simulation phases of the paper:

- **Phase 1 — ENAQT-analog efficiency model.** Computes signal-transport
  efficiency η(T) for the proposed ENAQT-inspired topology versus
  conventional CMOS across the operating temperature range
  T ∈ [25 °C, 80 °C].
- **Phase 2 — Decentralized thermal routing agent.** Simulates a 16-node
  4×4 grid under uniform and hotspot loads, comparing the H6 decentralized
  routing algorithm against thermally-oblivious centralized scheduling.

The paper presents two independent contributions:

1. A **decentralized thermal-state load-routing algorithm** validated by
   the Phase 2 simulation (92.1% thermal variance reduction, 14.2 °C peak
   reduction under 4× localized overload, O(1) communication per node).
2. An **ENAQT-inspired circuit topology** presented as a *theoretical
   design proposal*, with the Phase 1 simulation demonstrating the
   mathematical behavior of an ENAQT-analog efficiency model. Physical
   validation in fabricated silicon is required and is the subject of the
   proposed H7 experimental program.

---

## Repository structure

```
thermal-substrate-computing/
├── README.md               This file
├── LICENSE                 MIT License (code)
├── CITATION.cff            Machine-readable citation metadata
├── requirements.txt        Python dependencies
├── .gitignore              Python ignore patterns
├── simulation_phase1.py    Phase 1 — ENAQT-analog efficiency
├── simulation_phase2.py    Phase 2 — Decentralized thermal routing
└── data/
    ├── phase1_results.json Reference output (Phase 1)
    └── phase2_results.json Reference output (Phase 2)
```

---

## Quick start

Requires Python 3.10 or later.

```bash
# Clone
git clone https://github.com/gallori-ai/thermal-substrate-computing.git
cd thermal-substrate-computing

# Install
pip install -r requirements.txt

# Reproduce Phase 1 (Table 1 of the paper)
python simulation_phase1.py

# Reproduce Phase 2 (Table 2 of the paper)
python simulation_phase2.py
```

Both scripts are deterministic. Phase 2 uses a fixed random seed (42) for
symmetry-breaking initial conditions.

> **Note:** The default output path inside the scripts
> (`knowledge/papers/H6-thermal-substrate/`) reflects the author's
> internal project layout. Either create that directory before running,
> or edit the `path` argument in each `save_results(...)` call.

---

## Reference results

Reference outputs for both phases are checked into `data/`. After running
the simulations, your generated JSON should match these files bit-for-bit
(modulo float formatting).

### Phase 1 highlights

| T (°C) | η_H6  | η_conv | Δη rel.  | Energy advantage |
| -----: | :---: | :----: | :------: | :--------------: |
| 25     | 0.750 | 0.720  | +4.2 %   | 4.0 %            |
| 50     | 0.809 | 0.660  | +22.6 %  | 18.4 %           |
| 75     | 0.859 | 0.604  | +42.2 %  | **29.7 %**       |
| 80     | 0.868 | 0.594  | +46.1 %  | 31.6 %           |

### Phase 2 highlights

| Scenario          | Routing | Mean variance (°C²) | Mean T_peak (°C) |
| :---------------- | :-----: | ------------------: | ---------------: |
| Uniform load      | H6      | 0.008               | 35.04            |
| Uniform load      | Central | 0.008               | 35.04            |
| Hotspot (4× load) | H6      | **12.46**           | **49.71**        |
| Hotspot (4× load) | Central | 158.88              | 63.88            |

---

## Authoritative parameters

All simulation parameters are documented in **Appendix A** of the paper.

**Phase 1**
- Coupling parameter κ_m = 3.0
- Bath correlation time τ_bath = 1 ps
- CMOS baseline η₀ = 0.72, α = 3.5×10⁻³ K⁻¹
- Gate capacitance C = 5 fF, supply voltage V = 0.8 V

**Phase 2**
- N = 16 nodes, 4×4 grid, von Neumann 4-neighborhood
- T_ambient = 25 °C, T_initial = 35 °C, T_threshold = 38 °C
- Q_per_task = 0.8 °C/step, α_diss = 0.08
- Hotspot multiplier = 4.0 (nodes {0, 1, 4, 5})
- 300 simulation steps, random seed = 42

---

## Citation

If you use this code or build on this work, please cite the paper:

```bibtex
@misc{costa2026thermal,
  author       = {Costa, Cleber Barcelos},
  title        = {Decentralized Thermal-State Load Routing and an
                  ENAQT-Inspired Circuit Design Principle for
                  Energy-Efficient Manycore Architectures},
  year         = {2026},
  publisher    = {Zenodo},
  version      = {1.0},
  doi          = {10.5281/zenodo.19857070},
  url          = {https://doi.org/10.5281/zenodo.19857070}
}
```

A `CITATION.cff` file is included for GitHub's automatic citation feature.

---

## License

- **Code** (`*.py`): [MIT License](LICENSE)
- **Paper and result data** (`data/*.json`, PDFs): [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/)

---

## Contributing

Issues and pull requests are welcome — replications, parameter
explorations, ports to other languages, or extensions to larger N.

The natural next step after this work is **H7**: an FPGA prototype of the
routing algorithm (Contribution 1) and a fabricated test structure for
the ENAQT-analog topology (Contribution 2). If you're working on either,
please get in touch.
