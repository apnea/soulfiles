# Parallel Collaboration Implementation Plan

## Overview

Move ARIA-NOVA-SOL from sequential collaboration (problem → ARIA → NOVA → SOL) to parallel collaboration where all three agents work simultaneously in shared Discord sessions.

---

## Phase 1: Design Tasks (Resolve Unknowns)

- **DT1:** SESSION_STATE schema design
- **DT2:** Collaboration protocol step-by-step
- **DT3:** Discord channel structure and roles
- **DT4:** Voting mechanism
- **DT5:** Completion criteria for sessions

---

## Phase 2: Engineering Tasks (Documentation Updates)

- **ET1:** Update ARIA_SOUL.md with parallel collaboration protocols
- **ET2:** Update NOVA_SOUL.md with parallel collaboration protocols
- **ET3:** Update SOL_SOUL.md with parallel collaboration protocols
- **ET4:** Update TRIO_README.md with parallel workflow
- **ET5:** Create PARALLEL_PROTOCOL.md (orchestration rules)

---

## Phase 3: Implementation Tasks (Discord + Picoclaw Setup)

- **IT1:** Create Discord server structure
- **IT2:** Configure 3 Picoclaw instances with SOUL files
- **IT3:** Test session initiation and proposal-voting-lead flow
- **IT4:** Test session lifecycle (start, collaborate, complete, archive)

---

## Priority

Phase 1 → Phase 2 → Phase 3 (sequential, each phase builds on previous)

---

**Status:** Ready to iterate on Phase 1, DT1
