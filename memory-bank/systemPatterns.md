# System Patterns: Veritarium Architecture

## Core Architecture: Two Pipelines, One UI

### Pipeline A — Evidence-Anchored RAG (Reasoned Synthesis)

```
┌─────────┐    ┌──────────────┐    ┌─────────────────┐    ┌────────┐    ┌─────────────────┐    ┌──────┐
│   UI    │ -> │ Orchestrator │ -> │ Hybrid Retrieval│ -> │ Rerank │ -> │ Context Builder │ -> │ vLLM │
│(Kotaemon)│    │              │    │(semantic+lexical)│    │        │    │                 │    │      │
└─────────┘    └──────────────┘    └─────────────────┘    └────────┘    └─────────────────┘    └──────┘
                                                                                                   │
                                                                                                   v
                                                                                        ┌──────────────────┐
                                                                                        │ Answer + Citations│
                                                                                        └──────────────────┘
```

**Purpose**: High-quality answers to complex questions, with citations and links back to evidence anchors.

### Pipeline B — Deterministic Discovery & Highlighting (Guaranteed)

```
┌─────────────────┐    ┌────────────────────────┐    ┌─────────────────────────┐    ┌──────────────────┐
│ Query ("Mond")  │ -> │ Deterministic Finder   │ -> │ Complete Hit List       │ -> │ PDF Viewer       │
│                 │    │ (index over page text) │    │ (doc, page, position)   │    │ Highlights       │
└─────────────────┘    └────────────────────────┘    └─────────────────────────┘    └──────────────────┘
```

**Purpose**: Exact, complete match discovery (including substrings/compounds), with reliable "show me where it is" behavior.

---

## Evidence Synchronization Rule

The critical integration between pipelines:

1. **UI displays Pipeline B results immediately** (fast perceived responsiveness)
2. **LLM receives evidence before generation**: Orchestrator injects structured evidence summary from Pipeline B into Pipeline A context
3. **Retrieval bias**: Pipeline A may prioritize documents/pages already confirmed by Pipeline B
4. **Evidence Contract**: Every LLM claim must reference an anchor (enforced by orchestrator)

```
                    ┌─────────────────────────────────────────┐
                    │            ORCHESTRATOR                  │
                    │                                          │
   User Query ─────>│  ┌─────────────┐    ┌───────────────┐  │
                    │  │ Pipeline B  │───>│ Evidence      │  │
                    │  │ (fast)      │    │ Summary       │  │
                    │  └─────────────┘    └───────┬───────┘  │
                    │                             │          │
                    │  ┌─────────────┐            │          │
                    │  │ Pipeline A  │<───────────┘          │
                    │  │ (LLM)       │    (injected context) │
                    │  └─────────────┘                       │
                    └─────────────────────────────────────────┘
```

---

## Component Architecture

### Services

| Service | Language | Purpose | Container Base |
|---------|----------|---------|----------------|
| **Orchestrator** | Python (FastAPI) | Request routing, evidence coordination, streaming | Python 3.12 slim |
| **Finder** | Rust | Deterministic text search, position indexing | Rust alpine |
| **Ingestion** | Python | Document processing, canonicalization | Python 3.12 + poppler |
| **vLLM** | Python | LLM serving (OpenAI-compatible API) | NGC vLLM ARM64 |
| **Qdrant** | Rust | Vector store for dense embeddings | Official Qdrant |
| **Kotaemon** | Python | End-user UI (document-centric) | Custom build |

### Storage

| Store | Purpose | Backup Strategy |
|-------|---------|-----------------|
| **Qdrant** | Dense embeddings + metadata | Snapshots |
| **Document Store** | Canonical PDFs | File system backup |
| **Text Store** | Per-page extracted text | File system backup |
| **Index Store** | Deterministic finder index | File system backup |

---

## Deployment Patterns

### Compose Profiles

```yaml
# compose.yaml (base)
services:
  orchestrator:
    ...
  finder:
    ...
  qdrant:
    ...

# compose.dev.yaml (development)
services:
  orchestrator:
    build: ./containers/orchestrator
    volumes:
      - ./services/orchestrator/src:/app/src  # hot reload

# compose.prod.yaml (production)
services:
  orchestrator:
    image: veritarium/orchestrator:${VERSION}
    deploy:
      resources:
        limits:
          memory: 8G
```

### Network Topology

```
                    ┌─────────────────────────────────────┐
                    │           DGX Spark                  │
                    │                                      │
    Internet ──────>│  ┌────────────┐                     │
        :443        │  │ Reverse    │                     │
                    │  │ Proxy      │                     │
                    │  │ (TLS+Auth) │                     │
                    │  └─────┬──────┘                     │
                    │        │                            │
                    │  ┌─────v──────────────────────────┐ │
                    │  │      Internal Network          │ │
                    │  │  ┌────────┐  ┌────────┐       │ │
                    │  │  │ Orch.  │  │ Finder │       │ │
                    │  │  └────────┘  └────────┘       │ │
                    │  │  ┌────────┐  ┌────────┐       │ │
                    │  │  │ vLLM   │  │ Qdrant │       │ │
                    │  │  └────────┘  └────────┘       │ │
                    │  └────────────────────────────────┘ │
                    │                                      │
                    │  Default-deny egress                 │
                    └─────────────────────────────────────┘
```

---

## Development Patterns

### Cline Workflow Integration

- **Plan Mode**: Architecture decisions, multi-service coordination
- **Act Mode**: Single service at a time, quality gate after each step
- **Quality Gate**: Must pass for all services before commit

### Service Development

```bash
# Work on one service at a time
cd services/orchestrator
uv sync
uv run pytest

# Integration testing
docker compose -f compose/compose.yaml -f compose/compose.dev.yaml up
./scripts/smoke-test.sh
```

### 12 Workstreams

1. Workbench repo skeleton + pinned Compose profiles + reset scripts
2. Observability baseline (logs/metrics)
3. vLLM pinned + model cache + load/perf smoke tests
4. Qdrant schema + auth + snapshot automation
5. Canonicalization pipeline + docstore/textstore
6. Deterministic index build + Find-Lite API (fast)
7. Find-Full positions/anchors + viewer highlight integration
8. Dense embeddings pipeline (ARM64-safe)
9. Lexical + dense fusion (retriever aggregator)
10. Rerank + context builder + evidence enforcement
11. Kotaemon integration + Evidence Board + deep-links
12. Security hardening + offline bundle + restore runbook

---

*Update this file when architecture patterns change.*
