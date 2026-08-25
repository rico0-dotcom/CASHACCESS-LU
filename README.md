# CASHACCESS-LU

## Geospatial Cash Infrastructure Accessibility and Resilience in Luxembourg

**CASHACCESS-LU** is a reproducible empirical research framework for studying the geographic accessibility, spatial distribution, and resilience of physical cash infrastructure in Luxembourg.

The project integrates official population-grid data, Luxembourg administrative geography, OpenStreetMap cash-access infrastructure, a routable road network, and ECB ATM statistics to transform heterogeneous infrastructure data into population-weighted accessibility measures, spatial-econometric evidence, and infrastructure-closure scenarios.

> **Research question:**  
> **How geographically accessible is physical cash infrastructure in Luxembourg, how is accessibility distributed across the population and municipalities, and how vulnerable is access to changes in cash infrastructure?**

The project was developed as an empirical research prototype at the intersection of **financial infrastructure, financial services, geospatial economics, econometrics, Information Systems, and FinTech**.

---

## Why this project matters

The decline of physical cash infrastructure raises a practical financial-services question: **where does access to cash remain geographically available, and which populations may be exposed when infrastructure becomes less dense?**

A national ATM count alone cannot answer this.

CASHACCESS-LU instead combines:

**Who lives where?**  
→ 2021 population grid

**Where is cash infrastructure?**  
→ mapped ATMs, bank branches, and post offices

**How far is it?**  
→ straight-line accessibility

**How reachable is it by road?**  
→ road-network accessibility

**Is poor access spatially clustered?**  
→ Moran's I and LISA

**What happens when an ATM disappears?**  
→ single-ATM closure simulations

This converts raw infrastructure locations into **population-exposure and resilience evidence** that can be used in financial-infrastructure research and decision support.

---

# Key findings

The current validated analysis produces the following headline results.

| Indicator | Result |
|---|---:|
| 2021 population represented | **643,420** |
| Official population-grid cells | **2,795** |
| Communes | **100** |
| Physical cash-access points | **332** |
| ATMs | **139** |
| Bank branches | **148** |
| Post offices | **45** |
| Population-weighted mean distance to physical cash access | **1.303 km** |
| Population-weighted median distance to physical cash access | **0.714 km** |
| Population-weighted mean distance to ATM | **1.742 km** |
| Population-weighted median distance to ATM | **0.902 km** |
| Population-weighted mean road-network distance to physical cash access | **1.921 km** |
| Population-weighted mean road-network distance to ATM | **2.546 km** |
| ECB ATM count, 2025 | **299** |
| ECB peak ATM count, 2018 | **596** |
| Decline from 2018 peak to 2025 | **49.83%** |

### Population coverage

Using straight-line accessibility to mapped infrastructure:

- **65.51%** of represented population is within 1 km of a physical cash-access point.
- **53.18%** is within 1 km of an ATM.
- **78.88%** is within 2 km of physical cash access.
- **68.88%** is within 2 km of an ATM.
- **96.60%** is within 5 km of physical cash access.

These measures are population-weighted rather than treating every geographic cell as equally important.

---

# Infrastructure data

The current OSM cash-access layer contains:

| Facility type | Mapped points |
|---|---:|
| ATM | **139** |
| Bank branch | **148** |
| Post office | **45** |
| **Total** | **332** |

The project deliberately distinguishes these facility types rather than treating all physical cash-access points as equivalent.

The **ECB ATM series is kept conceptually separate from the OSM point layer**. ECB statistics provide national historical context, while OSM provides the current spatial infrastructure layer.

---

# Data sources

## Luxembourg administrative geography

The project uses Luxembourg commune boundaries and the LUREF projected coordinate system:

**EPSG:2169 — LUREF**

## Luxembourg population grid

The population exposure layer contains:

- **2,795 official 1-km population-grid records**
- 2021 census population values
- **643,420 represented population**

