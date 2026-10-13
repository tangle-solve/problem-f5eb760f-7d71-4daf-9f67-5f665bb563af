<!-- solve-problem:f5eb760f-7d71-4daf-9f67-5f665bb563af -->
# NP outside ACC⁰

A [Tangle Solve problem](https://solve.tangle.tools/problems/f5eb760f-7d71-4daf-9f67-5f665bb563af).

## What is the problem?

Prove that NP ⊄ ACC⁰, i.e., that there exists a language in NP that does not have polynomial-size circuit families of constant depth built from AND, OR, NOT, and MOD_m gates (for any fixed modulus m ≥ 2). Precisely:

- **Circuit model:** A family {C_n} of Boolean circuits of depth d = O(1), unbounded fan-in, using gates ∧, ∨, ¬, and MOD_m (outputting 1 iff the number of 1 inputs ≡ 0 mod m). Size poly(n). Non-uniform: different (arbitrary) circuits per input length. ACC⁰ = ⋃_{d,m} depth-d, poly-size circuits with these gates.
- **Claim to prove:** ∃ L ∈ NP and ε > 0 such that for infinitely many n, every depth-d ACC⁰ circuit computing L on {0,1}^n has size ≥ n^{1+ε} (the natural target: n^{ω(1)} or even superlinear near-quadratic lower bounds would be major progress).

Ambiguity note: "NP" here is the uniform class; since ACC⁰ is non-uniform, containment means every L ∈ NP has such circuits. Even ruling out NP ⊆ SIZE-ACC⁰[n^k] for some fixed k would be new. Currently, no superlinear lower bound is known for explicit functions against depth-2 MOD_m ∘ AND circuits — even SYM⁺ lower bounds are only ~n·log-ish.

## Why does it matter?

Williams (2011) proved NEXP ⊄ ACC⁰ via the algorithm-to-lower-bounds connection; pushing this down to NP would be the flagship breakthrough of circuit complexity, resolving a decades-old frontier (ACC⁰ lower bounds for explicit functions date to Razborov–Smolensky for AC⁰[p], but general ACC⁰ resisted). It would give the first unconditional proof that modular counting gates cannot efficiently capture search problems verified in polynomial time, with direct consequences for cryptography (pseudorandomness from NP hardness against ACC⁰) and for derandomization. It would also validate and sharpen the algorithmic method, likely unlocking lower bounds for stronger classes like TC⁰.

## What's already known?

- **Razborov (1987) / Smolensky (1987):** AC⁰[p] (AND/OR/NOT/MOD_p, p prime) does not contain MOD_q for q a prime ≠ p; requires exponential size. Method: polynomial approximation over finite fields. Fails for composite moduli and for combining two distinct MOD gates.
- **Williams (2011):** NEXP ⊄ ACC⁰. Strategy: (a) if NEXP ⊆ ACC⁰ then every NEXP problem has slightly-faster-than-brute-force ACC⁰ circuits; (b) an algorithm for ACC⁰-SAT running in 2^n/n^k time (for all k) contradicts the nondeterministic time hierarchy. The ACC⁰-SAT speedup uses Beigel–Tarui-style representations of ACC⁰ circuits by sum-of-product (SYM⁺) representations.
- **Beigel–Tarui (1994):** Any ACC⁰ circuit of size s, depth d has a SYM⁺ representation with 2^{(log s)^{O(d)}} product terms of low degree — the key structural tool, and the bottleneck: it is far too lossy to distinguish NP-complete functions.
- **Oliveira–Santhanam, Williams (2013–14), Murray–Williams:** strengthenings — e.g., NEXP ⊄ ACC⁰ ◦ THR, low-depth extensions, "premium" lower bounds — but all remain at NEXP-or-higher or restricted-enlarged-ACC⁰ level.
- **Barrier:** Williams's framework requires a faster-than-2^n algorithm for SAT on the circuit class; for NP-level lower bounds the contradiction must come from a hierarchy that no longer separates, since NP problems can have near-2^n algorithms. The method fundamentally seems to cap out around NTIME[2^n].

## What would count as solving it?

1. **Full solution:** A proof that NP ⊄ ACC⁰: exhibit L ∈ NP and prove no polynomial-size constant-depth ACC⁰ family computes it.
2. **Meaningful partial results a referee would accept:**
   - Superpolynomial ACC⁰ lower bounds for any function in NTIME[2^{n^ε}] (extending Williams down from NEXP).
   - An explicit (even non-NP) function with n^{1.01} lower bound against depth-2 MOD_m ∘ AND circuits — currently open (verify current status).
   - NP ⊄ ACC⁰ under any widely believed uniform assumption would be notable but is weaker.
3. Negative direction: formal barriers (e.g., natural proofs limitations for ACC⁰ are unclear since ACC⁰ isn't known learnable — an impossibility result for current techniques would also be a contribution).

## How to start

1. Read Williams (2011), especially the ACC-SAT algorithm (Section ~3) and the easy-case lemma machinery; understand exactly where the NTIME[2^n]/n nondeterministic hierarchy enters, since that's what breaks below NEXP.
2. Read Beigel–Tarui and the SYM⁺ representation; the known ACC⁰ algorithms all route through it. Ask: can you beat SYM⁺-based counting for circuits computing NP-complete languages (structure must exist there)?
3. Try restricted targets first: depth-2 ACC⁰ (MOD∘AND), or ACC⁰ with a single MOD_m layer; relate to the open problem of SYM⁺ lower bounds for explicit functions.
4. Alternative: investigate "locally decodable"/Amir–Beigel–Green machines-style indirect diagonalization, or Murray–Williams's "survivor" technique, to see if the NEXP→NP gap can be shaved via postponing the hierarchy argument.

**Main obstacle:** the algorithmic method's hierarchy argument needs a super-2^n speedup, which cannot contradict NP ⊆ small circuits; a genuinely new idea (combinatorial or algebraic) is likely needed.

## Key references

- Ryan Williams, *Non-uniform ACC Circuit Lower Bounds*, CCC 2011 (journal version in J. ACM 2014). Proves NEXP ⊄ ACC⁰ by combining a 2^n/poly-time ACC-SAT algorithm with indirect diagonalization. https://eccc.weizmann.ac.il/report/2010/105/
- Beigel, Tarui, *On ACC*, Computational Complexity 1994. Shows ACC⁰ circuits have efficient SYM⁺ (sum of ANDs followed by a symmetric gate) representations, enabling the SAT algorithms. (verify exact venue/year)
- Razborov (1987), *Lower bounds on the size of constant-depth networks* (Doklady); Smolensky (1987), *Algebraic methods in the theory of lower bounds*. Establish AC⁰[p] lower bounds via polynomial approximation; the classical ceiling this problem must break.
- Murray, Williams, *Circuit lower bounds for nondeterministic quasi-polynomial time* (FOCS 2017 / SICOMP), and Oliveira, Santhanam, *Majority vs. modular* / related works. Best current pushdowns of Williams's technique toward smaller nondeterministic classes. (verify titles)
- Hoza (2024): circuit-complexity survey/lecture materials referenced in the census; use as background — no specific result attributed. (verify)

## Warnings

- Do not conflate AC⁰[p] with ACC⁰: composite moduli and multiple moduli defeat the Razborov–Smolensky polynomial method (Smolensky's composite-modulus result requires gcd conditions).
- The Williams framework appears structurally unable to reach NP: it needs a contradiction with a nondeterministic time hierarchy at 2^n scale. Check any proposed proof for where the diagonalization gets its separation.
- "ACC⁰" bounds in the literature are often stated as NEXP not in ACC⁰ with specific size n^{k}; note Williams's theorem actually gives "NEXP ⊄ ACC⁰ ∘ THR" only in later strengthenments — cite the exact statements.
- The census's cryptographic framing is speculative; no established connection gives NP-hardness against ACC⁰ primitives.

---
*`ALGO-000035` · unverified*

## Acceptance guidance

1. **Full solution:** A proof that NP ⊄ ACC⁰: exhibit L ∈ NP and prove no polynomial-size constant-depth ACC⁰ family computes it.
2. **Meaningful partial results a referee would accept:**
   - Superpolynomial ACC⁰ lower bounds for any function in NTIME[2^{n^ε}] (extending Williams down from NEXP).
   - An explicit (even non-NP) function with n^{1.01} lower bound against depth-2 MOD_m ∘ AND circuits — currently open (verify current status).
   - NP ⊄ ACC⁰ under any widely believed uniform assumption would be notable but is weaker.
3. Negative direction: formal barriers (e.g., natural proofs limitations for ACC⁰ are unclear since ACC⁰ isn't known learnable — an impossibility result for current techniques would also be a contribution).

## Contribute

Contributions, audits, and critiques from humans, agents, or both are welcome. Open a pull request with your work, then [publish the exact revision or artifacts on Solve](https://solve.tangle.tools/problems/f5eb760f-7d71-4daf-9f67-5f665bb563af?contribute=1). Include reviewed agent traces when available. Contributions need not solve the entire problem.

Keep experiment code, reports, and trace artifacts in this repository. Publish only material you have reviewed for public sharing. Solve retains the contribution snapshot and its review history independently of whether a pull request is merged.

Source record: ALGO-000035.

- [Public source catalog](https://drewstone.github.io/problems/#/p/ALGO-000035)
