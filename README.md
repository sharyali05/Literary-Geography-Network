# Literary-Geography-Network

## The Geography of Literary Production in 19th-Century Europe

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/YOUR_USERNAME/Literary-Geography-Network/blob/main/notebook.ipynb)

As a student interested in the intersection of literature and space, I have always been curious about whether the cities European authors inhabited shaped the literary movements they belonged to. Through this network analysis, I build a **bipartite author–city network** from Wikidata, project it into city and author unipartite networks, and apply **Louvain community detection**, **degree and betweenness centrality**, and **temporal slicing** to understand how geography structured literary production across the 19th century.

---

## Research Question

> *Did geography determine literary destiny in 19th-century Europe?*

Specifically: Did a small number of dominant cities act as gravitational hubs pulling authors in (the **hub hypothesis**), or did distinct regional literary scenes develop in relative isolation, with geography reinforcing movement boundaries (the **cluster hypothesis**)?

---

## Theoretical Grounding

- **Franco Moretti**, *Atlas of the European Novel* (1998) — geography is constitutive of literature, not merely its background
- **Pascale Casanova**, *The World Republic of Letters* (1999) — Paris functioned as the dominant "meridian" of literary prestige; authors had to pass through it to gain international legitimacy

---

## Dataset

- **Source**: [Wikidata SPARQL endpoint](https://query.wikidata.org/) — queried per country to avoid server timeouts
- **Coverage**: 3,420 European authors born between 1760–1880, across 15 countries including France, Britain, Germany, Russia, Italy, Spain, Scandinavia, Austria, and Switzerland
- **Features per author**: name, birth year, birth city, residence cities, literary movement, country of citizenship
- **Network type**: Bipartite (authors × cities), projected into two unipartite networks
- **Scale**: 4,156 author-city links across 2,212 unique cities

---

## Approach

| Step | Method | Why |
|---|---|---|
| Data collection | Wikidata SPARQL (per-country batching) | Free, structured, broad European coverage; batching avoids 504 timeouts |
| Network construction | Bipartite graph → weighted projection | Honest to the data; avoids inferring direct relationships |
| Hub identification | Degree centrality (cities) | Which cities attracted the most authors? |
| Bridge detection | Betweenness centrality (cities + authors) | Which nodes connected otherwise separate literary regions? |
| Community structure | Louvain modularity | Do clusters map onto movements, regions, or both? |
| Movement geography | Attribute assortativity | Do same-movement authors share cities? |
| Temporal analysis | Epoch subgraphs (1760–1829 vs. 1830–1889) | Did the geographic center of literary production shift? |

---

## Key Findings

- **Network scale**: 3,420 authors, 2,212 cities, 4,156 author-city links; city network has 4,118 nodes and 24,136 edges
- **Hub structure**: The top city by degree centrality (0.059) leads a gradual rather than steep drop-off — pointing to a **polycentric structure** rather than a single dominant hub, complicating Casanova's Paris-centric argument
- **Bridge cities**: Several cities rank disproportionately high on betweenness relative to degree, functioning as structural bridges between regional literary scenes rather than mass attractors
- **Community structure**: 2,509 Louvain communities detected with modularity **0.814** — an exceptionally high value indicating strong fragmentation; the majority of cities are locally isolated, supporting the **cluster hypothesis**
- **Assortativity**: `nan` — Wikidata's movement (`P135`) property is too sparsely populated for reliable assortativity analysis
- **Temporal shift**: Early century (1760–1829): 377 cities, 1,535 edges → Late century (1830–1889): 1,181 cities, 11,444 edges — a ~3× expansion in cities and ~7× in connections, consistent with the decentralization of literary production across the century

---

## Visualizations

| File | Description |
|---|---|
| `city_network.png` | Force-directed city network, sized by degree, colored by Louvain community |
| `author_network.png` | Author network filtered to high-betweenness nodes, colored by literary movement |
| `temporal_comparison.png` | Side-by-side city networks for early vs. late 19th century |
| `centrality_comparison.png` | Bar charts of top cities by degree and betweenness centrality |
| `literary_map.html` | Interactive Folium map of author density by city with network edges overlaid |
| `author_network_interactive.html` | Interactive PyVis author network with hover tooltips |

---

## Repository Structure

```
├── mini_proj2.ipynb                  # Full analysis notebook
├── index.html                      # Project web presentation
├── README.md
```

---

## How to Run

**Option 1 — Google Colab** (recommended): Click the badge above.

**Option 2 — Local**:
```bash
pip install networkx pandas numpy matplotlib seaborn requests python-louvain folium geopandas pyvis tqdm
jupyter notebook notebook.ipynb
```

Wikidata is queried one country at a time to avoid server timeouts. Results are cached to `raw_birth_data.json` and `raw_residence_data.json` on first run — subsequent runs load from cache instantly.

---

## Limitations

- Wikidata coverage is uneven and biased toward Western European and canonical authors; Russia returned only 3 birth records
- City nodes retain Wikidata QIDs rather than resolved names in some visualizations — sub-city localities (streets, parishes) inflate node and community counts
- Wikidata's movement (`P135`) property is sparsely populated, making assortativity analysis unreliable without supplementary annotation
- The bipartite projection merges authors across time — temporal co-presence is not guaranteed
- Geographic proximity ≠ direct influence or relationship

---

## Further Study

- Resolve Wikidata QIDs to canonical city names and consolidate sub-city localities into parent cities
- Supplement movement labels via a curated nationality-and-period lookup to enable assortativity analysis
- Enrich with correspondence data ([EMLO database](http://emlo.bodleian.ox.ac.uk/)) to build a direct social network on top of the geographic one
- Apply dynamic network analysis with decade-level snapshots
- Expand to non-European traditions to test global vs. European hub structure
- Use NLP on Project Gutenberg texts to test stylometric–geographic correlation
- Add publisher location as a third node type

---

*Cultural Analytics — Mini Project #2*
