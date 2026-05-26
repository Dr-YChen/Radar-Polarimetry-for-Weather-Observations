# Chapter 3 Lecture Redesign — Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Fully rebuild three Chapter 3 Jupyter notebooks (`3_1_Range_Weighting_Function.ipynb`, `3_2_Powers_and_Correlations.ipynb`, `3_3_Polarimetric_Variables.ipynb`) based on ch3.md, with complete mathematical derivations and ipywidgets interactive controls.

**Architecture:** Three self-contained notebooks, each covering one major topic from ch3.md. Each notebook has: learning objectives markdown, theoretical content with LaTeX equations, interactive widgets, and visualization plots comparing theory with interactive results. Notebooks build on each other (3_1 → 3_2 → 3_3).

**Tech Stack:** Jupyter Notebook, ipywidgets, matplotlib, numpy, scipy.

---

## File Structure

```
Chapter_03_Scattering_by_Ensemble/
├── 3_1_Range_Weighting_Function.ipynb      (rebuild from scratch)
├── 3_2_Powers_and_Correlations.ipynb        (rebuild from scratch)
├── 3_3_Polarimetric_Variables.ipynb         (rebuild from scratch)
└── README.md                                 (update after all notebooks)
```

Each notebook lives in `Chapter_03_Scattering_by_Ensemble/` and is self-contained.

---

## Task Map

| Task | Notebook | Focus |
|------|----------|-------|
| 1 | 3_1 | Range Weighting Function — pulse geometry, matched filter, WSR-88D example |
| 2 | 3_2 | Powers & Correlations — double summation, ensemble average, SHV/HSHV/VSVH |
| 3 | 3_3 | Polarimetric Variables — definitions, orientation effects, Gaussian σ-dependence |

---

## Task 1: Rebuild `3_1_Range_Weighting_Function.ipynb`

**Files:**
- Create: `Chapter_03_Scattering_by_Ensemble/3_1_Range_Weighting_Function.ipynb`

### Step 1: Write notebook header and learning objectives

In a new notebook, add a markdown cell:
```
# 3.1 Range Weighting Function

**Learning objectives:**
- Understand the concept of range resolution (cτ/2) for rectangular pulses
- Derive the range weighting function |W(r)|² from pulse envelope p(t)
- Comprehend the matched filter principle and its effect on resolution
- Plot the range weighting function for WSR-88D parameters
```

### Step 2: Section 1 — Pulse Geometry and Range Resolution

Markdown cell with:
- Description of rectangular pulse `p(t)` propagation
- Derivation of overlap condition: if `r₂ - r₁ < cτ/2`, scatterers cannot be resolved
- Equation (3.1): `|W(r - r₀)|² = |p[2(r-r₀)/c]|²`
- Fig. 3.1 description (two overlapping rectangles at different ranges)

### Step 3: Interactive pulse width slider

Code cell with ipywidgets:
```python
import numpy as np
import matplotlib.pyplot as plt
from ipywidgets import interact, FloatSlider

def plot_range_weighting(tau_us=1.0):
    """Plot range weighting function for given pulse width tau (in microseconds)."""
    c = 3e8  # speed of light m/s
    tau = tau_us * 1e-6  # convert to seconds
    r_resolution = c * tau / 2  # range resolution in meters

    # Create range axis (centered at 0)
    r = np.linspace(-500, 500, 1000)  # meters
    # Rectangular pulse weighting
    W = np.where(np.abs(r) <= r_resolution, 1.0, 0.0)
    W_sq = W ** 2

    fig, ax = plt.subplots(figsize=(10, 4))
    ax.plot(r, W_sq, 'b-', linewidth=2, label=r'$|W(r)|²$ (rectangular)')
    ax.axvline(x=r_resolution, color='r', linestyle='--', label=f'r₆ = {r_resolution:.0f} m')
    ax.set_xlabel('Range relative to r₀ (m)')
    ax.set_ylabel(r'$|W(r)|²$')
    ax.set_title(f'Range Weighting Function: τ = {tau_us} μs → cτ/2 = {r_resolution:.0f} m')
    ax.legend()
    ax.grid(True, alpha=0.3)
    plt.tight_layout()
    plt.show()

interact(plot_range_weighting,
         tau_us=FloatSlider(value=1.0, min=0.5, max=2.0, step=0.1,
                            description=r'τ (μs):'))
```

