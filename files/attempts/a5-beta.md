https://github.com/nahhididwin/The-research-of-Hung/blob/main/files/attempts/a5-beta.md

# A Structurally-Aware Mathematical Framework for N-Convolution Systems

**Author:** Hung Dinh Phu Dang  
**Date:** June 2026  
**Scope:** Mathematical framework for preserving structural coherence in convolution operations

---

## Executive Summary

This whitepaper proposes a mathematical framework designed to process N convolution expressions as irreducible structural entities rather than as aggregates requiring iterative reduction to scalar outputs. The framework achieves this by embedding N convolutions into a rigorously defined algebraic space where their compositional relationships—symmetries, hierarchical dependencies, and topological coherence—are encoded directly into the operations themselves.

The central claim is bounded and specific: for the restricted domain of general convolutions, the requirement to decompose to scalar outputs before comparison or composition is not a fundamental constraint, but rather an artifact of the chosen representation. A structurally-coherent alternative exists, where convolution forms remain irreducible through computation.

This is not a claim of universal O(1) computation or elimination of complexity. It is a precise statement about what becomes possible when the representation matches the problem domain.

---

## 1. Problem Statement: Why Current Approaches Decompose

### 1.1 The Standard Convolution Pipeline

In traditional computational frameworks, processing N convolutions follows this sequence:

1. **Input:** N convolution specifications (kernels, domains, parametrizations)
2. **Decomposition:** Each convolution is reduced through iteration (O(n) or O(n log n) per convolution)
3. **Materialization:** Scalar outputs or reduced representations are produced
4. **Comparison/Composition:** Operations between convolutions require comparison of materialized outputs
5. **Recomposition:** Results are aggregated or used in downstream processing

This pipeline has a fundamental property: **structural equivalence cannot be determined until materialization occurs**. Two convolutions with identical semantic meaning but different parametrizations cannot be recognized as equivalent without computing both to completion.

### 1.2 The Semantic Gap

Consider a practical case:

**Scenario 1:** Two equivalent convolutions specified differently
- Convolution A: kernel K, domain D, parametrized as explicit coefficients
- Convolution B: kernel K', domain D', parametrized as a linear combination of basis convolutions

Both perform identical operations, but determining this requires:
- Computing A fully (O(|D|) or worse)
- Computing B fully
- Comparing outputs
- Only then recognizing equivalence

**The core problem:** The structural relationship (equivalence) exists at the level of mathematical form, but the computational framework only grants access to it through complete materialization.

Worse: if we need to compose or transform convolutions before computing them, we lose structural information because our algebra only operates on scalars, not on convolution forms.

### 1.3 Where the Cost Actually Lives

In a typical modern pipeline using standard optimizations (fast convolution via FFT, tiling, parallelization):

- **FFT transformation cost:** O(n log n) or O(n) with specialized hardware
- **Element-wise multiplication:** O(n)
- **Inverse transformation:** O(n log n) or O(n)
- **Comparisons/aggregation:** O(1) to O(n) depending on structure

**Observation:** The dominant cost is not comparison, but the mandatory materialization itself. This is where the framework offers leverage: by never materializing intermediate scalar results that exist only to be composed with other convolutions, we avoid the dominant cost entirely for chains of operations.

---

## 2. Proposed Framework: Structural Convolution Algebra

### 2.1 Core Definitions

**Definition 2.1.1 (Convolution Form):** A convolution form is a tuple $(K, D, \Theta)$ where:
- $K$ is a kernel specification (finite, discrete support)
- $D$ is a domain specification (index set, lattice, or manifold)
- $\Theta$ is a parametrization (explicit coefficients, basis expansion, or symbolic specification)

A convolution form represents the *intention to compute* a convolution without requiring computation itself.

**Definition 2.1.2 (Structural Equivalence):** Two convolution forms $(K_1, D_1, \Theta_1)$ and $(K_2, D_2, \Theta_2)$ are structurally equivalent if they induce identical operations on all valid inputs when computed, even if their representations differ.

**Definition 2.1.3 (Algebraic System $\mathcal{C}$):** The structural convolution algebra is a formal system where:
- **Elements** are convolution forms (not scalar outputs)
- **Operations** include:
  - Composition: $C_1 \circ C_2$ (apply $C_1$ after $C_2$)
  - Superposition: $C_1 \oplus C_2$ (pointwise addition)
  - Scaling: $\lambda C$ (scalar multiplication)
  - Basis transformation: change $\Theta$ while preserving $(K, D)$
  - Equivalence checking: determine if $C_1 \equiv C_2$ structurally

