# Possible experiments: optical search for mathematics

Working notes, October 28, 2026. Nothing here has been run.

## The idea in one paragraph

Many questions in this collection come down to search: find an extreme configuration, a counterexample, a large Fourier coefficient, a rare direction. Digital agents can simulate interference and relaxation, but that's wasteful when physical light does both natively and almost for free. The proposal is a division of labor:

- **Light explores.** It generates candidates, statistics and counterexamples cheaply and in parallel.
- **Conventional computation certifies.** Every candidate is checked exactly on a digital machine, and anything that becomes a theorem is checked in Lean.

Light never has to be trusted. It only has to be a good, cheap explorer. This is the "thermostat" step of the paper-review workflow with a physical temperature: spread effort widely at low cost, then concentrate expensive verification on what survives.

## What light does natively

| Capability | Physical mechanism | Mathematical use |
|---|---|---|
| Linear transforms | A lens computes a 2D Fourier transform; interferometer meshes multiply by a matrix | Fourier analysis, spectra |
| Sampling | Scattering through random media; photons through interferometers | Random matrices, permanents, random environments |
| Relaxation | Lasers and optical oscillator networks settle into minimum-loss states | Energy minimization: Ising, XY, spin glasses |

Two constraints shape every design below:

1. **Light is linear.** Finding a minimum needs a nonlinear element such as gain saturation or thresholding. That's why lasers and optical parametric oscillators, not passive optics, do the optimization.
2. **Analog precision is low and noisy.** Optical hardware behaves like a positive-temperature sampler: good at exploring, bad at pinpointing. Design for that rather than against it.

## Background: the family 212 pilot

These ideas came out of a pilot review of family 212 (planar first-passage percolation: no bigeodesics, and regularity of the limit shape). Findings worth keeping:

- **Independent routes converged on the paper's crux.** Three agents, given only the problem statements, all stalled at the same step: coalescence of geodesics, or exclusion of bigeodesics, at random directions that depend on the environment. The bigeodesic paper locates its new idea exactly there, which suggests it is aimed at the real obstacle.
- **Lean coverage is narrower than "partly formalized" suggests.** `OAI.PlanarFPP.manuscriptMain` proves differentiability of the time constant and a C¹ boundary for Exp(1) weights only. Strict convexity, the Gamma generalization and the entire no-bigeodesics paper are not formalized. The formalized files contain no `sorry` or extra axioms.
- **The one exactly checkable input holds up.** For Gamma(κ) weights scaled by 1−δ, the likelihood second moment is (1−δ²)^(−κ), as the paper states.
- **Lesson about the method.** Independent agents are good at locating the hard step and poor at producing alternative proofs. That is the motivation for cheaper, wider exploration.

