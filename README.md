RBN Laboratory Simulator

A self-contained, offline Random Boolean Network laboratory and simulation environment, delivered as a single HTML file with no installation, server, or external dependencies required. Download it, open it in any modern browser, and it runs immediately — there is no build step, no package manager, and no network connection required at any point.

<img width="1919" height="912" alt="Living tissue view — network mid-simulation" src="https://github.com/user-attachments/assets/7002dc38-5b06-4286-8af7-5b9993700b5f" />
What it does

Random Boolean Networks are a foundational model in complex systems science and computational biology. First introduced by Stuart Kauffman in 1969 as a model of gene regulation, they show how a network of simple, local, binary rules — each gene switching on or off based only on the state of a handful of regulator genes — can give rise to emergent, organism-like global behavior: stable cell identities, cyclical processes, and sudden shifts between order and chaos, all without any central coordination. That combination of simplicity at the local level and richness at the global level is why RBNs remain a standard tool for studying gene regulatory networks, cell differentiation, and self-organization more broadly, more than fifty years after they were first proposed.

This simulator is built around a specific idea: an RBN's behavior isn't fully defined by its wiring diagram alone. It also depends on how the network is updated — synchronously, asynchronously, or somewhere in between — and on whether the rules governing that update are deterministic or carry some randomness of their own. Different published frameworks classify this differently, and a network that looks chaotic under one classification scheme can look ordered under another. Rather than picking one method and treating it as ground truth, this simulator implements five separate, independently published classification schemes side by side, so a given network can be examined under each and the results compared directly:

Network	Timing	Rule
CRBN	Synchronous	Deterministic
ARBN	Asynchronous	Non-deterministic
DARBN	Asynchronous	Deterministic
GARBN	Semi-synchronous	Non-deterministic
DGARBN	Semi-synchronous	Deterministic

Each of these five regimes is drawn directly from the literature — Kauffman's original 1969 and 1993 formulations, Gershenson's 2002 and 2004 extensions, and the more recent higher-order construction from Stoltz & Joslyn (2024) — and implemented as working, interactive code rather than as a static description. The point of building all five into one tool is that a result can be checked against what the corresponding published model would actually predict, instead of relying on a simulation that merely looks plausible.

Construction

Every specimen in the simulator is defined by four core parameters, plus a choice of wiring strategy. The number of genes (n) sets the size of the network. The number of regulators (k) determines how many other genes feed into each gene's own update rule. The rule bias (p) skews the random Boolean rule table toward more 1s or more 0s, which in turn shifts the network toward order or chaos independently of connectivity. And the hyperedge order (L) controls how regulators are combined: rather than feeding k regulators directly into a gene's rule, they are first combined through random Boolean functions over hyperedges of order L, before that combined signal feeds the gene's own rule. This is the higher-order construction introduced by Stoltz & Joslyn (2024), and it deliberately generalizes the model — setting L=1 collapses it back down to the classical Kauffman N-K-p network, so the classical model is available as a special case rather than a separate mode.

Wiring itself can be homogeneous, where every gene has an equal chance of regulating any other gene, or preferential-attachment, where a small number of genes accumulate a disproportionate number of regulatory connections and become hub genes. The preferential-attachment option exists specifically because real biological regulatory networks are not wired uniformly at random — they tend to be scale-free, with a few heavily-connected hub genes dominating the network's behavior, a structural property documented by Gershenson (2004). Including this as an explicit wiring choice lets the simulator's networks resemble real regulatory topology rather than a purely mathematical abstraction.

Design

The interface deliberately avoids the look of a conventional technical dashboard. Instead of nodes and edges rendered as an abstract graph, the network is displayed as if it were a live fluorescence microscopy image of a tissue sample — the visual language of a wet lab, not a whiteboard. A dense Voronoi-based phase-contrast rendering forms the base "tissue," with individual cells shaped and shaded the way real phase-contrast microscopy renders cell boundaries. Genes that are currently expressed glow green, styled after a GFP reporter; the background carries a blue DAPI-style nuclear stain. Layered directly on top of that tissue, amber fluorescent signalling threads trace the live propagation of activation through the network, so watching the simulation run feels closer to watching a time-lapse of a live culture than to watching a graph algorithm animate.

<img width="1919" height="912" alt="biologically-realistic-visual" src="https://github.com/user-attachments/assets/6a4b28c3-b958-4e88-a7ac-e0210e13fe9c" />

