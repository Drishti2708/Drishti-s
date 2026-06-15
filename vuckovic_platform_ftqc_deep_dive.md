# Jelena Vučković's Platform: A Deep Dive into Fault-Tolerant Quantum Computing

**Focus:** How photonic crystal cavities, quantum dot / color-center spin qubits, and inverse-designed nanophotonics combine into an FTQC architecture.

---

## Overview: What Kind of Machine Is This?

The Vučković platform is **not** purely linear-optical (like KLM/PsiQuantum where photons ARE the qubits).

It is a **spin-qubit + photon-mediated gate** machine:
- **Qubits** = electron or hole spins inside quantum dots (GaAs platform) or spin defects in SiC/diamond
- **Photons** = flying mediators that carry quantum information between spin qubits and implement two-qubit gates
- **Photonic crystal cavities** = the coupling element that makes the spin-photon interaction strong and fast
- **Inverse design** = the engineering toolkit that makes each component near-perfect

This is architecturally closer to the trapped-ion / color-center networking approach than to PsiQuantum. It is well-suited for a **modular quantum computer** where each node hosts one to a few spin qubits, and photons stitch the nodes together.

---

## Part 1: The Physical Qubit

### 1.1 Quantum Dot Spin Qubit (GaAs Platform)

An **InAs/GaAs self-assembled quantum dot (QD)** is a ~10 nm semiconductor island that confines a single electron (or hole) in all three dimensions — it is an "artificial atom."

**Qubit definition:**

For an electron spin:
```
|0⟩_L = |↑⟩  (spin-up along quantization axis, e.g. B-field direction)
|1⟩_L = |↓⟩  (spin-down)
```

For a hole spin (less hyperfine noise, longer T₂):
```
|0⟩_L = |⇑⟩  (heavy-hole spin-up)
|1⟩_L = |⇓⟩  (heavy-hole spin-down)
```

**Energy structure and optical selection rules:**

Under an external magnetic field B (Faraday geometry: B ∥ growth axis ẑ), the QD has four levels:
```
Conduction band (electrons):   |↑⟩,  |↓⟩
Valence band (holes / trions): |↑↓⇑⟩ (trion: 2 electrons + 1 hole), |↑↓⇓⟩
```

The optical selection rules allow only specific transitions:
```
|↑⟩ ←σ+→ |↑↓⇓⟩      (σ+ photon couples spin-up to trion)
|↓⟩ ←σ-→ |↑↓⇑⟩      (σ- photon couples spin-down to trion)
```

This **spin-selective optical transition** is the key to the spin-photon interface: a σ+ photon only interacts with a |↑⟩ spin, and a σ- photon only interacts with a |↓⟩ spin. This creates a conditional phase shift when reflected off the cavity.

**Coherence times (state of the art, 2024):**

| Parameter | Electron spin | Hole spin | SiV in diamond | Divacancy in SiC |
|---|---|---|---|---|
| T₁ (energy relaxation) | ~1 ms | ~1 ms | ~1 ms (6K) | ~1 s |
| T₂* (dephasing, inhomogeneous) | ~1–10 ns | ~100 ns – 1 μs | ~10 ns | ~1 μs |
| T₂ (Hahn echo) | ~1 μs | ~1–10 μs | ~1 ms (6K) | ~1 ms |
| T₂ (dynamical decoupling) | ~100 μs | ~10 ms | ~10 ms | ~1 s |

The **hole spin** in a charge-stabilized QD is particularly attractive for quantum computing because the p-type orbital has very small hyperfine coupling (the dominant dephasing mechanism for electrons), giving much longer T₂*.

**Single-qubit gates:**

Single-qubit rotations are implemented via:
1. **Microwave pulses** at the spin resonance frequency (ESR): f_spin = g·μ_B·B/h ≈ 5–30 GHz for B = 0.5–3 T.
   - Rotation speed: Ω_Rabi ~ 10–100 MHz achievable
   - Gate time: ~10 ns for π-pulse
   - Fidelity: >99.9% demonstrated for electron spin in gate-defined QDs; similar expected for self-assembled QDs.