First-passage percolation is already a least-action problem: edge weights are travel times through random glass, geodesics are light rays (Fermat's principle), and the growing ball is a Huygens wavefront. The positive-temperature version, a random polymer summing over all paths weighted by e^(−β·cost), is the "probability mode" of the same problem. In two dimensions it shares the same fluctuation behavior (KPZ) at every temperature, so a noisy, soft physical probe should see the same exceptional structure as the zero-temperature mathematics.

## Experiments

Each entry gives the question, a setup sketch, what would count as success, and what would end it.

### 1. Branched flow as a coalescence experiment (family 212)

**Question.** In a weakly random medium, do light channels coalesce in every direction, or are there rare directions where they refuse to merge?

**Why.** Laser light in a thin random medium such as a soap film forms visible branching channels, an effect known as branched flow (Patsyk et al., 2020). These channels are physical rays coalescing in a random environment. In solvable models, coalescence fails on a random countable set of directions (jumps of the Busemann process), while bigeodesics never exist. The analogous structure in optics has never been looked for.

**Setup.** A laser, a soap film or other thin disordered film, and a camera. Launch beams over a fine sweep of angles, track channels, and record where neighbouring launch angles end in the same channel and where they split.

**Success.** A reproducible statistic of "splitting directions" that is stable under resampling the medium, and that can be compared with predictions from directed-polymer and last-passage theory.

**Failure.** The channels are dominated by the film's correlation length or by optical artefacts (thickness gradients, drainage), so nothing scale-free survives.

### 2. Optical Ising machines as counterexample hunters (families 217, 227, 239)

**Question.** Can optical Ising hardware find extreme configurations faster than digital search, often enough to stress-test claimed sharp rates?

**Target.** Family 239 claims that a uniform symmetric ±1 matrix is singular with probability (1/2 + o(1))^n. Searching for structured families of singular symmetric sign matrices, and checking every hit exactly, tests the lower-order behaviour of that claim. Families 217 and 227 (Sherrington–Kirkpatrick fluctuations and dynamics) supply natural ground-state and near-ground-state instances.

**Setup.** Coherent Ising machine or spatial-light-modulator Ising machine. Encode the objective as an Ising Hamiltonian; read out candidates; verify on a digital machine.

**Success.** On benchmark instances with known optima, the hardware finds true extremes, not just typical low-energy states, at a cost competitive with digital annealing.

**Failure.** Readout and encoding overhead dominate, or the machine reliably returns typical states rather than the rare extremes a counterexample needs.

### 3. Photonic sampling for permanent inequalities (family 238)

**Question.** Can photon-sampling statistics probe permanent inequalities on matrices too large for brute force?

**Why.** The Thorp-shuffle family relies on "A strict four-row permanent inequality and permutation moments". Output statistics of photons passing through a linear interferometer are governed by permanents of submatrices.

**Setup.** A programmable interferometer with single-photon sources and detectors. Choose interferometer matrices where the inequality is tight or nearly tight, and compare measured frequencies with the inequality's prediction.

**Success.** Empirical confirmation, or a credible violation, in regimes that exact computation can't reach, with any violation then checked exactly.

**Failure.** Photon loss and distinguishability noise swamp the margin the inequality needs.

### 4. Lenses for additive combinatorics (family 159)

**Question.** Is analog optical Fourier analysis precise enough to spot the structure that arithmetic-progression arguments rely on?

**Why.** Density-increment proofs hinge on locating large Fourier coefficients of a set. A lens displays the Fourier transform of a mask instantly.

**Setup.** Encode a subset of a grid as an amplitude mask on a spatial light modulator; image its Fourier plane; flag peaks; verify flagged frequencies digitally.

**Success.** The optical step finds the large coefficients that an exact computation confirms, at sizes where full digital transforms become the bottleneck.

**Failure.** The dynamic range and pixel precision of real modulators and cameras are too low to separate meaningful peaks from noise.

### 5. Photonic graphs for spectral universality (family 219)

**Question.** Do the eigenvalue spacings of a physically built random regular graph match random-matrix (GOE) statistics at small degree, as the family claims?

**Setup.** Coupled waveguide arrays or ring-resonator networks wired as random d-regular graphs, including the cubic case d = 3. Measure the transmission spectrum and extract level spacings.

**Success.** Spacing statistics agree with GOE predictions within fabrication tolerance, and deviate in a controlled way when weak diagonal disorder is added deliberately.

**Failure.** Fabrication disorder exceeds the regime the theorem covers, so the experiment tests the device rather than the mathematics.

## Infrastructure that would make the search easier

These don't need new hardware and could start now.

1. **A translation catalogue.** For each problem type (shortest path, ground state, spectrum, permanent, Fourier peak), record the known optical encodings, what precision each needs, and what each loses.
2. **Benchmarks with known answers.** Instances drawn from this collection where the true extreme or statistic is known, so optical results can be scored.
3. **A certificate pipeline.** Optical candidate → exact digital check → Lean formalization where applicable. The optics should never sit on the trust path.
4. **A noise model.** Treat hardware noise as an effective temperature, and record which questions survive at that temperature. The soft-versus-hard limit discussion in the 212 pilot suggests many statistical questions will survive, and exact extremal questions will not.

## Suggested order

1. Branched flow (experiment 1): cheapest hardware, most direct link to the pilot's central question.
2. The translation catalogue and benchmarks: needed before any machine results can be trusted or compared.
3. Ising-machine counterexample search (experiment 2): existing hardware, clear verification path.
4. The rest as access to photonic samplers, modulators and waveguide fabrication allows.