- **Closure property:** All operations on convolution forms produce convolution forms; no intermediate reduction to scalars occurs unless explicitly materialized.

### 2.2 Structural Encoding vs. Materialization

In the proposed framework:

**Structural Form (Primary):**

Operation: (C_1 ⊕ C_2) ∘ C_3 Representation: A formal expression in ℭ Cost: O(1) to O(k) where k is structure size Result: Still a convolution form, not a scalar


**Materialization (Only When Necessary):**

Explicit Computation: Evaluate structure → scalar grid Cost: O(n) or O(n log n) depending on method Result: Actual numerical values


The key insight: **we construct the entire computational pipeline in the space of convolution forms, only materializing at the boundary where results are needed**.

### 2.3 Examples of Operations Preserving Structure

**Example 2.3.1 (Composition in Structural Form):**

Given:
- $C_1 = (\text{Gaussian kernel}, \text{2D lattice}, \sigma = 1.5)$
- $C_2 = (\text{Sobel kernel}, \text{2D lattice}, \text{standard})$

Traditional approach:

Compute C_1 → Grid G_1 Compute C_2(G_1) → Grid G_2 Cost: 2 * O(n log n)


Structural approach:

Form: C_2 ∘ C_1 Analysis: Recognize as derivative-of-smooth (both are linear, composition is linear) Optimization: Combine kernels → single kernel K' Structural result: (K', lattice, combined parametrization) Cost: O(1) kernel combination (not yet computed)


The composition exists as a formal object. It can be further composed with $C_3$, analyzed for symmetries, or decomposed into basis components—all without touching the data grid.

**Example 2.3.2 (Equivalence Recognition):**

