# Effects of Sodium Isotopes on the Neutronic Properties of Sodium-Cooled Fast Reactors (SFR)

> Monte Carlo neutronics study using OpenMC — MOX assembly inspired by Superphénix

---

## Summary

This project investigates the influence of sodium isotopes (²³Na and ²⁴Na) on the neutronic properties of a sodium-cooled fast reactor (SFR). Simulations are performed using **OpenMC** on a full MOX assembly modeled after the Superphénix architecture.

Three aspects are studied:
- The effect of sodium on the **effective multiplication factor (k-eff)**
- The evolution of k-eff as a function of **coolant temperature** (600K–800K) and **burnup** (0–35 MWd/kg)
- The **energy-dependent** neutron capture behavior of sodium

---

## Physical Background

In a sodium-cooled fast reactor, sodium serves as the primary coolant due to its excellent thermophysical properties. However, it also interacts with the neutron flux — absorbing and slightly moderating neutrons. Two key isotopes are considered:

| Isotope | Half-life | Microscopic capture cross-section @ 1 MeV (barn) |
|---------|-----------|--------------------------------------------------|
| ²³Na    | Stable    | 1.65 × 10⁻⁴                                     |
| ²⁴Na    | 15 hours  | 2.3 × 10⁻⁴                                      |

The central question of this study is:

> **To what extent does neutron absorption by sodium affect reactor neutronics over time?**

---

## Model Description

The simulation models a **complete hexagonal MOX fuel assembly** with reflective boundary conditions (infinite geometry along the z-axis), representative of a Superphénix-type SFR core.

| Parameter              | Value                        |
|------------------------|------------------------------|
| Assembly pitch         | 23.6 cm                      |
| Cell pitch             | 1.275 cm                     |
| Sodium temperature     | 600 K (nominal)              |
| Cladding temperature   | 900 K                        |
| Fuel temperature       | 1200 K                       |
| Simulation mode        | k-eff search (eigenvalue)    |
| Iterations             | 100 batches                  |
| Particles per batch    | 10,000                       |
| Nuclear data           | ENDF/B-VII.1                 |

The void coefficient is modeled by progressively replacing sodium with inert Helium-4 (0%, 50%, 100% sodium fraction).

---

## Repository Structure

```
RNR-Na-Sodium-Study/
│
├── README.md
│
├── report/
│   └── Rapport_Fortil.pdf          # Full study report (French)
│
├── notebooks/
│   ├── RNR Na - Keff(bu).ipynb     # k-eff vs burnup analysis
│   ├── RNR Na - Keff(T).ipynb      # k-eff vs temperature analysis
│   └── RNR Na - Specte.ipynb       # Neutron spectrum analysis
│
├── openmc_model/
│   ├── geometry.xml
│   ├── materials.xml
│   ├── settings.xml
│   ├── tallies.xml
│   └── plots.xml
│
├── results/
│   ├── spectre_neutronique_0.png
│   ├── spectre_neutronique_25.png
│   ├── spectre_neutronique_50.png
│   ├── spectre_neutronique_75.png
│   ├── spectre_neutronique_100.png
│   ├── impact_temperature_sodium.png
│   └── Assembly.png
│
└── data/
    └── chain_endfb71_sfr.xml       # Depletion chain
```

---

## Key Results

### Effect on k-eff

- A **~3% reactivity gain** is observed at 50% sodium void fraction, across all temperatures studied
- This effect, while real, remains **marginal in the context of a voiding accident**
- Sodium tends to **smooth the reactivity loss** with burnup, but slightly **accelerates** it
- Temperature effects on neutronics are small and essentially linear

### Neutron Spectrum

- **High energy (> 10⁵ eV):** Little visible effect of sodium voiding on flux intensity
- **Low energy (< 10 eV):** Flux decreases with increasing void fraction
- **Intermediate range (10 eV – 10⁵ eV):** Absorption resonance dips become more pronounced as sodium is removed, due to reduced parasitic capture

Full sodium voiding hardens the spectrum further, making it even more "fast".

---

## How to Reproduce

### Prerequisites

```bash
pip install openmc numpy matplotlib
```

> ⚠️ OpenMC requires a nuclear data library (ENDF/B-VII.1 or later).  
> See the [OpenMC documentation](https://docs.openmc.org) for installation instructions.

### Running the model

```bash
# Export geometry, materials and settings
python openmc_model/build_model.py

# Run simulation
openmc --threads 4

# Analyze results
jupyter notebook notebooks/RNR\ Na\ -\ Keff\(T\).ipynb
```

---

## Limitations

- Single isolated assembly — no full-core leakage effects
- ENDF/B cross-sections available at 294K, 600K, 900K, 1200K only (interpolated elsewhere)
- MOX assembly representation only — no blanket or control rod modeling
- ²⁴Na buildup not explicitly tracked in burnup simulations

---

## References

- [Benchmark study of SFR neutronics – Taylor & Francis](https://www.tandfonline.com/doi/full/10.1080/00295639.2019.1614367)
- [CEA – Nuclear Fuels Monograph](https://www.cea.fr/Documents/monographies/combustibles-nucléaires_réacteurs-eau.pdf)
- [CEA – Sodium Fast Reactors Monograph](https://www.cea.fr/Documents/monographies/reacteurs-nucleaires-sodium-pourquoi-reacteurs-rapides-sodium.pdf)
- [IOP Conference Series – SFR study](https://iopscience.iop.org/article/10.1088/1742-6596/1689/1/012056/pdf)
- [Nuclear Energy and Technology – OpenMC application](https://nucet.pensoft.net/article/91090/)

---

## Author

**D. Quillaud** — February 2025  
Study conducted as part of a portoflio purspose.

---

## License

This project is shared for educational and research purposes.  
Feel free to open an issue or a pull request if you have suggestions.