### Step 4: Section 2 — Matched Filter and Digital Receiver

Markdown cell with:
- Convolution equation (3.2): `y(t_i) = Σ h(t_i - t_m) p(t_m)`
- Mirror image interpretation of W(r) — y(t) flipped about origin
- Equation (3.3): `W(r_i) = y(-c t_i / 2)`
- Note: matched filter for rectangular pulse → triangle shape for W(r), area 2/3 of rectangle
- Fig. 3.2 description: WSR-88D (KOUN) range weighting function, 40 dB skirts, r₆ = 240 m

### Step 5: Interactive matched filter demonstration

Code cell demonstrating matched filter effect:
```python
def plot_matched_filter_effect(tau_us=1.0):
    """Show how matched filter changes rectangular p(t) into triangular W(r)."""
    c = 3e8
    tau = tau_us * 1e-6
    r_res = c * tau / 2

    r = np.linspace(-600, 600, 1200)
    # Rectangular pulse envelope
    p = np.where(np.abs(r) <= r_res, 1.0, 0.0)
    # Matched filter: running sum = triangle
    # Use cumulative sum to approximate convolution
    W_triangle = np.convolve(p, np.ones(int(len(p)*tau_us/10))/int(len(p)*tau_us/10), mode='same')
    W_triangle = W_triangle / W_triangle.max()

    fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(12, 4))
    ax1.plot(r, p, 'b-', label='p(t) rectangular')
    ax1.set_title('Transmitted Pulse p(t)')
    ax1.set_xlabel('Time / Range')
    ax1.legend()
    ax1.grid(alpha=0.3)

    ax2.plot(r, W_triangle**2, 'r-', label=r'$|W(r)|²$ after matched filter')
    ax2.set_title('Range Weighting Function |W(r)|²')
    ax2.axvline(x=r_res, color='k', linestyle='--', alpha=0.5, label=f'r₆ = {r_res:.0f} m')
    ax2.legend()
    ax2.grid(alpha=0.3)
    plt.tight_layout()
    plt.show()

interact(plot_matched_filter_effect,
         tau_us=FloatSlider(value=1.0, min=0.5, max=2.0, step=0.1,
                            description=r'τ (μs):'))
```

### Step 6: WSR-88D real parameters plot

Code cell with realistic WSR-88D parameters:
```python
def plot_wsr88d_example():
    """Replicate Fig. 3.2-style plot with WSR-88D KOUN parameters."""
    # Real parameters from ch3.md
    r_6 = 240  # meters (6 dB width)
    sample_spacing = 50  # meters (research), 250m (operational)

    r = np.arange(-400, 401, sample_spacing)
    # Approximate real range weighting function shape (asymmetric, ~400m wide at -60dB)
    # Use shifted Gaussian mix to approximate the asymmetric shape
    r_center = 0
    # Split into two halves with different widths for asymmetry
    r_pos = np.abs(r[r >= 0])
    r_neg = np.abs(r[r < 0])
    W_pos = np.exp(-4 * np.log(2) * (r_pos / (r_6 * 0.6))**2)  # steeper on right
    W_neg = np.exp(-4 * np.log(2) * (r_neg / (r_6 * 0.8))**2)  # shallower on left
    W_sq = np.concatenate([W_neg[::-1], W_pos])
    W_sq_dB = 10 * np.log10(W_sq + 1e-10)

    fig, ax = plt.subplots(figsize=(10, 5))
    ax.plot(r, W_sq_dB, 'b-', linewidth=1.5)
    ax.scatter(r[::2], W_sq_dB[::2], c='red', s=20, zorder=5, label='samples (50 m)')
    ax.axhline(y=-6, color='gray', linestyle='--', alpha=0.7, label='-6 dB (r₆)')
    ax.axhline(y=-40, color='gray', linestyle=':', alpha=0.7, label='-40 dB')
    ax.axvline(x=-r_6/2, color='red', linestyle='--', alpha=0.5)
    ax.axvline(x=r_6/2, color='red', linestyle='--', alpha=0.5)
    ax.set_xlabel('Range relative to center (m)')
    ax.set_ylabel('|W(r)|² (dB)')
    ax.set_title('WSR-88D (KOUN) Range Weighting Function — Fig. 3.2 style')
    ax.legend()
    ax.set_ylim(-60, 5)
    ax.grid(True, alpha=0.3)
    plt.tight_layout()
    plt.show()

plot_wsr88d_example()
```

