# 📚 Literature Map – HSL-3D (Harmonic Stability in 3D Integrated Systems)

The literature on **electro-thermo-mechanical coupling in 3D-stacked semiconductors** is vast but fragmented.  
Research groups focus separately on thermal, mechanical, or electrical reliability; few integrate all three into a unified quantitative metric.  
The proposed **HSL-3D** framework and its **Harmonic Stability Index (HSI)** seek to bridge these domains by providing a single cross-field invariant.

---

## 1️⃣ Thermal–Mechanical Coupling Family 🌡️🔧

**Core Idea:**  
3D integration introduces complex thermo-elastic stresses due to CTE mismatch and constrained heat flow.  
Work in this family models **stress, warpage, and delamination** as functions of material and geometric parameters.

**Representative Works:**  
- Chan et al. (2025); Lee (2025) — hybrid bonding and CTE mismatch.  
- Jung et al. (2012) — pitch-dependent TSV stress.  
- Tian (2025); Wang (2024) — thermo-mechanical reliability reviews.  
- Gong et al. (2025) — VEM multi-scale coupling analysis.

**Takeaways:**  
- CTE mismatch (α) and interconnect pitch (P) dominate vertical stress gradients.  
- Thermal gradients (ΔT) amplify stress; uniform dissipation pathways are critical.  
- Most models stop at stress or displacement; none relate these to timing or signal integrity.

**Gap / Opportunity:**  
Introduce **dimensionless stability index HSI = f(P, α, ΔT, R_th)** that connects stress behavior to electrical timing stability.

---

## 2️⃣ Electro–Thermal Coupling Family ⚡🌡️

**Core Idea:**  
Electrical excitation → Joule heating → local temperature rise → material expansion → electrical parameter drift.  
These works explore closed-loop feedback between electrical and thermal domains.

**Representative Works:**  
- Lim et al. (2013) — TSV placement vs. thermal coupling.  
- Athikulwongse et al. (2012) — die-to-die thermal coupling in placement.  
- Weiss et al. (2013) — mutual thermal coupling in SiGe devices.  
- Rich et al. (2023) — thermal scaffolding for 12-tier 3D ICs.

**Takeaways:**  
- Thermal resistance (R_th) and electrical power density (P_el) form a tight feedback loop.  
- Local hotspots alter device parameters and phase relationships.  
- Existing research quantifies temperature rise, not harmonic alignment.

**Gap / Opportunity:**  
Extend coupling analysis to include **phase alignment (φ)** between electrical excitation and mechanical resonance — foundational for “harmonic stability.”

---

## 3️⃣ Thermo-Mechanical → Timing Coupling Family ⏱️

**Core Idea:**  
Studies that directly tie thermal stress or mechanical strain to timing drift or clock skew.  

**Representative Works:**  
- Shi & Srivastava (2014) — thermal-stress-aware timing analysis.  
- Jaurigue et al. (2015) — resonant feedback and jitter reduction in semiconductor lasers (analog precedent).  
- McNichol (2025) — industry observations linking heat-induced stress to late-stage timing failures.

**Takeaways:**  
- Timing jitter and drift correlate with local temperature and stress variations.  
- Resonant alignment (feedback) can *reduce* jitter — proof of “band-limited stability.”  

**Gap / Opportunity:**  
Define HSI bands predicting when timing drift and stress co-minimize — the *harmonic ridge* of stable operation.

---

## 4️⃣ Reliability Index / Lifetime Metrics Family 🧮

**Core Idea:**  
Power and reliability engineers already use scalar metrics (ΔTj, Tj,max) for lifetime prediction.  
This family demonstrates that **simple invariants** can guide complex multi-field systems.

**Representative Works:**  
- Power-device lifetime models (2023–2025).  
- Package fatigue and cycling tests (industry standards).  

**Takeaways:**  
- Lifetime correlates with amplitude and frequency of thermal cycles.  
- Compact metrics are powerful design tools when physically interpretable.

**Gap / Opportunity:**  
Translate that philosophy to 3D ICs:  
HSI → a scalar “health” variable integrating geometry, materials, and coupling.

---

## 5️⃣ Modelling & Co-Simulation Frameworks 🧩

**Core Idea:**  
Integrated numerical environments (FEM, VEM, multiphysics solvers) are rapidly advancing and could host an HSI module.

**Representative Works:**  
- Gong et al. (2025) — Virtual Element Method for multi-scale coupling.  
- STAMP-2.5D (2025) — structural–thermal-aware floorplanning.  
- Industrial FEA co-design pipelines (Siemens, ANSYS, Cadence).

**Takeaways:**  
- Multi-physics environments can compute all HSI terms directly (P, α, ΔT, φ, R_th).  
- Parameter extraction is feasible with current EDA/FEM tools.  

**Gap / Opportunity:**  
Integrate **HSI computation and visualization** into existing simulation flows — akin to a DRC or health-metric overlay.

---

## 6️⃣ Cross-Field Validation Family 🌐

**Core Idea:**  
Outside the semiconductor domain, *harmonic stability* phenomena are experimentally verified in MEMS/NEMS resonators and optical systems.

**Representative Works:**  
- MEMS frequency stability with nonlinear temperature compensation (2025).  
- Thermoelastic drift in resonators (2018–2025).  
- Semiconductor laser jitter control (2015).

**Takeaways:**  
- Stability bands emerge naturally where coupling is tuned (phase or temperature).  
- Confirms theoretical basis for harmonic band-limited stability in physical systems.

**Gap / Opportunity:**  
Generalize these validated “stability bands” to the architectural level of 3D stacks through HSI formalism.

---

## 🧭 Where HSL-3D Extends the Landscape

| Novelty | Description | Supported By |
|:--|:--|:--|
| **Unified Harmonic Stability Index (HSI)** | A dimensionless measure integrating P, α, φ, ΔT, R_th, σ, δt into one invariant describing electro-thermo-mechanical harmony. | All families above |
| **Stability Band Theory** | Defines quantitative zones of co-minimized stress and jitter — the “safe ridge.” | Thermo-Mechanical + Timing + Cross-Field |
| **Design Rule Integration** | Embeds HSI bands into EDA as predictive design-for-reliability tools. | Modelling + Reliability families |
| **Cross-Domain Validation** | Uses evidence from MEMS/NEMS and photonics to confirm harmonic coupling logic. | Cross-Field Validation |

---

### 🧩 Next Steps

1.  Expand each family with 1–2 illustrative figures or schematics.  
2.  Add KPI references: stress amplitude, temperature gradient, timing jitter, power density.  
3.  Prepare “Gap → Hypothesis” table for Phase 4 synthesis (next phase).

