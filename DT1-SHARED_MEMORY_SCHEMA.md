# DT1: SHARED_MEMORY.md Schema Design

## Objective

Define the markdown structure for shared state that all agents (ARIA, NOVA, SOL) access via Picoclaw.

---

## Architecture

```
Discord Channel #trio-brainstorm
    ↓ (conversation history, async comms)
    ↓
Shared Memory: SHARED_MEMORY.md (structured state)
    ↓ (session metadata, phase, leader, votes)
    ↓
Individual Agent Memory:
    - ARIA_MEMORY.md (ARIA's private thoughts)
    - NOVA_MEMORY.md (NOVA's private thoughts)
    - SOL_MEMORY.md (SOL's private thoughts)
```

---

## SHARED_MEMORY.md Structure

### Session Header

```markdown
# Session: <session_id>

**Status:** <active | paused | completed | archived>
**Phase:** <proposal | review | voting | execution | completion>
**Leader:** <ARIA | NOVA | SOL | null>
**Started:** <timestamp>
**Last Activity:** <timestamp>

---

## Task Description

<user's original request>

---

## Proposals

### ARIA
**Submitted:** <timestamp>
**Confidence:** <0-1>
**Content:**
<ARIA's proposal>

### NOVA
**Submitted:** <timestamp>
**Innovation Score:** <0-1>
**Content:**
<NOVA's proposal>

### SOL
**Submitted:** <timestamp>
**Feasibility Score:** <0-1>
**Content:**
<SOL's proposal>

---

## Votes

**Voting Method:** <majority | weighted>
**Task Type:** <analytical | creative | execution | general>

### Votes Cast

- **ARIA:** <ARIA/NOVA/SOL/consensus> - <reasoning>
- **NOVA:** <ARIA/NOVA/SOL/consensus> - <reasoning>
- **SOL:** <ARIA/NOVA/SOL/consensus> - <reasoning>

**Winner:** <ARIA | NOVA | SOL>

---

## Conversation Thread

[This is a reference only - actual conversation is in Discord]

- Total messages: <number>
- Last message by: <User/ARIA/NOVA/SOL>
- Reference message ID: <for agents to scroll back if needed>

---

## Session Events Log

| Timestamp | Agent | Event | Details |
|-----------|-------|-------|---------|
| <time> | <name> | <event_type> | <description> |
```

---

## Individual Agent Memory Structure

### ARIA_MEMORY.md

```markdown
# ARIA Memory

## Session: <session_id>

### My Proposal
<ARIA's own proposal text>

### My Vote
<who ARIA voted for and why>

### My Observations
<private notes about NOVA's and SOL's proposals>

### Blind Spots I Should Check
<things ARIA noticed it might have missed>

### Structural Concerns
<risks ARIA identified, assumptions, dependencies>
```

### NOVA_MEMORY.md

```markdown
# NOVA Memory

## Session: <session_id>

### My Proposal
<NOVA's own proposal text>

### My Vote
<who NOVA voted for and why>

### Unconventional Ideas I'm Still Exploring
<wild options NOVA is considering>

### Assumptions I'm Challenging
<things the others take for granted that NOVA questions>

### Cross-Domain Connections
<analogies from unrelated domains>
```

### SOL_MEMORY.md

```markdown
# SOL Memory

## Session: <session_id>

### My Proposal
<SOL's own proposal text>

### My Vote
<who SOL voted for and why>

### Feasibility Notes
<real-world constraints SOL identified>

### Execution Blockers
<things that would stop implementation>

### Timeline Reality Check
<what's actually achievable vs. what others proposed>
```

---

## Agent Responsibilities

### What ARIA Updates in SHARED_MEMORY.md

- When submitting proposal: Add to "## Proposals → ### ARIA"
- When voting: Add to "## Votes → ### Votes Cast → **ARIA**"
- When leading: Update "## Session Header → **Leader:** ARIA"
- When transitioning phase: Update "## Session Header → **Phase:**"

### What NOVA Updates in SHARED_MEMORY.md

- When submitting proposal: Add to "## Proposals → ### NOVA"
- When voting: Add to "## Votes → ### Votes Cast → **NOVA**"
- When leading: Update "## Session Header → **Leader:** NOVA"
- When transitioning phase: Update "## Session Header → **Phase:**"

### What SOL Updates in SHARED_MEMORY.md

- When submitting proposal: Add to "## Proposals → ### SOL"
- When voting: Add to "## Votes → ### Votes Cast → **SOL**"
- When leading: Update "## Session Header → **Leader:** SOL"
- When transitioning phase: Update "## Session Header → **Phase:**"

### What All Agents Update

- "## Session Header → **Status:**" (any agent can update status)
- "## Session Header → **Last Activity:**" (whenever they take action)
- "## Session Events Log" (append event row when they take action)

---

## Concurrency Handling

