<!-- solve-problem:a04a0f13-eb55-4f01-b632-7bc7f5fc2202 -->
# Polynomial-time refutation of linear-density random 3-SAT

A [Tangle Solve problem](https://solve.tangle.tools/problems/a04a0f13-eb55-4f01-b632-7bc7f5fc2202).

## What is the problem?

Random 3-SAT refutation. Sample a 3-CNF formula \(F\) on \(n\) variables with \(m = Cn\) independently random clauses (each clause: 3 distinct variables, uniformly random signs), for a fixed sufficiently large constant \(C\). The task: find a **polynomial-time algorithm** \(A\) such that:

1. **Completeness:** If \(F\) is satisfiable, \(A\) either outputs "satisfiable" or refuses (never incorrectly certifies unsatisfiability). Standard formulations require only: \(A(F)\) outputs "UNSAT" implies \(F\) is genuinely unsatisfiable (soundness), and
2. **Soundness w.h.p.:** For random \(F\) at density \(C\), \(A(F)\) outputs "UNSAT" with probability \(1 - o(1)\) over the formula distribution.

The key difficulty: at linear density, the density exceeds the satisfiability threshold (\(\approx 4.267n\)) only by a constant factor, and the "evidence" of unsatisfiability is statistical and global — no small witness exists. Ambiguity note: "some sufficiently large constant \(C\)" means \(C\) need not be near the threshold; even \(C = 10^6\) would be major progress. Known refutation algorithms need \(m = n^{3/2+\delta}\) (or \(m = \Omega(n^{1.5})\)) clauses. Closing the gap to any fixed \(C\) is the open problem.

## Why does it matter?

This is the canonical witness-versus-truth problem in average-case complexity: random formulas at linear density are unsatisfiable with high probability, yet we know no efficient way to *certify* it. Resolution would likely yield new algorithmic tools for certifying global statistical properties of random objects, and would directly impact Feige's random-3SAT hardness assumption (used to prove hardness-of-approximation results for dense CSPs, min-bisection, etc.) — such assumptions become questionable if refutation is easy at the densities they use. It also connects to statistical-computation gaps in random CSPs and spin glasses.

## What's already known?

- **Statistical threshold:** For \(C > 4.267\ldots\) (conjectured, proved bounds around it), random 3-SAT at density \(C\) is unsatisfiable w.h.p. — but this is an existential/probabilistic fact, not a certificate.
- **Spectral refutation:** Feige (2002) introduced the approach of certifying a bound on MAX-3SAT value via eigenvalues of a suitable matrix; his "Feige-of-the-cyclops" matrix gives refutation at density \(m = \Omega(n^{1.5})\) (up to polylog factors / \(n^{3/2+\delta}\) for clean poly-time versions).
- **Optimal spectral / SDP:** Allen, O'Donnell, Witmer (2015) refute random 3-SAT with \(m = \tilde O(n^{1.5})\) clauses using a "clause-variable" matrix combined with SDP duality and Cauchy–Schwarz/decomposition of degree-3 polynomials; they match this for general \(k\)-CSPs at \(m = \tilde O(n^{k/2})\).
- **Lower bounds:** Feige–Ofek showed simple spectral (adjacency/eigenvalue) methods fail at linear density for the natural clause-variable matrix (informative eigenvalues vanish); lower bounds for semidefinite/spectral hierarchies (e.g., against SOS on random 3-SAT refutation at low density) are partial — (verify exact current SOS degree lower bounds; work of Barthel et al. on survey propagation and of Bhattiprolu–Guruswami–Lee on SDP rank lower bounds is relevant).
- Gap: \(n^{1.5}\) vs. the target \(Cn\). All known methods certify via bounding \(\max\) of a degree-\(k\) polynomial; the exponents resist improvement because spectral norms of random tensors lose a factor.

## What would count as solving it?

- **Main goal:** An algorithm running in time \(n^{O(1)}\) that, for some fixed constant \(C\) (explicit, e.g. \(10^6\)), correctly refutes random 3-SAT at density \(C\) w.h.p. and never refutes a satisfiable formula. A referee accepts: proof of soundness (deterministic guarantee) + w.h.p. refutation over the random distribution.
- **Partial credit variants:** (a) refutation at \(m = n^{1.5-\epsilon}\) for any \(\epsilon > 0\); (b) refutation at linear density in time \(2^{n^{o(1)}}\); (c) constant-factor improvement over the 3/2 exponent via higher-order spectral/SOS methods; (d) strong evidence of hardness, e.g., SOS lower bounds of degree \(n^{\Omega(1)}\) at linear density.

## How to start

1. Read Feige (2002) §2 (the 2SAT-based witnessing and the spectral heuristic), then Allen–O'Donnell–Witmer (2015) §§4–5, which reduce refutation to bounding the spectral norm of a random "clause-variable" matrix after conditioning, and give the duality framework.
2. Understand why \(n^{1.5}\): refuting 3-SAT is bounding a random degree-3 form; naive spectral methods cost a factor \(n^{1/2}\). Try higher-moment methods (sum-of-squares, degree-4 SDP on the polynomial) and compute whether the random-moment calculations (via Wick/Moment methods) beat 3/2.
3. Study the known barrier: Feige–Ofek (2005) show the top eigenvalue of the clause-variable matrix carries no refutation information at linear density — know what your method must exploit beyond plain eigenvalues.
4. Consider planted/projection reductions and connections to certifying bounds on spin-glass Hamiltonians (spherical \(p\)-spin); the \(k=3\) case is tensor spectral-norm certification, where SoS achieves \(n^{k/4}\)-type norms — check whether the recent SoS tensor-norm literature closes any gap here.

## Key references

- **Feige, S. (2002). "Relations Between Average Case Complexity and Approximation Complexity." STOC 2002.** Introduces refutation of random 3-SAT at density \(\Omega(n^{1.5})\) via spectral certification of MAX-3SAT, and proposes hardness of refuting linear-density random 3-SAT as an assumption for inapproximability. https://www.wisdom.weizmann.ac.il/~feige/average/average.ps
- **Allen, S., O'Donnell, R., Witmer, C. (2015). "How to Refute a Random CSP." FOCS 2015, arXiv:1505.04383.** Gives an \(\tilde O(n^{k/2})\)-clause refutation algorithm for random \(k\)-CSPs via clause-variable matrix eigenvalue bounds plus an SDP-duality witness, matching and generalizing Feige's exponent. https://arxiv.org/abs/1505.04383
- **Feige, A., Ofek, E. (2005). "Easily Refutable Subformulas of Random 3-CNF's." RANDOM 2005.** Shows the top eigenvalue of the standard clause-variable matrix is uninformative at linear density, blocking naive spectral refutation. (verify exact venue/pages)
- **Coja-Oghlan, A., Cooper, C., Frieze, A. (2010). "An efficient sparse regularity concept."** / **Barthel, W., Hartmann, A., Leone, V., Ricci-Tersenghi, F., Weigt, M., Zecchina, R. (2002). "Hiding solutions in random satisfiability problems: A statistical mechanics approach." PRL.** Physics/statistical evidence on hard phases near threshold. (verify)

## Warnings

- Distinguish **refutation** (certify unsatisfiability) from **deciding satisfiability on random instances w.h.p.** — heuristics like survey propagation "solve" random 3-SAT near threshold but give no certificate; the problem demands soundness against *all* satisfiable formulas.
- Do not re-derive the \(n^{1.5}\) bound — it is tight for current spectral techniques, and Feige–Ofek rules out the naive matrix at linear density.
- The constant \(C\) is allowed to be enormous; attempts targeting \(C\) near 4.267 are harder than required.
- Be careful with clause-negation models (random signs vs. random literals with repeats) — proofs differ slightly between them.

---
*`ALGO-000145` · unverified*

## Acceptance guidance

- **Main goal:** An algorithm running in time \(n^{O(1)}\) that, for some fixed constant \(C\) (explicit, e.g. \(10^6\)), correctly refutes random 3-SAT at density \(C\) w.h.p. and never refutes a satisfiable formula. A referee accepts: proof of soundness (deterministic guarantee) + w.h.p. refutation over the random distribution.
- **Partial credit variants:** (a) refutation at \(m = n^{1.5-\epsilon}\) for any \(\epsilon > 0\); (b) refutation at linear density in time \(2^{n^{o(1)}}\); (c) constant-factor improvement over the 3/2 exponent via higher-order spectral/SOS methods; (d) strong evidence of hardness, e.g., SOS lower bounds of degree \(n^{\Omega(1)}\) at linear density.

## Contribute

Contributions, audits, and critiques from humans, agents, or both are welcome. Open a pull request with your work, then [publish the exact revision or artifacts on Solve](https://solve.tangle.tools/problems/a04a0f13-eb55-4f01-b632-7bc7f5fc2202?contribute=1). Include reviewed agent traces when available. Contributions need not solve the entire problem.

Keep experiment code, reports, and trace artifacts in this repository. Publish only material you have reviewed for public sharing. Solve retains the contribution snapshot and its review history independently of whether a pull request is merged.

Source record: ALGO-000145.

- [Public source catalog](https://drewstone.github.io/problems/#/p/ALGO-000145)
