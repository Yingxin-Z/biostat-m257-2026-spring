# HW4 Grading

**Student:** Yingxin Zhang (yingxin-z)

## Overall Grade: 110/110

### Late penalty

- Tag `hw4` pushed at 2026-05-08 23:37 PT (deadline 2026-05-10 11:59 PM PT). **No late penalty.**

### Q1. Recognize structure: 5/5

- Decomposes $\mathbf{P} = p\mathbf{D}\mathbf{A} + \mathbf{z}\mathbf{1}^T$ with $\mathbf{D} = \operatorname{diag}(1/r_i)$ (zero for dangling rows) and $z_i = (1-p)/n$ on normal pages, $1/n$ on dangling pages. Correct sparse + rank-1 split. ✓

### Q3. Explore data: 10/10

- `Base.summarysize(A) = 50.81 MB`; dense `Float64` estimate $\approx 6.11$ TB. ✓
- $n = 916{,}428$ pages, $nnz(A) = 5{,}105{,}039$ edges, $176{,}974$ dangling nodes. ✓
- In-/out-degree histograms (log scale) rendered; average in/out = 5.57. ✓
- Top 20 by in-degree and top 20 by out-degree printed; sparsity pattern of `A[1:10000, 1:10000]` plotted. ✓

### Q4. Dense linear algebra: 5/5

- LU storage estimate: $n^2 \cdot 8$ bytes $\approx 6.11$ TB. ✓
- Throughput-based time estimate from a $5000 \times 5000$ `mul!` benchmark (≈0.30 TFLOP/s) → ≈ 19.98 days for $(2/3)n^3$ LU flops. ✓
- Conclusion that dense LU and dense asymmetric eigen-solver are both infeasible. ✓

### Q5 Step 1. PageRankImPt + mul! (correctness): 15/15

- `PageRankImPt` struct holds `A`, `telep`, `store_n`, `rinv`, `z`; constructor pre-scales `rinv = p/r_i` (zero on dangling) and `z`-vector splits dangling vs. normal nodes. ✓
- `mul!(out, M, v)` uses `mul!(out, Transpose(M.A), store_n, -1, 1)` plus a `dot(M.z, v)` shift; `mul!(out, transpose(M), v)` mirrors with `mul!(store_n, M.A, v)`. Both in-place. ✓
- Both `@assert norm(transpose(M) * ones(n)) < 1e-12` and `@assert norm(M * xsol) < 1e-12` pass. ✓

### Q5 Step 2. mul! benchmarks (memory): 20/20

- `bm_mv` median: 16.253 ms, **0 bytes / 0 allocs**.
- `bm_mtv` median: 14.871 ms, **0 bytes / 0 allocs**.
- `clamp(10 - 0/100, 0, 10) + clamp(10 - 0/100, 0, 10) = 20.0`. ✓

### Q5 Step 3. GMRES: 20/20

- `linsolve` converges in 3 iterations / 72 mat-vec applications; `time_gmres ≈ 3.22 s` (matches hint of 3-4 s).
- `@assert norm(x_gmres - xsol) < 1e-6` passes. ✓
- `clamp(20 / time_gmres * 20, 0, 20) = 20.0`. ✓

### Q5 Step 4. Arnoldi: 20/20

- `eigsolve` converges in 6 iterations / 93 mat-vec applications; eigenvalue ≈ $2.07 \times 10^{-14}$; `time_arnoldi ≈ 4.95 s`.
- Both `@assert abs(Real(vals[1])) < 1e-8` and `@assert norm(x_arnoldi - xsol) < 1e-6` pass. ✓
- `clamp(20 / time_arnoldi * 20, 0, 20) = 20.0`. ✓

### Q6. Results: 5/5

- Top 20 pages by PageRank (GMRES) and by Arnoldi listed alongside top 20 by in-degree in a 20x3 matrix. GMRES and Arnoldi rankings agree exactly.
- Student correctly notes the lists overlap but differ in ordering, and explains that PageRank weights link source quality whereas in-degree counts links equally. ✓

### Q8. AI assistant: 10/10

- Identifies Claude Sonnet 4.6 (claude.ai) with a productivity discussion. ✓
- Five distinct instances of incorrect/misleading AI answers, each tied to the actual debugging journey: (1) Claude assumed the wrong `A[i,j]` edge convention and pushed `dims=1` for out-degrees, breaking the transpose assertion; (2) Claude's first `mul!` used an extra `tmp` scratch + two-pass `rinv .* v` then `mul!(out, A, tmp)` instead of the equivalent single pre-allocated `store_n`; (3) Claude wrongly warned that `mul!(out, Transpose(A), v)` allocates and rewrote both methods with manual CSC loops; (4) Claude suggested a dense `AbstractMatrix` fallback constructor that masks the real issue (convert once to sparse) instead of fixing the call site; (5) Claude repeatedly insisted the assertion bug was a `dims` issue even after row/column sums were shown equal, only acknowledging the convention mismatch after several rounds. ✓

### Git / Reproducibility (folded into above; no separate deduction)

- `hw4/` folder at repo root with `Project.toml`, `hw4.ipynb`, `hw4.html`. Filenames are `hw4.*` rather than `hw04.*` (soft naming nit, no deduction — same precedent as HW3). ✓
- `hw4` tag on `main`, pushed pre-deadline. First HW with a properly pushed tag (HW1/HW2/HW3 all lost 2 points for missing tags); HW4 fixes this. ✓
- `pgrksol.csv.gz` correctly added to `.gitignore`; not committed. `.DS_Store` also ignored. ✓
- Six well-scoped commits documenting progress through Q3 → Q4 → Q5 → Q6 → Q8 → render. ✓
