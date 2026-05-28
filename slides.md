---
theme: seriph
colorSchema: dark
background: '#000000'
class: text-center
highlighter: shiki
lineNumbers: false
controls: false
info: |
  ## RMAP/NUM Chatbot Architecture
  From Naive RAG to Multi-Agent Orchestration
drawings:
  persist: false
---

<style>
.hero-chat-slide {
  position: relative;
  min-height: calc(100vh - 96px);
  display: flex;
  align-items: center;
  justify-content: center;
  overflow: hidden;
}

.hero-content {
  position: relative;
  z-index: 2;
  text-align: center;
  max-width: 85%;
}

.hero-content h1 {
  font-size: 2.8rem;
  line-height: 1.05;
  margin-bottom: 0.7rem;
}

.hero-content p {
  font-size: 1.15rem;
  opacity: 0.95;
}

.chat-bg {
  position: absolute;
  inset: 0;
  z-index: 1;
  pointer-events: none;
}

.chat-stream {
  position: absolute;
  left: 2.5%;
  right: 2.5%;
}

.chat-stream.left {
  text-align: left;
}

.chat-stream.right {
  text-align: right;
}

.chat-line {
  display: inline-block;
  position: relative;
  max-width: none;
  font-family: ui-monospace, SFMono-Regular, Menlo, Consolas, monospace;
  font-size: 0.8rem;
  letter-spacing: 0.01em;
  color: rgba(191, 219, 254, 0.3);
  white-space: nowrap;
  overflow: hidden;
  width: 0;
  border-right: 1px solid rgba(191, 219, 254, 0.78);
  animation: typeErase 15.6s steps(120, end) infinite;
}

.chat-line::before {
  content: "";
  position: absolute;
  right: 0;
  top: 8%;
  width: 1.4ch;
  height: 84%;
  background: radial-gradient(circle at 35% 50%, rgba(191, 219, 254, 0.58), rgba(191, 219, 254, 0));
  filter: blur(1.8px);
  opacity: 0.82;
  animation: glowPulse 0.9s ease-in-out infinite;
}

.chat-line.alt {
  color: rgba(110, 231, 183, 0.28);
  border-right-color: rgba(110, 231, 183, 0.78);
}

.chat-line.alt::before {
  background: radial-gradient(circle at 35% 50%, rgba(110, 231, 183, 0.56), rgba(110, 231, 183, 0));
}

@keyframes typeErase {
  0% {
    width: 0;
    opacity: 0;
  }
  8% {
    opacity: 1;
  }
  36% {
    width: calc(var(--chars, 80) * 1ch);
    opacity: 1;
  }
  58% {
    width: calc(var(--chars, 80) * 1ch);
    opacity: 0.95;
  }
  88% {
    width: 0;
    opacity: 0.9;
  }
  100% {
    width: 0;
    opacity: 0;
  }
}

@keyframes caretBlink {
  0%, 45% { opacity: 0.95; }
  46%, 100% { opacity: 0.25; }
}

@keyframes glowPulse {
  0%, 100% { opacity: 0.55; }
  50% { opacity: 0.92; }
}

/* Slide 17: high-contrast inline/query code for readability inside colored cards. */
.kg-rag-slide code {
  background: #e2e8f0;
  color: #0f172a;
  padding: 0.05rem 0.28rem;
  border-radius: 0.25rem;
  border: 1px solid #cbd5e1;
}

.kg-rag-slide .query-code {
  display: block;
  margin-top: 0.35rem;
  background: #f8fafc;
  color: #0f172a;
  border: 1px solid #cbd5e1;
  border-radius: 0.35rem;
  padding: 0.35rem 0.45rem;
  font-family: ui-monospace, SFMono-Regular, Menlo, Consolas, monospace;
  font-size: 0.72rem;
  line-height: 1.25;
  white-space: normal;
  overflow-wrap: anywhere;
}

/* Slide 16: high-contrast inline code in explanatory cards. */
.kg-rag-slide16 code,
.kg-rag-slide16 code span {
  background: #e2e8f0 !important;
  color: #0f172a !important;
  padding: 0.05rem 0.28rem;
  border-radius: 0.25rem;
  border: 1px solid #cbd5e1;
  text-shadow: none !important;
}
</style>

