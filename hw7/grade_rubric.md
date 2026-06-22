# HW7 Grading

**Student:** yingxin-z (Yingxin Zhao)

## Overall Grade: 110/110

> Rubric: Q1 (10) + Q2 (20) + Q3 (20) + Q6 (30) + Q9 (20) + Q10 (10) = 110.

### Late penalty

- Tag `hw7` → commit `2026-06-07 18:43 PT`. Deadline **June 10 11:59 PM PT**. **On time (3 days early). No late penalty.**

### Q1. Refresher on normal-normal model: 10/10

- Completing-the-square: collects quadratic and linear terms in γ, defines V = (σ⁻²Z'Z+Σ⁻¹)⁻¹ and μ = σ⁻²VZ'r. Identifies posterior as MVN(μ, V). ✓
- Shows the direct form (V = (σ⁻²Z'Z+Σ⁻¹)⁻¹) and the Woodbury form via the Woodbury matrix identity (V = Σ − ΣZ'(ZΣZ'+σ²I)⁻¹ZΣ). ✓
- Alternative mean form ΣZ'(ZΣZ'+σ²I)⁻¹r also derived. ✓

### Q2. Derive EM algorithm: 20/20

- **Complete-data log-likelihood:** Correctly factors as Σᵢ[log f(yᵢ|γᵢ) + log f(γᵢ)], substitutes Gaussian densities. ✓
- **E-step:** Q-function derived using posterior moments E[γᵢ] = γ̃ᵢ and E[γᵢγᵢ'] = Vᵢ + γ̃ᵢγ̃ᵢ'. ✓
- **M-step:** All three updates correctly derived in closed form:
  - β^(t+1) = (ΣᵢXᵢ'Xᵢ)⁻¹ Σᵢ Xᵢ'(yᵢ − Zᵢγ̃ᵢ) ✓
  - Σ^(t+1) = (1/m) Σᵢ(Vᵢ + γ̃ᵢγ̃ᵢ') ✓
  - σ²^(t+1) = (1/N) Σᵢ[‖yᵢ−Xᵢβ^(t+1)−Zᵢγ̃ᵢ‖² + tr(ZᵢVᵢZᵢ')] ✓
- Correctly notes this is ECM (uses β^(t+1) in the σ² update). ✓

### Q3. Objective of a single datum: 20/20

- Correctness assertions pass: `logl = −3261.3155007293944`, `obs.μγ`, `obs.νγ`. ✓
- Efficiency: median **635.658 ns** → `clamp(10 / (median/1e3) * 10, 0, 10) = 10.0`. ✓
- Memory: **0 bytes** → no allocation. ✓

### Q6. Test data: 30/30

- Correctness: `obj1 = −2.840600224113277e6` ✓, `obj2 = −2.840594581606888e6` ✓ (both assertions pass). ✓
- Efficiency: `update_em!` median **887.250 µs** → `clamp(10 / (median/1e6) * 10, 0, 10) = 10.0`. ✓
- Memory: **0 bytes / 0 allocs** → `clamp(10 − mem/100, 0, 10) = 10.0`. ✓

### Q9. Test drive: 20/20

- EM converges in 4 iterations (iter=0,1,2,3).
- Correctness: `update_em!(lmm) = −2.8405944513217555e6 > −2.84060e6` assertion passes. ✓
- Efficiency: `fit!` median **4.069 ms** → `clamp(1 / (median/1e9) * 10, 0, 10) = 10.0`. ✓
- Memory: **0 bytes**. ✓

### Q10. EM vs Newton-type algorithms: 10/10

- **Stability:** EM monotone by construction; Newton-type methods may need line search/safeguards. ✓
- **Convergence rate:** EM linear; Newton quadratic near optimum; gradient-based methods intermediate. ✓
- **Final objective:** Both reach same MLE (~−2,840,594.45). ✓
- **Total runtime:** EM ~4ms vs HW6 NLP ~50ms; notes this is problem-dependent. ✓
- **Derivation effort:** EM requires conditional distribution + closed-form M-step; Newton needs marginal log-likelihood derivatives (more algebraically complex). ✓
- **Implementation effort:** EM closed-form updates easier once E-step quantities available; Newton requires general optimizer integration. ✓
- Comprehensive, well-organized discussion covering all required dimensions. ✓

### Git / Reproducibility

- `hw7/` with `Project.toml`, `hw7.html`, `hw7.ipynb`. ✓
- `hw7` tag on `main`, pushed 3 days before deadline. ✓
- No `.ipynb_checkpoints/` or junk files. ✓