2. **Stimulated Raman transitions** using two optical beams (Λ-scheme): faster, all-optical, avoids microwave engineering at cryogenic temperatures.

### 1.2 SiC Color Center Spin Qubit

In the SiC platform (Lukin et al., Nature Photonics 2020), the qubit is a **spin-1 divacancy center** or **SiV⁻ center** in 4H-SiC.

**Divacancy qubit:**
```
|0⟩_L = |ms = 0⟩
|1⟩_L = |ms = -1⟩  (or ms = +1)
```
Zero-field splitting D ≈ 1.3 GHz (4H-SiC divacancy). Single-qubit gates via microwave at this frequency.

**Optical interface:**
- Zero-phonon line (ZPL) at ~1100 nm (kh center) or ~1080 nm (kk center) — near telecom
- Debye-Waller factor (fraction in ZPL): ~5–10% at cryogenic temperature — this limits photon collection efficiency and requires Purcell enhancement to improve it

**Why SiC is compelling:**
- Mature semiconductor industry (power electronics); can in principle leverage large-wafer fabrication
- CMOS-compatible processing (unlike diamond)
- Near-telecom ZPL reduces fiber loss for networking
- Long T₂ (up to seconds with dynamical decoupling for nuclear spin qubits)

---

## Part 2: The Photonic Crystal Cavity

### 2.1 What Is a Photonic Crystal Nanocavity?

A **photonic crystal (PhC)** is a periodic dielectric structure that creates a photonic bandgap — a range of frequencies for which no photons can propagate inside the crystal. By introducing a defect into this lattice, you create a **nanocavity** that confines light to an extremely small volume.

**The L3 cavity** (most common in Vučković group): created by removing 3 holes from a triangular lattice of air holes in a GaAs membrane (~200 nm thick).

Key parameters:
- **Quality factor Q**: number of optical cycles before the photon leaks out. Higher Q = longer photon lifetime τ_c = Q/ω.
  - State of the art in GaAs PhC: Q ~ 10⁵–10⁶
- **Mode volume V**: spatial volume over which the electromagnetic field is concentrated.
  - PhC nanocavities: V ~ (λ/n)³ ≈ 0.04 μm³ — near the diffraction limit
- **Purcell factor**: F_P = (3/4π²)(λ/n)³ × Q/V

With Q = 10⁵ and V = (λ/n)³: F_P ≈ (3/4π²) × Q ≈ 7,600 — extraordinary enhancement of spontaneous emission rate.

**The Purcell effect on the QD:**
When a QD is placed at the cavity field maximum and spectrally resonant with the cavity:
- Emission rate into cavity mode: Γ_cavity = F_P × Γ₀
- Emission rate into other modes (leakage): Γ_leak ≈ Γ₀
- β-factor (fraction of photons into cavity): β = Γ_cavity / (Γ_cavity + Γ_leak) = F_P·Γ₀ / (F_P·Γ₀ + Γ₀) = F_P/(F_P + 1) → 1 for large F_P

With F_P ~ 100: β > 99%. Nearly all photons emitted by the QD go into the single cavity mode.

### 2.2 Light-Matter Coupling Regimes

The cavity-QD system is described by the **Jaynes-Cummings Hamiltonian**:

```
H = ℏω_c a†a + ℏω_QD σ†σ + ℏg(a†σ + aσ†)
```

where g is the **coupling strength** (vacuum Rabi coupling), κ = ω_c/Q is the cavity field decay rate, and γ is the QD spontaneous emission rate into non-cavity modes.

**Two regimes:**

**Weak coupling (Purcell regime):** g < κ
- Photon leaks out before completing a round-trip between QD and cavity
- Purcell enhancement increases QD emission rate
- Used for bright single-photon sources (β → 1, short T₁ → high indistinguishability)
- Applicable: most PhC + QD experiments, including Vučković group's single-photon work

**Strong coupling regime:** g > κ, γ
- Photon oscillates coherently between QD and cavity multiple times before escaping
- Dressed states (polaritons): |±⟩ = (|e,0⟩ ± |g,1⟩)/√2 split by 2g
- **Photon blockade** possible: once one photon is inside, the anharmonicity of the dressed-state ladder prevents a second photon from entering
- This is the regime of Faraon et al. Nature Physics 2008