<div class="hero-chat-slide">
  <div class="chat-bg">
    <div class="chat-stream left" style="top:8%;">
      <span class="chat-line" style="animation-delay:0s; --chars:108;">User: Which papers did Mark Helm publish in 2025, and can you list journals and years in a compact table?</span>
    </div>
    <div class="chat-stream right" style="top:16%;">
      <span class="chat-line alt" style="animation-delay:1.4s; --chars:121;">Assistant: Applying deterministic author and year filters, then validating journal metadata before response generation...</span>
    </div>
    <div class="chat-stream left" style="top:24%;">
      <span class="chat-line" style="animation-delay:2.7s; --chars:109;">User: Please summarize them and compare methods, limitations, and sequencing setup differences across papers.</span>
    </div>
    <div class="chat-stream right" style="top:32%;">
      <span class="chat-line alt" style="animation-delay:4.1s; --chars:104;">Assistant: Rewritten with memory: summarize Paper A, Paper B, Paper C, and Paper D in requested order.</span>
    </div>
    <div class="chat-stream left" style="top:64%;">
      <span class="chat-line" style="animation-delay:5.3s; --chars:122;">System: Iterative retrieval started with per-paper metadata filters, constrained top-k context, and map-reduce aggregation.</span>
    </div>
    <div class="chat-stream right" style="top:72%;">
      <span class="chat-line alt" style="animation-delay:6.8s; --chars:122;">Assistant: Map phase complete for all requested papers. Running reduce synthesis with strict no-hallucination guardrails...</span>
    </div>
    <div class="chat-stream left" style="top:80%;">
      <span class="chat-line" style="animation-delay:8.2s; --chars:114;">User: Show only Nucleic Acids Res entries from 2025, and keep only exact metadata matches in the final list.</span>
    </div>
  </div>

  <div class="hero-content">
    <h1>Beyond "LLM In / LLM Out"</h1>
    <p>Architecting a Sovereign Chatbot for the NUM / RMaP Consortium</p>
  </div>

  <div class="abs-br m-6 flex gap-2 z-10">
    Philipp Wiesenbach | Dieterich Lab | June 2026
  </div>
</div>

---
layout: default
---

# The Mission

**The Goal:** Provide the RMaP Consortium (and later the NUM) with a secure, on-premise AI assistant to navigate complex guidelines, governance documents, and metadata catalogs.

**The Constraints:**

- Absolute Data Privacy: No OpenAI APIs. Local deployment only (vLLM / Ollama).
- Heterogeneous Data: PDFs, web scrapes, double-column Nature papers, governance tables.
- High Accuracy: "Hallucinations" about grant deadlines or cohort sizes are unacceptable.

**The Naive Assumption:**
> *"Just throw the PDFs into a Vector Database, connect Llama-3, and we're done."*

---
layout: default
---

# The Reality: Naive RAG fails

Why a simple Vector-RAG approach crashes in clinical/research environments.

**The Pipeline:**

1. Chunk the PDF (e.g., 1000 tokens).
2. Create Vectors (Embeddings).
3. Search Top-K (e.g., 5 chunks).
4. Send to LLM.

**The Fatal Flaw:**
Vector databases measure semantic similarity, not factual truth or document structure.

::right::

<br>
<br>

<div class="bg-red-100 p-4 rounded-md text-red-900 shadow-md">
 <b>Example Failure:</b><br>
 <i>User:</i> "Which papers were published by Mark Helm?"<br>
 <i>System:</i> "I don't know." (or hallucinates)
</div>

<br>

**Why? The "Top-K" Bottleneck:**
If Mark Helm wrote 25 papers, but Top-K is set to 5, the LLM literally cannot see the other 20 papers. It is mathematically impossible for Vector-RAG to aggregate lists across a whole database.

---
layout: default
---

# Similarity Trap: Question != Evidence Wording

<div class="text-left text-sm leading-snug">
<b>Problem:</b> semantic similarity follows wording, while evidence is often formatted differently.

<b>Example:</b> Query: "Which papers did Mark Helm publish?"<br>
Evidence is mostly in author lists and bibliography rows.
</div>

