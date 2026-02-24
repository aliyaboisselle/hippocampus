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
The central hypothesis: A slightly less capable model that genuinely remembers you will outperform a more capable model that treats every conversation as a first meeting.

---

## Project Status
This repository currently contains in progress architecture and interface design documentation. It does not yet include a full implementation.

| Document | Status |
|---|---|
| Architecture Reference (v1.1) | ✅ Complete |
| Component Interface Specification (v1.1) | 🔄 In development |
| Repository Structure & Setup Guide | 🔄 In development |


The V1 prototype will target three core components: the Memory Store (Hippocampus), the Reconsolidation Engine, and Socrates Mode 1. These components represent the core experimental mechanisms of the architecture and the safety guardrail required for responsible deployment.

---

## Documentation
| Document | Description |
|---|---|
| `docs/architecture_v1.docx` | Full architecture reference. Biological foundation, twelve-layer system, memory lifecycle, refusal framework, open problems. |
| `docs/interfaces_v1.docx` | Component interface specification. Method signatures, types, configuration. The bridge between architecture and code. |
| `docs/repository_guide.docx` | Setup instructions, repository structure, contributing guide, ADR templates, end-to-end walkthrough. |


*February 2026 · v1*
*Licensed under Apache 2.0. This work may be freely shared, cited, and extended.*
