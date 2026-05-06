# HW3 Grading

**Student:** Yingxin Zhang (yingxin-z)

## Overall Grade: 108/110

### Late / submission status

- No `hw3` tag pushed.
- Latest commit touching `hw3/` on `main`: `c87f045 complete hw3` at 2026-04-30 23:36 PT (~3 days before the deadline). **No late penalty.**

### Q1. Formula: 10/10

- Correct derivation: $\mathbf{Y}_i \sim N(\mathbf{X}_i\boldsymbol{\beta}, \mathbf{V}_i)$ with $\mathbf{V}_i = \mathbf{Z}_i\boldsymbol{\Sigma}\mathbf{Z}_i^\top + \sigma^2\mathbf{I}_{n_i}$, log-likelihood boxed cleanly. ✓

### Q3. Correctness: 15/15

- `logl!` matches `logpdf(mvn, y)`. ✓
- `@assert ...` runs without `AssertionError`. ✓

### Q4. Efficiency: 30/30

- `bm1` median 675.917 μs; `bm2` median 411.460 ns → speedup ≈ 1644×.
- `clamp(median(bm1).time / median(bm2).time / 1000 * 30, 0, 30) = 30.0`. ✓

### Q5. Memory: 30/30

- `bm2` memory: 0 bytes, 0 allocs → `clamp(30 - 0/1024, 0, 30) = 30.0`. ✓

### Q6. Misc (style / Git / repro): 13/15

- **Style ✓** — 4-space indent, 92-char lines, in-place BLAS / LAPACK calls (`LAPACK.potrf!`, `BLAS.trmm!`, `BLAS.trsv!`, `BLAS.gemv!`).
- **Git −2** — three descriptive commits (`create hw3 and complete Q1`, `complete Q1-5`, `complete hw3`); `hw3/` folder at repo root. **No `hw3` tag pushed** — same precedent as HW1 (no `hw1` tag, −2) and HW2 (no `hw2` tag, −2).
- **Reproducibility ✓** — `Project.toml`, `hw3.ipynb`, `hw3.html` shipped on `main`. Filenames are `hw3.*` rather than `hw03.*` (soft naming nit, not a deduction). Notebook re-runs from a clean checkout.

### Q7. AI assistant: 10/10

- Identifies Claude Sonnet 4.6 with rationale (BLAS routine flag arguments, struct constructor debugging) and a productivity discussion. ✓
- Five distinct, well-documented instances of incorrect/misleading AI answers, each with the actual offending code + the corrected zero-allocation version: (1) wrong scaling-side in `BLAS.trmm!` for $\sigma^{-2} L^\top Z^\top Z L$; (2) suggested an unnecessary `storage_n :: Vector{T}` field and forming explicit residual $r = y - X\beta$; (3) `Symmetric()` wrapper + `cholesky!` returning a struct caused heap allocations vs. direct `LAPACK.potrf!`; (4) `2 * sum(log, diag(Mc.L))` allocates a $q$-vector instead of reading diagonal entries directly; (5) incorrect `r^\top r` expansion that left `storage_p` holding $X^\top y$ instead of $(X^\top X)\beta$ at the dot call. ✓
