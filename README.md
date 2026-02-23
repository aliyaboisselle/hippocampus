# Hippocampus
**Conceptual cognitive architecture for persistent, evolving AI memory.**

**Author:** Aliya Boisselle

**Status:** Architecture & Interface Design In progress

**License:** Apache 2.0

---

Hippocampus explores memory architectures inspired by biological systems, with the goal of enabling persistent, evolving cognitive state in LLM applications.

---

## Overview
Foundation LLMs maintain context within a single session but have no persistent memory across sessions. Every new conversation begins at zero. No accumulated relationship, no record of prior exchanges, no through-line connecting who you were last week to who you are today. This is not an intelligence problem. The models are capable. It is an architecture problem: there is no mechanism for continuity across time.
**Hippocampus** is a neuro-inspired memory architecture for LLM systems, mapped to biological structures and designed for practical implementation. It introduces persistent memory as a foundational system layer rather than a bolt-on retrieval mechanism enabling models to accumulate experience, maintain coherent identity over time, and evolve behavior based on prior context.

**The central hypothesis:** A slightly less capable model that remembers you will outperform a more capable model that treats every conversation as a first meeting.

---

## What Makes This Different
Most memory approaches for LLMs fall into two categories: **simple note-taking** (storing explicit facts in a profile which is brittle and doesn't capture how someone thinks) or **long-context windows** (expensive, treats all context as equally weighted, doesn't scale across sessions).

Hippocampus takes a different approach, inspired by principles observed in biological memory:

-**Episodic and semantic memory as distinct stores.** Raw experience (episodic) is append-only and immutable. The permanent ground truth of what happened. Derived beliefs (semantic) are mutable and updated by reconsolidation as new experience arrives. Every semantic memory carries a traceable link back to the episodes that produced it. This distinction is structural, not procedural: the data model enforces it, not a verification step that could be bypassed.

- **Reconsolidation, not archiving.** Every semantic memory retrieval triggers re-summarization in current context, producing denser and more relevant memories over time. Memory quality improves with use rather than simply being preserved.

- **Novelty-weighted encoding (Merzenich).** Memories are weighted by significance, not just frequency. A conversation that changes how you think about something gets elevated weight and paradigm shifts survive even without frequent retrieval. Routine noise does not persist simply by repetition.

- **Temperature-scaled judgment (Kant).** A dedicated adjudication layer routes between novelty encoding and contradiction surfacing, outputting a weight vector rather than a binary decision. Errors self-correct through reconsolidation rather than compounding permanently.

- **Two-mode cognitive guardrail (Socrates).** Mode 1 surfaces self-contradictions from the user's own prior statements across sessions. Mode 2 provides firm correction when a persistent belief conflicts with established knowledge AND harm potential is present and refuses memory encoding. Both modes are included as core architectural safeguards.
- 
- **Hard memory refusal.** The architecture has a defined set of immutable categories: harm, illegal activity, health advice, crisis signals, and others where memory encoding is blocked by design. A configurable operator layer sits above this floor but cannot weaken it.

- **Portable, model-agnostic storage.** Memories are stored as structured text summaries, not raw vector embeddings. This makes them portable across model upgrades, readable by humans, and improves handling of migration when the underlying model changes.

---

## Architecture at a Glance
| Layer | Biological Analog | Role |
|---|---|---|
| Sensory Layer | Sensory organs | Raw input entry point |
| Reflex Layer | Spinal reflex arc | Pattern-matched responses, no LLM invoked. Compounding efficiency over time. |
| Agentic Orchestration | Spinal cord | Tool calls, workflows, coordination |
| Perception Layer | Prefrontal monitoring | Memory/fact ratio auditing. Helps prevent confirmation drift. |
| Base LLM | Cerebellum | Frozen procedural knowledge. Factual anchor. |
| Episodic Store | Hippocampus — episodic | Per-user raw experience as text summaries. Append-only. Immutable after write. Ground truth. |
| Semantic Store | Hippocampus — semantic | Derived beliefs, updated by reconsolidation. Always traceable to source episodes. |
| Merzenich | Cortical remapping | Novelty weighting at encoding. Dual baseline: LLM (human generally) + personal hippocampus (this user). |
| Two-Pass Retrieval | Frontal lobe | Pre-reasoning retrieval + post-reasoning re-rank |
| Kant | Reflective judgment | Temperature-scaled routing. Weight vector output, not binary. |
| Socrates *(Required)* | Elenctic method | Two-mode cognitive guardrail + hard memory refusal |
| Agents / Output | Motor cortex | Reflexive and deliberate action. Feeds back into reconsolidation cycle. |
| Interoperability | Foundation | User-owned, portable memory. Cryptographic integrity on export/import. |