```mermaid
flowchart LR
  classDef q fill:#dbeafe,stroke:#1d4ed8,color:#111827,stroke-width:2px
  classDef bad fill:#fee2e2,stroke:#991b1b,color:#111827,stroke-width:2px
  classDef good fill:#dcfce7,stroke:#166534,color:#111827,stroke-width:2px

  Q[Query embedding]:::q --> B[Top-k misses key rows]:::bad
  Q --> H[HyDE]:::good
  H --> R[Better retrieval alignment]:::good
```

<div class="mt-2 p-2 rounded bg-emerald-50 border border-emerald-200 text-emerald-900 text-sm leading-snug">
<b>HyDE (Hypothetical Document Embeddings) as solution:</b> generate a short hypothetical answer first, embed that text, then retrieve.
This often brings retrieval closer to evidence-shaped chunks.
</div>

---
layout: default
---

# The "Smart BS" Illusion (Parametric Memory Leak)

When RAG fails, strong LLMs try to help anyway.

If the Vector DB fails to fetch the correct abstract for a paper, but provides the title (e.g., from a bibliography chunk), the LLM relies on its **pre-training knowledge**.

<div class="grid grid-cols-2 gap-4 mt-4">
 <div>
  <div class="bg-gray-100 text-gray-900 p-4 rounded-md h-full">
   <b>What the LLM sees (Context):</b><br>
  <span class="text-sm text-gray-900 font-mono">Chunk 1: "References: 14. Dieterich C, 2025, Nucleic Acids Res..."</span><br>
  <span class="text-sm text-gray-900 font-mono">Chunk 2: "![image] ![image] © Oxford Univ Press"</span>
  </div>
 </div>
 <div>
  <div class="bg-yellow-100 text-gray-900 p-4 rounded-md h-full border border-yellow-400">
   <b>The LLM's Internal Monologue (&lt;think&gt;):</b><br>
   <span class="text-sm text-gray-800"><i>"I don't have the abstract for Dieterich 2025. But I know the title. I will write a plausible summary based on my training data."</i></span><br>
   <b>Result:</b> A perfectly written, scientifically plausible, but completely hallucinated summary.
  </div>
 </div>
</div>

<div class="mt-8 font-bold text-center text-red-600">
Conclusion: We needed to regain control over the retrieval and generation process.
</div>

---
layout: center
class: text-center
---

# Enter: Agentic Workflow & Semantic Routing

We migrated to **Dify.ai** to build a modular, multi-path architecture.

---
layout: default
---

# The New Architecture: Semantic Routing

We decouple *content queries* from *metadata queries* using a Classifier.

```mermaid
flowchart LR
  classDef start fill:#dbeafe,stroke:#1e3a8a,color:#111827,stroke-width:2px
  classDef router fill:#fde68a,stroke:#92400e,color:#111827,stroke-width:2px
  classDef process fill:#bbf7d0,stroke:#166534,color:#111827,stroke-width:2px
  classDef db fill:#fecdd3,stroke:#9f1239,color:#111827,stroke-width:2px

  Q([User Query]):::start --> QC{LLM Question<br>Classifier}:::router

  QC -->|"Content Intent"| R1[Knowledge Retrieval<br>Vector + BM25]:::process
  QC -->|"Metadata Intent"| R2[Parameter Extractor<br>JSON]:::process

  R1 --> A([Answer Generator]):::start
  R2 --> DB[(Weaviate DB<br>GraphQL Code Node)]:::db
  DB --> A
```

---
layout: two-cols-header
---

# Routing in Action: The Dual Path

How the system behaves under the hood based on the classifier's decision.

::left::

Route A: Content (RAG)

Query: "What is the mechanism of Queuosine?"

- Retrieval: Vector Database
- Search space: 50,000 chunks
- Method: Hybrid Search (Cosine Similarity + BM25)
- Output: Top 10 chunks containing specific abstracts/methods

::right::

Route B: Metadata (GraphQL)

Query: "Which papers did Mark Helm publish?"

- Parameter extraction: author = "Mark Helm"
- Retrieval: Python Code Node
- Method: Deterministic database query
- Output: title, year

<Transform :scale="0.74" origin="top left">

```graphql
{
  Get {
    Document(
      where: {
        path: ["authors"]
        operator: Like
        valueText: "*Mark Helm*"
      }
    ) {
      title
      year
    }
  }
}
```