### Step 7: Commit

```bash
git add Chapter_03_Scattering_by_Ensemble/3_1_Range_Weighting_Function.ipynb
git commit -m "feat(ch3): rebuild 3_1 Range Weighting Function notebook

- Add learning objectives and pulse geometry section
- Interactive slider for pulse width τ (0.5-2 μs)
- Matched filter demonstration with triangular W(r)
- WSR-88D KOUN replica plot (Fig. 3.2 style)

Co-Authored-By: Claude Opus 4.7 <noreply@anthropic.com>"
```

---

## Task 2: Rebuild `3_2_Powers_and_Correlations.ipynb`

**Files:**
- Create: `Chapter_03_Scattering_by_Ensemble/3_2_Powers_and_Correlations.ipynb`

### Step 1: Write notebook header

Markdown:
```
# 3.2 Powers and Correlations: Ensemble of Scatterers

**Learning objectives:**
- Derive composite voltage V_hh from N_s scatterers using double summation
- Distinguish single-sum (permanent) vs double-sum (fluctuating) terms
- Understand ensemble average and its relationship to temporal average
- Master the six covariance equations for SHV, HSHV, and VSVH modes
```

### Step 2: Section 1 — Composite Voltage

Markdown with eq. (3.4), (3.5a/b):
- `V_hh = B Σ F_i G_i s_hh^(i) exp[-j 2 Re(k_h) r_i]`
- `B = (P_h^t)^(1/2) g λ / 4π · exp(-j φ_h^(sys))`
- `F_i = 1/(r_i² l_h(r_i))`, `G_i = f²(θ_i, φ_i) W_i`

### Step 3: Section 2 — Double Summation Derivation

Markdown with eq. (3.6):
- First term (i=k): individual scatterer power — permanent info
- Second term (i≠k): pair interactions — fluctuates with scatterer positions
- Temporal averaging → double sum → 0; single sum → dominant

### Step 4: Interactive — Scatterer count effect on power distribution