---

## Memory Lifecycle

Memory flows across two stores with defined transitions:

**Encoding** — At session end, the session is compressed into one or more `EpisodicMemory` records by the Session Manager via LLM summarization. Episodes are written to EpisodicStore. Memory Refusal Gate runs before every write and blocks the write entirely if a hardcoded floor category is matched. Kant and Merzenich modulate initial weight before the write completes.

**Reconsolidation** — Every semantic memory retrieval triggers reconsolidation in v1. The retrieved `SemanticMemory` is re-summarized in current context via LLM call. Dual drift verification runs: embedding similarity (surface drift) + low-temperature LLM comparison (semantic drift). These methods fail in complementary ways requiring both to agree gives substantially stronger fidelity signal than either alone. On passing verification, SemanticStore is updated. EpisodicStore is never touched during reconsolidation.

**Decay** — Semantic memory weight decays over time. Users control the deprecation rate, not specific content. Content-selective deprecation would enable intentional removal of inconvenient memories, turning the system into a confirmation bias engine. High-Merzenich semantic memories decay more slowly regardless of deprecation settings — significance is partially protected from the passage of time.

**Pruning** — Semantic memories whose weight falls below the pruning threshold are removed from the active store. Episodic memories are append-only and are not pruned; they may be archived.

---

## Project Status
This repository currently contains in progress architecture and interface design documentation. It does not yet include a full implementation.

| Document | Status |
|---|---|
| Architecture Reference (v1) | 🔄 In development |
| Component Interface Specification (v1.1) | 🔄 In development |
| Design Principles (v1.0) | 🔄 In development |
| Architecture Decision Records (ADR-001 – ADR-004) | 🔄 In development |
| Repository Structure & Setup Guide | 🔄 In development |
| V1 Prototype Implementation | 🔄 In development |
| V2 Components (Kant, Merzenich, Perception Layer) | 📋 Specified, not yet implemented |

The V1 prototype targets the core memory components: EpisodicStore, SemanticStore, Reconsolidation Engine, Memory Refusal Gate, Socrates, Session Manager, Retrieval Engine (Pass 1), and LLM Connector.

--- 

## Repository Structure

Repository in progress - not all files created or available yet. 

```
hippocampus/
├── README.md
├── LICENSE
├── .env.example
├── requirements.txt
│
├── docs/
│   ├── architecture_v1.docx          # Full architecture reference
│   ├── interfaces_v1.docx            # Component interface specification
│   ├── design_principles_v1.docx     # Non-negotiable constraints for contributors
│   ├── repository_guide.docx         # Setup and contribution guide
│   ├── CHANGELOG.md
│   └── decisions/                    # Architecture Decision Records
│       ├── ADR-001-text-summaries.md
│       ├── ADR-002-local-embeddings.md
│       ├── ADR-003-apache-license.md
│       └── ADR-004-episodic-semantic-split.md
│
├── src/
│   ├── episodic_store.py             # EpisodicStore — V1 core, append-only
│   ├── semantic_store.py             # SemanticStore — V1 core, derived beliefs
│   ├── reconsolidation.py            # Reconsolidation Engine — V1 core
│   ├── refusal_gate.py               # Memory Refusal Gate — V1 core
│   ├── retrieval.py                  # Retrieval Engine — V1 Pass 1
│   ├── socrates.py                   # Socrates Mode 1 — V1 core
│   ├── session_manager.py            # Session orchestration
│   ├── llm_connector.py              # Anthropic API wrapper
│   └── stubs/                        # V2 interface stubs
│
├── config/
│   ├── default.json
│   └── refusal_rules.json            # Operator-configurable refusal layer
│
└── tests/
```
---