**Achieved parameters (Vučković group):**

| Parameter | Value | Significance |
|---|---|---|
| g/2π | ~20–40 GHz | Coupling strength |
| κ/2π | ~10–30 GHz | Cavity loss rate (Q ~ 10⁴–10⁵) |
| γ/2π | ~0.1–1 GHz | QD dephasing rate |
| g/κ | ~1–3 | Borderline strong coupling |
| Cooperativity C = g²/κγ | ~10–100 | Key figure of merit |

The **cooperativity C = g²/(κγ)** is the central figure of merit:
- C ≫ 1 → strong coupling, efficient spin-photon entanglement, large conditional phase shifts
- C ~ 1–10 → current state of the art for PhC + InAs QD at cryogenic temperatures

---

## Part 3: The Spin-Photon Interface — The Gate Mechanism

### 3.1 Conditional Reflection Phase Shift

This is the **core gate mechanism** (Fushman, Englund et al., Science 2008).

Consider a PhC cavity coupled to a waveguide (single-sided, reflective geometry). A photon propagating in the waveguide reaches the cavity and is reflected.

**Reflection coefficient without QD (empty cavity):**
```
r_empty(ω) = (κ_ext - i·δ) / (κ/2 + i·δ)    where δ = ω - ω_c
```
At resonance (δ = 0): r = κ_ext / (κ/2) = 2 (overcoupled) → |r| = 1 for critical coupling, but phase is 0 or π depending on overcoupling.

For a critically coupled cavity (κ_ext = κ/2): r_empty(ω_c) = -1 (phase π).

**Reflection coefficient with resonant QD:**
```
r_QD(ω) = [κ_ext·(γ/2 + i·δ_QD) - (κ/2)(γ/2 + i·δ_QD) - g²] /
           [(κ/2 + i·δ)(γ/2 + i·δ_QD) + g²]
```

In the strong coupling limit (g ≫ κ, γ) at resonance: r_QD → +1 (phase 0).

**The conditional phase flip:**
```
Qubit in |↑⟩ (resonant with photon): photon reflects with phase π  → r = -1
Qubit in |↓⟩ (non-resonant with photon): photon reflects with phase 0 → r = +1
```

So the full transformation is:
```
|↑⟩|α⟩  →  |↑⟩(-|α⟩)  =  |↑⟩|α, reflected with π phase⟩
|↓⟩|α⟩  →  |↓⟩(+|α⟩)  =  |↓⟩|α, reflected with 0 phase⟩
```

For a single-photon Fock state |1⟩:
```
|↑⟩|1⟩  →  -|↑⟩|1⟩
|↓⟩|1⟩  →  +|↓⟩|1⟩
```

This is a **Controlled-Z (CZ) gate** between the spin qubit and the photon!

Written as a matrix in the {|↑,0⟩, |↑,1⟩, |↓,0⟩, |↓,1⟩} basis:
```
CZ_spin-photon = diag(1, -1, 1, +1)
```

### 3.2 Building a Spin-Spin CZ Gate from Two Spin-Photon CZ Gates

To perform a CZ gate between two spin qubits (QD1 and QD2) located in two different cavities connected by a waveguide:

**Protocol (Barrett & Kok, 2005; Duan & Kimble, 2004):**

Step 1: Prepare photon in |+⟩_photon = (|0⟩ + |1⟩)/√2 (path/number superposition)

Step 2: Route photon to QD1 cavity.
```
(|↑⟩₁ + |↓⟩₁)/√2 ⊗ (|↑⟩₂ + |↓⟩₂)/√2 ⊗ (|0⟩ + |1⟩)/√2
```
After CZ₁ (spin1–photon CZ):
```
= (1/2√2)[|↑⟩₁(|0⟩ - |1⟩) + |↓⟩₁(|0⟩ + |1⟩)] ⊗ (|↑⟩₂ + |↓⟩₂)
```

Step 3: Route photon to QD2 cavity. After CZ₂ (spin2–photon CZ):
```
= (1/4)[|↑⟩₁|↑⟩₂(-|0⟩+|1⟩) + |↑⟩₁|↓⟩₂(-|0⟩-|1⟩)
       + |↓⟩₁|↑⟩₂(|0⟩-|1⟩) + |↓⟩₁|↓⟩₂(|0⟩+|1⟩)]
```