```python
from ipywidgets import IntSlider
import numpy as np
import matplotlib.pyplot as plt

def plot_power_distribution(N_s=50, seed=42):
    """Show exponential power distribution emerging as N_s increases."""
    np.random.seed(seed)
    # Simulate N_s scatterers with random phases
    phases = np.random.uniform(0, 2*np.pi, N_s)
    # Composite voltage (simplified model)
    V = np.sum(np.exp(1j * phases))
    power = np.abs(V)**2 / N_s

    # Run M trials
    M = 500
    powers = []
    for _ in range(M):
        phases = np.random.uniform(0, 2*np.pi, N_s)
        V = np.sum(np.exp(1j * phases))
        powers.append(np.abs(V)**2 / N_s)

    fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(12, 4))

    # Histogram
    ax1.hist(powers, bins=50, density=True, alpha=0.7, color='steelblue')
    # Exponential fit
    mean_p = np.mean(powers)
    x_exp = np.linspace(0, max(powers)*1.5, 200)
    ax1.plot(x_exp, (1/mean_p) * np.exp(-x_exp/mean_p), 'r-', linewidth=2,
             label=f'Exponential (μ={mean_p:.2f})')
    ax1.set_xlabel('Normalized Power')
    ax1.set_ylabel('Probability Density')
    ax1.set_title(f'Power Distribution (N_s={N_s}, M={M} trials)')
    ax1.legend()
    ax1.grid(alpha=0.3)

    # Variance vs N_s
    Ns_range = np.arange(5, 201, 5)
    variances = []
    for N in Ns_range:
        trial_powers = []
        for _ in range(100):
            phases = np.random.uniform(0, 2*np.pi, int(N))
            V = np.sum(np.exp(1j * phases))
            trial_powers.append(np.abs(V)**2 / N)
        variances.append(np.var(trial_powers))

    ax2.plot(Ns_range, variances, 'b-', linewidth=2)
    ax2.axhline(y=0, color='gray', linestyle='--', alpha=0.5)
    ax2.set_xlabel('Number of Scatterers N_s')
    ax2.set_ylabel('Variance of Power Estimate')
    ax2.set_title('Variance Reduction with More Scatterers')
    ax2.grid(alpha=0.3)

    plt.tight_layout()
    plt.show()

interact(plot_power_distribution,
         N_s=IntSlider(value=50, min=10, max=500, step=10,
                       description=r'N_s:'),
         seed=IntSlider(value=42, min=1, max=100, step=1,
                        description='Seed:'))
```

### Step 5: Section 3 — Ensemble Average and Weather Radar Equation

Markdown with eq. (3.7a/b/c) through (3.13):
- Temporal average definition
- Ensemble average `<P_hh>` = time average as M → ∞
- Eq. (3.9): `<|s_hh|²> = ∫ N(X) |s_hh|² dX`
- Weather radar equation (3.13) with system constant C₁ (3.22)
- Range weighting loss l_r = 1.5 (1.76 dB) for matched rectangular pulse

### Step 6: Section 4 — Correlations

Markdown with eq. (3.14a) through (3.21):
- `R_hv` cross-correlation eq. (3.14a): `<V_hh* V_vv>`
- `R_hh(T_s)` autocorrelation eq. (3.14b): Doppler information
- Doppler velocity eq. (3.20): `<v> = -λ/(4π T_s) arg[<R_hh(T_s)>]`
- Spectrum width eq. (3.21): `σ_v` from |⟨R_hh⟩|/⟨P_hh⟩ ratio`

### Step 7: Section 5 — SHV/HSHV/VSVH Mode Equations

Markdown with eq. (3.22) through (3.28):
- System constant C₁ (3.22)
- SHV mode: eq. (3.23), (3.24), (3.25)
- HSHV mode: eq. (3.23), (3.24), (3.26), (3.27)
- VSVH mode: eq. (3.23), (3.26), (3.28)
- Covariance matrix summary (3.42): `[Z_hh, ρ_xh, ρ_hv; _, Z_hv, ρ_xv; _, _, Z_vv]`

### Step 8: Interactive — Mode comparison

```python
from ipywidgets import Dropdown

