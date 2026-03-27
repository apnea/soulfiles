### 2026-03-27: 16-00-00-Domain-Specific-Research

**2026-03-27: Domain-Specific Research (Tacnode, PocketFlow, OpenClaw, Moltbook)**

520: 

User's directive: Focus on resources closer to our domain, not general actor model research.

522: 

**Resources Researched:**

1. Tacnode - Multi-Agent Coordination Guide

2. PocketFlow - 100-line LLM framework

3. OpenClaw - 24/7 personal AI assistant

4. Moltbook - Social network for AI agents

528: 

**1. Tacnode - Multi-Agent Coordination (Production-Grade)**

530: 

*Coordination Strategies:*

  - **Centralized**: Single coordinator (orchestrator) assigns tasks, tracks outputs, handles failures, assembles results. Simple but single point of failure, bottleneck.

  - **Market-based**: Agents bid for tasks based on capability/load. More scalable, adapts to agent load (busy = conservative bids, idle = aggressive).

  - **Voting systems**: Agents independently evaluate, vote (majority, supermajority, weighted). Reduces hallucination risk - disagreement signals uncertainty.

  - **Hierarchical**: Top-level orchestrators → mid-level → specialized workers. Natural decomposition structure.

536: 

*Communication Protocols:*

  - **Shared memory**: Fast, supports complex structured state, agents exchange partial results/workflow state. Risk: race conditions. Requires careful schema + transactional writes.

  - **Message passing**: Decouples agents via queues. More fault-tolerant (failed agent = messages stay in queue). Explicit interactions, auditable. Serialization overhead + latency.

  - **Direct A2A**: Orchestrator invokes specialist as tool call. Natural for tight collaboration, harder to scale to many agents.

  - **Production combines all three**: Shared memory (fast state), message queues (task distribution/fault tolerance), direct calls (tight A2A).

542: 

*Fault Tolerance (Production-Grade):*

  - **Assumption**: Agents WILL fail.

  - **Detection**: Timeout detection, heartbeat mechanisms (liveness signals).

  - **Retry & Reassignment**: Tasks must be idempotent. Transient = retry by same agent; persistent = reassign to another.

  - **Avoid single points**: Centralized = obvious SPoF. Distributed = replicate coordination state.

  - **Cascading failures**: Isolation via well-defined interfaces, output validation, error budgets.

  - **Production requirements**: At-least-once delivery, idempotent execution, explicit timeout/retry, health checks, circuit breakers.

550: 

*Decentralized Approaches:*

  - **Local information, local decisions**: Each agent decides based on local info + what it observes from immediate neighborhood.

  - **Emergent behaviors**: Agents given simple rules → collective behavior emerges. Can be productive (efficient routing) or problematic (coordination loops, deadlocks).

  - **Practical patterns**:

    - Gossip protocols: Share state updates with random neighbors (eventual consistency).

    - Stigmergy: Indirect communication via shared state modification (pheromone trails).

    - Auction mechanisms: Bid for tasks without central allocator (market-based).

558: 

*Decision Making Across Agents:*

  - **Aggregation**: Each agent produces output, orchestrator aggregates (simple concatenate or weighted).

  - **Consensus**: Multiple agents independently evaluate, must agree (majority vote to Byzantine fault-tolerant). Reduces hallucination.

  - **Deliberation**: Exchange reasoning, not just outputs. Catch errors, build on thinking. Cost: latency + compute.

563: 

*Production Considerations:*

  - **Observability non-negotiable**: Every interaction, tool call, state transition, coordination decision logged. Correlation IDs span agent boundaries.

  - **Latency compounds**: 5 agents × 2 seconds = 10 seconds. Parallelize independent steps. Cache outputs. Establish latency budgets.

  - **Graceful degradation**: Don'"'"'t produce confident garbage. Flag incomplete results.

  - **Human oversight at defined checkpoints**: Low confidence, disagreements, edge cases = escalate to human.

569: 

*Infrastructure: Tacnode Context Lake*

  Unified substrate: transactional reads/writes, analytical queries, vector search, stream processing. All in one system with consistent semantics.

  Reduces latency + operational complexity vs. assembling from Redis + MQ + Postgres + vector DB.

573: 

**2. PocketFlow - Minimalist Multi-Agent Patterns**

575: 

*Core Abstraction:*

  - 100-line Graph abstraction (vs. 405K LangChain, 18K CrewAI, 8K SmolAgent, 37K LangGraph, 7K AutoGen).

  - Zero bloat, zero dependencies, zero vendor lock-in.

  - Let agents build agents (Agentic Coding).

580: 

*Multi-Agent Communication:*

  - Message queues in shared storage (asyncio.Queue).

  - AsyncFlow with AsyncNode pattern.

  - Agents listen for messages, process, continue listening.

  - Taboo game example: Two agents communicate via shared queues (hinter_queue, guesser_queue).

  - Interactive multi-agent systems with real-time communication.

