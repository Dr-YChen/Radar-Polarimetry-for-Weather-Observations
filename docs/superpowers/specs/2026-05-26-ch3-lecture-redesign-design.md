# Chapter 3 Lecture Redesign — Design Spec

## Context

User provided `ch3.md` (Chapter 3 of Ryzhkov & Zrnic's book, ~550 lines) and requested redesign of Chapter 3 lectures. Existing three notebooks cover similar topics but miss ~60% of ch3.md content (HSHV/VSVH modes, full covariance matrix derivation, orientation effects with A1-A5 moments).

**Decision:** Fully rebuild all three notebooks, preserving complete mathematical derivations.

---

## Design

### Notebook 1 — `3_1_Range_Weighting_Function.ipynb`

**Sections:**
1. Pulse geometry — rectangular pulse, leading/trailing edge overlap, range resolution `cτ/2`
2. Range weighting function definition — eq. (3.1), `|W(r-r₀)|² = |p[2(r-r₀)/c]|²`
3. Digital receiver and matched filter — eq. (3.2) convolution, filter weights `h(t)`
4. WSR-88D example — Fig. 3.2 replica, gate spacing 50m vs 250m, `r_6` width

**Interactive:** Slider for pulse width `τ` (0.5–2 μs), observe how `|W(r)|²` shape and `r_6` width change.

**Math:** Full derivation of matched filter → triangular `W(r)`, area ratio 2/3.

---

### Notebook 2 — `3_2_Powers_and_Correlations.ipynb`

**Sections:**
1. Composite voltage from N_s scatterers — eq. (3.4), (3.5a/b)
2. Power from double summation — single-sum (permanent) + double-sum (fluctuating) terms, eq. (3.6)
3. Temporal averaging → ensemble average — eq. (3.7a/b/c), (3.8)
4. Weather radar equation — eq. (3.10)–(3.13), range weighting loss `l_r = 1.5`
5. Correlations — `R_hv` eq. (3.14a), `R_hh(T_s)` eq. (3.14b), Doppler velocity eq. (3.20), spectrum width eq. (3.21)
6. SHV / HSHV / VSVH mode equations — eq. (3.23)–(3.28), system constant `C₁` eq. (3.22)

**Interactive:** Slider for number of scatterers `N_s` (10–500), observe exponential power distribution emerging as `N_s` increases.

**Math:** Complete double-summation derivation, ensemble average justification, all six covariance equations.

---

### Notebook 3 — `3_3_Polarimetric_Variables.ipynb`

**Sections:**
1. Equivalent reflectivity factor — eq. (3.29), (3.30), `|K_w|² = 0.93`
2. Differential reflectivity — eq. (3.32)–(3.34), attenuation-corrected vs intrinsic `Z_DR`
3. Cross-correlation coefficient — eq. (3.35)–(3.38), backscatter differential phase `δ`
4. HSHV/VSVH modes — eq. (3.39)–(3.41), linear depolarization ratio `L_DR`, `ρ_xh`, `ρ_xv`
5. Covariance matrix compact form — eq. (3.42), three power + three correlation coefficients
6. Particle orientation effects — angular moments `A₁`–`A₅` eq. (3.43)–(3.45), axisymmetric Gaussian distribution eq. (3.46)
7. Three special cases:
   - (a) Completely random orientation → `Z_DR = 0, δ = 0, K_DP = 0`, `ρ_hv = 1 - 2·L_DR`
   - (b) Noncanted (vertical axis) → `A₁ = A₃ = 1, A₂ = A₄ = A₅ = 0`
   - (c) Gaussian distribution with width `σ` → eq. (3.51), (3.53), `r_σ = exp(-2σ²)`

**Interactive:** Sliders for canting angle dispersion `σ` (0°–50°) and axis ratio `s_b/s_a` (1.0–2.0), live plots of `Z_DR`, `L_DR`, `ρ_hv` vs `σ` (replicating Fig. 3.4).

**Math:** All nine equations from (3.29) to (3.53), orientation factor `F_orient = A₁ - A₂` Fig. 3.5 replication.

---

## Implementation Notes

- All existing notebooks in `Chapter_03_Scattering_by_Ensemble/` are replaced
- Markdown cells retain full LaTeX equations matching ch3.md notation
- Each notebook opens with learning objectives + chapter overview markdown
- References section at bottom of each notebook citing ch3.md sources
- Use `ipywidgets` for all sliders; ensure at least 2 overlay curves per plot (theory vs interactive result)
- Commit spec as `docs/superpowers/specs/2026-05-26-ch3-lecture-redesign-design.md`