def plot_mode_comparison(mode='SHV'):
    """Display available measurements per radar mode."""
    modes = {
        'SHV': {
            'powers': [r'$P_{hh}$', r'$P_{vv}$', r'$P_{hv}$'],
            'correlations': [r'$R_{hv}$'],
            'derived': [r'$Z_{hh}$', r'$Z_{DR}$', r'$\rho_{hv}$'],
            'description': 'Simultaneous H+V transmit, simultaneous H+V receive'
        },
        'HSHV': {
            'powers': [r'$P_{hh}$', r'$P_{vv}$', r'$P_{hv}$'],
            'correlations': [r'$R_{hv}$', r'$R_{xh}$'],
            'derived': [r'$Z_{hh}$', r'$Z_{DR}$', r'$\rho_{hv}$', r'$L_{drh}$', r'$\rho_{xh}$'],
            'description': 'H transmit, simultaneous H+V receive'
        },
        'VSVH': {
            'powers': [r'$P_{hh}$', r'$P_{vv}$', r'$P_{hv}$'],
            'correlations': [r'$R_{hv}$', r'$R_{xv}$'],
            'derived': [r'$Z_{hh}$', r'$Z_{DR}$', r'$\rho_{hv}$', r'$\rho_{xv}$'],
            'description': 'V transmit, simultaneous V+H receive'
        }
    }
    m = modes[mode]
    fig, ax = plt.subplots(figsize=(8, 4))
    ax.axis('off')
    ax.set_title(f'Radar Mode: {mode}')
    text = f"{m['description']}\n\n"
    text += f"Powers ({len(m['powers'])}): {', '.join(m['powers'])}\n\n"
    text += f"Correlations ({len(m['correlations'])}): {', '.join(m['correlations'])}\n\n"
    text += f"Derived Variables ({len(m['derived'])}): {', '.join(m['derived'])}"
    ax.text(0.1, 0.5, text, fontsize=12, va='center', fontfamily='monospace')
    plt.tight_layout()
    plt.show()

interact(plot_mode_comparison,
         mode=Dropdown(options=['SHV', 'HSHV', 'VSVH'],
                        description='Mode:'))
```

### Step 9: Commit

```bash
git add Chapter_03_Scattering_by_Ensemble/3_2_Powers_and_Correlations.ipynb
git commit -m "feat(ch3): rebuild 3_2 Powers and Correlations notebook

- Double summation derivation with single/double sum terms
- Interactive: power distribution converges to exponential as N_s grows
- Ensemble average → weather radar equation (Eq. 3.13)
- Correlations R_hv and R_hh(T_s) with Doppler formulas
- SHV/HSHV/VSVH mode equations (3.23)-(3.28)
- Interactive mode comparison widget

Co-Authored-By: Claude Opus 4.7 <noreply@anthropic.com>"
```

---

## Task 3: Rebuild `3_3_Polarimetric_Variables.ipynb`

**Files:**
- Create: `Chapter_03_Scattering_by_Ensemble/3_3_Polarimetric_Variables.ipynb`

### Step 1: Write notebook header

Markdown:
```
# 3.3 Polarimetric Variables and Orientation Effects

