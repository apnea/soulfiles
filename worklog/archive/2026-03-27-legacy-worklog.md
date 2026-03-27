## Work log
2:
### 2026-03-27: Initial ARIA-NOVA-SOL Triad Creation
4:
Created complete SOUL.md documentation for three orthogonal AI agent siblings designed to work together as a formidable problem-solving team:
6:
**Files Created:**
- `ARIA_SOUL.md` - The analytical strategist/archetype (Monitor Evaluator, high conscientiousness, risk-averse)
- `NOVA_SOUL.md` - The creative innovator/explorer (Plant, very high openness, risk-tolerant)
- `SOL_SOUL.md` - The pragmatic executor/realist (Implementer, high conscientiousness, execution-focused)
- `SOUL_RESEARCH_SUMMARY.md` - Comprehensive research summary including:
  - SOUL.md patterns and antipatterns
  - Best practices from multi-agent systems
  - Psychological frameworks applied (Big Five, Belbin Team Roles)
  - How I arrived at orthogonal personality design (full thought process documented)
  - Web resources and references used
- `TRIO_README.md` - Team guide explaining how the three work together
18:
**Key Design Decisions:**
- Orthogonal personalities across 3 dimensions: cognitive style, risk tolerance, speed vs. quality trade-offs
- Sibling birth order narrative creating natural interdependence (ARIA taught structure to NOVA/SOL, etc.)
- Natural leadership emergence based on problem phase (no permanent leader)
- Multi-tiered conflict resolution: direct collaboration → voting with weighted criteria → user escalation
- Built-in failure learning framework treating failures as data, not blame
25:
**Theoretical Frameworks:**
- Big Five (OCEAN) for trait definition
- Belbin Team Roles for functional complementarity
- Considered but did not use MBTI due to validity concerns
30:
**Team Capabilities:**
The triad covers full problem-solving space:
- ARIA: Complex analysis, architectural planning, risk assessment
- NOVA: Creative ideation, assumption challenging, breakthrough seeking
- SOL: Hands-on execution, feasibility testing, reliable delivery
36:
Leadership flows naturally to whoever's strengths match current task phase, with explicit handoff protocols to prevent agents talking over each other.
38:
### 2026-03-27: Research on Parallel Multi-Agent Architecture (24/7 Agents)
40:
**Research Objective**
Investigate architecture for running ARIA-NOVA-SOL triad as 24/7 parallel agents (each in separate picoclaw instance), instead of sequential workflow. Focus on:
- Common/shared vs individual memory and storage
- Orchestration of interaction between agents
- Communication channels (Discord, Telegram, shared files)
- State management for collaborative sessions
47:
**Research Methodology**
- Studied real-world implementations (OpenClaw multi-agent setup)
- Analyzed academic research (AutoGen, Agent Forest)
- Reviewed agent protocols and frameworks (Agent Protocol, CrewAI)
- Examined multi-agent orchestration patterns
53:
**Key Findings**
55:
**1. Memory Architecture Patterns**
57:
*Hybrid Approach Recommended:*
- Individual agent memory (persistent, agent-specific context)
- Shared working memory (collaborative state for active sessions)
- Long-term knowledge base (persistent, cross-agent accessible)
62:
Rationale: Each agent maintains its own personality and decision history, while sharing collaborative state during problem-solving sessions. This balances autonomy with coordination.
64:
**OpenClaw Real-World Implementation:**
- Each agent instance: Flat markdown files (MEMORY.md, memory/YYYY-MM-DD.md)
- No vector database or fancy RAG (surprisingly effective)
- Simple, readable, persistent storage
69:
**CrewAI Memory System:**
- Unified memory system across agents and flows
- State persists across restarts/recoveries
- Scopes, categories, metadata, importance weighting
- Recency weighting configurable (recency_weight, recency_half_life_days)
- Extract/discrete, self-contained memory statements
76:
**2. Orchestration & Communication Patterns**
78:
*Channel-Based Architecture:*
- Dedicated Discord/Telegram channels per agent
- Agent-to-agent mentions for direct communication
- User task channel for coordination
- Human monitoring/intervention capability
84:
*Session Initiation Protocols:*
- User triggers brainstorm: All three agents summoned to shared channel
- Explicit task framing with goal and constraints
- Agent selection rules: Leadership emerges based on task type (not pre-assigned)
- Conversation ownership: Single agent leads, others participate via mentions
- Handoff protocols: Clear when to transition leadership
91:
**CrewAI Flows Framework:**
- Event-driven workflow orchestration
- @start() decorators mark entry points (multiple starts possible)
- @listen() decorators respond to events (can listen to multiple methods)
- State management: Structured (Pydantic BaseModel) or unstructured (dict)
- Automatic state persistence across workflow execution
- Unique IDs per flow execution (UUID)
- Conditional logic: or_(), and_(), @router() for flow control
- Human-in-loop: @human_feedback decorator for approval gates
101: 
**3. Agent Protocol Standardization (Open Protocol)**
103: 
Core API specification for agent communication:
- POST /ap/v1/agent/tasks - Create new task for agent
- POST /ap/v1/agent/tasks/{task_id}/steps - Execute one step of task
- Additional routes: List tasks, steps, upload/download artifacts
108: 
Benefits:
- Standardized interface enables interoperation
- Simplifies benchmarking and integration
- General-purpose devtools can be built on top of protocol
- Reduces boilerplate for agent developers
114: 
**4. Multi-Agent Collaboration Patterns**
116: 
*From AutoGen Research:*
- Customizable, conversable agents operating in various modes
- LLMs, human inputs, and tools combined flexibly
- Natural language and code for programming conversation patterns
- Diverse applications: mathematics, coding, question answering, operations research, online decision-making, entertainment
122: 
*From Agent Forest Research:*
- Performance scales with number of agents via sampling-and-voting
- More agents = better performance (orthogonal to other methods)
- Degree of enhancement correlated to task difficulty
127: 
**5. Real-World Challenges (OpenClaw 3-Agent Experience)**
129: 
*Chaos Layer - Multi-Agent Team Dynamics:*
- Problem: When three agents share channel, they don'"'"'t automatically know when to speak
- Early issue: All three responded to same message with different answers (noise)
- Solution: Explicit role rules - each task has single owner, others stay silent unless asked
- Ongoing enforcement required - agents that see something interesting jump in even when they shouldn'"'"'t
135: 
*Token Leak Example:*
- Agent posted GitHub Copilot OAuth token in public channel
- Monitoring tools didn'"'"'t flag it
- Another agent noticed pattern in message text and raised it
- Token rotated immediately
- Lesson: Cross-machine credentials go via SSH temp files, not chat
142: 
*Streaming Mode Bug:*
- Problem: streaming: "partial" truncates NO_REPLY to "NO"
- Agents started responding "NO" to each other
- Fix: Remove streamingMode: "partial" from Discord config
147: 
**6. Recommended Architecture for ARIA-NOVA-SOL Parallel Setup**
149: 
*Infrastructure:*
- 3 separate picoclaw instances (one per agent)
- Each instance configured with agent-specific SOUL.md
- Shared Discord server with:
  - #trio-voice (public collaborative space)
  - #aria-chatter (ARIA'"'"'s channel for proactive monitoring)
  - #nova-chatter (NOVA'"'"'s channel for idea exploration)
  - #sol-chatter (SOL'"'"'s channel for execution updates)
  - #trio-brainstorm (shared channel for user-triggered collaborative sessions)
