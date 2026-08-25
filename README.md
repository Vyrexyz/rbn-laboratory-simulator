RBN Laboratory Simulator

A self-contained, offline Random Boolean Network laboratory and simulation environment — delivered as a single HTML file with no installation, server, or external dependencies required. Open it in any browser and it runs.

<img width="1919" height="912" alt="Living tissue view — network mid-simulation" src="https://github.com/user-attachments/assets/7002dc38-5b06-4286-8af7-5b9993700b5f" />
What it does

Random Boolean Networks are a foundational model in complex systems and computational biology, used to study how simple local rules produce emergent, organism-like dynamics — gene regulatory networks, cell differentiation, and self-organizing behavior all sit on this same mathematical foundation.

This simulator lets you build, run, and classify RBNs using five separate published classification schemes, rather than a single ad hoc method:

Network	Timing	Rule
CRBN	Synchronous	Deterministic
ARBN	Asynchronous	Non-deterministic
DARBN	Asynchronous	Deterministic
GARBN	Semi-synchronous	Non-deterministic
DGARBN	Semi-synchronous	Deterministic

Each framework translates peer-reviewed theoretical work from network science literature — Kauffman (1969, 1993), Gershenson (2002, 2004), Stoltz & Joslyn (2024) — into interactive, working code, so results can be checked against the regime a real published model would predict, not just a plausible-looking simulation.

Construction

Every specimen is defined by four parameters plus a wiring rule:

Genes (n) — network size
Regulators (k) — how many genes regulate each gene
Rule bias (p) — probability skew in the random Boolean rule table
Hyperedge order (L) — regulators are combined via random Boolean functions over hyperedges before feeding a gene's own rule, the higher-order construction from Stoltz & Joslyn (2024), which reduces to the classical Kauffman N-K-p model at L=1
Wiring — homogeneous/uniform, or preferential-attachment wiring that produces scale-free hub genes, echoing topologies observed in real regulatory networks (Gershenson, 2004)
Design

Instead of a conventional technical dashboard, the interface uses a biologically realistic visual language, styled as a live fluorescence microscopy view (GFP reporter / DAPI stain) rather than a graph-theory diagram:

A dense Voronoi-based phase-contrast tissue rendering system represents network structure the way a microscopy image would.
Amber fluorescent signalling-thread overlays are layered directly on top to visualize signal propagation through the network in real time.
A field-notebook, parchment-style aesthetic throughout — built to feel like a lab instrument, not a generic app.
<img width="1919" height="912" alt="biologically-realistic-visual" src="https://github.com/user-attachments/assets/6a4b28c3-b958-4e88-a7ac-e0210e13fe9c" />

A Field Readings panel runs alongside the tissue view, live: elapsed time, active regime, current expression pattern, fixed-point status, recurrence/period, and a network complexity score (C = 4·E·(1−E)). An Expression Record panel renders the full state history as an autoradiograph-style banding pattern — one lane per gene, one exposure per time step. A Regulatory Logic panel exposes each gene's regulators and full truth table directly.

Analysis tools

Beyond running a single specimen, the simulator includes real experimental instruments for probing network behavior:

Damage Spreading — clones a "twin" specimen with one gene flipped and traces the Hamming distance between original and damaged twin over time, under identical firing decisions each tick. A closing gap indicates the ordered regime; a growing gap indicates chaotic — the Harvey & Bossomaier method.
Derrida Plot — samples many state pairs at a range of initial Hamming distances, applies one synchronous step to each, and plots the resulting distance. Reports a Lyapunov exponent and classifies the network as ordered or chaotic directly from measured dynamics, rather than assuming the regime from k and p alone (Derrida & Pomeau, 1986).
Exhaustive Census — walks all 2ⁿ possible expression patterns under synchronous updating, follows each to its attractor, and tallies the exact (not sampled) census: attractor count, average cycle length, and the fraction of garden-of-Eden states — states with no predecessor (after Wuensche).
Attractor Statistics — sweeps random networks at a fixed k across a range of n, runs every regime from a random state, and plots the average recurrence period found — designed to test the paper's core finding that determinism, not synchronicity, is what actually separates the ordered and chaotic regimes.
Firing Clock — used by DARBN and DGARBN: each gene fires only when t mod pᵢ = qᵢ, a deterministic rhythm in place of pure chance.
Specimen Archive — save the exact current specimen (every gene's wiring, rule table, hyperedges, and clock) to a .json file, load one back in, or export the current tissue plate view as a .png for a lab notebook or slide deck.
How to run

Download rbn-laboratory.html and open it in any modern browser. That's it — no build step, no dependencies, no server.

Status

Actively developed, currently v14+. On the roadmap: basin-of-attraction diagrams, additional complexity measures, and multi-specimen side-by-side comparison.

License

MIT