**Learning objectives:**
- Define equivalent reflectivity Z_hh, differential reflectivity Z_DR, cross-correlation ρ_hv
- Distinguish measured (attenuated) vs intrinsic polarimetric variables
- Understand HSHV/VSVH mode variables: LDR, ρ_xh, ρ_xv
- Analyze effects of particle orientation distributions on polarimetric variables
- Plot Z_DR, LDR, ρ_hv vs canting angle dispersion σ (replicating Fig. 3.4)
```

### Step 2: Section 1 — Equivalent Reflectivity Factor

Markdown with eq. (3.29), (3.30), (3.31):
- `Z_hh = 4λ⁴/(π⁴|K_w|²) <|s_hh|²>` with `K_w = (ε_w - 1)/(ε_w + 2)`, `|K_w|² ≈ 0.93`
- Weather radar equation (3.30): absolute constant in brackets, `|K_w|² = 0.93` for all scatterers
- Attenuated reflectivity (3.31): `Z̄_hh = Z_hh / l_h²`

### Step 3: Section 2 — Differential Reflectivity

Markdown with eq. (3.32) through (3.34):
- `Z̄_dr = P_hh/P_vv = (l_v²/l_h²) <|s_hh|²>/<|s_vv|²>` — concentration cancels
- True Z_DR in dB: `Z_DR = 10 log(<|s_hh|²>/<|s_vv|²>)`
- Attenuation correction: `Z̄_DR = Z_DR - 20 log(l_h/l_v)`

### Step 4: Section 3 — Cross-Correlation Coefficient

Markdown with eq. (3.35) through (3.38):
- `ρ̄_hv = <R_hv>/(⟨P_hh⟩⟨P_vv⟩)^(1/2)` — complex
- Intrinsic `ρ_hv` (3.37): cancels concentration
- Backscatter differential phase `δ = arg <s_hh* s_vv>` (3.38)
- `ρ_hv` influenced by shape and canting angle distribution

### Step 5: Section 4 — HSHV/VSVH Mode Variables

Markdown with eq. (3.39) through (3.41):
- Linear depolarization ratio: `L_dr = P_vh/P_hh = <|s_vh|²>/<|s_hh|²> · (l_h/l_v)`
- `ρ_xh` (3.40): co-cross correlation coefficient
- `ρ_xv` (3.41): co-cross correlation from V transmit
- Covariance matrix (3.42): upper triangular 3×3

### Step 6: Section 5 — Particle Orientation Effects

Markdown with eq. (3.43) through (3.45):
- Angular moments: `A₁ = <sin²ψ cos²α>`, `A₂ = <sin²ψ sin²α>`, `A₃ = <sin⁴ψ cos⁴α>`, `A₄ = <sin⁴ψ sin⁴α>`, `A₅ = <sin⁴ψ cos²α sin²α>`
- Second-order moment equations (3.43): `<|s_hh|²>`, `<|s_vv|²>`, `<|s_hv|²>`, `<s_hh* s_vv>` in terms of A₁-A₅
- First-order forward scattering moments (3.44): `<s_hh^(0)>`, `<s_vv^(0)>`
- Axisymmetric Gaussian distribution (3.46)

### Step 7: Interactive — Orientation effects (Fig. 3.4 replication)

```python
from ipywidgets import FloatSlider
import numpy as np
import matplotlib.pyplot as plt

def compute_orientation_variables(sigma_deg, s_b_s_a=1.5):
    """Compute Z_DR, L_DR, rho_hv for given sigma and axis ratio.

    Uses simplified model based on Ryzhkov 2001 and ch3.md Eq. (3.43), (3.53).
    """
    sigma = np.radians(sigma_deg)
    r_sigma = np.exp(-2 * sigma**2)

    # Angular moments for axisymmetric Gaussian (Eq. B.9 approximation)
    # A1 - A2 = 0.5 * r_sigma * (1 + r_sigma) (Eq. 3.53)
    A1_minus_A2 = 0.5 * r_sigma * (1 + r_sigma)

    # For completely random: A1=A2=1/3, A3=A4=1/5, A5=1/15
    # For Gaussian: interpolate between random and aligned
    # Simplified: use r_sigma to interpolate angular moments
    A1 = 1/3 * (1 - r_sigma) + r_sigma  # from 1/3 (random) to 1 (aligned)
    A2 = 1/3 * (1 - r_sigma)             # from 1/3 (random) to 0 (aligned)
    A5 = 1/15 * (1 - r_sigma)           # from 1/15 (random) to 0 (aligned)

    # Simplified Z_DR model: Z_DR (dB) = 10*log10((s_b/s_a)^2) * (A1 - A2) / (A1 + A2)
    # But for oblate spheroids with symmetry axis vertical:
    # Z_dr = |s_b|²/|s_a|² when aligned (A1=1, A2=0) → Z_DR = 20*log10(s_b/s_a)
    s_ratio_sq = s_b_s_a**2
    Z_dr_linear = s_ratio_sq  # at full alignment
    Z_dr_eff = Z_dr_linear * (A1_minus_A2 / 0.5)  # scale by alignment
    Z_dr_dB = 10 * np.log10(max(Z_dr_eff, 0.01))

    # LDR: increases with randomness
    # At aligned: LDR → -∞ (no cross-pol); at random: LDR significant
    LDR_dB = 10 * np.log10(A5 / (1/15) * 0.1)  # simplified

    # rho_hv: 1 at aligned, decreases with randomness
    # ρ_hv = 1 - 2*L_dr (random case, Eq. 3.48), interpolated
    rho_hv = 1 - 2 * 10**(LDR_dB/10) if LDR_dB > -30 else 1.0
    rho_hv = max(min(rho_hv, 1.0), 0.0)

    return Z_dr_dB, LDR_dB, rho_hv