This visual choice isn't purely decorative — it reflects what the tool is actually modeling. Alongside the tissue view, a Field Readings panel reports the state of the simulation in real time: elapsed simulation time, the active regime (CRBN, ARBN, DARBN, GARBN, or DGARBN), the current expression pattern as a raw bit string, whether the network has settled into a fixed point, its recurrence and period if it has entered a cycle, and a live network complexity score computed as C = 4·E·(1−E), where E is the fraction of genes currently expressed. An Expression Record panel beneath the tissue view renders the network's entire state history as an autoradiograph-style banding pattern, one lane per gene and one exposure per time step, echoing how gel electrophoresis results are traditionally read. A separate Regulatory Logic panel exposes the underlying mechanism directly, listing each gene's specific regulators alongside its full truth table, so the biological visualization and the raw computational logic are both available side by side rather than one being hidden behind the other.

Analysis tools

Beyond simply running and watching a single specimen, the simulator includes a set of proper experimental instruments for probing how a network actually behaves, each implementing a specific, citable method rather than an ad hoc heuristic.

The Damage Spreading tool clones a "twin" specimen from the current network with exactly one gene flipped, then runs the twin alongside the original under identical firing decisions at every tick. The Hamming distance between the two is traced over time: if that distance closes back toward zero, the network is behaving in the ordered regime, where small perturbations die out; if the distance grows, the network is in the chaotic regime, where small perturbations amplify. This is the classic method described by Harvey and Bossomaier for empirically testing which regime a given network actually occupies, rather than assuming it from its parameters.

The Derrida Plot tool takes a more systematic version of the same idea. It samples many pairs of states at a range of starting Hamming distances, applies a single synchronous update step to each pair, and plots the resulting distance against the starting distance. Points that fall below the diagonal indicate that perturbations are shrinking — the ordered regime; points above the diagonal indicate perturbations are growing — the chaotic regime. From the resulting curve, the tool also reports a Lyapunov exponent and an explicit ordered/chaotic classification, measured directly from the network's actual dynamics rather than inferred from its k and p parameters alone. This is the diagnostic originally described by Derrida and Pomeau in 1986, applied here to the specimen currently loaded rather than to a theoretical population of networks.

The Exhaustive Census tool takes the most computationally direct approach available: for networks small enough to make this tractable, it walks every one of the 2ⁿ possible expression patterns under synchronous updating, follows each one forward to whichever attractor it eventually reaches, and produces an exact tally rather than a statistical estimate — the total number of distinct attractors, the average cycle length among them, and the fraction of all states that are "garden-of-Eden" states, meaning they have no predecessor and can only ever occur as an initial condition, never be reached partway through a trajectory. This concept and terminology come from Wuensche's work on exhaustive attractor analysis.

The Attractor Statistics tool shifts from analyzing one specimen in depth to sweeping across many. It generates random networks at a fixed connectivity k across a range of network sizes n, runs every one of the five timing/rule regimes from a random starting state, and plots the average recurrence period observed in each case. The specific purpose of this sweep is to test one of the more counterintuitive findings in the literature: that it is determinism in the update rule, not synchronicity of timing, which actually separates ordered behavior from chaotic behavior — a claim that is easy to state but only really convincing once reproduced experimentally, which is what this tool is built to do.

Two further pieces of machinery support all of the above. The Firing Clock, used specifically by the DARBN and DGARBN regimes, gives each gene a deterministic firing rhythm rather than a probabilistic one: a gene fires only when the current time step t satisfies t mod pᵢ = qᵢ for that gene's own assigned p and q values, producing a fixed rhythm in place of chance. And the Specimen Archive makes any given network persistent and shareable: the exact state of a specimen — its full wiring, rule table, hyperedge structure, and firing clock — can be saved to a .json file and reloaded later, restoring the specimen exactly as it was, while the current tissue view itself can be exported as a .png image for a lab notebook, a slide deck, or a written report.

How to run

Download rbn-laboratory.html and open it in any modern browser. There is nothing to install, no build process to run, and no server to start — the file is fully self-contained.

Status

Actively developed, currently at version 14 and continuing. The next items on the roadmap are basin-of-attraction diagrams, additional complexity measures beyond the current C = 4·E·(1−E) score, and multi-specimen side-by-side comparison for examining several networks at once.

License

MIT