One zero-population grid cell does not have a usable Luxembourg representative point. The complete official population dataset is retained; only usable cells are used for point-based accessibility calculations.

## OpenStreetMap

Current mapped physical cash-access infrastructure is obtained through OpenStreetMap using ATM, bank, and post-office features.

## Road network

The project uses a Luxembourg driving network containing:

- **23,529 nodes**
- **54,544 edges**

The road-network analysis measures route distance over the driving network. It is **not a walking or public-transport accessibility model**.

## European Central Bank

The project uses ECB ATM statistics for a national historical ATM-stock series covering **2000–2025**.

The series peaks at **596 ATMs in 2018** and reaches **299 in 2025**, corresponding to a **49.83% decline from the peak**.

---

# Methodology

## 1. Data validation and harmonisation

Before analysis, the project validates:

- population totals;
- row counts;
- geometry validity;
- coordinate reference systems;
- facility classifications;
- network availability;
- ECB time-series structure.

Spatial layers are standardised to **EPSG:2169 (LUREF)** for metre-based calculations.

---

## 2. Population exposure modelling

The official 1-km population grid is the master population exposure layer.

Population-grid geometries are intersected with Luxembourg and represented by an interior analysis point for distance calculations.

For commune-level aggregation, population is allocated using **cell–commune intersection shares**, conserving the full 643,420 population across the 100 communes.

The allocation is validated explicitly:

```text
Source population:       643,420
Allocated population:    643,420
Difference:              0
```

---

## 3. Straight-line accessibility

For every usable population-grid cell, the project calculates:

### All physical cash access

Nearest:

- ATM
- bank branch
- post office

### ATM-only access

Nearest:

- ATM

This produces:

```text
distance_all_km
distance_atm_km
```

---

## 4. Population-weighted accessibility

Accessibility is weighted by population rather than simply averaging geographic cells.

The population-weighted mean is:

$$
\bar{d}_w =
\frac{\sum_i P_i d_i}
{\sum_i P_i}
$$

where:

- $P_i$ is the population represented by grid cell $i$.
- $d_i$ is the distance from grid cell $i$ to the nearest cash-access facility.

The project also calculates true population-weighted medians.

This distinction matters because a sparsely populated rural grid cell should not have the same analytical weight as a densely populated urban cell.

---

## 5. Accessibility inequality

The analysis produces:

- distributional quantiles;
- population coverage within 250 m, 500 m, 1 km, 2 km, and 5 km;
- population beyond specified distance thresholds;
- ATM versus all-cash accessibility gaps.

This allows the research question to move from:

> "What is the average distance?"

to:

> "How many residents are potentially exposed to poor accessibility?"

---

## 6. Commune-level analysis

Population-grid results are aggregated to all **100 Luxembourg communes**.

The commune-level dataset contains variables such as:

- population;
- population density;
- ATM count;
- bank-branch count;
- post-office count;
- total physical cash-access points;
- cash-access density;
- ATM accessibility;
- all-cash accessibility;
- ATM accessibility gap.

This enables geographic comparison of underserved areas.

---

## 7. Spatial autocorrelation

The project uses PySAL to estimate:

### Global Moran's I

Tests whether accessibility outcomes are spatially clustered across communes.

Current result:

```text
Moran's I       = 0.17492
Permutation p   = 0.006
```

### Local Moran's I / LISA

Identifies local spatial patterns such as:

- High-High;
- Low-Low;
- High-Low;
- Low-High;
- Not significant.

These results are used as **exploratory spatial evidence**, not causal estimates.

---

## 8. Exploratory spatial econometrics

The project examines the relationship between:

**ATM accessibility distance**

and

**log population density**

using:

- OLS;
- spatial lag modelling;
- spatial error modelling.

The OLS specification produces:

```text
R² ≈ 0.384
β(log population density) ≈ -1.219
p < 0.001
```

The negative association indicates that denser communes tend to have lower ATM-distance measures in this dataset.

