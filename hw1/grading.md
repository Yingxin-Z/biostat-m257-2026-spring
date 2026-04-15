# HW1 Grading

**Student:** Yingxin Zhang
**Total: 131 / 136**

## Usage of Git: 10/10

- Are there enough commits? Are commit messages clear? Yes.
- Is the hw submission tagged? Yes.
- Are the folders (`hw1`, `hw2`, ...) created correctly? Yes.
- Do not put a lot of auxiliary files into version control. Yes.

## Reproducibility: 10/10

- Are the materials (files and instructions) submitted to the `master` branch sufficient for reproducing all the results? Yes.
- If necessary, are there clear instructions, either in report or in a separate file, how to reproduce the results? Yes.

## Julia Code Style: 20/20

- Rule (1). Four space indenting rule. ✓
- Rule (2). 92 character rule. ✓
- Rule (3). Space after comma rule. ✓
- Rule (4). No space before comma rule. ✓
- Rule (5). Space around binary operator rule. ✓

## Q2. Computer arithmetics: 18/18

All six sub-parts correctly demonstrated with valid counter-examples and explanations.
- Q2.1: Associativity of addition — correct counter-example, explained rounding at different stages.
- Q2.2: Associativity of multiplication — correct counter-example.
- Q2.3: Distributivity — correct counter-example.
- Q2.4: `0 * x == 0` — correctly shows `0 * Inf = NaN` and `0 * NaN = NaN`.
- Q2.5: `x / a == x * (1 / a)` — correct counter-example.
- Q2.6: Commutativity — correctly notes `NaN` breaks `==`.

## Q3. Multiple dispatch and JIT: 18/18

- Q3.1 a–b: LLVM bitcode shown for both `Int64` and `Float64` inputs. ✓
- Q3.1 c: Correct comparison — integer vs floating-point instructions, explained via multiple dispatch + JIT.
- Q3.1 d: `@fastmath` versions shown for both types. ✓
- Q3.1 e: Correctly explains `@fastmath` relaxes IEEE semantics and links to Q2 (associativity/distributivity not exact).
- Q3.2: Benchmarks show ~1e6× speedup for lazy range vs materialized vector; explanation correct.

## Q4. Sample variance: 20/20

- Q4.1: Textbook two-pass algorithm implemented; flop count `4n` correct. 0 allocations.
- Q4.2: One-pass algorithm implemented; flop count `3n + 5 ≈ 3n` correct. 0 allocations.
- Q4.3: Benchmarks show one-pass ~2× faster, consistent with flop count; zero memory allocations.
- Q4.4: Correctly identifies catastrophic cancellation — one-pass returns −1033.9 due to subtracting two large nearly-equal quantities.
- Q4.5: Correctly investigates Julia's `var`, identifies it as a numerically stable centered two-pass algorithm, benchmarked and compared.

## Q5. Woodbury formula: 12/12

All four identities demonstrated numerically with `isapprox`:
- Q5.1: Sherman-Morrison ✓
- Q5.2: Woodbury ✓
- Q5.3: Binomial inversion ✓
- Q5.4: Determinant identity ✓

## Q6. Triangular matrix and orthogonal matrix: 18/18

- Q6.1: Product of upper/lower triangular matrices. ✓
- Q6.2: Inverse of upper/lower triangular. ✓
- Q6.3: Product of unit upper/lower triangular is unit triangular. ✓
- Q6.4: Inverse of unit upper/lower triangular is unit triangular. ✓
- Q6.5: Proof — `Q^T = Q^{-1}`; since `Q^T` lower triangular and `Q^{-1}` upper triangular, off-diagonals must be zero. Correct.
- Q6.6: Proof `(Q_1 Q_2)^T (Q_1 Q_2) = I` and numerical verification. ✓

## Q7. AI assistant: 5/10

Mentions ChatGPT (GPT-5 Thinking) usage and how it helped with source control, debugging, and algorithm understanding. Productivity discussion present.

**Deduction (−5):** The question explicitly asks for **5 instances where the AI model gave incorrect or misleading answers** (with screenshots or copy-pasted Q&A). The submission does not provide these examples.
