# Frequency Synchronization Analysis of an Oscillator-Based Power Grid

**Moosung Kim, Heetae Kim**  
Department of Energy Engineering, Korea Institute of Energy Technology (KENTECH)

> Published in *New Physics: Sae Mulli* **76**, 691–704 (2026).<br>
> Published online August 31, 2026.<br>
> DOI: [10.3938/NPSM.76.691](https://doi.org/10.3938/NPSM.76.691) · [Journal article](https://www.npsm-kps.org/journal/view.html?uid=8096)

---

## Overview

This repository contains the simulation code for the paper:

> **Frequency synchronization analysis of an oscillator-based power grid**  
> Moosung Kim, Heetae Kim  
> *New Physics: Sae Mulli* **76**, 691–704 (2026).<br>
> [https://doi.org/10.3938/NPSM.76.691](https://doi.org/10.3938/NPSM.76.691)

We model the Korean power grid (KPG-193, 193 buses) as a network of coupled oscillators governed by the inertial Kuramoto model (equivalently, the swing equation). Kron reduction eliminates load buses, leaving 41 generator nodes as the sole dynamical agents. Under generator trip disturbances, we show that:

- **RoCoF** is dominated by local inertia
- **Frequency nadir** is further shaped by network structure
- **Eigenvector centrality** provides the strongest correlation with nadir vulnerability
- High-centrality nodes exhibit a dual role: sensitive to remote disturbances, yet self-stabilizing when disturbed directly

## Reproducing the Results

### 1. Install

```bash
git clone https://github.com/moosung001/Power-grid-swing-analysis.git
cd Power-grid-swing-analysis
pip install -e .
pip install scipy pandas numpy networkx matplotlib seaborn
```

### 2. Run batch simulation

Simulates generator trip events across all 41 generator buses with multiple ΔP magnitudes:

```bash
python scripts/main_batch_step.py
```

Results are saved under `outputs/aggregates/`.

### 3. Reproduce paper analysis

```bash
# Frequency nadir & RoCoF summary
python scripts/analyze_batch_step.py

# Network centrality vs. frequency vulnerability (Fig. 3–4 in paper)
python scripts/analyze_network_vs_response.py

# Bus-field analysis (Fig. 5 in paper)
python scripts/analyze_busfield_vs_network
```

## Repository Structure

```
├── src/kpg_swing/          # Core simulation package
│   ├── engine/             # Physics engine
│   │   ├── swing_api.py        # Swing ODE integrator
│   │   ├── events.py           # Disturbance event handling
│   │   ├── internal_kron.py    # Kron reduction
│   │   ├── dcflow.py           # DC power flow
│   │   ├── bus_restore.py      # Inverse Kron mapping
│   │   └── islanding.py        # Island detection
│   └── core/
│       ├── loader.py           # System data loader
│       ├── metrics.py          # Nadir, RoCoF, angle spread
│       └── checks.py           # Input validation
│
├── scripts/                # Analysis pipeline
│   ├── main_batch_step.py          # Batch simulation entry point
│   ├── analyze_batch_step.py       # Result aggregation
│   ├── analyze_network_vs_response.py  # Centrality correlation
│   ├── analyze_busfield_vs_network     # Bus-field analysis
│   ├── analyze_injbus_centrality_corr.py
│   └── analyze_generator_vs_params.py
│
├── data_static/            # Static input data
│   ├── dyn_params.csv          # Generator inertia & damping (H, M, D)
│   ├── bus_location.csv        # Geographic coordinates
│   └── line_catalog.csv        # Line ratings
│
└── KPG193_ver1_2/          # KPG-193 network model (MATPOWER format)
```

## Data

The Korean power grid model (KPG-193) is a publicly available research model comprising 193 buses and 41 generators. Generator dynamic parameters (inertia constant H, damping D) are assigned by generator type following standard references (Coal: H = 5.0 s, LNG: H = 3.0 s, Nuclear: H = 6.0 s).

## Citation

If you use this code, please cite:

```bibtex
@article{kim2026frequency,
  title   = {Frequency synchronization analysis of an oscillator-based power grid},
  author  = {Kim, Moosung and Kim, Heetae},
  journal = {New Physics: Sae Mulli},
  year    = {2026},
  volume  = {76},
  pages   = {691--704},
  doi     = {10.3938/NPSM.76.691},
  url     = {https://doi.org/10.3938/NPSM.76.691}
}
```

## Contact

Heetae Kim — hkim@kentech.ac.kr