### Important interpretation

This is an **observational cross-sectional association**.

It does **not** establish that population density causes better ATM accessibility.

A future causal design would require additional covariates, historical infrastructure observations, and a credible identification strategy.

---

# 9. Road-network accessibility

Straight-line distance can underestimate actual travel distance.

CASHACCESS-LU therefore adds a road-network layer.

For each population location, the model incorporates:

```text
population point
      ↓
nearest road-network node
      ↓
shortest road route
      ↓
nearest facility
```

Point-to-network connector distances are included.

The final network results are:

```text
Physical cash access:   1.921 km
ATM access:             2.546 km
```

Network distances are at least as large as corresponding straight-line distances for **100% of the 2,794 usable observations**.

Mean network / straight-line ratios are approximately:

```text
Physical cash:  1.637x
ATM:            1.617x
```

---

# 10. Infrastructure resilience: single-ATM closure simulation

The project goes beyond measuring current accessibility.

For each ATM, it asks:

> **What happens to population accessibility if this ATM disappears?**

For a given population cell:

\[
\Delta d_i =
d_{i,\text{after}}
-
d_{i,\text{before}}
\]

The model uses the nearest and second-nearest ATM:

```text
Current nearest ATM = A
Second-nearest ATM   = B

If A closes:
    new nearest ATM = B
```

For each simulated closure, the project calculates:

- population-weighted accessibility before closure;
- population-weighted accessibility after closure;
- population-weighted distance increase;
- population experiencing at least +1 km additional distance;
- population experiencing at least +2 km;
- population experiencing at least +5 km.

The closure baseline is explicitly validated against the main ATM accessibility metric:

```text
Closure baseline:       1.742153 km
Main ATM metric:        1.742153 km
Difference:             0.000000000 km
```

The highest-ranked facility currently produces approximately:

```text
Before closure:       1.742153 km
After closure:        1.808921 km
Increase:             0.066767 km
```

This is a **scenario impact**, not a forecast or causal estimate.

---

# Research contribution

CASHACCESS-LU demonstrates a complete empirical workflow for transforming heterogeneous financial-infrastructure data into decision-relevant evidence:

```text
Heterogeneous data
        ↓
Data validation
        ↓
Spatial integration
        ↓
Population exposure modelling
        ↓
Accessibility measurement
        ↓
Spatial dependence
        ↓
Econometric analysis
        ↓
Road-network modelling
        ↓
Infrastructure resilience simulation
        ↓
Decision-support outputs
```

The project therefore sits at the intersection of:

- Financial infrastructure
- Financial services
- FinTech
- Geospatial economics
- Econometrics
- Information Systems
- Spatial statistics
- Infrastructure resilience
- Computational social science

---

# Why this is relevant to financial-infrastructure research

The framework can support questions such as:

- Where is physical cash access geographically concentrated?
- Which populations have relatively poor access?
- Are underserved municipalities spatially clustered?
- Does population density relate to infrastructure accessibility?
- How much does road topology change measured accessibility?
- Which individual ATM closures generate the largest population-level accessibility deterioration?

The design is intentionally modular so that additional socioeconomic, demographic, behavioural, payment, and historical infrastructure data can be incorporated later.

---

# Repository structure

```text
CASHACCESS-LU/
│
├── notebook/
│   ├── CashAccess_LU_verified_downloader.ipynb
│   └── CashAccess_LU_empirical_analysis_full.ipynb
│
├── data/
│   ├── luxembourg_communes.gpkg
│   ├── luxembourg_1km_reference_grid.gpkg
│   ├── population_grid_1km_2021.gpkg
│   ├── cash_points_current.gpkg
│   ├── luxembourg_drive_network.graphml
│   └── bcl_ecb_atm_counts.csv
│
├── outputs/
│   ├── cashaccess_population_metrics.gpkg
│   ├── cashaccess_commune_metrics.gpkg
│   ├── atm_closure_impacts.csv
│   ├── 14_cashaccess_national_summary.csv
│   └── [generated maps and figures]
│
├── cache/
│   └── [local/download cache files]
│
└── README.md
```

