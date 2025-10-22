# Validation Plan: The HSL-3D Harmonic Scaling Law

The following plan outlines an empirical test to validate the **Harmonic Scaling Law (HSL-3D)** using public data, standard tools, and generic 3D system archetypes.

---

## Quick Navigation

| Section | Link Text (Concise) |
| :--- | :--- |
| [Goal](#goal) | **Goal: Harmonic Stability Ratio (H)** |
| [Phase 0](#phase-0-baselines--data-public-only) | Phase 0: Baselines & Public Data |
| [Phase 1](#phase-1-models-standard-public-tools) | Phase 1: Standard Public Models |
| [Phase 2](#phase-2-define-phase-alignment-factor) | Phase 2: Define $\phi$ (Phase-Alignment) |
| [Phase 3](#phase-3-define-alpha-effective-mismatch) | Phase 3: Define $\alpha_{eff}$ (Effective Mismatch) |
| [Phase 4](#phase-4-parameter-sweeps) | Phase 4: Parameter Sweeps |
| [Phase 5](#phase-5-stability-index--h-law-fit) | Phase 5: Stability Index & H-Law Fit |
| [Phase 6](#phase-6-sanity-checks--falsification) | Phase 6: Sanity Checks & Falsification |
| [Phase 7](#phase-7-minimal-hardware-proxy-still-public) | Phase 7: Minimal Hardware Proxy |
| [Phase 8](#phase-8-acceptance-criteria-what-success-looks-like) | Phase 8: Acceptance Criteria |

---

## Goal

Empirically test whether a **harmonic stability ratio** $H \triangleq \frac{P}{\alpha \cdot \phi}$ stays $\sim$**constant** along the locus of **minimum thermo-elastic stress + timing drift** in stacked/packaged 3D systems, where:

* $P$ = interconnect pitch (TSV / hybrid-bond / $\mu$bump) $[\mu\text{m}]$
* $\alpha$ = **effective** CTE mismatch between stacked layers $[\text{ppm/K}]$
* $\phi$ (phi) = **phase-alignment factor** $(0\ldots 1)$ between **electrical excitation** (clock + harmonics) and **mechanical response** (dominant eigenmodes at interconnects). ($\phi=1$ means strong alignment/worst coupling; $\phi\to 0$ means detuned/orthogonal.)

---

## Phase 0 — Baselines & Data (public only)

### Inputs
* **Materials**: Si, Cu, $\text{SiO}_2$/low-k, underfill/adhesives, mold compounds (CTE, E, $\nu$, k, $\rho$). Use handbooks/public datasheets.
* **Generic geometries**:
    * TSV: $\emptyset = 2–5 \mu\text{m}$, pitch $P = 5–20 \mu\text{m}$
    * Hybrid bond: pitch $P = 2–10 \mu\text{m}$
    * $\mu$bump: $\emptyset = 15–25 \mu\text{m}$, pitch $P = 40–120 \mu\text{m}$
* **Power maps**: Public "AI accelerator–like" heat densities (e.g., $10–50 \text{W/cm}^2$ hot spots), uniform idle vs burst profiles.
* **Clocks**: $500 \text{MHz}–5 \text{GHz}$ (include harmonics to $10\times$ for spectrum).

### Constants
* Define **three stack archetypes**: A) $\mu$bump 2.5D interposer, B) fine-pitch hybrid bond 3D, C) TSV 3D.
* Pick $2–3$ **ambient profiles**: $25^\circ\text{C}$, $60^\circ\text{C}$, $85^\circ\text{C}$; include JEDEC thermal cycles.

---

## Phase 1 — Models (standard public tools)

### Mechanical/Thermal FEA (ANSYS Mechanical or COMSOL Multiphysics)
1.  Build axisymmetric unit-cell + full 3D tiles for A/B/C.
2.  Assign $T$-dependent properties; include CTE mismatch and underfill.
3.  Thermal loads: steady-state + transient (power bursts), JEDEC thermal cycling.
4.  **Outputs**: von Mises $\sigma$, interfacial shear $\tau$, warpage $w$, TSV/UBM plastic strain, $\Delta T$ maps.

### Modal & Frequency Response
5.  Modal analysis $\to$ eigenfrequencies/modes relevant to interconnect regions.
6.  Harmonic response at **clock & harmonics** ($500 \text{MHz} \ldots 5 \text{GHz}$): FRF magnitude at interconnect hotspots.

### Electrical/Timing
7.  Interconnect RC/EM extraction (Cadence Clarity/Sigrity or Ansys SIwave; open alt: FastHenry/FastCap for coarse).
8.  Timing/jitter sims (SPICE/Spectre, open alt: Ngspice) on representative paths; include random + deterministic jitter and **supply/thermal-induced delay variation**.

---

## Phase 2 — Define $\phi$ (phase-alignment factor)

Compute $\phi \in [0,1]$ as spectral overlap between **signal excitation** and **mechanical response** at interconnect loci:

$$
\phi = \max_{f \in F_{\text{clk,harm}}} \left( \frac{|H_{\text{mech}}(f)|}{\max_{f'}|H_{\text{mech}}(f')|} \cdot \frac{|S_{\text{sig}}(f)|}{\max_{f'}|S_{\text{sig}}(f')|} \right)
$$

* $H_{\text{mech}}(f)$: FRF magnitude (displacement or stress) at interconnect nodes.
* $S_{\text{sig}}(f)$: signal PSD at the same location (post-PDN/IO path).
* **Interpretation**: $\phi\to 1$ when a strong mechanical mode coincides with strong electrical spectral content (worst coupling).

---

## Phase 3 — Define $\alpha$ (effective mismatch)

For a stack with $N$ layers (thickness $t_i$, CTE $\alpha_i$) above/below the bond plane, compute mismatch across the interface:

$$
\alpha_{\text{eff}} = \left| \frac{\sum_{i \in \text{top}} \alpha_i t_i}{\sum_{i \in \text{top}} t_i} - \frac{\sum_{i \in \text{bottom}} \alpha_i t_i}{\sum_{i \in \text{bottom}} t_i} \right|
$$

*(