## Key Design Decisions

A few non-obvious choices are documented explicitly:

**Episodic and semantic memory as separate stores** — The original design used a single mutable memory unit for both raw experience and derived belief. This was replaced with a structural split: EpisodicStore is append-only (no update or delete path exists), SemanticStore is mutable and always carries a `derived_from` list linking back to source episodes. The split enforces the integrity guarantee structurally rather than relying on the reconsolidation engine to maintain it procedurally. A bug in reconsolidation can no longer corrupt the historical record. See ADR-004.

**Text summaries over vector embeddings** — Embeddings are model-specific. When the underlying LLM is updated, embeddings from the old model may not map cleanly to the new model's semantic space, corrupting years of accumulated memory. Text summaries are model-agnostic, re-embedded at retrieval time by whatever model is current, and readable by humans. Portability and upgrade resilience in a single decision. See ADR-001.

**Local embeddings (sentence-transformers) for prototype** — Free, runs entirely offline after first model download (~90MB), and keeps memory data local. No API calls, no cost per embedding, no third-party dependency for a core privacy-sensitive operation. See ADR-002

**Every retrieval triggers reconsolidation in V1** — Simpler, predictable, and testable for a single-user prototype. A production system would trigger only when retrieval context diverges significantly from last-verified context. This tradeoff is documented explicitly in the architecture.

**User-controlled deprecation rate, not content** — Users can set how quickly their semantic memories decay but cannot selectively remove specific memories. Content-selective deprecation would allow a user to surgically remove inconvenient truths, turning the system into a confirmation bias engine. Rate control gives meaningful privacy agency without enabling that failure mode.

---

## Who This Is For

- AI engineers exploring persistent memory systems for LLM applications
- Developers building long-lived AI assistants or agents
- Researchers studying cognitive architectures and machine memory
- Technical architects designing AI system infrastructure
- Anyone interested in the question of what it would take for an AI to include evolving user memory

---

## Getting Started

To be completed once prototype is available.

---

## Contributing

- The highest-value contribution area is implementing V2 stubs — each has a fully specified interface in docs/interfaces_v1.docx. Open an issue to claim a component before starting work.
- The hardcoded safety floor in refusal_gate.py is intentionally immutable. Pull requests that weaken or remove hardcoded protections will not be merged.
- See docs/repository_guide.docx for the full contributing guide, issue labels, and code style expectations.

## Documentation
| Document | Description |
|---|---|
| `docs/architecture_v1.docx` | Full architecture reference. Biological foundation, twelve-layer system, memory lifecycle, open problems. |
| `docs/interfaces_v1.docx` | Component interface specification. Method signatures, types, configuration. The bridge between architecture and code. |
| `docs/design_principles_v1.docx` | Non-negotiable constraints for contributors. Read before implementing a component or opening a PR that touches core architecture. |
| `docs/decisions/` | Architecture Decision Records. Full reasoning behind each significant design choice. |
| `docs/repository_guide.docx` | Setup instructions, repository structure, contributing guide, ADR templates, end-to-end walkthrough. |

---

## Open Problems
This architecture is honest about what it does not yet know:

- **Kant's internal implementation** is specified conceptually but requires an engineering decision on how three input signals combine into a weight vector before V2 implementation.
- **Ethical imperative threshold categories** must be defined explicitly by humans before production deployment. The architecture specifies the structure; the categories require human judgment.
- **Reconsolidation at scale** — every retrieval triggering reconsolidation is correct for V1 but requires a confidence-based trigger mechanism for production use.
- **Affect proxy depth** — current engagement signals (response length, follow-up rate) cannot distinguish curiosity from anxiety from confusion. De-scoped to V3.

Full open problems documentation is in the architecture reference.

*February 2026 · v1*
*Licensed under Apache 2.0. This work may be freely shared, cited, and extended.*
