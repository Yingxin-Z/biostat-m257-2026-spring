# HW5 Grading

**Student:** Yingxin Zhang (006140202)

## Overall Grade: 109/110


### Late penalty

- Tag `hw5` → commit `fb3f7a3`, committed 2026-05-21 (see Git section). Deadline **May 22 11:59 PM PT**. **On time. No late penalty.**

### Q1.1 Projection to the Stiefel manifold: 10/10

- Correct, complete derivation. Expands $\|G-O\|_F^2 = \|G\|_F^2 - 2\langle G,O\rangle_F + \|O\|_F^2$, notes $\|O\|_F^2 = \min(m,n)$ is constant, reduces to maximizing $\operatorname{tr}(O^\top G)$. Uses thin SVD $G=U\Sigma V^\top$, the substitution $W = V^\top O^\top U$ (orthogonal), and Von Neumann's trace inequality $\operatorname{tr}(W\Sigma) \le \operatorname{tr}(\Sigma)$ with equality iff $W=I$ to conclude $O^* = UV^\top$. Verifies the constraint for $m\ge n$, $m\le n$, and $m=n$. ✓

### Q1.2 SVD implementation: 15/15

- `proj_svd(G) = U*V'` via thin `svd(G)`. ✓
- Orthogonality check `norm(O'O − I) = 2.38e-13`. ✓

### Q1.3 Newton-Schulz iteration: 9/10

- Correctly recasts the update as $G \mapsto U\,\Sigma\,p(\Sigma^2)\,V^\top$, so each singular value follows the scalar map $h(\sigma)=\sigma(a+b\sigma^2+c\sigma^4)$ with $U,V$ fixed. Shows the fixed point $h(1)=2-1.5+0.5=1$ and $h'(1)=a+3b+5c=0$, correctly concluding **quadratic** convergence ($\varepsilon_{k+1}=\tfrac{h''(1)}{2}\varepsilon_k^2 + O(\varepsilon_k^3)$). Notes the initial normalization gives $\sigma_i\in(0,1]$. ✓
- **Deduction (−1):** The basin-of-attraction condition is asserted, not established — she writes "One can verify that $h$ maps $(0,1]$ into itself" but does not show it (the monotonicity / $p([0,1])\subset[0,1]$ and $h(x)>x$ on $(0,1)$ conditions that the reference solution proves). Same bar as the calibration anchor (anela HW5 Q1.3, −1).

### Q1.4 NS implementation: 15/15

- `proj_ns` implemented in-place with preallocated `storage_mn`, `storage_nn_1/2`, using `mul!`/`lmul!`/`axpy!` and an explicit `add_diagonal!` for the $aI$ term (with a separate `MtlArray` method for GPU to avoid scalar indexing). ✓
- `norm(O_ns − O_svd) = 1.53e-13` — matches the SVD projection. ✓

### Q1.5 Benchmark: 20/20

- **Full benchmark present.** Platform is **Apple M1 Pro (Metal)** per `versioninfo()`. Benchmarks SVD vs NS across `Float64`/`Float32`/`Float16` on CPU **and** Newton-Schulz on **Metal GPU** for `Float32`/`Float16` (completed timings: e.g. CPU F64 SVD 1.56 s / NS 2.01 s; Metal F32 NS 0.497 s; Metal F16 NS 0.374 s), with orthogonality residuals reported for every cell. ✓
- `BFloat16` is excluded with a valid hardware justification (no native BLAS kernel on this hardware; sub-32-bit CPU types upcast to Float32). Acceptable. ✓
- Thorough discussion of the speed (GPU 2–3× faster for NS; SVD CPU-only since Metal lacks `svd`), memory, and accuracy (residuals scale with unit roundoff; NS self-correcting in F64; F16 GPU residual 0.023 from accumulated rounding) trade-offs, with a clear recommendation. ✓
- The required GPU run is present and the low-precision sweep is complete, so the −5 (missing GPU / incomplete low-precision) applied to the anchor case does **not** apply here.

### Q2. Optimal Design: 40/40


### Git / Reproducibility

- `hw5/` folder with `Project.toml`, `hw5.ipynb`, `hw5.html`. ✓ (HTML shipped.)
- `hw5` tag on `main`, committed 2026-05-21 (pre-deadline). ✓
- No `.ipynb_checkpoints/` or `.DS_Store` committed inside `hw5/`. ✓
- *(Note: no `Manifest.toml` shipped, but the rubric only penalizes a missing `Project.toml`, which is present — no deduction.)*