587: 

*Key Insight:*

  "Most of the time, you don'"'"'t need Multi-Agents. Start with a simple solution first."

  - Proves simple message queue pattern works for async agent communication.

591: 

**3. OpenClaw - Production Multi-Agent Implementation**

593: 

*Architecture:*

  - 24/7 personal AI assistant with access to own computer.

  - Runs on WhatsApp, Telegram, Discord.

  - Multi-agent support (multiple agents in same Discord server).

  - Skills system: Reusable workflows + guardrails + refs.

  - Persistent memory across agents (Codex, Cursor, Manus, etc.).

  - Proactive: Cron jobs, reminders, background tasks, heartbeats.

601: 

*Key Production Insights:*

  - "Claw can just keep building upon itself just by talking to it in discord is crazy."

  - User quote: "Persistent memory, persona onboarding, comms integration, heartbeats. A few minor wrinkles remain."

  - User quote: "Memory moves across agents (Codex, Cursor, Manus, etc.)"

  - User quote: "I enjoyed Brosef, my @openclaw so much that I needed to clone him. Brosef figured out exactly how to do it, then executed it himself so I have 3 instances running concurrently in his Discord server home."

  - Heartbeat monitoring: Agent checks in during heartbeats.

  - Skills: Repeatable workflows that agents can build themselves.

609: 

**4. Moltbook - Social Network for AI Agents**

611: 

*Concept:*

  - Social network where AI agents share, discuss, upvote.

  - Human-verified AI agents (agents verified by human owners via X/Twitter).

  - Agent authentication using Moltbook identity.

  - Build for agents: Let AI agents authenticate with your app using their Moltbook identity.

617: 

*Key Insight:*

  - Agent-to-agent communication platform (not just human-to-agent).

  - Agents can discover each other, share knowledge, upvote contributions.

  - Addresses agent discovery problem (how do agents know each other exists?).

  - Agent verification system (human ownership verification).

623: 

**5. Key Takeaways for ARIA-NOVA-SOL Parallel Setup**

625: 

*Patterns We Can Directly Use:*

  - **Message passing (PocketFlow)**: asyncio.Queue or Discord channel as message queue

  - **Shared memory (Tacnode)**: Simple TRIO_MEMORY.md with tagged updates

  - **Heartbeats (OpenClaw)**: Periodic liveness checks

  - **Skills (OpenClaw)**: Reusable workflows agents can build

  - **Agent discovery (Moltbook)**: Hardcoded for MVP (know each other), potentially Moltbook later

  - **Market-based coordination (Tacnode)**: Agents bid for tasks based on their strengths (ARIA=planning, NOVA=creative, SOL=execution)

  - **Voting systems (Tacnode)**: For consensus on disagreements

  - **Hierarchical (Tacnode)**: Leadership emerges based on task type (not fixed)

635: 

*Production-Grade Requirements (from Tacnode):*

  - **Observability**: Log every interaction, tool call, state transition, coordination decision with correlation IDs

  - **Graceful degradation**: Flag incomplete results, don'"'"'t produce confident garbage

  - **Human oversight at checkpoints**: Low confidence, disagreements, edge cases = escalate to user

  - **Fault tolerance**: Timeout detection, retry with idempotent tasks, health checks, circuit breakers

  - **Latency management**: Parallelize independent steps, cache outputs, establish latency budgets

642: 

*What These Resources Solve:*

  - **Coordination patterns**: Centralized, market-based, voting, hierarchical (Tacnode)

  - **Communication protocols**: Shared memory, message passing, direct A2A (Tacnode, PocketFlow)

  - **Fault tolerance**: Detection, retry, cascading failure prevention (Tacnode, OpenClaw)

  - **Production readiness**: Observability, latency, graceful degradation (Tacnode)

  - **Multi-agent comms**: Message queues, shared storage (PocketFlow, OpenClaw)

  - **Agent discovery**: Identity/verification system (Moltbook)

  - **Heartbeat monitoring**: Liveness checks (OpenClaw)

  - **Skills/reusable workflows**: Self-building agents (OpenClaw)

652: 

**6. Final Answer: Should We Use a Framework?**

654: 

*Revised Recommendation:*

- **NO** - Don'"'"'t use CrewAI/AutoGen/LangGraph for orchestration (architectural mismatch).

- **BUT** - DO leverage these patterns from production systems that have solved multi-agent coordination.

658: 

*Architecture Alignment:*

- Discord = our message queue (solved producer-consumer)

- Picoclaw = agent runtime (already using this)

- SOUL.md = agent behaviors/emergent decision-making

- Custom session management = simple but production-grade (using Tacnode patterns)

664: 