def plot_orientation_effects(s_b_s_a=1.5):
    """Replicate Fig. 3.4: Z_DR, L_DR, rho_hv vs sigma for two axis ratios."""
    sigmas = np.linspace(0, 50, 200)
    ratios = [1.2, 1.5] if s_b_s_a == 1.5 else [s_b_s_a]

    fig, axes = plt.subplots(1, 3, figsize=(15, 4))

    for ratio in [1.2, 1.5]:
        Z_dr_vals, LDR_vals, rho_vals = [], [], []
        for sig in sigmas:
            Z, L, r = compute_orientation_variables(sig, ratio)
            Z_dr_vals.append(Z)
            LDR_vals.append(L)
            rho_vals.append(r)

        axes[0].plot(sigmas, Z_dr_vals, label=f's_b/s_a={ratio}', linewidth=2)
        axes[1].plot(sigmas, LDR_vals, label=f's_b/s_a={ratio}', linewidth=2)
        axes[2].plot(sigmas, rho_vals, label=f's_b/s_a={ratio}', linewidth=2)

    axes[0].set_xlabel(r'Canting angle dispersion σ (deg)')
    axes[0].set_ylabel(r'$Z_{DR}$ (dB)')
    axes[0].set_title('(a) $Z_{DR}$ vs σ')
    axes[0].legend()
    axes[0].grid(alpha=0.3)

    axes[1].set_xlabel(r'Canting angle dispersion σ (deg)')
    axes[1].set_ylabel(r'$LDR$ (dB)')
    axes[1].set_title('(b) $LDR$ vs σ')
    axes[1].legend()
    axes[1].grid(alpha=0.3)

    axes[2].set_xlabel(r'Canting angle dispersion σ (deg)')
    axes[2].set_ylabel(r'$\rho_{hv}$')
    axes[2].set_title('(c) $\\\rho_{hv}$ vs σ')
    axes[2].legend()
    axes[2].grid(alpha=0.3)
    axes[2].set_ylim(0.5, 1.05)

    plt.suptitle('Fig. 3.4: Effects of Particle Orientation Distribution', fontsize=12)
    plt.tight_layout()
    plt.show()

interact(plot_orientation_effects,
         s_b_s_a=FloatSlider(value=1.5, min=1.0, max=2.0, step=0.1,
                              description=r'$s_b/s_a$:'))
```

### Step 8: Section 6 — Three Special Cases

Markdown:
- **(a) Completely random orientation**: `A₁=A₂=1/3, A₃=A₄=1/5, A₅=1/15` → `Z_DR = 0 dB, δ = 0, K_DP = 0, A_DP = 0`; `ρ_hv = 1 - 2·L_DR`
- **(b) Noncanted (vertical axis)**: `A₁=A₃=1, A₂=A₄=A₅=0` → `Z_dr = <|s_b|²>/<|s_a|²>`, `K_DP = λ Re(<s_b^(0)> - <s_a^(0)>)`, `L_DR = 0`
- **(c) Gaussian orientation**: `r_σ = exp(-2σ²)`, `F_orient = A₁ - A₂ = 0.5 r_σ (1 + r_σ)`

### Step 9: Orientation factor plot (Fig. 3.5 replication)

```python
def plot_orientation_factor():
    """Replicate Fig. 3.5: Orientation factor F_orient = A1 - A2 vs sigma."""
    sigmas = np.linspace(0, 60, 300)
    r_sigma = np.exp(-2 * (np.radians(sigmas))**2)
    F_orient = 0.5 * r_sigma * (1 + r_sigma)

    plt.figure(figsize=(8, 4))
    plt.plot(sigmas, F_orient, 'b-', linewidth=2)
    plt.xlabel(r'Canting angle dispersion σ (deg)')
    plt.ylabel(r'$F_{orient} = A_1 - A_2$')
    plt.title('Fig. 3.5: Orientation Factor $F_{orient}$ vs σ')
    plt.grid(alpha=0.3)
    plt.ylim(0, 1)
    plt.tight_layout()
    plt.show()