Step 4: Measure photon in X basis {|+⟩, |-⟩}:
- Result |+⟩: apply Z₁ correction → net effect is CZ on (spin1, spin2) ✓
- Result |-⟩: no correction needed → net effect is CZ on (spin1, spin2) ✓

**The two-spin CZ gate is deterministic** (100% success probability), conditioned on photon being successfully transmitted and detected. The protocol is **heralded** (failure = no detector click → we know to retry).

**Gate fidelity:**
The fidelity is limited by:
- Finite cooperativity C: phase shift deviates from π by ~1/C → fidelity error ~1/C²
- QD pure dephasing during gate: error ~t_gate/T₂*
- Photon transmission loss η: reduces success probability but not fidelity (heralded protocol)

With C = 100, T₂* = 1 μs, t_gate = 1 ns: fidelity ≈ 1 - (1/C²) - (t_gate/T₂*) ≈ 99.9% — sufficient for FTQC!

### 3.3 The Two-Emitter Cavity Result (Lukin et al. PRX 2023)

Lukin et al. took this one step further: instead of two separate cavities, they placed **two SiC color centers inside a single photonic crystal cavity**.

When two emitters couple to the same cavity mode:
```
H = Σᵢ ℏg_i (a†σᵢ⁻ + a σᵢ⁺) + ℏω_c a†a + Σᵢ ℏω_i σᵢ⁺σᵢ⁻
```

This creates a **photon-mediated spin-spin interaction**. In the dispersive regime (large cavity detuning Δ ≫ g):

**Effective spin-spin coupling:**
```
J₁₂ = g₁ g₂ / Δ
```

This is an **always-on ZZ coupling** between the two spins, mediated by virtual photon exchange through the cavity. It can be used to implement a CZ gate by letting the system evolve for time t = π/(2J₁₂):

```
U(t) = exp(-i π Z₁Z₂/4) = CZ gate (up to local rotations)
```

This is analogous to the capacitive ZZ coupling in superconducting transmon qubits, but here the mediator is the photonic crystal cavity mode.

**Achieved in the PRX 2023 paper:**
- Two SiC divacancy centers coupled to a PhC cavity
- Demonstrated photon-mediated coherent interaction between the two spins
- J₁₂/2π ~ 1–10 MHz (depending on coupling and detuning)
- Two-qubit gate time: ~100 ns – 1 μs
- This is the first on-chip demonstration of the key primitive for cavity-mediated spin-spin entanglement in SiC

---

## Part 4: Inverse Design — Engineering Perfect Components

Every component in the quantum photonic circuit must work near-perfectly. Inverse design (adjoint optimization) is how the Vučković group achieves this.

### 4.1 The Adjoint Method

**Forward problem**: given a dielectric structure ε(r), compute the optical field E(r) using Maxwell's equations. This costs one FDTD simulation.

**Objective function**: a figure of merit like transmission T into a target mode, or Q/V for a cavity.

**Gradient computation** (naive): vary each pixel of ε(r) individually → N_pixels FDTD simulations → intractable for millions of pixels.

**Adjoint trick**: compute the gradient of the objective with respect to ALL pixels using only **2 FDTD simulations**:
1. Forward simulation: source → compute E(r) everywhere
2. Adjoint simulation: virtual "adjoint source" at the output → compute adjoint field E_adj(r)
3. Gradient: ∂F/∂ε(r) = Re[E(r) · E_adj(r)] (pointwise product of two field distributions)

Then update the structure: ε(r) → ε(r) + η × ∂F/∂ε(r) (gradient ascent).

Iterate until convergence (typically 100–1000 iterations). Each iteration costs 2 FDTD simulations regardless of structure complexity.

### 4.2 What Inverse Design Achieves for Quantum Photonics

Key components designed by the Vučković group:

**Grating couplers** (chip-to-fiber coupling):
- Conventional design: ~30–50% coupling efficiency
- Inverse-designed: >90% coupling efficiency, broadband
- Critical because every photon that leaks at a fiber-chip interface is lost