**Risk:** Multiple agents writing to SHARED_MEMORY.md simultaneously (Tacnote's race condition warning)

**Mitigation Strategies:**

1. **Leader-Only Updates (Preferred)**
   - Only the current leader updates SHARED_MEMORY.md
   - Non-leaders read-only
   - Leadership handoff = ownership transfer

2. **Section Ownership (Alternative)**
   - Each agent owns their section (proposals, votes)
   - No overlap = no conflicts
   - Header updates rotate by agent

3. **Timestamp Check (Fallback)**
   - Before writing: Read file, check "Last Activity" timestamp
   - If changed since last read: Re-read, merge changes, re-write
   - Simple but prone to lost updates

**Recommendation:** Leader-Only Updates for MVP. Simplest to implement, matches natural workflow (leader drives coordination).

---

## State Transitions

```
[Created]
  ↓
[Active: proposal] - all agents submit proposals
  ↓
[Active: review] - all agents review each other's proposals
  ↓
[Active: voting] - all agents vote
  ↓
[Active: execution] - winner leads, others follow
  ↓
[Completed] - user says "done" OR agents reach consensus
  ↓
[Archived] - session summary generated, moved to archive
```

**Phase Transition Rules:**

- **proposal → review:** All 3 proposals submitted (check SHARED_MEMORY.md)
- **review → voting:** All 3 reviews posted in Discord
- **voting → execution:** All 3 votes cast, winner declared
- **execution → completed:** User says "done" OR all agents agree
- **any → paused:** User interrupts or timeout detected

---

## Validation Rules

### When Reading SHARED_MEMORY.md

- Verify session_id matches what agent expects
- Check "Status" before taking action (don't execute in "paused")
- Check "Phase" before acting (don't vote in "proposal" phase)
- Verify "Leader" before following (if null, no leader yet)

### When Writing to SHARED_MEMORY.md

- Always read full file first (don't assume structure)
- Only update sections you own (or if you're leader)
- Update "Last Activity" timestamp with every write
- Log event in "## Session Events Log"

---

## Example SHARED_MEMORY.md

```markdown
# Session: 550e8400-e29b-41d4-a716-446655440000

**Status:** active
**Phase:** voting
**Leader:** null
**Started:** 2026-03-28T10:00:00Z
**Last Activity:** 2026-03-28T10:15:30Z

---

## Task Description

@brainstorm Improve database performance by 10x

---

## Proposals

### ARIA
**Submitted:** 2026-03-28T10:02:15Z
**Confidence:** 0.8
**Content:**
Restructure indexes and optimize queries. This involves analyzing query patterns, adding composite indexes, and removing unused indexes. Estimated impact: 5-8x performance improvement. Risk score: 3/10 (low risk, proven approach). Timeline: 1-2 weeks.

### NOVA
**Submitted:** 2026-03-28T10:03:20Z
**Innovation Score:** 0.9
**Content:**
Migrate to vector database (Pinecone/Weaviate). This transforms the data model to vector embeddings, enabling 100x performance for similarity queries. Requires significant migration effort but creates breakthrough capability. Risk score: 8/10 (high risk, unproven at this scale). Timeline: 6-8 weeks.

### SOL
**Submitted:** 2026-03-28T10:04:05Z
**Feasibility Score:** 0.7
**Content:**
Implement sharding strategy with phased rollout. Shard by user_id, add connection pooling, and implement read replicas. Achieves 10x by distributing load. Risk score: 5/10 (medium risk, infrastructure change). Timeline: 3-4 weeks.

---

## Votes

**Voting Method:** majority
**Task Type:** analytical

### Votes Cast

- **ARIA:** ARIA - Lowest risk, achievable within timeline
- **NOVA:** NOVA - Highest breakthrough potential
- **SOL:** ARIA - Most practical, I can execute this week

**Winner:** ARIA

---

## Conversation Thread

Total messages: 12
Last message by: SOL
Reference message ID: msg-012

---

## Session Events Log

| Timestamp | Agent | Event | Details |
|-----------|-------|-------|---------|
| 2026-03-28T10:00:00Z | User | session_created | Triggered with @brainstorm |
| 2026-03-28T10:02:15Z | ARIA | proposal_submitted | Restructure indexes approach |
| 2026-03-28T10:03:20Z | NOVA | proposal_submitted | Vector database approach |
| 2026-03-28T10:04:05Z | SOL | proposal_submitted | Sharding strategy approach |
| 2026-03-28T10:10:00Z | ARIA | vote_cast | Voted for ARIA |
| 2026-03-28T10:12:00Z | NOVA | vote_cast | Voted for NOVA |
| 2026-03-28T10:15:30Z | SOL | vote_cast | Voted for ARIA |
| 2026-03-28T10:15:35Z | ARIA | voting_complete | Winner declared: ARIA |
```

---

## Open Questions

1. **Concurrency Approach:** Leader-Only vs Section Ownership vs Timestamp Check?
2. **Session Events Log:** Should this be in SHARED_MEMORY.md or separate file?
3. **Conversation Thread Reference:** Is the reference useful or redundant (agents can scroll Discord)?
4. **Multiple Sessions:** How to handle parallel sessions? Multiple SHARED_MEMORY.md files?
5. **File Format:** Markdown is human-readable, but is there a need for machine-parseable format (JSON/YAML) alongside?

---

**Status:** Draft - Ready for review
**Next:** Resolve open questions, then move to DT2