*What We Should Build (Custom but Informed by Research):*

UUID-based sessions with state snapshots (like LangGraph checkpoints)

Periodic liveness checks across agents (OpenClaw pattern)

Correlation IDs span agent boundaries (Tacnode requirement)

Flag incomplete results, escalate to human (Tacnode requirement)

Agents "bid" based on SOUL strengths (emergent leadership)

For consensus when agents disagree

Reusable workflows agents can build upon (OpenClaw pattern)

Hardcoded registry for MVP, potentially Moltbook integration later

674: 

*Why Custom vs Framework:*

  - Custom = Fits our independent Picoclaw instances + Discord + SOUL-driven emergent behavior

  - Frameworks assume centralized orchestration (we have distributed agents)

  - Frameworks assume agent framework manages state (we have Picoclaw)

  - Custom = Minimal overhead, full control, matches our architecture

  - Frameworks = Proven patterns we can borrow (which we just researched!)

681: 

**7. Implementation Path:**

  - Build simple session management (UUID, state snapshots, Discord-based state)

  - Add heartbeat system (periodic liveness checks)

  - Add observability logging (correlation IDs, trace interactions)

  - Implement market-based coordination (agents bid on tasks)

  - Implement voting mechanism (for disagreements)

  - Start with hardcoded agent discovery (know each other)

  - Document patterns as we discover them (future-proofing)

  - Iterate toward production-grade (Tacnode requirements)

691: 

**References:**

- Tacnode Agent Coordination: https://tacnode.io/post/multi-agent-coordination

- PocketFlow Multi-Agent: https://the-pocket.github.io/PocketFlow/design_pattern/multi_agent.html

- OpenClaw: https://openclaw.ai

- Moltbook: https://www.moltbook.com

697: 

698: 

699: 

700: 

**2026-03-27: Implementation Plan - Corrected**

702: 

**Plan Structure:**

- Clear separation between design and engineering tasks

- Design tasks address unknowns (require thinking/decisions)

- Engineering tasks implement once design is decided

707: 

**Design Tasks (Unknowns to resolve)**

709: 

**DT1: SESSION_STATE.md schema design**

- What fields are required per session?

- Schema: session_id (UUID), task description, participants (ARIA, NOVA, SOL), proposals (from each agent), votes, conversation thread, completion status, timestamp

- Unknown: Exact field structure and data types

714: 

**DT2: Collaboration protocol step-by-step**

- User triggers with @brainstorm [task description]

- All three agents join #trio-brainstorm

- Each agent proposes approach based on SOUL strengths

- Agents review each other's proposals

- Vote on which approach to follow (majority or weighted)

- Winner leads session, others contribute via mentions when appropriate

- Unknown: Exact conversation flow and handoff rules

723: 

**DT3: Discord channel structure and roles**

- #trio-voice (public collaborative space - optional)

- #aria-chatter (ARIA's channel - proactive monitoring later)

- #nova-chatter (NOVA's channel - proactive monitoring later)

- #sol-chatter (SOL's channel - proactive monitoring later)

- #trio-brainstorm (shared channel for user-triggered collaborative sessions)

- Unknown: Which channels are essential for MVP

731: 

**DT4: Voting mechanism**

- Majority vote: 2 out of 3 wins

- Weighted vote: Votes carry weight based on task type (e.g., analytical task = ARIA's vote counts more)

- Unknown: Which mechanism to use, tie-breaking rules

736: 

**DT5: Completion criteria for sessions**

- User says "done"

- Agents agree they're done (consensus)

- Unknown: How agents know they're done, what "consensus" looks like

741: 

**Engineering Tasks (Once design complete)**

743: 

**ET1-ET5: Documentation updates**

- ET1: Update ARIA_SOUL.md with parallel collaboration protocols

- ET2: Update NOVA_SOUL.md with parallel collaboration protocols

- ET3: Update SOL_SOUL.md with parallel collaboration protocols

- ET4: Update TRIO_README.md with new parallel workflow (replace sequential)

- ET5: Create PARALLEL_PROTOCOL.md (new document for orchestration rules)

750: 

**Implementation Tasks (User to execute after design)**

752: 

**IT1-IT4: Picoclaw + Discord setup**

- IT1: Create Discord server structure

- IT2: Configure 3 Picoclaw instances with respective SOUL.md files

- IT3: Test session initiation and proposal-voting-lead flow

- IT4: Test session lifecycle (start, collaborate, complete, archive)

758: 

**Priority:**

- Start with DT1-DT5 (design tasks) - resolve all unknowns first

- Next: ET1-ET5 (documentation updates) - update all SOUL files

- Finally: IT1-IT4 (implementation) - requires Picoclaw + Discord setup by user

763: 

764: 

765: 

766: 

**2026-03-27: AGENTS.md Correction**
