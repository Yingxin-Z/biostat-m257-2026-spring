# HW2 Grading

**Student:** Yingxin Zhang

## Overall Grade: 99/105

### Late penalty

- Last `hw2`-related commit landed 2026-04-25 13:40 PT, before the 11:59 PM PT deadline on 2026-04-26. **No late penalty.**
- **Note:** the submission was **not tagged** `hw2` — see Usage of Git deduction below.

### Quality of report: 10/10

- Final report rendered to HTML (`hw2.html`). ✓
- Prepared as a Jupyter Notebook with Quarto-style YAML for reproducibility. ✓
- Author field filled in. Report follows the question order.

### Completeness, correctness and efficiency of solution: 41/45

- **Q1.1 Develop code (12/12)** — `nnmf` pre-allocates a tight set of workspaces (`XWt`, `VWWt`, `VtX`, `VtVW`, plus a single shared `temp` for `WWᵀ` and `VᵀV`), uses `mul!` for every matrix product, and updates `V`/`W` with `@. V *= XWt / max(VWWt, epsT)` (uses `max(·, epsT)` rather than `+ epsT` as the denominator guard, which is equivalent for positive iterates). Objective is computed via the trace identity `‖X‖² − 2·dot(W, VtX) + dot(VtVW, W)` floored at zero with `max(·, zero(T))`, and `normXsq = dot(X, X)` is precomputed. Convergence test matches the spec.

- **Q1.2 Data (3/3)** — `readdlm` reads the face data; one sample face displayed via `colorview(Gray, reshape(X[10, :], 19, 19))`.

- **Q1.3 Correctness and efficiency (12/12)** — Benchmarks run for `r ∈ {10, 20, 30, 40, 50}`. **Allocations match the reference exactly (14–15)**, e.g., `r=10: 88.69 ms (14 allocations: 442.23 KiB)` vs reference `69.93 ms (14 allocations)`. `obj`/`niter` match the reference (`obj=11730.39, niter=239` at r=10 through `obj=4430.20, niter=698` at r=50). Times are 1.3–5× the reference, well within the order-of-magnitude tolerance.

- **Q1.4 Non-uniqueness (5/6)** — At `r = 20` with `Random.seed!(257)`, runs the algorithm with the provided `V0`/`W0` (`obj = 8497.22, niter = 394`) and from a random `Vrand`, `Wrand` start (`obj = 8469.51, niter = 352`). Verifies via `isapprox(V₁, V₂) && isapprox(W₁, W₂) → false`.
  - **Deduction (−1):** The question asks "Explain what you find." The submission only prints `Are the solutions the same? false` — no narrative explanation of why (non-convex objective, different local minima, factorization scaling indeterminacy).

- **Q1.5 Fixed point (5/6)** — At `r = 20` started from `V = W = 1`. Reports `obj = 25297.02, niter = 3` and confirms `isapprox(V₁, V₃) && isapprox(W₁, W₃) → false`.
  - **Deduction (−1):** Again no narrative explanation. The expected discussion (the all-ones start is symmetric so all rows of `V` and all rows of `W` remain identical under the multiplicative update — a degenerate rank-1 fixed point) is missing.

- **Q1.6 Interpret NNMF (4/6)** — At `r = 50` with the provided `V0`/`W0`, computes the factorization and constructs all 50 basis images via `[colorview(Gray, reshape(What[k, :], 19, 19)) for k in 1:r]`, assembled with `mosaicview(...; nrow = 5)` and upscaled 8× for visibility — a clean, well-labeled mosaic.
  - **Deduction (−2):** The question asks *"Plot the basis images ... What do you find?"* — the submission shows the basis images but provides **no markdown discussion** at all between the plotting cell and the Q1.7 header. The standard parts-based-representation interpretation (localized features, role of the non-negativity constraint) is missing.

- **Q1.7 GPU (optional)** — Implemented with `Metal.jl` on Apple Silicon (Float32 with `MtlArray`); only `r = 10` is benchmarked (21.77 s vs ~89 ms CPU). ✓ (Optional — no extra credit but fully attempted.)

### Q2 AI assistant: 10/10

- AI assistant identified (ChatGPT GPT-5.5 Thinking) with a thorough usage description (debugging, error messages, benchmarking syntax, GPU backend exploration) and a productivity discussion ("AI-generated answers were not always correct, especially for technical details involving Julia benchmarking scope, GPU backends, and plotting behavior. Therefore, I still needed to verify the suggestions").
- Five labeled instances of incorrect/misleading AI behavior, each with the original prompt, the AI's answer, and a clear "why this was misleading" explanation: (1) trying to fix a missing-`IJulia` kernel from inside the notebook; (2) `@btime` setup variable-scope error; (3) wrong variable names in plotting code; plus two more.

### Usage of Git: 8/10

- Six HW2 commits with descriptive messages (`create hw2`, `complete Q1.1 - 1.4`, `complete hw2`, `generate html`, `revise codes`, `Ignore system and manifest files`). ✓
- `hw2/` folder created correctly at the repo root. ✓
- No auxiliary clutter under version control.
- **Deduction (−2):** **Submission is not tagged `hw2`.** The course policy is that tagging time is the submission time; no `hw2` tag was pushed to the remote. (HW1 was tagged correctly, so this is a regression.)

### Reproducibility: 10/10

- `Project.toml`, `hw2.ipynb`, `hw2.html`, `V0.txt`, `W0.txt`, `nnmf-2429-by-361-face.txt`, and `nnmf.png` all shipped on `main`. ✓
- After `Pkg.activate(pwd()); Pkg.instantiate()` the environment resolves and the notebook can be re-rendered.

### Julia code style: 20/20

- Rule (1) Four-space indenting. ✓
- Rule (2) 92-character lines. ✓
- Rule (3) Space after comma. ✓
- Rule (4) No space before comma. ✓
- Rule (5) Space around binary operators. ✓ (e.g., `obj = max(normXsq - 2 * dot(W, VtX) + dot(VtVW, W), zero(T))`).
