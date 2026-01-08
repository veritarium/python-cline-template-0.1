# Project Brief: Veritarium

## Vision

Veritarium is an **offline on-prem AI appliance** running on **DGX Spark** (GB10 / ARM64 / unified memory) that combines **reasoned RAG answers** with **deterministic, fully verifiable document evidence**.

## Core Principle: Evidence-First

Users must be able to verify claims directly in the source document, not by trusting the model. Every answer links back to exact, highlightable evidence in the original documents.

## Two Kinds of Truth

1. **Reasoned Answers** (probabilistic, but evidence-anchored)
   - High-quality answers to complex questions
   - Citations and links back to evidence anchors
   - LLM-generated with retrieval context

2. **Deterministic Evidence** (exact, complete, verifiable)
   - Exact word/substring discovery
   - Visual highlighting in PDF viewer
   - Complete hit lists (no missing results)
   - Position-accurate (document, page, character offset)

## Hard Constraints

- **Offline operation**: After mirroring container images and model weights, no external APIs
- **No NVIDIA NIM dependency**: vLLM as the serving layer
- **Single-box deployment**: DGX Spark is the target appliance
- **Reproducible builds**: Pinned versions, frozen releases, restore-tested backups
- **Security posture**: Single entry point (443), internal-only backends, default-deny egress

## Target Platform

- **Hardware**: NVIDIA DGX Spark
- **Architecture**: ARM64 (aarch64) - Grace Blackwell GB10
- **Memory**: Unified memory (CPU/GPU shared)
- **OS**: Ubuntu Linux
- **GPU**: NVIDIA Blackwell

## Development Environment

- **NVIDIA AI Workbench**: Development capsule (reproducible, deletable, re-creatable)
- **Cline in VS Code**: Engineering accelerant for planning, implementing, testing
- **Not runtime dependencies**: Development tools don't run in production

---

*This brief captures the "why" and constraints. See systemPatterns.md for architecture.*
- Must maintain quality gate green before commits
