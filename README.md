# Peeling a network by its local hubs

An interactive, browser-only demo of the **local-edge decomposition (LED)** introduced in

> W. Jeong, U. Yu, and S. H. Lee, "Global decomposition of networks into multiple cores formed by local hubs," *Phys. Rev. E* **111**, 054302 (2025). [doi:10.1103/PhysRevE.111.054302](https://doi.org/10.1103/PhysRevE.111.054302)

The demo lets you peel a network shell by shell, see where the hub-centrality pruning curve breaks, compare LED with k-core decomposition, and find the best core–periphery boundary, for the whole network or inside each community.

## The method in brief

The hub centrality of node *i* is the fraction of its neighbours with strictly smaller degree:

    h_i = N_{<k_i} / k_i,        E_ij = h_i · h_j

At each level, LED computes *h* on the current backbone and removes every edge with *E_ij* = 0, that is, every edge touching a locally smallest node. The largest remaining connected component becomes the next backbone. The nodes that drop out form that level's shell. This repeats until no edges are left, which yields an onion-like hierarchy of levels.

Because *h* is relative to a node's neighbours, a node that dominates a small group survives as long as a global hub does. k-core decomposition ranks by absolute degree and peels such nodes early.

## Features

- **Networks**
  - Communities of mixed size: preferential-attachment groups of very different sizes, loosely linked.
  - Barabási–Albert model with adjustable size and *m* (Fig. 4).
  - Three-tier stochastic block model with the densities from Fig. 10 (Sec. III C).
  - A small worked example in the spirit of Fig. 3.
  - Zachary's karate club.
  - Your own edge list, pasted in (up to 2,000 nodes; the largest connected component is used).
- **Decomposition**: LED or k-core, applied to the whole network or within each Louvain community (adjustable resolution γ), following Sec. III B.
- **coloring**: level, hub centrality, optimal core vs. periphery, or community.
- **Charts**
  - Giant-component size vs. fraction of removed edges for the hub-centrality product, the degree product and edge betweenness. The cusp at *p*<sub>c</sub> = *e*<sub>0</sub> is marked (Figs. 1–2).
  - Links from each level to higher, lower and same levels (Fig. 5).
  - Core–core, core–periphery and periphery–periphery densities, plus the core–periphery score *S*<sub>cp</sub> (Eq. 4) against the boundary level *L*<sub>B</sub>, with the optimum *L*\*<sub>B</sub> marked (Figs. 7–9).
- A force-directed drawing with draggable nodes and per-node tooltips.

## Running it

The whole demo is one self-contained file, `index.html`, with no build step and no dependencies apart from Google Fonts.

- **Locally:** open `index.html` in any modern browser.
- **GitHub Pages:** push the repository, then go to *Settings → Pages* and choose *Deploy from a branch* (for example `main`, root folder). The demo will be served at `https://<user>.github.io/<repo>/`.

## Checks against the paper

These results come from the demo's own algorithms and match the paper qualitatively:

- The Fig. 3-style example splits into levels 0, 1 and 2, with a 4-clique at the top.
- For the BA model (*N* = 2000, *m* = 4), the cusp is at *p*<sub>c</sub> ≈ 0.40 at every level, and *g* ≈ 1 − *n*<sub>0</sub>, reproducing the level-to-level collapse of Fig. 4.
- For the three-tier block model, LED's optimal boundary picks a core of about 20 nodes, close to the primary core. k-core's picks about 55 nodes, the primary and secondary cores together (Sec. III C).

## Implementation notes

- Hub centrality is computed once per level and is not recalculated during pruning, as in the paper.
- Ties in edge importance are broken at random when drawing the pruning curves.
- Communities come from a small Louvain implementation (local moving plus aggregation, with resolution γ). Results vary slightly between runs, as noted in the paper.
- k-core numbers use the Batagelj–Zaversnik algorithm. Edge betweenness uses Brandes' algorithm; it is skipped for very large pasted networks to keep the page responsive.

## Credits

- Method: Wonhee Jeong, Unjong Yu, and Sang Hoon Lee (Phys. Rev. E 111, 054302, 2025; CC BY 4.0).
- Karate club data: W. W. Zachary, "An information flow model for conflict and fission in small groups," *J. Anthropol. Res.* **33**, 452 (1977).
- Demo design and code: generated with **Claude Opus 5.5** (Anthropic).

This is an independent educational demo and is not an official implementation by the paper's authors.