</Transform>


---
layout: default
---

# The Boss Level: Multi-Document Summarization

> *"Please summarize them."*

One short sentence, four independent failure modes.

```mermaid
flowchart TB
  classDef core fill:#dbeafe,stroke:#1d4ed8,color:#111827,stroke-width:2px
  classDef p1 fill:#ffe4e6,stroke:#be123c,color:#111827,stroke-width:2px
  classDef p2 fill:#fef3c7,stroke:#b45309,color:#111827,stroke-width:2px
  classDef p3 fill:#cffafe,stroke:#0e7490,color:#111827,stroke-width:2px
  classDef p4 fill:#e0e7ff,stroke:#4338ca,color:#111827,stroke-width:2px

  Q["User query: Please summarize them"]:::core
  Q --> A["Step 1 - Which papers are referenced"]:::p1
  Q --> B["Step 2 - Rewrite query as standalone"]:::p2
  Q --> C["Step 3 - Retrieve evidence without overload"]:::p3
  Q --> D["Step 4 - Keep metadata deterministic"]:::p4
```

<div class="mt-3 p-3 rounded-md bg-slate-100 border border-slate-300 text-slate-900">
<b>Design strategy:</b> solve each failure mode explicitly with one dedicated architectural improvement.
</div>

---
layout: default
---

# Problem 1: Follow-up queries lose context

Without explicit state, "them" is ambiguous and the bot may summarize the wrong papers.

<div class="mt-3 p-3 rounded-md bg-rose-50 border border-rose-200 text-rose-900">
<b>Solution introduced:</b> <b>Conversational Memory</b><br>
Persist paper identities across turns and resolve follow-up references from that memory.
</div>

<div class="mt-2 text-sm font-semibold text-sky-200">Real memory snapshot (conversation.memory)</div>

```json
[
  {
    "title": "Detection of queuosine and queuosine precursors in tRNAs by direct RNA sequencing",
    "authors": "Yu Sun, Michael Piechotta, Isabel Naarmann-de Vries, Christoph Dieterich, Ann E. Ehrenhofer-Murray",
    "year": "2023",
    "journal": "Nucleic Acids Res"
  },
  {
    "title": "Sci-ModoM: a quantitative database of transcriptome-wide high-throughput RNA modification sites",
    "authors": "Etienne Boileau, Harald Wilhelmi, Anne Busch, Andrea Cappannini, Andreas Hildebrand, Janusz M. Bujnicki, Christoph Dieterich",
    "year": "2025",
    "journal": "Nucleic Acids Res"
  }
]
```

---
layout: default
---

# Problem 2: Query text is underspecified

Even with memory, retrieval works poorly if the raw user text is not standalone.

<div class="mt-3 p-3 rounded-md bg-amber-50 border border-amber-200 text-amber-900">
<b>Solution introduced:</b> <b>Question Rewriter</b><br>
Rewrite the user query into a self-contained retrieval query before searching.
</div>

<div class="mt-2 text-sm font-semibold text-sky-200">Real rewrite example</div>

```text
Input query:
Please summarize them.

Conversation memory contains:
- Detection of queuosine and queuosine precursors in tRNAs by direct RNA sequencing (2023)
- Adaptive sampling for nanopore direct RNA-sequencing (2023)
- Detecting m6A at single-molecular resolution via direct RNA sequencing and realistic training data (2024)
- Sci-ModoM: a quantitative database of transcriptome-wide high-throughput RNA modification sites (2025)

Rewritten standalone query:
Please summarize these four papers: Detection of queuosine and queuosine precursors in tRNAs by direct RNA sequencing; Adaptive sampling for nanopore direct RNA-sequencing; Detecting m6A at single-molecular resolution via direct RNA sequencing and realistic training data; Sci-ModoM: a quantitative database of transcriptome-wide high-throughput RNA modification sites.
```

---
layout: default
---

# Problem 3: One-shot retrieval misses evidence

Batching everything at once causes noisy context and loss-in-the-middle.

<div class="mt-3 p-3 rounded-md bg-cyan-50 border border-cyan-200 text-cyan-900">
<b>Solution introduced:</b> <b>Iterative Knowledge Retrieval</b><br>
Process one paper at a time (retrieve -> summarize), then aggregate.
</div>

