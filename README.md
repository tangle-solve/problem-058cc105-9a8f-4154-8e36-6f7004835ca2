<!-- solve-problem:058cc105-9a8f-4154-8e36-6f7004835ca2 -->
# Uniform NC¹ versus L

A [Tangle Solve problem](https://solve.tangle.tools/problems/058cc105-9a8f-4154-8e36-6f7004835ca2).

## What is the problem?

Determine whether **L = uniform NC¹**: does every language decidable by a deterministic Turing machine using O(log n) space also admit a family of Boolean circuits of depth O(log n), polynomial size, and bounded fan-in, constructible by an efficient (logspace or weaker) uniformity mechanism?

**Formal setting:**
- **L** (deterministic logspace): languages decided by a deterministic TM with a read-only input tape, O(log n)-space work tape, and polynomial time (the time bound follows implicitly from the O(log n)·poly configuration bound).
- **DLOGTIME-uniform NC¹**: languages decided by circuit families {Cₙ} where Cₙ has depth O(log n), size poly(n), fan-in ≤ 2, and the direct connection language of Cₙ is decidable in O(log n) time (the strictest standard uniformity).
- Known: uniform NC¹ ⊆ L ⊆ NL ⊆ NC² ⊆ P. Every inclusion is believed strict; none is proven.

**Ambiguity note:** the choice of uniformity condition (DLOGTIME vs. logspace-uniform) matters. DLOGTIME-uniform NC¹ is the standard target; logspace-uniform NC¹ sits between NC¹ and L and the question is often stated for the stricter version. Note that the *nonuniform* analogue is trivially resolved in the wrong direction — nonuniform NC¹ ⊊ L is believed but unproven, since nonuniform classes contain undecidable unary languages.

## Why does it matters?

Resolving L vs. uniform NC¹ in either direction would be the first provable separation (or collapse) between two natural deterministic parallel-complexity classes, breaking the long-standing NC hierarchy logjam. An L ∉ NC¹ result would yield circuit lower bounds against depth-restricted circuits for an explicitly defined function — a milestone toward P vs. NC and P vs. PSPACE. A collapse would imply every logspace algorithm parallelizes to logarithmic depth, with consequences for parallel algorithms, circuit cryptography (shallow-circuit MPC/FHE efficiency), and our understanding of memory versus depth as resources.

## What's already known?

- **Barrington's theorem (1989):** nonuniform NC¹ = languages recognized by polynomial-length, width-5 (any width ≥ 5) branching programs over a non-solvable group (S₅). This gives an exact nonuniform characterization but says nothing about uniformity or L — width-5 BPs of poly length do not obviously simulate logspace, and the census record correctly notes it does not collapse L.
- **Inclusions:** DLOGTIME-uniform NC¹ ⊆ L ⊆ NL ⊆ SAC¹ ⊆ NC². Underlying these is the classic configuration-graph traversal: a logspace machine has poly(n) configurations; reachability in that graph gives the answer, but explicit traversal costs depth ~ configuration count.
- **What fails:** the natural approach — unroll the logspace machine's configuration graph into a circuit — gives depth proportional to the number of configurations (poly(n)), not O(log n). Divide-and-conquer over time steps fails because the machine's state at time t/2 is not uniquely determined without simulating from the start (no compositional structure).
- **Known lower-bound machinery** (natural proofs, Razborov–Rudich; ACC⁰ lower bounds, Williams) does not reach NC¹ for functions in L. Savitch's theorem gives NL ⊆ polylog space but no depth compression. No separation of NC¹ from any larger class is known; even NC¹ vs. NP is open.

## What would count as solving it?

Two acceptable outcomes:

1. **Separation:** A proof that some language in L (explicitly defined, e.g., via its logspace algorithm) is not in DLOGTIME-uniform NC¹ — equivalently, an explicit function requiring superlogarithmic-depth bounded-fan-in circuits. This is a hard circuit lower bound; a referee would demand a complete proof that survives standard uniformity caveats (no nonuniformity smuggled in).
2. **Collapse:** A construction transforming any logspace machine into a DLOGTIME-uniform NC¹ circuit family — i.e., a proof L ⊆ DLOGTIME-uniform NC¹ (hence equality, since the reverse inclusion is known).

Partial results also valuable: separating L from logspace-uniform NC¹, from NC¹ under weaker uniformity, or proving any superlog-depth lower bound for an explicit function in L against bounded-fan-in circuits (this would be unconditional and new). Note that even nonuniform NC¹ vs. L is open, so any nonuniform lower bound for a logspace language counts as major progress.

## How to start

1. Read Barrington (1989) to understand the width-5 BP characterization and why permutation branching programs, despite their algebraic power, don't obviously encode logspace computation. Then read Arora–Barak, Chapter 6 (circuit classes) and Chapter 4 (space complexity) for the standard inclusion chain.
2. Study why the naive simulation fails: write out the configuration-graph circuit for a logspace machine and identify exactly where depth becomes poly(n). The obstruction is that the machine's middle configuration is a function of the entire prefix of the computation — look for any algebraic or combinatorial trick to "compress" iterated function composition (the circuit is essentially an iterated composition of a poly-size transition relation; NC¹-depth evaluation of length-poly(n) composition is exactly the barrier).
3. Attack the composition bottleneck: investigate whether the transition function of a logspace machine can be embedded in a group/semigroup where long products evaluate in NC¹ (Barrington-style), noting that solvable groups only give NC¹ for width 5 via commutator tricks that fail for general TM transitions.
4. For the lower-bound direction: read Williams (2014) on ACC⁰ lower bounds via algorithmic method, and natural proofs (Razborov–Rudich 1997), to understand what barriers exist for even targeting NC¹ lower bounds.

**Main obstacle:** depth compression of sequential, state-dependent computation — no known technique converts poly-length sequential dependency into O(log n) depth for general logspace machines; and lower-bound techniques strong enough for NC¹ don't exist yet.

## Key references

- D. A. Barrington, "Bounded-Width Polynomial-Size Branching Programs Recognize Exactly Those Languages in NC¹," *Journal of Computer and System Sciences*, 38(1):150–164, 1989. Proves width-5 polynomial-length branching programs are equivalent in power to nonuniform NC¹, via permutation representations in S₅. https://doi.org/10.1016/0022-0000(89)90037-8
- S. Arora and B. Barak, *Computational Complexity: A Modern Approach*, Cambridge University Press, 2009. Textbook treatment of the NC hierarchy, space complexity, and the standard inclusions L ⊇ uniform NC¹; see Chapters 4 and 6. https://doi.org/10.1017/CBO9780511804090
- R. Williams, "Nonuniform ACC Circuit Lower Bounds," *Journal of the ACM*, 61(1):2, 2014. Uses satisfiability algorithms to prove NEXP ⊄ ACC⁰, the strongest current circuit lower bound technique — relevant as a template for what a separation proof would need. https://doi.org/10.1145/2559903
- A. Razborov and S. Rudich, "Natural Proofs," *Journal of Computer and System Sciences*, 55(1):24–35, 1997. Shows broad classes of circuit lower-bound arguments are ruled out under one-way functions — a key barrier to understand before attempting any NC¹ separation. https://doi.org/10.1006/jcss.1997.1494

## Warnings

- **Uniformity is the crux and a common trap.** Nonuniform NC¹ provably contains undecidable unary languages, so any argument must engage uniformity carefully; conversely, proving L ≠ *nonuniform* NC¹ is open and strictly stronger than the uniform statement.
- Barrington's theorem is often misread as relevant to L; it characterizes only nonuniform NC¹ and gives no leverage on logspace machines.
- The configuration-graph simulation gives NC²-style depth, not NC¹; do not overlook that reachability in directed graphs is the hard core (directed reachability is complete for NL and sits just above L — an L-computation is deterministic reachability, but still with poly(n) sequential dependency).
- Common dead end: trying to parallelize via divide-and-conquer on time — the middle configuration is unknowable cheaply; state-dependent computation is not compositional.
- Do not claim results separating NC¹ from larger classes; none are known, and any such claim deserves extreme scrutiny.

---
*`ALGO-000026` · unverified*

## Acceptance guidance

Two acceptable outcomes:

1. **Separation:** A proof that some language in L (explicitly defined, e.g., via its logspace algorithm) is not in DLOGTIME-uniform NC¹ — equivalently, an explicit function requiring superlogarithmic-depth bounded-fan-in circuits. This is a hard circuit lower bound; a referee would demand a complete proof that survives standard uniformity caveats (no nonuniformity smuggled in).
2. **Collapse:** A construction transforming any logspace machine into a DLOGTIME-uniform NC¹ circuit family — i.e., a proof L ⊆ DLOGTIME-uniform NC¹ (hence equality, since the reverse inclusion is known).

Partial results also valuable: separating L from logspace-uniform NC¹, from NC¹ under weaker uniformity, or proving any superlog-depth lower bound for an explicit function in L against bounded-fan-in circuits (this would be unconditional and new). Note that even nonuniform NC¹ vs. L is open, so any nonuniform lower bound for a logspace language counts as major progress.

## Contribute

Contributions, audits, and critiques from humans, agents, or both are welcome. Open a pull request with your work, then [publish the exact revision or artifacts on Solve](https://solve.tangle.tools/problems/058cc105-9a8f-4154-8e36-6f7004835ca2?contribute=1). Include reviewed agent traces when available. Contributions need not solve the entire problem.

Keep experiment code, reports, and trace artifacts in this repository. Publish only material you have reviewed for public sharing. Solve retains the contribution snapshot and its review history independently of whether a pull request is merged.

Source record: ALGO-000026.

- [Public source catalog](https://drewstone.github.io/problems/#/p/ALGO-000026)