**Waveguide crossings** (two waveguides crossing without leakage):
- Conventional: ~1 dB loss per crossing
- Inverse-designed: <0.1 dB loss

**Multimode beam splitters and directional couplers**:
- Designed to be exactly 50/50 over a broad bandwidth (>100 nm)
- Critical for high-visibility quantum interference (HOM effect)

**Photonic crystal cavities with positioned QD**:
- Inverse design optimizes hole positions to maximize Q/V at the QD location
- Achieves Q > 10⁶ while maintaining high coupling to the waveguide

**Diamond photonic components** (Dory et al. 2019):
- All components (couplers, beam splitters, cavities) designed for the complex dielectric environment of diamond
- Particularly challenging because diamond cannot be etched with CMOS processes → requires focused ion beam or oxygen plasma, yielding rough surfaces → inverse design compensates for fabrication imperfections

**Key insight for FTQC**: Gate fidelity scales with the quality of every photonic component. Inverse design pushes each component toward unity efficiency, reducing the overhead needed for fault tolerance.

---

## Part 5: Fault-Tolerant Quantum Computing Architecture on the Vučković Platform

### 5.1 The Physical Architecture: A Network of Cavity Nodes

The natural architecture for FTQC on the Vučković platform is a **2D array of cavity nodes**, each hosting one or two spin qubits, connected by on-chip waveguides.

```
     [node]---[node]---[node]
       |         |         |
     [node]---[node]---[node]
       |         |         |
     [node]---[node]---[node]
```

Each node: 1 photonic crystal cavity + 1–2 spin qubits (QD or SiC defect).
Each edge: a waveguide enabling photon-mediated CZ gates between adjacent nodes.

For a distance-d surface code with N_L logical qubits: need d² × N_L physical qubit nodes in the bulk, plus d × N_L nodes on the boundary → total ~d² × N_L nodes.

At d = 13 (enough to run useful algorithms): ~170 physical qubits per logical qubit.

### 5.2 Quantum Error Correcting Code: The Surface Code

The **surface code** is the leading FTQC code for 2D hardware architectures due to its high threshold and nearest-neighbor connectivity requirements.

**Logical qubit:** a 2D patch of d×d physical qubits.

**Stabilizers**: operators whose eigenvalues are continuously measured to detect errors:
- Face stabilizers (Z-type): Z⊗Z⊗Z⊗Z on the 4 qubits around each face → detect X errors
- Vertex stabilizers (X-type): X⊗X⊗X⊗X on the 4 qubits around each vertex → detect Z errors

**Logical operators:**
- Z_L: product of Z operators along any path from top to bottom boundary
- X_L: product of X operators along any path from left to right boundary

**Threshold:** for depolarizing noise, the surface code threshold is p_th ≈ 1% (per physical gate). For erasure noise (photon loss, known location): p_th ≈ 50% — much higher. This is why photonic platforms (where loss is detected) have a significant advantage.

### 5.3 Syndrome Measurement Using Photon-Mediated Gates

To measure a 4-body X stabilizer (X₁X₂X₃X₄) on the surface code:

**Method 1: Ancilla spin + sequential CZ gates**

```
Step 1: Prepare ancilla spin in |+⟩ = (|↑⟩ + |↓⟩)/√2
Step 2: CZ_spin-spin(ancilla, qubit 1) — via photon mediation
Step 3: CZ_spin-spin(ancilla, qubit 2)
Step 4: CZ_spin-spin(ancilla, qubit 3)
Step 5: CZ_spin-spin(ancilla, qubit 4)
Step 6: Measure ancilla in X basis → eigenvalue ±1 of X₁X₂X₃X₄
```
Each CZ gate uses the photon-mediation protocol of §3.2. Total cycle time: ~4–8 gate times ≈ 10–100 ns per gate → syndrome cycle ≈ 40–800 ns.

**Method 2: Photon as ancilla (flying qubit syndrome measurement)**