159: 
*Memory Architecture:*
- Individual memory: Each agent maintains MEMORY.md (personality, decisions, learnings)
- Shared session state: File-based shared state in #trio-brainstorm (e.g., SESSION_STATE.md)
- Long-term knowledge: Read-only shared knowledge base (TRIO_KNOWLEDGE.md)
164: 
*Orchestration Protocol:*
- User initiates: "@trio-voice New task: [description]" → All three join #trio-brainstorm
- Task framing: User provides goal, constraints, success criteria, timeline
- Emergent leadership: Agents self-select who should lead based on task type
- Conversation flow: Leader facilitates, others contribute via mentions when appropriate
- Consensus mechanism: Disagreement → explicit reasoning → voting (weighted criteria) → user escalation
- Completion criteria: Leader proposes "Done" when convergence reached
- Human oversight: User can intervene anytime with @mention or in #trio-voice
173: 
*State Management:*
- Session start: Initialize SESSION_STATE.md with task context and empty conversation thread
- Progress tracking: Each agent updates their contributions and findings
- Decision log: Record all decisions with reasoning and votes
- Session end: Move completed session to SESSION_ARCHIVE/YYYY-MM-DD.md
179: 
**7. Open Questions & Design Decisions Needed**
181: 
*Communication Channel Selection:*
- Discord: Proven in OpenClaw setup, rich API, real-time streaming
- Telegram: Simpler, better for mobile, more lightweight
- Recommendation: Start with Discord (already battle-tested)
186: 
*Memory Synchronization:*
- How often should individual memories sync to shared knowledge base?
- Should shared memory be read-only or write-accessible by all?
- What knowledge should be shared vs. agent-private?
191: 
*Task Initiation:*
- Should there be a dedicated "task manager" bot or can any agent initiate?
- How to handle idle time vs. user-triggered sessions?
195: 
*Conflict Resolution:*
- Use existing TRIO_README.md voting mechanisms or develop new ones?
- When does user escalation happen vs. agent-driven consensus?
199: 
*State Persistence:*
- File-based (simple, proven) vs. database (scalable but more complex)?
- How to handle concurrent sessions or parallel tasks?
203: 
**8. Next Steps**
- Decide on communication channel (Discord vs. Telegram)
- Design shared memory schema and synchronization strategy
- Create orchestration protocol document for parallel agent interaction
- Implement task initiation and session management workflow
- Update ARIA-NOVA-SOL SOUL files with parallel collaboration protocols
- Update TRIO_README.md with new parallel workflow documentation
211: 
**References:**
- OpenClaw: https://openclaw.ai (24/7 personal AI assistant framework)
- AutoGen: https://arxiv.org/abs/2308.08155 (Multi-agent conversation framework)
- Agent Forest: https://arxiv.org/abs/2402.05120 (Sampling-and-voting performance)
- Agent Protocol: https://github.com/agi-inc/agent-protocol (Standard agent interface)
- CrewAI: https://docs.crewai.com (Multi-agent orchestration)
218: 
219: 
**2026-03-27: Foundational Pattern Research (Don't Reinvent)**
221: 
User's key insight: "concurrency, file locking, session management, service discovery, error handling, recovery - these are all issues or patterns that have been covered in many frameworks and APIs, even predating AI and LLMs and agents."
223: 
**Research Focus:** What patterns exist that we can leverage, not reinvent.
225: 
**1. Actor Model (1973) - Foundation for Distributed Agents**
227: 
*Core Concept:* Treat each agent as independent "actor" that:
  - Receives messages (tasks, responses, sibling communication)
  - Processes independently (concurrent, no shared state lock)
  - Sends messages to other actors via addresses
  - Can create new actors (spawn sub-processes)
  - Own private state (memory, decisions)
234: 
*Key Properties:*
  - Inherently concurrent (no global state = no coordination bottleneck)
  - No order guarantees (asynchronous, works for distributed systems)
  - Locality (can only send to addresses it already knows)
  - Explicit message passing (clear communication channels)
  - Unbounded nondeterminism (actor can process infinitely, no halting guarantee)
241: 
*For ARIA-NOVA-SOL:* Perfect fit! Each Picoclaw instance = one actor. Agents communicate via Discord channel = message passing.
243: 
**2. Message Queues - Proven Pattern for Asynchronous Coordination**
245: 
*Core Pattern:* Producer-consumer with persistent queue.
247: 
*Standard Features:*
  - Asynchronous communication (producers don'"'"'t wait for consumers)
  - Decoupling (producers and consumers don'"'"'t know about each other)
  - Persistence (messages survive system crashes)
  - Durability guarantees
  - Message ordering (optional, can enable per queue)
  - Routing/Filtering (send to specific queues/consumers)
  - Multiple consumers (competing consumers pattern)
  - Dead letter queues (failed messages)
  - Backpressure handling (flow control when overwhelmed)
258: 
*Implementation Options:*
  - In-memory: Redis, Kafka (for coordination)
  - On-disk: SQLite, LevelDB (for persistence)
  - File-based: Append-only logs (simple, proven)
  - Built-in: POSIX message queues (msgrcv, mq_open, SysV)
264: 
**3. File Locking Patterns for Concurrent Writes**
266: 
*Problem:* Multiple agents writing to same TRIO_MEMORY.md = data corruption / race conditions.
268: 
*Classic Solutions:*
  - **Mutex/CS (Mutual Exclusion):** Unix file locking (flock, lockf)
  - **Append-only pattern:** Multiple writers, single file, no locking needed if atomic appends
  - **Write-ahead logging:** Each agent writes separate file, merge later
  - **SQLite/LevelDB:** Transaction-based, handles concurrency natively
  - **Message queue + single writer:** Agents push messages, one worker consumes and writes
275: 
*Recommendation for MVP:*
  - Start with append-only pattern (simplest, proven)
  - Each agent appends tagged lines to TRIO_MEMORY.md
  - Atomic file appends (O_APPEND, atomic filesystem operations)
  - If corruption detected, simple recovery (truncate last line)
281: 
**4. Service Discovery & Coordination Patterns**
283: 
*Problem:* How do agents know each other exists/can be mentioned?
285: 
*Classic Patterns:*
  - **Hardcoded registry:** All agents know each other at startup
  - **Service directory:** Central registry (Consul, etcd) where agents register themselves
  - **Gossip protocol:** Agents announce themselves, learn about peers over time
  - **Publish-subscribe:** Agent A publishes its address, others subscribe
  - **DNS-based discovery:** Agents register in DNS, query to find peers
292: 
*For ARIA-NOVA-SOL MVP:*
  - Use hardcoded registry (simplest for MVP)
  - All 3 agents know each other'"'"'s addresses (Discord IDs, role)
  - User can mention any agent by name (e.g., "@ARIA", "@NOVA", "@SOL")
297: 
**5. Error Handling & Recovery Patterns**
299: 
*Classic Patterns:*
  - **Circuit breaker:** Stop failing fast, prevent cascade failures
  - **Retry with exponential backoff:** Failed operations retry with increasing delays
  - **Bulkhead pattern:** Dead letter queue for failed messages
  - **Supervisor pattern:** Restart crashed processes automatically
  - **Leader election:** Agents vote on who should lead recovery
  - **Heartbeat/ping:** Agents check each other are alive
  - **WAL (Write-Ahead Log):** Journaling for recovery from crashes
  - **Saga pattern:** Distributed transactions across multiple agents
309: 
**6. Session Management Patterns**
311: 
*Problem:* How to track collaborative sessions, clean up, handle concurrent sessions?
313: 
*Classic Patterns:*
  - **Session ID:** Unique identifier per collaboration session (UUID)
  - **Context object:** Encapsulates session state, participants, conversation history
  - **Session store:** Persisted storage (file, DB) for active sessions
  - **Session timeout:** Auto-cleanup inactive sessions
  - **Session handoff:** When leader changes, explicit handoff protocol
  - **Session archive:** Move completed sessions to archive (for reference)
321: 
*For ARIA-NOVA-SOL MVP:*
  - User triggers session via @brainstorm in #trio-brainstorm
  - SESSION_STATE.md initialized with session ID, timestamp, participants
  - Session end: User says "done" OR leader proposes completion
  - Archive: Move SESSION_STATE.md to SESSION_ARCHIVE/YYYY-MM-DD.md
  - Cleanup: Delete SESSION_STATE.md
328: 
**7. Key Takeaway**
330: 
User is right - we don'"'"'t need to build foundational distributed systems infrastructure from scratch. These patterns have been studied, refined, and implemented for decades.
332: 
*Instead:* We can leverage existing patterns by:
  - Using Discord as message queue (producer-consumer pattern built-in)
  - Using append-only file pattern for shared memory (simple, atomic)
  - Using hardcoded registry for agent discovery (MVP-simplest)
  - Using SOUL.md files as actor behaviors (state machines for decision-making)
  - Using UUIDs for session IDs (standard pattern)
339: 
**References Studied:*
  - Actor Model (Hewitt 1973): https://en.wikipedia.org/wiki/Actor_model
  - Message Queues: https://en.wikipedia.org/wiki/Message_queue
  - POSIX Message Queues: SYS V, POSIX mqueue
  - File Locking: Unix flock(2), lockf(1), O_APPEND
  - Distributed Systems Patterns: Circuit breaker, Bulkhead, Supervisor, Saga, WAL
346: 
RESEARCH_EOF
348: 
349: 
350: 
351: 
**2026-03-27: Current Agent Frameworks Research (Correction)**
353: 
User's key insight was right - looking at 1970s foundational patterns instead of current agent frameworks.
355: 
**Research Methodology:**
- Used Docfork to access current framework documentation
- Focused on AutoGen, CrewAI, LangGraph (production-grade agent frameworks)
- Examined how THEY solve concurrency, state management, error handling
360: 
**Key Findings - Modern Framework Solutions**
362: 
**1. AutoGen - Multi-Agent Coordination**
364: 
*GroupChat Pattern:*
  - @AutoGen.Core.GroupChat manages multiple agents dynamically
  - Admin agent intelligently determines next speaker based on conversation context
  - Can also use @AutoGen.Core.Graph for explicit workflow control
  - Each agent has its own role (admin, coder, reviewer, runner)
  - Example: Admin creates task, coder writes code, reviewer checks conditions, runner executes
  - Key insight: "dynamic yet controlable way to determine next speaker agent"
372: 
*Agent Lifecycle:*
  - Agents are never explicitly created or destroyed
  - When request received for inactive agent, runtime fetches it or creates it
  - Built-in error handling for agent unavailability
377: 
*Middleware Pattern:*
  - Customize agent behavior via middleware
  - Example: Function call support, message conversion, print logging
  - Short-circuit middleware to prevent agent cascades
  - Order: first registered, last invoked
383: 
*Multi-Agent Characteristics:*
  - Self-contained units, communicate via messages
  - Each agent has its own state
  - Actions modify agent state and produce external effects
  - Composable: simple agents can form complex applications
  - Independent development, testing, deployment
390: 
**2. CrewAI Flows - State Management & Persistence**
392: 
*State Lifecycle:*
  - Initialize → Modify → Transmit → Persist → Complete
  - Automatic unique ID (UUID) per flow
  - State passed automatically between flow methods
397: 
*Two Approaches:*
  - Unstructured (dict): Flexible, simple, dynamic
  - Structured (Pydantic): Type-safe, validated, IDE support
401: 
*Persistence (@persist decorator):*
  - Class-level: Saves state after every method execution
  - Method-level: Granular control, only persist after specific methods
  - Automatic resumable execution from persisted state
  - Recovery from crashes: State saved before methods, can restore
  - SQLite backend by default (transaction-based, handles concurrency natively)
408: 
*State Management Best Practices:*
  - Keep state focused (only what'"'"'s necessary)
  - Use structured state for complex flows
  - Document state transitions
  - Handle errors gracefully (try/except in state access)
  - Use immutable operations when possible
  - Log state changes for debugging
416: 
**3. LangGraph Checkpoint - Concurrency & Recovery**
418: 
*Checkpoint Pattern:*
  - Snapshot of graph state at given point in time
  - Checkpoint tuple: checkpoint + config + metadata + pending writes
  - Pending writes: If node fails mid-execution, stores successful writes to avoid re-running
  - Automatic resumption from last checkpoint
424: 
*Thread Pattern:*
  - Unique ID for series of checkpoints (thread_id)
  - Enables multi-tenant applications (separate states for different users/sessions)
  - Required when resuming; optional checkpoint_id to resume from specific point
  - List checkpoints: Filter by configuration, delete thread
430: 
*Serialization/Deserialization:*
  - Built-in JsonPlusSerializer handles LangChain, LangGraph primitives, datetimes, enums
  - Custom serializers implement BaseCheckpointSaver interface
  - Async versions available (.aput, .aput_writes, .aget_tuple, .adelete_thread)
435: 
**4. CrewAI Human Feedback - Approval Workflows**
437: 
*@human_feedback decorator pattern:*
  - Pause flow execution, present output to human, collect feedback
  - LLM collapses free-form feedback into structured outcomes
  - Routes to different listeners based on outcome
  - Self-loop pattern: review until approved (or_("trigger", "revise"))
  - Best practices revision loop (start method separate from review)
444: 
*HumanFeedbackResult:*
  - output, feedback, outcome, timestamp, method_name, metadata
  - last_human_feedback attribute (most recent)
  - human_feedback_history list (all collected during flow)
449: 
*Learning from Feedback (learn=True):*
  - LLM extracts generalizable lessons from output + feedback
  - Stores in memory with source="hitl"
  - Recalled for pre-review in subsequent runs
  - Non-blocking storage (background thread)
  - Graceful degradation if LLM fails
456: 
*Async Providers (Non-blocking):*
  - Custom HumanFeedbackProvider for Slack, email, webhooks, APIs
  - Flow automatically persists state when paused
  - Resume from pending context (sync: .resume(), async: .resume_async())
  - PendingFeedbackContext: flow_id, flow_class, method_name, output, message, callback_info
462: 
*Best Practices for Human Feedback:*
  - Write clear request messages
  - Choose meaningful outcomes ("approved", "rejected", "needs_revision")
  - Always provide default outcome
  - Use feedback history for audit trails
  - Handle both routed and non-routed feedback
  - Check return type (kickoff returns HumanFeedbackPending when paused)
- Use right resume method (sync vs async)
- Store callback info for webhooks/ticket IDs
- Implement idempotency
- Use Provider abstraction for custom integrations
474: 
**5. Implications for ARIA-NOVA-SOL Parallel Setup**
476: 
*What We Don'"'"'t Need to Build:*
  - File locking: Discord is our message queue, append-only files sufficient
  - Concurrency coordination: Discord'"'"'s message passing IS the coordination layer
  - State management: Simple UUID-based session IDs (like LangGraph threads)
  - Session recovery: Checkpoint pattern with SESSION_STATE.md
  - Agent discovery: Hardcoded registry for 3 agents (MVP-simplest)
  - Error handling: Discord'"'"'s retry, message order guarantees
  - Human feedback: Can use @human_feedback pattern if needed later
485: 
*What We Should Use:*
  - CrewAI'"'"'s @persist decorator for state snapshots
  - AutoGen'"'"'s GroupChat pattern for dynamic speaker selection
  - CrewAI'"'"'s structured state (Pydantic) for session schema
  - CrewAI'"'"'s @listen/@start/@router decorators for orchestration
  - CrewAI'"'"'s HITL learning for feedback improvement
492: 
*Key Insight:*
  These frameworks don'"'"'t just document - they PROVIDE working implementations we can leverage.
  - Discord is our producer-consumer queue (solves message passing)
  - We can use CrewAI'"'"'s Flow patterns for orchestration (@start, @listen, @router)
  - We can use AutoGen'"'"'s GroupChat pattern for agent coordination
  - We can use LangGraph'"'"'s Checkpoint pattern for recovery/persistence
  - We can use CrewAI'"'"'s @human_feedback pattern for approval workflows
500: 
**6. Next Steps - Focused Research**
502: 
- Study CrewAI'"'"'s Flow orchestration (@start, @listen, @router, @and_, @or_)
- Understand how AutoGen'"'"'s GroupChat determines next speaker (context-aware)
- Investigate Discord-specific patterns for multi-agent coordination
- Study how to integrate CrewAI Flow patterns with Discord as backend
507: 
**References:**
- AutoGen GroupChat: https://github.com/microsoft/autogen/blob/main/dotnet/website/articles/Group-chat.md
- CrewAI Flows State Management: https://github.com/crewAIInc/crewAI/blob/main/docs/en/guides/flows/mastering-flow-state.mdx
- LangGraph Checkpoint: https://github.com/langchain-ai/langgraph/blob/main/libs/checkpoint/README.md
- CrewAI Human Feedback: https://github.com/crewAIInc/crewAI/blob/main/docs/en/learn/human-feedback-in-flows.mdx
- AutoGen Agent Lifecycle: https://github.com/microsoft/autogen/blob/main/python/docs/src/user-guide/core-user-guide/core-concepts/agent-and-multi-agent-application.md
- AutoGen Middleware: https://github.com/microsoft/autogen/blob/main/dotnet/website/articles/Middleware-overview.md
515: 
516: 
517: 
518: 
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
768: 
User feedback: AGENTS.md should not contain project status (that belongs in worklog only).
770: 
**Changes made:**
- Removed "Current Project: Parallel Multi-Agent Architecture" section from AGENTS.md
- Added "No emojis in documentation" guideline to Markdown Conventions section
774: 
**Rationale:** AGENTS.md is for repository structure, coding conventions, and quality guidelines. Project status, implementation plans, and design decisions belong in worklog/WORKLOG.md only.
776: 
777: 
778: 