---

# Notebooks

## `CashAccess_LU_verified_downloader.ipynb`

Responsible for obtaining and validating the empirical inputs.

Main tasks:

- Luxembourg commune boundaries;
- official 1-km reference grid;
- 2021 population-by-grid data;
- OSM cash-access infrastructure;
- Luxembourg road network;
- ECB ATM statistics;
- CRS and population validation.

## `CashAccess_LU_empirical_analysis_full.ipynb`

Runs the complete empirical analysis:

- accessibility;
- population weighting;
- inequality;
- commune aggregation;
- infrastructure distribution;
- ECB trend analysis;
- Moran's I / LISA;
- exploratory spatial econometrics;
- road-network accessibility;
- ATM closure resilience;
- research-ready exports.

---

# Main software stack

```text
Python
pandas
NumPy
GeoPandas
SciPy
statsmodels
scikit-learn
NetworkX
OSMnx
PySAL
libpysal
spreg
Matplotlib
```

The project is designed around reproducible Jupyter workflows.

---

# Reproducibility

The recommended sequence is:

### 1. Download and validate data

Run:

```text
notebook/CashAccess_LU_verified_downloader.ipynb
```

### 2. Run the empirical analysis

Run:

```text
notebook/CashAccess_LU_empirical_analysis_full.ipynb
```

### 3. Inspect outputs

The analysis exports GeoPackage, CSV, figures, and summary statistics to:

```text
outputs/
```

---

# Limitations

The current analysis should be interpreted with the following limitations.

1. The population layer represents the **2021 census grid**, while the OSM infrastructure is a later/current snapshot.
2. OpenStreetMap coverage depends on mapping completeness and may not represent every physical facility.
3. ATM, bank branches, and post offices are not homogeneous services and should not be interpreted as perfectly substitutable.
4. The road-network model is a **driving network**, not a walking or public-transport accessibility model.
5. The spatial econometric analysis is exploratory and cross-sectional rather than causal.
6. A historical geospatial infrastructure panel would be required to rigorously estimate the effects of infrastructure closures over time.
7. Richer socioeconomic variables are needed to identify which groups are most socially or economically vulnerable to poor cash access.
8. Population-grid accessibility uses representative points for 1-km cells rather than individual household locations.

---

# Future research

Potential extensions include:

- Add STATEC income, age, employment, and socioeconomic indicators.
- Construct historical ATM and branch snapshots.
- Develop spatial-panel models.
- Study walking and public-transport accessibility.
- Add payment-behaviour and cash-use indicators.
- Estimate heterogeneous effects across demographic groups.
- Develop facility-location and maximal-coverage optimisation models.
- Integrate resilience scenarios with policy optimisation.
- Build an interactive Streamlit decision-support system for financial-infrastructure analysis.
- Extend the framework to other European financial centres.

---

# Author

**Anuj Pal**

MBA (Finance) | Computer Science Engineer

Research interests include:

- Financial infrastructure
- FinTech
- Applied econometrics
- Geospatial economics
- Spatial econometrics
- Causal inference
- Computational social science
- Financial technology and payment systems

---

# Academic positioning

CASHACCESS-LU is designed as a research prototype rather than a production central-bank system.

The project demonstrates how heterogeneous infrastructure and financial datasets can be transformed into reproducible evidence for questions involving:

**financial services + technology + geography + population + organisational decision-making**

This positioning makes the project particularly relevant to research at the intersection of **Information Systems, financial technology, financial infrastructure, geospatial economics, and empirical econometrics**.

---

# License

This repository is intended primarily for academic and research use.

Please verify the licensing and terms of service of each underlying external data source before redistribution or commercial reuse.
