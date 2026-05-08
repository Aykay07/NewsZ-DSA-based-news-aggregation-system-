<div align="center">

```
███╗   ██╗███████╗██╗    ██╗███████╗███████╗
████╗  ██║██╔════╝██║    ██║██╔════╝╚══███╔╝
██╔██╗ ██║█████╗  ██║ █╗ ██║███████╗  ███╔╝ 
██║╚██╗██║██╔══╝  ██║███╗██║╚════██║ ███╔╝  
██║ ╚████║███████╗╚███╔███╔╝███████║███████╗
╚═╝  ╚═══╝╚══════╝ ╚══╝╚══╝ ╚══════╝╚══════╝
```

**The GenZ way of North East West South**

*Because news shouldn't just be a list — it should be a map.*

---

![JavaScript](https://img.shields.io/badge/JavaScript-ES6+-F7DF1E?style=flat-square&logo=javascript&logoColor=black)
![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=flat-square&logo=html5&logoColor=white)
![CSS3](https://img.shields.io/badge/CSS3-1572B6?style=flat-square&logo=css3&logoColor=white)
![DSA](https://img.shields.io/badge/DSA-10_Structures-0f172a?style=flat-square&color=22c55e)
![Status](https://img.shields.io/badge/Status-All_Tests_Passing-brightgreen?style=flat-square)
![License](https://img.shields.io/badge/License-MIT-blue?style=flat-square)

</div>

---

## What is this?

Most news platforms throw articles at you in a list. chronological. maybe popularity-sorted. but when something big happens — a geopolitical crisis, a market crash, a scientific breakthrough — you're left staring at 20 unrelated articles with zero idea of:

- which one to read first
- how `article A` even connects to `article B`  
- whether you should go broad or dig deep into one thread

**NewsZ** doesn't do that. it takes a topic you care about, fetches 10–12 live articles, and builds a **weighted knowledge graph** where each article is a node and shared keywords become edges. then you pick an algorithm — BFS, DFS, Dijkstra, Prim, or Kruskal — and it literally walks you through the news.

step. by. step.

---

## The stack

```
browser  →  GNews API (live articles)
              ↓
         Merge Sort (sort by date)
              ↓
         Keyword Extraction (per article)
              ↓
         Graph Builder (adjacency list)
              ↓
         vis-network (render)
              ↓
         Algorithm Engine (BFS / DFS / Dijkstra / Prim / Kruskal)
              ↓
         Playback Engine (750ms step animation)
              ↓
         [optional] Anthropic Claude API (AI context + reading order)
```

zero backend. zero build step. open `index.html` and go.

---

## Algorithms → what they actually do for you

| algorithm | reading mode | what it means |
|-----------|-------------|---------------|
| **BFS** | overview sweep | reads articles level by level from your start point — great for getting the full picture fast |
| **DFS** | deep dive | follows one thread as far as it goes before backtracking — for when you want to go deep on one angle |
| **Dijkstra** | quickest path | finds the shortest path (fewest hops between topics) to any article — cuts to what's relevant |
| **Prim** | core structure | builds a minimum spanning tree — strips the graph down to only the most essential connections |
| **Kruskal** | backbone | same goal as Prim, different strategy — builds from the strongest edges up |

---

## Data structures under the hood

```
Graph (Adjacency List)          — Map of arrays, O(V+E) space
                                  sparse graph, ~12 nodes / 20-30 edges
                                  edge weight = 1 / (shared_keywords + 1)

Priority Queue (Binary Min-Heap) — custom class, push/pop in O(log n)
                                   used by Dijkstra, Prim, and Trending System

Union-Find (DSU)                 — path compression + union-by-rank
                                   ~O(1) amortised cycle detection for Kruskal

HashMap (JS Map)                 — O(1) lookup, powers the Trending System
                                   tracks click count per article ID

Merge Sort                       — sorts articles by publish date, O(n log n)
```

---

## Complexity

| operation | best | average | worst |
|-----------|------|---------|-------|
| insert edge | O(1) | O(1) | O(V+E) |
| delete node | O(V+E) | O(V+E) | O(V+E) |
| BFS / DFS | O(V+E) | O(V+E) | O(V+E) |
| Dijkstra / Prim | O(V+E) | O((V+E) log V) | O((V+E) log V) |
| Kruskal | — | O(E log E) | O(E log E) |
| trending (heap) | — | O(k log n) | O(k log n) |

**space:** `O(V + E)` for the graph + `O(V)` for Union-Find + `O(n)` for Merge Sort buffers.

---

## Features

- **live news** — GNews API pulls fresh articles per topic query
- **5 exploration modes** — each algorithm is a genuinely different reading strategy
- **step-by-step playback** — watch the algorithm move through the graph, 750ms per step
- **custom nodes** — paste in any article URL and it joins the graph dynamically
- **trending system** — max-heap ranks articles by your click count in real time
- **localStorage caching** — same topic within 30 min? no API call. ~90% reduction in requests
- **comparison mode** — runs 3 algorithms in parallel, highlights overlap nodes
- **AI layer (optional)** — Anthropic Claude for context generation + smart reading order
- **disconnected graph handling** — algorithms still traverse reachable nodes gracefully
- **AI fallback** — if the Claude API fails, rule-based context generation kicks in

---

## Project structure

```
newsz/
├── index.html        ← UI shell, vis-network loaded via CDN
├── style.css         ← dark theme, monospace, minimal
└── app.js            ← everything else
    ├── Graph class         (adjacency list)
    ├── PriorityQueue class (binary heap)
    ├── UnionFind class     (DSU)
    ├── extractKeywords()   (stopword filtering)
    ├── buildGraph()        (keyword → edge mapping)
    ├── bfs()
    ├── dfs()
    ├── dijkstra()
    ├── renderGraph()       (vis-network wrapper)
    └── event handlers
```

---

## Getting started

no npm. no webpack. no vite. just:

```bash
git clone https://github.com/yourusername/newsz.git
cd newsz
# open index.html in Chrome / Firefox / Edge
```

if you want live articles, drop your GNews API key in `app.js`:

```js
const GNEWS_API_KEY = "your_key_here";
```

if you want the AI features, add your Anthropic key:

```js
const ANTHROPIC_API_KEY = "your_key_here";
```

both are optional — the graph engine works without either.

---

## Test cases

```
input:  "AI is transforming the world"
result: graph built from keywords {ai, transforming, world, technology...}
        BFS traversal visits all nodes ✓

input:  "Climate change affects global weather patterns significantly"
result: keywords extracted {climate, change, affects, global, weather, patterns}
        DFS visits all nodes ✓

input:  "Stock market fluctuation"
result: graph built, Dijkstra runs, shortest path distances computed ✓
```

all three pass. all algorithms complete in under 1ms for 12-node graphs.  
(the bottleneck is the GNews API call at 300–800ms, not the algorithms.)

---

## Performance notes

| component | timing |
|-----------|--------|
| all graph algorithms | < 1ms (12-node graph) |
| GNews API fetch | 300–800ms |
| vis-network physics layout | ~500ms |
| localStorage cache hit | 0ms (no API call) |

the cache window is 30 minutes per topic string. repeat queries within that window skip the API entirely.

---

## Future stuff

things that would make this genuinely better:

- [ ] **IndexedDB** — persistent graph storage across sessions
- [ ] **multi-topic merging** — combine two topic graphs, find cross-topic bridges
- [ ] **community trending** — shared backend so trending reflects everyone's reading
- [ ] **Named Entity Recognition** — better keyword extraction, proper noun awareness  
- [ ] **mobile layout** — responsive graph canvas
- [ ] **export** — save your reading path as a sharable URL

---


<div align="center">

*graphs are just connections waiting to be discovered*

</div>
