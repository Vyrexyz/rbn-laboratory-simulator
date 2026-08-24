
# Random Boolean Network (RBN) Laboratory Simulator

A self-contained, offline Random Boolean Network laboratory and simulation environment — delivered as a single HTML file with no installation, server, or external dependencies required. Open it in any browser and it runs.

## What it does
<img width="690" height="380" alt="rbn-tissue" src="https://github.com/user-attachments/assets/3ab128a5-530d-4071-8e13-ec1d36f0bb39" />
Random Boolean Networks are a foundational model in complex systems and computational biology, used to study how simple rules produce emergent, organism-like dynamics. This simulator lets you build, run, and classify RBNs using five separate published classification schemes, rather than a single ad hoc method:

- Gershenson (2002)
- Gershenson (2004)
- Kauffman (1969)
- Kauffman (1993)
- Stoltz & Joslyn (2024)

Each framework translates peer-reviewed theoretical work from network science literature into interactive, working code.

## Design

Instead of a conventional technical dashboard, the interface uses a biologically realistic visual language:

- A dense Voronoi-based phase-contrast tissue rendering system to represent network structure the way a microscopy image would
- Amber fluorescent signalling-thread overlays layered on top to visualize signal propagation through the network
- A field-notebook, parchment-style aesthetic — built to feel like a lab instrument, not a generic app

## How to run

Download `rbn-laboratory.html` and open it in any modern browser. That's it — no build step, no dependencies, no server.

## Status

Actively developed, currently v14+. On the roadmap: Derrida plots, basin-of-attraction diagrams, additional complexity measures, multi-specimen comparison, and save/load with PNG export.

## License

MIT