More natural for the photonic platform:
```
Step 1: Prepare photon in |+⟩ = (|H⟩ + |V⟩)/√2 (polarization)
Step 2: Route photon to cavity 1 → CZ_spin-photon(qubit 1, photon) — picks up spin-1 phase
Step 3: Route photon to cavity 2 → CZ_spin-photon(qubit 2, photon)
Step 4: Route photon to cavity 3 → CZ_spin-photon(qubit 3, photon)
Step 5: Route photon to cavity 4 → CZ_spin-photon(qubit 4, photon)
Step 6: Measure photon in X basis: |+⟩ or |-⟩ → eigenvalue of X₁X₂X₃X₄
```

This requires **fast on-chip routing** (electro-optic switches — LiNbO₃, switchable ring resonators) to direct the photon to each cavity in sequence. Gate time per step: ~1 ns (dominated by photon transit time + cavity interaction time ~ 1/κ).

Total syndrome cycle: ~5–10 ns — potentially 10–100× faster than superconducting qubit syndrome cycles.

### 5.4 Error Budget and Threshold Analysis

**Dominant error sources and their contributions to infidelity:**

| Error source | Typical magnitude | Mitigation |
|---|---|---|
| Finite cooperativity C | ~1/C² ≈ 0.01–0.1% (C=30–100) | Higher Q cavities, better spectral alignment |
| QD pure dephasing (T₂*) | ~t_gate/T₂* ≈ 0.1–1% | Charge-stabilized QDs, resonant excitation |
| Photon loss in waveguide | Loss per gate × # gates | Ultra-low-loss waveguides, shorter circuits |
| Mode mismatch (HOM visibility) | (1-V)/2 per fusion | Inverse-designed splitters, narrow linewidth |
| Spectral diffusion (QD inhomogeneity) | Varies | Resonance fluorescence, Stark tuning |
| Detector efficiency | 1 - η_det ≈ 1–2% | On-chip SNSPDs |
| Timing jitter | Δt/T_pulse | Short pulses (ps), fast detectors |

**Current status vs. FTQC threshold:**

| Parameter | Current best | FTQC requirement |
|---|---|---|
| Single-qubit fidelity | 99.9% (microwave on QD) | > 99.9% ✓ |
| Two-qubit gate fidelity (photon-mediated) | ~95–98% | > 99.3% ✗ (gap ~1–3%) |
| Photon collection efficiency into waveguide | > 98% (β-factor) | > 99% ~ ✓ |
| Photon indistinguishability | 96–99% | > 99% ~ |
| Measurement fidelity | > 99% | > 99.3% ~ |
| T₂ (with dynamical decoupling) | 100 μs – 10 ms | > 1 ms ✓ |

**The key gap**: two-qubit gate fidelity via photon mediation is currently 95–98%, while the surface code needs ~99.3%. This ~1–3% gap is the primary obstacle.

**Path to closing the gap:**
1. **Higher cooperativity**: Q from 10⁵ to 10⁶ → C from 30 to 300 → gate error 1/C² from 0.1% to 0.001%
2. **Charge-stabilized QDs**: p-i-n structure around QD → suppress charge noise → extend T₂* from ~1 ns to ~1 μs
3. **Resonant excitation** (resonance fluorescence): drive QD at cavity frequency with laser → eliminates pure dephasing due to above-gap excitation
4. **Deterministic QD positioning**: place QD at cavity field maximum (inverse design + site-controlled growth) → maximizes g
5. **SiC platform**: divacancy T₂ > 1 ms even without dynamic decoupling; less fabrication-induced noise than GaAs

### 5.5 Logical Gates on the Surface Code

Once physical qubits are error-corrected, logical gates are implemented via:

**Transversal gates** (naturally fault-tolerant — errors don't spread):
- Logical H: apply H to all d² physical qubits transversally (but surface code is NOT transversal for H directly — requires magic state injection or code deformation)
- Logical Z_L, X_L: bit-flip all physical qubits in a column/row
- Logical CNOT: transversal CNOT between two logical patches ✓

**Code deformation / lattice surgery:**
- Logical H: deform the code boundary (topological code deformation), merging and splitting patches
- Logical CZ: merge two patches along a boundary, measure joint stabilizers, split back

**Magic state distillation (for non-Clifford T gate):**
The surface code protects Clifford operations naturally, but the T gate (T = diag(1, e^{iπ/4})) is not transversal in the surface code.

Method: prepare many low-fidelity "magic states" |T⟩ = (|0⟩ + e^{iπ/4}|1⟩)/√2, distill them into high-fidelity magic states using Clifford circuits + measurements (which are fault-tolerant), then use gate teleportation to inject the T gate.

**Magic state overhead** is the dominant cost of universal FTQC:
- ~15:1 distillation ratio per round; need ~3–4 rounds for fault-tolerant T gate
- Each T gate costs ~1000× more resources than a Clifford gate
- For Shor's algorithm on 2048-bit RSA: ~10⁶–10⁷ T gates → massive overhead

### 5.6 Clock Speed and Resource Estimates

Syndrome cycle time: ~100 ns (4 photon-mediated gates × 25 ns each)
Surface code cycle time: ~100 ns × d rounds (for decoding convergence) ≈ 1.3 μs for d=13

For a useful algorithm (e.g., quantum chemistry on 100 logical qubits, 10¹⁰ T gates):
- Physical qubits: 100 × 13² ≈ 17,000 + ~10,000 for magic state factories ≈ 30,000 nodes
- Total runtime: 10¹⁰ T gates × (distillation time per T) ≈ years at 1 μs cycle time, or hours at nanosecond cycle times

The **photonic platform's key advantage**: if syndrome cycles can be pushed to <10 ns (using the fast photon-mediated gates), runtime improves by 100× compared to superconducting qubits (~microsecond cycles).

---

## Part 6: The SiC Platform — Why It May Win

### 6.1 Why SiC Over GaAs for FTQC?

| Property | InAs/GaAs QD | SiC (divacancy/SiV) |
|---|---|---|
| ZPL fraction (Debye-Waller) | 0–5% (InAs QD, phonon sideband dominates) | 5–10% (color center) |
| T₂ (electron spin) | ~1 μs | ~1 ms |
| T₂ (nuclear spin, quantum memory) | ~1 ms | >1 s |
| Emission wavelength | 900–1000 nm | 1050–1250 nm (near-telecom) |
| Fabrication platform | GaAs/AlGaAs, not CMOS | SiC-on-insulator, CMOS-compatible |
| Scalability | Small wafers, difficult | 150–200 mm SiC wafers |
| Operation temperature | 4 K (InAs QD lines up at 4 K) | 4 K or potentially 77 K |

The nuclear spin quantum memory is particularly important: while performing photon-mediated gates (which require a flying photon), you can store quantum information in the nuclear spin (T₂ > 1 s) and retrieve it later. This enables:
- **Quantum memory during syndrome extraction**: store data qubit in nuclear spin while ancilla qubit (electron spin) interacts with photon
- **Quantum repeater nodes**: store entanglement while waiting for heralding signals from distant nodes

### 6.2 The SiC Roadmap from the Vučković Lab

Based on published work, the roadmap is:

**Achieved (as of 2023–2024):**
1. Low-loss 4H-SiC-on-insulator waveguides (~1 dB/cm) [Lukin 2020]
2. High-Q ring resonators (Q > 10⁵) in SiC [Lukin 2020]
3. Two emitters coupled to a single PhC cavity [Lukin 2023]
4. Second-harmonic generation in SiC resonators [Lukin 2020]
5. Identification and individual addressing of SiC divacancies

**Near-term goals:**
1. Demonstrate photon-mediated two-qubit gate between SiC spins
2. Achieve Q > 10⁶ in SiC PhC cavities (to boost cooperativity C > 100)
3. On-chip integration of SNSPDs with SiC waveguides
4. Increase ZPL fraction via Purcell enhancement (target: F_P > 100)

**Long-term (FTQC) goals:**
1. 2D array of SiC cavity nodes with photon-mediated CZ gates at >99.3% fidelity
2. On-chip classical control electronics (FPGA/ASIC) for feed-forward
3. Integration with quantum memory (nuclear spin) for reduced overhead
4. Wafer-scale fabrication of identical cavity nodes (inverse design ensures uniformity)

---

## Part 7: Comparison with Other Platforms

| Platform | Qubit type | Gate mechanism | T₂ | Clock speed | FTQC status |
|---|---|---|---|---|---|
| Vučković / PhC | Spin (QD, SiC) | Photon-mediated CZ | 1 μs – 1 ms | ~10 ns/gate | Pre-threshold |
| PsiQuantum (FBQC) | Photon (dual-rail) | Linear optics fusion | N/A (photons) | ~GHz | Architecture defined |
| Xanadu (GKP) | Squeezed mode | Gaussian + non-Gaussian | N/A (CV) | ~MHz | Advantage shown |
| IBM (superconducting) | Transmon | Capacitive ZZ | ~300 μs | ~100 ns/gate | 99.5% CZ, near threshold |
| Google (superconducting) | Transmon | Resonant iSWAP | ~200 μs | ~50 ns/gate | Surface code below threshold |
| Harvard/MIT (neutral atom) | Rydberg atom | Rydberg blockade | ~1 s | ~1 μs/gate | 99.5% CZ, near threshold |
| Delft (NV center in diamond) | NV electron + nuclear | Photon mediation | ~1 ms e; >1 s n | ~1 μs/gate | Small-scale demonstrations |

**Vučković platform's unique position**: combines the **long coherence times** of spin qubits (like NV centers) with the **scalable nanofabrication** of semiconductor photonics and the **speed** of photon-mediated gates. The SiC platform adds **CMOS-compatible fabrication** and **near-telecom wavelengths**.

---

## Part 8: Key Equations Summary

**Purcell factor:**
```
F_P = (3/4π²) × (λ/n)³ × (Q/V)
```

**Cooperativity (key figure of merit for spin-photon gate):**
```
C = g² / (κ × γ)        where g = coupling, κ = cavity loss, γ = QD dephasing
```

**Conditional phase shift (from cavity reflection):**
```
φ_cond ≈ π × C/(C+1) × 2/(1 + (Δ/g)²)     (near resonance, Δ = QD-cavity detuning)
```
→ For C → ∞: φ_cond → π (perfect π phase flip)

**Two-qubit gate fidelity (dominant cooperativity-limited term):**
```
F ≈ 1 - 1/C² - γ·t_gate     (at optimal cooperativity)
```

**Photon-mediated spin-spin coupling (dispersive regime):**
```
J₁₂ = g₁ × g₂ / Δ         (two emitters, same cavity, detuning Δ from cavity)
```

**CZ gate time via dispersive coupling:**
```
t_CZ = π/(2J₁₂) = πΔ/(2g₁g₂)
```

**Surface code threshold (depolarizing noise):**
```
p_th ≈ 1%                (error per gate must be below this)
```

**Surface code threshold (erasure / loss noise):**
```
p_th ≈ 50%               (loss location is known → much more forgiving)
```

---

## Key Papers for This Specific Topic (Reading Order)

1. **Fushman et al., Science 320, 769 (2008)** — Conditional phase shifts with a single QD. The gate mechanism.
2. **Faraon et al., Nature Physics 4, 859 (2008)** — Photon blockade on chip. Strong coupling in PhC.
3. **Lukin et al., Nature Photonics 14, 330 (2020)** — SiC-on-insulator platform introduction.
4. **Lukin et al., Physical Review X 13, 011036 (2023)** — Two-emitter cavity QED in SiC. Spin-spin gates.
5. **Dory et al., Nature Communications 10, 3309 (2019)** — Inverse-designed diamond photonics.
6. **Piggott et al., Nature Photonics 9, 374 (2015)** — Inverse design methodology.
7. **Duan & Kimble, PRL 92, 127902 (2004)** — Scalable photon-mediated gates (theoretical framework used by Vučković platform).
8. **Barrett & Kok, PRA 71, 060310 (2005)** — Heralded spin-spin entanglement via photons.
9. **Nickerson, Li, Benjamin, Nature Communications 4, 1756 (2013)** — Topological FTQC with noisy spin-photon networks (the architectural blueprint for using Vučković-type nodes for FTQC).
10. **Appel et al., PRL 128, 233602 (2022)** — Spin-photon entanglement from a QD waveguide; toward cluster state generation.

---

*Document compiled June 2026.*
