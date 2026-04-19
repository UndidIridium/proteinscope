# ProteinScope

**Unified protein information dashboard — single HTML file, no build step, no dependencies to install.**

Enter a gene symbol, UniProt accession, or PDB code and get a consolidated view of structure, interactions, domains, pathways, diseases, and sequence — all pulled live from public databases.

> Built by **Aditya Bhargava** · [github.com/UndidIridium](https://github.com/UndidIridium)

---

## Live demo

> Deploy to GitHub Pages (see [Deployment](#deployment)) and paste the URL here.

---

## What it does

Search by **gene name** (`TP53`), **UniProt accession** (`P04637`), or **PDB code** (`1TUP`) — case-insensitive. When multiple proteins match a query a disambiguation picker is shown so you can select the right one.

### Panels

| Panel | Source | What you get |
|---|---|---|
| **Protein header** | UniProt | Full name, gene, organism, Swiss-Prot/TrEMBL status, keywords, function text with IEEE-formatted literature citations |
| **Stats bar** | UniProt / STRING / InterPro / Reactome / PDB | Length, mass, interaction partners, domain count, pathway count, structure count |
| **3D Structure Viewer** | RCSB PDB via NGL.js | Interactive 3D viewer; sidebar lists curated high-quality structures sorted by resolution (X-ray ≤3.5 Å, cryo-EM ≤4.0 Å, NMR); chain colour legend; cartoon / surface / ball+stick / licorice / spacefill representations |
| **Interaction Network** | STRING DB | Full web of interactions — hub→partner edges coloured by evidence channel, plus inter-partner edges shown as grey lines; draggable force-directed graph with zoom; score badges on nodes |
| **InterPro Domains & Families** | InterPro / EBI | Domain, family, repeat, site, and homologous superfamily entries with type badges and direct links |
| **Reactome Pathways** | Reactome | Scrollable pathway list linked to Reactome detail pages |
| **Structural Classification** | PDBe SIFTS | CATH superfamily and SCOP fold classifications mapped from the top structures, with hierarchy breadcrumbs and links |
| **Subcellular Localisation** | UniProt | Location pills (nucleus, membrane, cytoplasm, etc.) |
| **Associated Diseases** | UniProt | Disease names and descriptions |
| **Protein Sequence** | UniProt | Formatted FASTA-style sequence viewer with one-click FASTA download |

---

## Usage

### Run locally

```bash
# No server needed — just open the file
open index.html          # macOS
start index.html         # Windows
xdg-open index.html      # Linux
```

Or serve with any static server if you want proper MIME types:

```bash
npx serve .
# or
python3 -m http.server 8080
```

### Search examples

```
TP53          # gene symbol (case-insensitive)
tp53          # same thing
P04637        # UniProt accession
1TUP          # PDB code
BRCA1
EGFR
cftr
```

---

## Deployment

### GitHub Pages (free, < 2 min)

1. Push the repo to GitHub
2. Go to **Settings → Pages**
3. Source: **Deploy from a branch**, branch: `main`, folder: `/ (root)`
4. Save — your site is live at `https://<username>.github.io/<repo-name>`

### Netlify / Vercel (drag and drop)

Drag the folder onto [netlify.com/drop](https://app.netlify.com/drop) or [vercel.com/new](https://vercel.com/new). Done.

---

## Data sources

All data is fetched live from public APIs. No API keys required. No backend.

| Source | What it provides | API |
|---|---|---|
| [UniProt](https://www.uniprot.org) | Protein metadata, sequence, function, disease, localisation | REST API v2 |
| [STRING](https://string-db.org) | Protein–protein interaction network | STRING API v11 |
| [InterPro](https://www.ebi.ac.uk/interpro) | Domain and family annotations | InterPro REST API |
| [Reactome](https://reactome.org) | Biological pathways | Reactome Content Service |
| [RCSB PDB](https://www.rcsb.org) | 3D structure files and metadata | RCSB GraphQL + file server |
| [PDBe SIFTS](https://www.ebi.ac.uk/pdbe/docs/sifts/) | CATH/SCOP structural classifications | PDBe REST API |
| [PubMed](https://pubmed.ncbi.nlm.nih.gov) | Literature metadata for IEEE citations | NCBI eUtils |

---

## Tech stack

| Library | Version | Purpose |
|---|---|---|
| [D3.js](https://d3js.org) | 7.8.5 | Interaction network force graph |
| [NGL Viewer](https://github.com/arose/ngl) | v2.0.0-dev.37 | WebGL 3D structure rendering |
| [IBM Plex](https://fonts.google.com/specimen/IBM+Plex+Mono) | — | UI font |

Everything else is vanilla JS and CSS. No React, no bundler, no npm.

---

## File structure

```
proteinscope/
├── index.html    ← the entire app (rename from ProteinScope_v9.html)
└── README.md
```

---

## Known limitations

- **CORS**: all APIs used allow browser requests. If a panel fails silently it is usually a transient API rate limit — refresh and retry.
- **STRING inter-partner edges**: the second STRING call (for partner–partner edges) occasionally times out on large networks; hub edges are always shown.
- **CATH/SCOP**: classifications are pulled from the first 5 PDB structures only, so entries with many structures may show incomplete coverage.
- **PDB viewer**: structures load from RCSB over the network; very large assemblies (>10k residues) may be slow.

---

## Licence

MIT © 2026 Aditya Bhargava — free to use, modify, and distribute. See [LICENSE](LICENSE) for the full text.
