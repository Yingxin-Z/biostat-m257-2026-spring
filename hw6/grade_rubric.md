# HW6 Grading

**Student:** yingxin-z (Yingxin Zhao)

## Overall Grade: 110/110 (+25 bonus)

### Late penalty

- Tag `hw6` → commit `2026-05-28 18:40 PT`. Deadline **May 31 11:59 PM PT**. **On time (3 days early). No late penalty.**

### Q1. Derivatives (optional, 30 bonus pts): +25/30

- **Part 1 — Gradient derivation (15/15):** All three gradients correctly and fully derived using matrix differential calculus. ∇β = Xᵢ'Ωᵢ⁻¹rᵢ ✓; ∇σ² = −(1/2)tr(Ωᵢ⁻¹) + (1/2)rᵢ'Ωᵢ⁻²rᵢ ✓; ∂ℓᵢ/∂L = −Zᵢ'Ωᵢ⁻¹ZᵢL + Zᵢ'Ωᵢ⁻¹rᵢrᵢ'Ωᵢ⁻¹ZᵢL ✓. Uses cyclic trace and symmetry arguments throughout.
- **Part 2 — Information matrices (10/15):** Observed information: β-β block = ΣᵢXᵢ'Ωᵢ⁻¹Xᵢ ✓; β-σ² block = ΣᵢXᵢ'Ωᵢ⁻²rᵢ ✓; σ²-σ² block = −(1/2)tr(Ωᵢ⁻²) + rᵢ'Ωᵢ⁻³rᵢ ✓. L-L and L-cross blocks acknowledged as "algebraically more involved" but not derived. Fisher information: β-σ² block = 0 ✓; β-β block = ΣᵢXᵢ'Ωᵢ⁻¹Xᵢ ✓; σ²-σ² block = (1/2)tr(Ωᵢ⁻²) ✓; L-L block stated in vectorized form ✓; also notes ℐ_F = E[∇ℓᵢ∇ℓᵢ'] as alternative derivation ✓. L-block derivation in observed info incomplete.

### Q2. Objective and gradient evaluator for a single datum: 20/20

- `logl!(obs, β, L, σ², true)` correctness: objective `−3261.3155`, ∇β and ∇σ² assertions pass. ✓
- Efficiency: median **551.753 ns** → `clamp(10 / (median/1e3) * 10, 0, 10) = 10.0`. ✓
- Memory: 0 bytes. ✓

### Q4. Test data: 20/20

- Correctness assertions pass: `obj = −2.8406002241132786e6`, ∇β, ∇L, ∇σ². ✓
- Efficiency: median **570.708 µs** → `clamp(10 / (median/1e6) * 10, 0, 10) = 10.0`. ✓
- Memory: **0 bytes / 0 allocs** → `clamp(10 − mem/100, 0, 10) = 10.0`. ✓

### Q5. Starting point: 30/30

- **Derivation (10/10):** Vectorizes using ‖A‖²_F = ‖vec(A)‖², applies Kronecker identity (Z⊗Z)'(Z⊗Z) = Z'Z⊗Z'Z, derives Kronecker system. ✓
- **Correctness (10/10):** Starting log-likelihood `−3.3413349534894354e6 > −3.3414e6` threshold → 10. ✓
- **Efficiency (10/10):** `init_ls!` median **82.250 µs** → `clamp(1 / (median/1e6) * 10, 0, 10) = 10.0`. ✓

### Q7. Test drive: 20/20

- **Correctness (10/10):** Both assertions pass — `logl!(lmm) = −2.840594449981705e6 > −2.84060e6` ✓; gradient norm = 0.09746 < 0.1 ✓. Student tightened tolerances: `ftol_rel=1e-10`, `ftol_abs=1e-10`, `xtol_rel=1e-10`, `maxeval=50000`.
- **Efficiency (10/10):** median **53.859 ms** → `clamp(1 / (median/1e9) * 10, 0, 10) = 10.0`. ✓

### Q8. Gradient-free vs gradient-based vs Hessian-based: 10/10

- **Written analysis:** "LN_COBYLA (gradient-free) was the slowest (21s) and failed to converge within the iteration limit ... LD_MMA (gradient-based) converged quickly (0.05s) ... Ipopt L-BFGS achieved the tightest convergence (gradient norm ~1e-5) in 1.6s, benefiting from curvature information via L-BFGS." Written before the code. ✓
- Table: LN_COBYLA (20.96s, −2840594.45250912, grad=2.17), LD_MMA (0.05s, −2840594.44998170, grad=0.097), Ipopt L-BFGS (1.64s, −2840594.44997881, grad=0.0000111). ✓

### Q9. Compare with existing art: 10/10

- Table:(0.05s, −2840594.449982), lme4 (82.37s, −2840594.449980), MixedModels.jl (0.71s, −2840594.449982). ✓

### Q1. Derivatives (optional, 30 bonus pts): see above

### Git / Reproducibility

- `hw6/` with `Project.toml`, `hw6.html`, `hw6.ipynb`. ✓
- `hw6` tag on `main`, pushed 3 days before deadline. ✓
- No `.ipynb_checkpoints/` or junk files. ✓