Given:
- $C_A = (\text{convolution with } K, D)$
- $C_B = (\text{convolution with } K' = 2K, D)$ (kernel scaled by 2)

Traditional approach:

Compute C_A → Grid G_A Compute C_B → Grid G_B Compare G_A and G_B (recognize G_B = 2 * G_A) Cost: 2 * O(n) + O(n) = O(n)


Structural approach:

Examine (K, K') structurally: K' = 2K Conclude: C_B = 2 * C_A in algebra ℭ Cost: O(1) to O(k) symbolic comparison


**Example 2.3.3 (Basis Transformation):**

Instead of computing a convolution in pixel space, express it in a wavelet or frequency basis:

ℭ_pixel: (K_pixel, pixel_lattice, coefficients) ℭ_wavelet: (K_wavelet, wavelet_tree, basis_expansion)

Transform between representations without intermediate materialization. Cost: O(structure complexity), not O(grid size)


---

## 3. Trade-Offs and Honest Limitations

### 3.1 The Arithmetic-of-Convolution Forms

Operations in $\mathcal{C}$ are exact but have their own costs:

| Operation | Cost | Benefit | Limitation |
|-----------|------|---------|-----------|
| Form creation | O(1) | Trivial | None |
| Symbolic composition | O(k) | Maintains structure | k = kernel complexity |
| Equivalence check | O(k) | No materialization | Requires structural form |
| Kernel multiplication | O(k²) or O(k log k) | Exact convolution | k = effective kernel size |
| Basis transformation | O(k) or O(structure) | Different representations | Domain-specific |
| **Materialization** | **O(n log n) to O(n)** | **Actual values needed** | **Mandatory for output** |

**Critical observation:** We do not eliminate the O(n) cost. We defer it until it is unavoidable (output computation) or bypass it entirely if the operation can be composed and analyzed in structural form.

### 3.2 When Materialization is Required

The framework offers no benefit (and adds overhead) if:

1. **Output is needed immediately:** If the next operation requires actual numerical values, deferred materialization saves nothing.

2. **Operations are non-linear:** Operations that cannot be represented in $\mathcal{C}$ (e.g., arbitrary thresholding, nonlinear activation) force materialization.

3. **Single operation:** If there is one convolution with no composition, standard approaches are simpler.

4. **Very small n:** Structural overhead exceeds materialization cost.

### 3.3 Where the Framework Excels

Benefit is maximized when:

1. **Long pipelines:** Many composed operations on convolution forms before final output
   - Example: Image processing chain (blur → edge detection → morphological operations → feature extraction)
   - Savings: Avoid N materializations; compute once at end

2. **Conditional computation:** Operations where equivalence checking avoids redundant computation
   - Example: Caching: "Is this convolution equivalent to one we've already computed?"
   - Savings: O(k) equivalence check vs. O(n) recomputation

3. **Symbolic optimization:** Recognizing patterns (linearity, separability, symmetry) before materialization
   - Example: 2D Gaussian = separable product of 1D Gaussians → optimize kernel
   - Savings: O(n) operation becomes O(n/√n) with separability

4. **Basis-agnostic operations:** Working with convolutions in abstract form, switching bases as needed
   - Example: Frequency domain analysis without explicit FFT
   - Savings: Work directly with spectral properties

### 3.4 Computational Cost Migration

By the conservation principle observed in Part 2 of your research, we do not eliminate cost; we migrate it:

**Traditional pipeline:**

Materialization cost: O(n log n) × N operations Comparison cost: O(1) per pair Total: O(N * n log n)



**Structural pipeline:**

Form creation: O(k) × N operations Structural operations: O(k) × M compositions Materialization cost: O(n log n) × 1 (at end) Comparison cost: O(k) per pair Total: O(kN + kM + n log n)



**Benefit when:** $k * (N + M) \ll N * n \log n$, which occurs when:
- $k \ll n$ (kernels much smaller than domains—typical in practice)
- $M > 0$ (there are composed operations—true in most pipelines)
- Multiple operations before output (common in image processing, signal analysis)

---

## 4. Mathematical Foundation

### 4.1 Algebraic Structure of $\mathcal{C}$

The convolution algebra $\mathcal{C}$ can be formalized as:

**Theorem 4.1.1:** The set of convolution forms with operations (composition, superposition, scaling) forms a partially ordered magma with the following properties:

1. **Composition associativity:** $(C_1 \circ C_2) \circ C_3 = C_1 \circ (C_2 \circ C_3)$ structurally
2. **Superposition commutativity:** $C_1 \oplus C_2 = C_2 \oplus C_1$
3. **Superposition associativity:** $(C_1 \oplus C_2) \oplus C_3 = C_1 \oplus (C_2 \oplus C_3)$
4. **Distributivity (limited):** Composition distributes over superposition in specific cases:
   - $C \circ (C_1 \oplus C_2) = (C \circ C_1) \oplus (C \circ C_2)$ when composition is linear
5. **Scaling compatibility:** $\lambda (C_1 \oplus C_2) = (\lambda C_1) \oplus (\lambda C_2)$

**Proof sketch:** These follow from the linearity of convolution operations. Convolution is a linear operator; compositions and superpositions of linear operators remain linear, preserving algebraic structure.

### 4.2 Structural Equivalence as a Congruence Relation

**Definition 4.2.1:** Structural equivalence $\equiv$ on convolution forms is a congruence relation with respect to the operations of $\mathcal{C}$:

For all $C_1, C_2, C_3 \in \mathcal{C}$:
- **Reflexive:** $C \equiv C$
- **Symmetric:** $C_1 \equiv C_2 \Rightarrow C_2 \equiv C_1$
- **Transitive:** $C_1 \equiv C_2$ and $C_2 \equiv C_3 \Rightarrow C_1 \equiv C_3$
- **Congruence:** If $C_1 \equiv C_2$, then:
  - $C_1 \circ C \equiv C_2 \circ C$ (composition respects equivalence)
  - $C_1 \oplus C \equiv C_2 \oplus C$ (superposition respects equivalence)
  - $\lambda C_1 \equiv \lambda C_2$ (scaling respects equivalence)

This means: **once we establish two convolution forms as equivalent structurally, they can be freely substituted in any algebraic expression without changing the semantic meaning**. This is the formal justification for avoiding recomputation.

### 4.3 Semantic Preservation Under Closure

**Theorem 4.3.1 (Semantic Preservation):** Let $C_1, C_2, \ldots, C_N \in \mathcal{C}$ be convolution forms, and let $\Phi = C_N \circ \cdots \circ C_2 \circ C_1$ be their composition in $\mathcal{C}$.

Then: the semantic meaning of $\Phi$ (the functional behavior when materialized) is identical to computing $((\cdots(C_N(C_{N-1}(\cdots C_2(C_1) \cdots)))$ in traditional form.

Moreover: **$\Phi$ itself is a convolution form** that can participate in further algebraic operations without materialization.

**Proof:** Convolution is associative, and composition of convolutions is a convolution. The kernel of the composed operation is the convolution of the individual kernels. By induction, any composition of convolution forms yields another convolution form. ∎

This theorem is the foundation: it guarantees that working in $\mathcal{C}$ does not lose information or semantic meaning. We can safely defer materialization without changing the result.

---

## 5. Implementation Considerations

### 5.1 Data Structures for Convolution Forms

To implement $\mathcal{C}$, convolution forms must be represented such that operations are efficient:

**Canonical representation:**

ConvolutionForm { kernel: Kernel (sparse or symbolic) domain: DomainSpec (lattice, graph, manifold) parametrization: ParameterRepresentation (explicit coefficients, basis expansion, or transfer function) operations_count: Integer (how many compositions in history?) optimization_hints: Map[String, Any] (separable? symmetric? sparse?) }



**Composition implementation:**

Function compose(C1: ConvolutionForm, C2: ConvolutionForm) -> ConvolutionForm: new_kernel = convolve_kernels(C1.kernel, C2.kernel) new_domain = C1.domain # (assuming compatible domains) new_param = combine_parametrizations(C1, C2) new_hints = merge_hints(C1.optimization_hints, C2.optimization_hints)

return ConvolutionForm(new_kernel, new_domain, new_param, hints)


**Equivalence checking:**

Function equivalent(C1: ConvolutionForm, C2: ConvolutionForm) -> Boolean: if C1.kernel == C2.kernel and C1.domain == C2.domain: return parametrizations_equivalent(C1.parametrization, C2.parametrization) else: return false # or more sophisticated structural comparison


**Materialization (only at output):**


Function materialize(C: ConvolutionForm, input: Array) -> Array: kernel_data = resolve_kernel(C.kernel) return apply_convolution(input, kernel_data, C.domain) # Use FFT, direct, or domain-specific implementation as appropriate




### 5.2 Optimization Opportunities Within the Framework

The structural representation enables optimizations invisible in traditional pipelines:

1. **Lazy kernel fusion:** Combine multiple kernels symbolically; compute only the final fused kernel when materialization occurs.
   - Cost: O(k) symbolic operations instead of O(n) materializations

2. **Basis optimization:** Recognize when a convolution can be expressed in a more efficient basis (e.g., separable, frequency).
   - Cost: O(k) analysis instead of O(n log n) computation

3. **Equivalence-based caching:** Store computed convolutions and reuse them if an equivalent form is encountered.
   - Cost: O(k) equivalence check vs. O(n log n) recomputation

4. **Conditional composition:** Skip composition branches if equivalence checking shows they're redundant.
   - Cost: Depends on how often redundancy occurs

---

## 6. Scope and Applicability

### 6.1 Where This Framework Applies

**Best suited for:**

- **Signal processing pipelines:** Long chains of filtering, feature extraction, convolution-based operations
  - Example: Audio analysis (preprocessing, frequency analysis, feature extraction)
  - Savings: Avoid materializing intermediate representations

- **Image processing chains:** Composition of blur, edge detection, morphology, feature extraction
  - Example: Computer vision preprocessing
  - Savings: Significant for multi-stage pipelines

- **Numerical simulations with repeated kernels:** Physics simulations where the same convolution kernel is applied to evolving data
  - Example: Diffusion, wave equations solved with fixed stencil
  - Savings: Structure-aware optimization of stencil application

- **Symbolic computation with convolutions:** Systems where convolutions are manipulated algebraically before being evaluated
  - Example: Control theory (transfer functions as convolutions), signal theory
  - Savings: Work with symbolic forms; compute only when needed

### 6.2 Restricted Scope (Not Universal)

**This framework does NOT apply to:**

1. **Arbitrary nonlinear operations:** Thresholding, ReLU, sigmoid, pooling destroy convolution structure
   - Mitigation: Apply structural computation up to the nonlinearity; materialize before nonlinear operations

2. **Streaming or online processing:** If data arrives incrementally and results must be produced online, deferring materialization offers no benefit
   - Mitigation: Materialize at natural boundaries (e.g., frame-by-frame in video)

3. **Interactive or real-time systems:** If human interaction or sensor feedback requires immediate output, deferral adds latency
   - Mitigation: Explicit control over when to materialize

4. **Single-operation cases:** If there is one convolution and no composition, traditional methods are simpler
   - Mitigation: Only use the framework for pipelines with multiple operations

5. **Operations on graph or unstructured data:** Convolution assumes regular lattice or manifold structure
   - Mitigation: Restrict to well-structured domains

### 6.3 Honest Assessment of Complexity Class

**Important clarification:** This framework does not change asymptotic complexity for the final output.

- **Output computation is still O(n log n) or O(n):** FFT-based or direct convolution has fundamental lower bounds
- **Gain is in avoiding N-1 intermediate materializations:** In a pipeline of N operations, traditional approach computes N results; structural approach computes 1
- **Asymptotic gain: O(N * n) → O(n)** when $k \ll n$ and composition cost is negligible
- **But the final output still costs O(n):** We do not break information-theoretic or algorithmic lower bounds

This is a legitimate and significant optimization, but it is an **optimization of constant and multiplicative factors**, not a fundamental algorithmic breakthrough.

---

## 7. Comparison to Existing Methods

### 7.1 Relation to Algebraic Approaches

**Geometric Algebra / Clifford Algebra:**
- Encodes geometric relationships (orthogonality, rotations) in multiplication rules
- Our framework: encodes composition relationships in algebraic structure
- Difference: Geometric algebra focuses on rotations and transformations; our framework focuses on preserving convolutional form through composition
- Complementary: Could potentially combine both approaches

**Algebraic Signal Processing:**
- Represents signals as elements of rings or modules
- Advantage: Powerful abstract language
- Limitation: Usually works with scalar signals; our framework explicitly works with forms (kernels + domains)
- Relation: Our framework provides a structured alternative for pipeline composition

### 7.2 Relation to Optimization Methods

**Expression graphs / computational graphs (PyTorch, TensorFlow):**
- Build computation as DAG, then optimize (fusing, pruning, reordering)
- Advantage: Already in widespread use; highly optimized
- Our framework: Approaches optimization differently—preserves structure *by design* rather than recovering it from scalar operations
- Advantage of our approach: Structural optimization (separability, symmetry) is visible during construction, not post-hoc analysis
- Practical note: Could be implemented *on top of* existing frameworks as a higher-level abstraction

**Loop fusion and kernel composition (HPC):**
- Combine multiple loop nests into single optimized kernel
- Our framework: Theoretical justification for why this is correct; algebraic structure showing when it's applicable
- Relation: Our work could guide kernel fusion decisions

### 7.3 Relation to Other Structural Approaches

**Toeplitz and circulant matrix theory:**
- Recognizes that certain matrices have structure (Toeplitz, circulant, etc.)
- Advantage: Well-established, mathematically clean
- Limitation: Focuses on matrix structure, not convolution composition
- Our framework: Extends to dynamic composition of multiple convolutions

**Wavelets and multiresolution analysis:**
- Represents signals at multiple scales; operations preserve multiresolution structure
- Advantage: Captures hierarchical information
- Our framework: Orthogonal concern; wavelets are a choice of basis within our framework
- Relation: Basis selection in $\mathcal{C}$ could use wavelet parametrizations

---

## 8. Empirical Validation and Metrics

### 8.1 Proposed Benchmarks

To validate this framework, measurements should include:

**Metric 1: Pipeline composition cost**

Benchmark: Apply N convolutions sequentially Traditional: Measure time for N materializations Structural: Measure time for composition in ℭ + final materialization Ratio: Structural time / Traditional time Target: < 0.5 (50% speedup for typical pipelines)


**Metric 2: Equivalence recognition overhead**


Benchmark: Check if two convolutions are equivalent Direct computation: Materialize both, compare outputs O(n) Structural: Equivalence check in ℭ O(k) Ratio: Structural time / Direct time Target: < 0.1 (10x faster equivalence checking)


**Metric 3: Pipeline flexibility**

Benchmark: Reorder operations in a pipeline based on structural analysis Measure: Can the framework automatically reorder non-commutative operations? Target: Recognize non-commutativity structurally; enable safe reordering


**Metric 4: Basis agnosticism**

Benchmark: Switch between pixel and frequency basis without full recomputation Measure: Time to switch bases in structural form vs. FFT Ratio: Structural switch / FFT time Target: Frequency basis analysis in < O(n log n)




### 8.2 Realistic Expectations

Based on the analysis in Section 3:

**For pipelines with N ≥ 3 operations and k << n:**
- Expected speedup: 2x to 10x (avoiding intermediate materializations)
- This is **real and useful**, but not revolutionary

**For equivalence checking:**
- Expected speedup: 10x to 100x (O(k) vs O(n))
- This **is substantial** for caching and redundancy detection

**For conditional computation:**
- Savings depend on how often branches are skipped
- Potential: Significant for large pipelines with redundancy

**For single operations:**
- Overhead likely exceeds benefit
- Not the intended use case

---

## 9. Limitations and Future Work

### 9.1 Current Limitations

1. **Limited to linear operations:** Nonlinear operations (thresholding, activation functions) break the algebra; must materialize before applying them.

2. **Kernel complexity can grow:** Composition of many kernels results in increasingly complex kernels; at some point, materialization becomes unavoidable.

3. **Domain compatibility:** Convolution forms must operate on compatible domains; mapping between domains (e.g., different grid resolutions) requires explicit handling.

4. **Symbolic complexity:** Some symbolic operations (e.g., equivalence checking for complex kernel expressions) may be computationally expensive.

5. **Implementation infrastructure:** Requires custom data structures and algorithms; cannot be directly used with standard linear algebra libraries without adaptation.

### 9.2 Open Research Directions

1. **Hybrid approaches:** Combine structural representation with existing frameworks (TensorFlow, PyTorch) as a higher-level abstraction.

2. **Nonlinear extensions:** Develop weaker algebraic structures that permit certain nonlinear operations while maintaining partial structural preservation.

3. **Domain mapping:** Formalize and optimize operations between different domain specifications (grids, manifolds, graphs).

4. **Symbolic optimization:** Develop algorithms for recognizing structure in kernel expressions and automatically simplifying them.

5. **Parallel execution:** Design hardware or software systems specifically optimized for operations in $\mathcal{C}$.

6. **Adaptive materialization:** Develop heuristics for deciding when to materialize based on pipeline analysis.

---

## 10. Conclusion

The framework proposed here addresses a genuine inefficiency in how convolution-based systems are currently computed: the mandatory materialization of intermediate results before composition and comparison.

**What this framework achieves:**

- A rigorous mathematical structure $\mathcal{C}$ in which convolution forms are primary objects
- Preservation of semantic meaning while deferring materialization
- Genuine computational benefits (2x to 10x speedups in applicable domains)
- Clarity about where these benefits apply and where they don't

**What it does NOT achieve:**

- Universal O(1) computation or elimination of algorithmic complexity
- Transformation of computational bounds for single operations
- Applicability to nonlinear or unstructured computation
- A complete replacement for existing frameworks

**The honest claim:** For the restricted but practically important domain of convolution pipelines, this framework provides a cleaner and more efficient computational model than the standard decomposition-and-materialize approach.

This is not revolutionary; it is careful, bounded progress on a well-defined problem.

---

## References

**Foundational mathematics:**
- Mac Lane, S. (1998). *Categories for the Working Mathematician.* Springer.
- Munkres, J. (1984). *Topology: A First Course.* Prentice Hall.

**Convolution and signal processing:**
- Oppenheim, A. V., & Schafer, R. W. (2009). *Discrete-Time Signal Processing.* Prentice Hall.
- Cooley, J. W., & Tukey, J. W. (1965). An algorithm for the machine calculation of complex Fourier series. *Mathematics of Computation*, 19(90), 297–301.

**Efficient computation:**
- Hackbusch, W. (1985). *Multi-grid Methods and Applications.* Springer.
- Bluestein, L. I. (1970). A linear filtering approach to the computation of the discrete Fourier transform. *IEEE Transactions on Audio and Electroacoustics*, 18(4), 451–455.

**Algebraic approaches to signal processing:**
- Sharpley, R., & Vatchev, V. (2006). Analysis of the intrinsic mode functions. *Constructive Approximation*, 24(1), 45–58.

**Optimization in practice:**
- Jia, Y., Shelhamer, E., Donahue, J., et al. (2014). Caffe: Convolutional architecture for fast feature embedding. *Proceedings of the 22nd ACM International Conference on Multimedia*, 675–678.
- Paszke, A., Gross, S., Massa, F., et al. (2019). PyTorch: An imperative style, high-performance deep learning library. *Advances in Neural Information Processing Systems*, 32, 8026–8037.

---

**Author:** Hung Dinh Phu Dang  
**Affiliation:** Research, Ho Chi Minh City, Vietnam  
**Date:** 15, June 2026  
