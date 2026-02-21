# Hippocampus
**Conceptual cognitive architecture for persistent, evolving AI memory.**

**Author:** Aliya Boisselle

**Status:** Architecture & Interface Design In progress

**License:** Apache 2.0

---

Hippocampus explores memory architectures inspired by biological systems, with the goal of enabling persistent, evolving cognitive state in LLM applications.

---

## Overview
Foundation LLMs are inherently stateless, and persistent memory must be implemented at the system architecture level. Every conversation begins at zero. No accumulated relationship, no persistent perspective, no through-line connecting who you were yesterday to who you are today. This is not an intelligence problem, the models are capable. It is an architecture problem: there is no mechanism for continuity.
**Hippocampus** is a neuro-inspired memory architecture for LLM systems, mapped to biological structures and designed for practical implementation. It introduces persistent memory as a foundational system layer rather than a bolt-on retrieval mechanism enabling models to accumulate experience, maintain coherent identity over time, and evolve behavior based on prior context.
The central hypothesis: A slightly less capable model that genuinely remembers you will outperform a more capable model that treats every conversation as a first meeting.

---

## What Makes This Different
Most memory approaches for LLMs fall into two categories: **simple note-taking** (storing explicit facts in a profile — brittle, doesn't capture how someone thinks) or **long-context windows** (expensive, treats all context as equally weighted, doesn't scale across sessions).

Hippocampus takes a different approach, inspired by principles observed in biological memory:

- **Reconsolidation, not archiving.** Every memory retrieval triggers re-summarization in current context, producing denser and more relevant memories over time. Memory quality improves with use rather than simply being preserved.

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
| User Memory Store | Hippocampus | Per-user episodic memory as text summaries. Reconsolidation on every recall. |
| Merzenich | Cortical remapping | Novelty weighting at encoding. Dual baseline: LLM (human generally) + personal hippocampus (this user). |
| Two-Pass Retrieval | Frontal lobe | Pre-reasoning retrieval + post-reasoning re-rank |
| Kant | Reflective judgment | Temperature-scaled routing. Weight vector output, not binary. |
| Socrates *(Required)* | Elenctic method | Two-mode cognitive guardrail + hard memory refusal |
| Agents / Output | Motor cortex | Reflexive and deliberate action. Feeds back into reconsolidation cycle. |
| Interoperability | Foundation | User-owned, portable memory. Cryptographic integrity on export/import. |

---

## Memory Lifecycle

Memory exists in five explicit states with defined transitions:
```
Raw Input → Encoded Axon → Active Memory → Reconsolidation → Pruned
```

- **Encoding** is triggered at session end or significance threshold. Kant and Merzenich modulate initial axon weight before the write completes. Memory refusal blocks the write entirely if an ethical imperative threshold is met.
- **Reconsolidation** runs on every retrieval in v1. Dual drift verification (embedding similarity + low-temperature LLM comparison) catches both surface and semantic drift. These methods fail in complementary ways — requiring both to agree gives substantially stronger fidelity signal than either alone.
- **Decay occurs over time.** Users control the deprecation rate, not specific content. Content-selective deprecation would enable intentional removal of inconvenient memories.
- **High-Merzenich axons** decay more slowly regardless of deprecation settings. Significance is partially protected from the passage of time.

---

## Project Status
This repository currently contains in progress architecture and interface design documentation. It does not yet include a full implementation.

| Document | Status |
|---|---|
| Architecture Reference (v4.1) | 🔄 In development |
| Component Interface Specification (v1.0) | 🔄 In development |
| Repository Structure & Setup Guide | 🔄 In development |
| V1 Prototype Implementation | 🔄 In development |
| V2 Components (Kant, Merzenich, Perception Layer) | 📋 Specified, not yet implemented |

The V1 prototype will target three core components: the Memory Store (Hippocampus), the Reconsolidation Engine, and Socrates Mode 1. These components represent the core experimental mechanisms of the architecture and the safety guardrail required for responsible deployment.

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
│   ├── architecture_v1.docx         # Full architecture reference
│   ├── interfaces_v1.docx           # Component interface specification
│   ├── repository_guide.docx        # Setup and contribution guide
│   ├── CHANGELOG.md
│   └── decisions/                   # Architecture Decision Records
│
├── src/
│   ├── memory_store.py              # Hippocampus — V1 core
│   ├── reconsolidation.py           # Reconsolidation Engine — V1 core
│   ├── refusal_gate.py              # Memory Refusal Gate — V1 core
│   ├── retrieval.py                 # Retrieval Engine — V1 Pass 1
│   ├── socrates.py                  # Socrates Mode 1 — V1 core
│   ├── session_manager.py           # Session orchestration
│   ├── llm_connector.py             # Anthropic API wrapper
│   └── stubs/                       # V2 interface stubs
│
├── config/
│   ├── default.json
│   └── refusal_rules.json           # Operator-configurable refusal layer
│
└── tests/
```
---

## Key Design Decisions

A few non-obvious choices are documented explicitly:

**Text summaries over vector embeddings** — Embeddings are model-specific. When the underlying LLM is updated, embeddings from the old model may not map cleanly to the new model's semantic space, corrupting years of accumulated memory. Text summaries are model-agnostic, re-embedded at retrieval time by whatever model is current, and readable by humans. This also allows for interoperability, keeping ahead of likely future policy for user memory. Portability and upgrade resilience in a single decision.

**Local embeddings (sentence-transformers) for prototype** — Free, runs entirely offline after first model download (~90MB), and keeps memory data local. No API calls, no cost per embedding, no third-party dependency for a core privacy-sensitive operation.

**Every retrieval triggers reconsolidation in V1** — Simpler, predictable, and testable for a single-user prototype. A production system would trigger only when retrieval context diverges significantly from last-verified context. This tradeoff is documented explicitly in the architecture.

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
| `docs/architecture_v1.docx` | Full architecture reference. Biological foundation, twelve-layer system, memory lifecycle, refusal framework, open problems. |
| `docs/interfaces_v1.docx` | Component interface specification. Method signatures, types, configuration. The bridge between architecture and code. |
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
