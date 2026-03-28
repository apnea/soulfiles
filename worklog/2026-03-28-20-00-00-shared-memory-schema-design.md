### 2026-03-28: Shared Memory Schema Design

**2026-03-28: DT1 - SHARED_MEMORY.md Schema Design**

**Context:**
We're implementing parallel collaboration for the ARIA-NOVA-SOL triad using Discord + Picoclaw. Each agent runs in its own Picoclaw Docker container with its own LLM and context.

**Architecture Decisions Made:**

1. **Shared Memory Pattern (from OpenClaw/Tacnode research):**
   - Discord channel = async communication, conversation history (message queue)
   - SHARED_MEMORY.md = structured state (session metadata, phase, leader, proposals, votes)
   - Individual agent memory files = private thoughts, observations, concerns
   - This avoids race conditions by having single place for coordinated state

2. **Why SHARED_MEMORY.md is necessary:**
   - Discord conversation is messy (hard to parse programmatically)
   - Agents need structured state they can quickly read (phase, leader, who voted)
   - Avoids agents needing to parse entire Discord thread to know where they are

**Files Created Today:**

1. **PLAN.md**
   - High-level implementation plan (Phases 1-3)
   - Phase 1: Design tasks (DT1-DT5)
   - Phase 2: Engineering tasks (ET1-ET5)
   - Phase 3: Implementation tasks (IT1-IT4)

2. **DT1-SHARED_MEMORY_SCHEMA.md**
   - Complete schema for SHARED_MEMORY.md
   - Schema for individual agent memory files (ARIA_MEMORY.md, NOVA_MEMORY.md, SOL_MEMORY.md)
   - Agent responsibilities (who updates what sections)
   - Concurrency handling strategies
   - State transitions
   - Example SHARED_MEMORY.md

**Key Design Decisions in DT1:**

**SHARED_MEMORY.md Structure:**
- Session Header: status, phase, leader, timestamps
- Task Description: user's original request
- Proposals: section for each agent (ARIA, NOVA, SOL) with scores
- Votes: voting method, task type, votes cast, winner
- Conversation Thread: reference only (actual conversation in Discord)
- Session Events Log: chronological event tracking

**Individual Agent Memory Structure:**
- Each agent has their own memory file for private thoughts
- ARIA: observations, blind spots, structural concerns
- NOVA: unconventional ideas, assumptions challenging, cross-domain connections
- SOL: feasibility notes, execution blockers, timeline reality

**Concurrency Handling (Open Question):**
- Option A: Leader-Only Updates (recommended for MVP) - simplest, matches natural workflow
- Option B: Section Ownership - each agent owns their proposal/vote sections
- Option C: Timestamp Check - read before write, merge changes

**State Transitions:**
Created → Active (proposal → review → voting → execution) → Completed → Archived
Any active phase can → Paused (user interruption or timeout)

**Open Questions to Resolve Tomorrow:**

1. **Concurrency Approach:** Leader-Only (simplest) vs Section Ownership (more parallel) vs Timestamp Check (fallback)?
2. **Multiple Sessions:** How to handle parallel sessions? One SHARED_MEMORY.md per session, or single file with sessions as sections?
3. **Conversation Thread Reference:** Is reference useful in SHARED_MEMORY.md or redundant (agents can scroll Discord)?
4. **File Format:** Markdown is human-readable, but is there a need for machine-parseable format (JSON/YAML) alongside?

**Current Status:**
- DT1 (SHARED_MEMORY.md schema) is drafted but not finalized
- Need to resolve open questions before moving to DT2
- DT2: Collaboration protocol step-by-step (next after DT1 finalization)

**Next Steps (Tomorrow):**
1. Resolve DT1 open questions (concurrency, multiple sessions, conversation reference)
2. Finalize DT1-SHARED_MEMORY_SCHEMA.md
3. Move to DT2: Collaboration protocol step-by-step
4. Continue through remaining design tasks (DT3-DT5)
5. Then move to Phase 2 engineering tasks (ET1-ET5)

**Key Context for Tomorrow:**
- Each agent runs in separate Picoclaw Docker container
- Each agent has its own LLM with all accoutrements (independent context)
- Discord channel = async message queue
- SHARED_MEMORY.md = shared structured state
- Individual memory files = private agent thoughts
- Picoclaw agents can read/write shared memory file
- We're NOT using frameworks like CrewAI/AutoGen - custom implementation informed by Tacnode/OpenClaw patterns

**Reference Files:**
- PLAN.md - high-level plan
- DT1-SHARED_MEMORY_SCHEMA.md - detailed schema design (draft)
- worklog/2026-03-27-16-00-00-domain-specific-research.md - Tacnode/OpenClaw patterns
- SOUL_CORE.md - core decision-making principles
- ARIA_SOUL.md, NOVA_SOUL.md, SOL_SOUL.md - agent personas