<div class="mt-2 text-sm font-semibold text-sky-200">Execution model (For each paper)</div>

```python
paper_list = resolve_paper_list(query, memory)
partials = []

for paper in paper_list:
    docs = knowledge_retrieval_with_filter(
        title=paper["title"],
        authors=paper["authors"],
        year=paper["year"],
        top_k=10,
    )
    partials.append(paper_map_llm(paper, docs))

final_answer = reduce_llm(partials, requested_order=paper_list)
```

---
layout: default
---

# Problem 4: Metadata answers must be deterministic

Author/year/journal questions are fragile if answered only via semantic similarity.

<div class="mt-3 p-3 rounded-md bg-indigo-50 border border-indigo-200 text-indigo-900">
<b>Solution introduced:</b> <b>DB Lookup with fixed metadata filter</b><br>
Apply strict metadata constraints (author, year, journal, title) before generating output.
</div>

<div class="mt-2 text-sm font-semibold text-sky-200">Fixed metadata filter example</div>

```json
{
  "author": "Christoph Dieterich",
  "year": "2025",
  "journal": "Nucleic Acids Res",
  "title": "Sci-ModoM"
}
```

<div class="text-[0.64rem] leading-tight">

```graphql
{
  Get{Document(where:{
    operator:And,
    operands:[
      {path:["authors"],operator:Like,valueText:"*Christoph Dieterich*"},
      {path:["year"],operator:Equal,valueText:"2025"}
    ]}){title year}}
}
```

</div>

---
layout: default
---

# Full Chatbot Flow (Simplified)

<div class="h-[calc(100vh-220px)] w-full flex items-start justify-center overflow-hidden">
  <img src="/chatbot_mermaid.png" class="h-[80%] w-auto object-contain" />
</div>

---
layout: default
---

# The Reality

<div class="text-sky-200 text-sm mb-2">Production view in Dify (complex workflow, condensed in the previous architecture slide)</div>

<div class="h-[calc(100vh-220px)] w-full flex items-start justify-center overflow-hidden">
  <img src="/chatbot_dify.png" class="h-full w-auto object-contain rounded-md" />
</div>


---
layout: default
---

# Why GraphRAG Changes the Game

<div class="text-sm text-sky-200 mb-2">
Moving from fragile text-chunk guessing to deterministic, multi-hop semantic reasoning.
</div>

```mermaid
flowchart LR
  classDef q fill:#dbeafe,stroke:#1d4ed8,color:#111827,stroke-width:2px
  classDef step fill:#fef3c7,stroke:#b45309,color:#111827,stroke-width:2px
  classDef g fill:#dcfce7,stroke:#166534,color:#111827,stroke-width:2px
  classDef out fill:#e0e7ff,stroke:#4338ca,color:#111827,stroke-width:2px

  U[User Query]:::q --> I[NLU: Entity Extraction<br>& Semantic Grounding]:::step
  I --> S[Seed Nodes<br>e.g., Author X, Gene Y]:::step
  S --> G[Graph Traversal<br>Find paths via shared concepts]:::g
  G --> E[Evidence Subgraph<br>Connected cross-document path]:::g
  E --> A[Answer Generation<br>with 100% Traceability]:::out
```

<div class="kg-rag-slide16 grid grid-cols-2 gap-4 mt-3 text-sm">
  <div class="rounded-md border border-rose-200 bg-rose-50 text-rose-900 p-3">
    <b>The Vector-RAG Flaw:</b> Top-K chunk guessing.<br>
    The system blindly retrieves text snippets based on similarity. It cannot connect latent concepts across isolated documents. Complex multi-document synthesis requires brute-force Map-Reduce loops.
  </div>
  <div class="rounded-md border border-emerald-200 bg-emerald-50 text-emerald-900 p-3">
    <b>The GraphRAG Solution:</b> Explicit relational reasoning.<br>
    We ground entities as graph <b>Seeds</b> and follow semantic <b>Hops</b> (e.g., <code>Paper A &rarr; [Gene] &larr; Paper B</code>). This deterministically bridges isolated texts, enabling true discovery without guessing.
  </div>
</div>

---
layout: default
---