plot_orientation_factor()
```

### Step 10: References section

Markdown cell at bottom citing ch3.md sources:
- Doviak & Zrnic (2006) Doppler Radar and Weather Observations
- Ryzhkov (2001) J. Atmos. Oceanic Technol.
- Sachidananda & Zrnic (1985)

### Step 11: Commit

```bash
git add Chapter_03_Scattering_by_Ensemble/3_3_Polarimetric_Variables.ipynb
git commit -m "feat(ch3): rebuild 3_3 Polarimetric Variables notebook

- Full derivations: Z_hh (3.29), Z_DR (3.32-34), rho_hv (3.35-38)
- HSHV/VSVH modes: LDR (3.39), rho_xh (3.40), rho_xv (3.41)
- Orientation effects: angular moments A1-A5, Eq. (3.43)-(3.53)
- Interactive: Z_DR, LDR, rho_hv vs sigma (Fig. 3.4 replication)
- Orientation factor F_orient plot (Fig. 3.5 replication)
- Three special cases: random, noncanted, Gaussian

Co-Authored-By: Claude Opus 4.7 <noreply@anthropic.com>"
```

---

## Task 4: Update README.md

**Files:**
- Modify: `Chapter_03_Scattering_by_Ensemble/README.md`

Update to reflect new content structure matching ch3.md, with updated table.

### Step 12: Commit

```bash
git add Chapter_03_Scattering_by_Ensemble/README.md
git commit -m "docs(ch3): update README for rebuilt notebooks

Co-Authored-By: Claude Opus 4.7 <noreply@anthropic.com>"
```

---

## Spec Coverage Check

| Spec Requirement | Notebook | Task |
|-----------------|----------|------|
| Range weighting function (3.1) | 3_1 | Task 1 |
| Pulse geometry, cτ/2 resolution | 3_1 | Task 1 |
| Matched filter, convolution (3.2) | 3_1 | Task 1 |
| WSR-88D example Fig. 3.2 | 3_1 | Task 1 |
| Powers from double summation (3.6) | 3_2 | Task 2 |
| Ensemble average (3.7)-(3.9) | 3_2 | Task 2 |
| Weather radar equation (3.13) | 3_2 | Task 2 |
| Correlations R_hv, R_hh (3.14) | 3_2 | Task 2 |
| Doppler velocity (3.20), spectrum width (3.21) | 3_2 | Task 2 |
| SHV/HSHV/VSVH eq. (3.23)-(3.28) | 3_2 | Task 2 |
| System constant C₁ (3.22) | 3_2 | Task 2 |
| Z_hh (3.29)-(3.30) | 3_3 | Task 3 |
| Z_DR (3.32)-(3.34) | 3_3 | Task 3 |
| rho_hv (3.35)-(3.38) | 3_3 | Task 3 |
| LDR, rho_xh, rho_xv (3.39)-(3.41) | 3_3 | Task 3 |
| Covariance matrix (3.42) | 3_3 | Task 3 |
| Angular moments A₁-A₅ (3.43)-(3.45) | 3_3 | Task 3 |
| Gaussian orientation (3.46) | 3_3 | Task 3 |
| Three special cases | 3_3 | Task 3 |
| F_orient (3.53)-(3.54), Fig. 3.5 | 3_3 | Task 3 |
| Fig. 3.4 replication (Z_DR, LDR, rho_hv vs σ) | 3_3 | Task 3 |