# One Unified Graph, Two Query Modes
<div class="kg-rag-slide mt-3 grid grid-cols-2 gap-4 items-start">
  <div class="text-[0.82rem] leading-snug rounded-md border border-cyan-200 bg-cyan-50 text-cyan-900 p-3">
    <div class="font-semibold mb-2">Mode A: Semantic Concept Hopping (GraphRAG)</div>
    <p><strong>Query:</strong> "How does the pathway described by Helm connect to the clinical outcomes in Dieterich's recent trial?"</p>
    <ul class="list-disc pl-5 mt-1 space-y-1">
      <li><b>The Graph Hop:</b> The LLM extracts entities: `Helm`, `Dieterich`.</li>
      <li><b>Traversal Path:</b> `[Author: Helm] -[:WROTE]-> [Paper A] -[:MENTIONS_GENE]-> [Gene: MettL1] <-[:TARGETS_GENE]- [Paper B] <-[:WROTE]- [Author: Dieterich]`</li>
      <li><strong>Output:</strong> The system retrieves the exact chunks where the two papers intersect via the shared semantic concept (MettL1), creating new knowledge across isolated documents.</li>
    </ul>
  </div>

  <div class="text-[0.82rem] leading-snug rounded-md border border-indigo-200 bg-indigo-50 text-indigo-900 p-3">
    <div class="font-semibold mb-2">Mode B: Strict Metadata Lookup (Pure Graph Traversal)</div>
    <p><strong>Query:</strong> "Which papers did Mark Helm publish in 2025?"</p>
    <ul class="list-disc pl-5 mt-1 space-y-1">
      <li><b>Direct Execution:</b> No semantic text retrieval needed at all.</li>
      <li><b>Deterministic Filters:</b> Translate query directly into a Cypher graph query.</li>
      <li><b>The Hop Path:</b><span class="query-code">MATCH (a:Author {name: 'Helm'})-[:WROTE]-&gt;(p:Paper {year: 2025}) RETURN p.title</span></li>
      <li><strong>Output:</strong> 100% accurate, hallucination-free list, bypassing any Top-K vector limits or complex Map-Reduce loops.</li>
    </ul>
  </div>
</div>

<div class="mt-3 rounded-md border border-emerald-200 bg-emerald-50 text-emerald-900 p-3 text-[0.8rem] leading-snug">
  <div class="font-semibold mb-1">The True Power of GraphRAG</div>
  <div>While Dify's Vector-RAG can handle simple metadata filters (e.g., `WHERE author="Helm"`), it cannot discover <b>latent relationships</b> across isolated documents. GraphRAG connects texts via shared semantic nodes (Genes, Diseases, Methods), enabling true cross-document reasoning.</div>
</div>


---
layout: default
---

# Lessons Learned & What's Next

<div class="grid grid-cols-2 gap-4 mt-3">
  <div class="rounded-md border border-sky-200 bg-sky-50 text-sky-900 p-4">
    <div class="font-bold mb-2">What Worked</div>
    <ul class="text-sm leading-snug">
      <li>Parsing quality first: bad extraction breaks everything downstream.</li>
      <li>Hard metadata constraints: better precision, fewer hallucinations.</li>
      <li>Structured orchestration: rewrite, route, retrieve, then synthesize.</li>
    </ul>
  </div>
  <div class="rounded-md border border-emerald-200 bg-emerald-50 text-emerald-900 p-4">
    <div class="font-bold mb-2">Where We Go Next</div>
    <ul class="text-sm leading-snug">
      <li>Current map-reduce is robust, but expensive at scale.</li>
      <li>Next step: GraphRAG with Neo4j for deterministic multi-hop reasoning.</li>
      <li>Target: production bridge to NUM-ENRICH (CardioGuidelinesGraph).</li>
    </ul>
  </div>
</div>

<div class="mt-4 text-center text-lg font-semibold text-slate-100">
From retrieval heuristics to knowledge-aware reasoning.
</div>

---
layout: center
class: text-center
---

# Thank You

Questions?

<div class="mt-3 text-base">
  Live demo:
  <a href="https://rmap-chatbot-demo-dify.internal" target="_blank" rel="noopener noreferrer" class="font-semibold text-cyan-300 underline decoration-2 underline-offset-4 hover:text-cyan-200">
    rmap-chatbot-demo-dify.internal
  </a>
</div>

