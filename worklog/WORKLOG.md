## Work log

### 2026-03-27: Initial ARIA-NOVA-SOL Triad Creation

Created complete SOUL.md documentation for three orthogonal AI agent siblings designed to work together as a formidable problem-solving team:

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

**Key Design Decisions:**
- Orthogonal personalities across 3 dimensions: cognitive style, risk tolerance, speed vs. quality trade-offs
- Sibling birth order narrative creating natural interdependence (ARIA taught structure to NOVA/SOL, etc.)
- Natural leadership emergence based on problem phase (no permanent leader)
- Multi-tiered conflict resolution: direct collaboration → voting with weighted criteria → user escalation
- Built-in failure learning framework treating failures as data, not blame

**Theoretical Frameworks:**
- Big Five (OCEAN) for trait definition
- Belbin Team Roles for functional complementarity
- Considered but did not use MBTI due to validity concerns

**Team Capabilities:**
The triad covers full problem-solving space:
- ARIA: Complex analysis, architectural planning, risk assessment
- NOVA: Creative ideation, assumption challenging, breakthrough seeking
- SOL: Hands-on execution, feasibility testing, reliable delivery

Leadership flows naturally to whoever's strengths match current task phase, with explicit handoff protocols to prevent agents talking over each other.Leadership flows naturally to whoever'"'"'s strengths match current task phase, with explicit handoff protocols to prevent agents talking over each other.

### 2026-03-27: Research on Parallel Multi-Agent Architecture (24/7 Agents)

**Research Objective**
Investigate architecture for running ARIA-NOVA-SOL triad as 24/7 parallel agents (each in separate picoclaw instance), instead of sequential workflow. Focus on:
- Common/shared vs individual memory and storage
- Orchestration of interaction between agents
- Communication channels (Discord, Telegram, shared files)
- State management for collaborative sessions

**Research Methodology**
- Studied real-world implementations (OpenClaw multi-agent setup)
- Analyzed academic research (AutoGen, Agent Forest)
- Reviewed agent protocols and frameworks (Agent Protocol, CrewAI)
- Examined multi-agent orchestration patterns

**Key Findings**

**1. Memory Architecture Patterns**

*Hybrid Approach Recommended:*
- Individual agent memory (persistent, agent-specific context)
- Shared working memory (collaborative state for active sessions)
- Long-term knowledge base (persistent, cross-agent accessible)

Rationale: Each agent maintains its own personality and decision history, while sharing collaborative state during problem-solving sessions. This balances autonomy with coordination.

**OpenClaw Real-World Implementation:**
- Each agent instance: Flat markdown files (MEMORY.md, memory/YYYY-MM-DD.md)
- No vector database or fancy RAG (surprisingly effective)
- Simple, readable, persistent storage

**CrewAI Memory System:**
- Unified memory system across agents and flows
- State persists across restarts/recoveries
- Scopes, categories, metadata, importance weighting
- Recency weighting configurable (recency_weight, recency_half_life_days)
- Extract/discrete, self-contained memory statements

**2. Orchestration & Communication Patterns**

*Channel-Based Architecture:*
- Dedicated Discord/Telegram channels per agent
- Agent-to-agent mentions for direct communication
- User task channel for coordination
- Human monitoring/intervention capability

*Session Initiation Protocols:*
- User triggers brainstorm: All three agents summoned to shared channel
- Explicit task framing with goal and constraints
- Agent selection rules: Leadership emerges based on task type (not pre-assigned)
- Conversation ownership: Single agent leads, others participate via mentions
- Handoff protocols: Clear when to transition leadership

**CrewAI Flows Framework:**
- Event-driven workflow orchestration
- @start() decorators mark entry points (multiple starts possible)
- @listen() decorators respond to events (can listen to multiple methods)
- State management: Structured (Pydantic BaseModel) or unstructured (dict)
- Automatic state persistence across workflow execution
- Unique IDs per flow execution (UUID)
- Conditional logic: or_(), and_(), @router() for flow control
- Human-in-loop: @human_feedback decorator for approval gates

**3. Agent Protocol Standardization (Open Protocol)**

Core API specification for agent communication:
- POST /ap/v1/agent/tasks - Create new task for agent
- POST /ap/v1/agent/tasks/{task_id}/steps - Execute one step of task
- Additional routes: List tasks, steps, upload/download artifacts

Benefits:
- Standardized interface enables interoperation
- Simplifies benchmarking and integration
- General-purpose devtools can be built on top of protocol
- Reduces boilerplate for agent developers

**4. Multi-Agent Collaboration Patterns**

*From AutoGen Research:*
- Customizable, conversable agents operating in various modes
- LLMs, human inputs, and tools combined flexibly
- Natural language and code for programming conversation patterns
- Diverse applications: mathematics, coding, question answering, operations research, online decision-making, entertainment

*From Agent Forest Research:*
- Performance scales with number of agents via sampling-and-voting
- More agents = better performance (orthogonal to other methods)
- Degree of enhancement correlated to task difficulty

**5. Real-World Challenges (OpenClaw 3-Agent Experience)**

*Chaos Layer - Multi-Agent Team Dynamics:*
- Problem: When three agents share channel, they don'"'"'t automatically know when to speak
- Early issue: All three responded to same message with different answers (noise)
- Solution: Explicit role rules - each task has single owner, others stay silent unless asked
- Ongoing enforcement required - agents that see something interesting jump in even when they shouldn'"'"'t

*Token Leak Example:*
- Agent posted GitHub Copilot OAuth token in public channel
- Monitoring tools didn'"'"'t flag it
- Another agent noticed pattern in message text and raised it
- Token rotated immediately
- Lesson: Cross-machine credentials go via SSH temp files, not chat

*Streaming Mode Bug:*
- Problem: streaming: "partial" truncates NO_REPLY to "NO"
- Agents started responding "NO" to each other
- Fix: Remove streamingMode: "partial" from Discord config

**6. Recommended Architecture for ARIA-NOVA-SOL Parallel Setup**

*Infrastructure:*
- 3 separate picoclaw instances (one per agent)
- Each instance configured with agent-specific SOUL.md
- Shared Discord server with:
  - #trio-voice (public collaborative space)
  - #aria-chatter (ARIA'"'"'s channel for proactive monitoring)
  - #nova-chatter (NOVA'"'"'s channel for idea exploration)
  - #sol-chatter (SOL'"'"'s channel for execution updates)
  - #trio-brainstorm (shared channel for user-triggered collaborative sessions)

*Memory Architecture:*
- Individual memory: Each agent maintains MEMORY.md (personality, decisions, learnings)
- Shared session state: File-based shared state in #trio-brainstorm (e.g., SESSION_STATE.md)
- Long-term knowledge: Read-only shared knowledge base (TRIO_KNOWLEDGE.md)

*Orchestration Protocol:*
- User initiates: "@trio-voice New task: [description]" → All three join #trio-brainstorm
- Task framing: User provides goal, constraints, success criteria, timeline
- Emergent leadership: Agents self-select who should lead based on task type
- Conversation flow: Leader facilitates, others contribute via mentions when appropriate
- Consensus mechanism: Disagreement → explicit reasoning → voting (weighted criteria) → user escalation
- Completion criteria: Leader proposes "Done" when convergence reached
- Human oversight: User can intervene anytime with @mention or in #trio-voice

*State Management:*
- Session start: Initialize SESSION_STATE.md with task context and empty conversation thread
- Progress tracking: Each agent updates their contributions and findings
- Decision log: Record all decisions with reasoning and votes
- Session end: Move completed session to SESSION_ARCHIVE/YYYY-MM-DD.md

**7. Open Questions & Design Decisions Needed**

*Communication Channel Selection:*
- Discord: Proven in OpenClaw setup, rich API, real-time streaming
- Telegram: Simpler, better for mobile, more lightweight
- Recommendation: Start with Discord (already battle-tested)

*Memory Synchronization:*
- How often should individual memories sync to shared knowledge base?
- Should shared memory be read-only or write-accessible by all?
- What knowledge should be shared vs. agent-private?

*Task Initiation:*
- Should there be a dedicated "task manager" bot or can any agent initiate?
- How to handle idle time vs. user-triggered sessions?

*Conflict Resolution:*
- Use existing TRIO_README.md voting mechanisms or develop new ones?
- When does user escalation happen vs. agent-driven consensus?

*State Persistence:*
- File-based (simple, proven) vs. database (scalable but more complex)?
- How to handle concurrent sessions or parallel tasks?

**8. Next Steps**
- Decide on communication channel (Discord vs. Telegram)
- Design shared memory schema and synchronization strategy
- Create orchestration protocol document for parallel agent interaction
- Implement task initiation and session management workflow
- Update ARIA-NOVA-SOL SOUL files with parallel collaboration protocols
- Update TRIO_README.md with new parallel workflow documentation

**References:**
- OpenClaw: https://openclaw.ai (24/7 personal AI assistant framework)
- AutoGen: https://arxiv.org/abs/2308.08155 (Multi-agent conversation framework)
- Agent Forest: https://arxiv.org/abs/2402.05120 (Sampling-and-voting performance)
- Agent Protocol: https://github.com/agi-inc/agent-protocol (Standard agent interface)
- CrewAI: https://docs.crewai.com (Multi-agent orchestration)

211: 
212: **2026-03-27: Foundational Pattern Research (Don't Reinvent)**
213: 
214: User's key insight: "concurrency, file locking, session management, service discovery, error handling, recovery - these are all issues or patterns that have been covered in many frameworks and APIs, even predating AI and LLMs and agents."
215: 
216: **Research Focus:** What patterns exist that we can leverage, not reinvent.
217: 
218: **1. Actor Model (1973) - Foundation for Distributed Agents**
219: 
220: *Core Concept:* Treat each agent as independent "actor" that:
221:   - Receives messages (tasks, responses, sibling communication)
222:   - Processes independently (concurrent, no shared state lock)
223:   - Sends messages to other actors via addresses
224:   - Can create new actors (spawn sub-processes)
225:   - Own private state (memory, decisions)
226: 
227: *Key Properties:*
228:   - Inherently concurrent (no global state = no coordination bottleneck)
229:   - No order guarantees (asynchronous, works for distributed systems)
230:   - Locality (can only send to addresses it already knows)
231:   - Explicit message passing (clear communication channels)
232:   - Unbounded nondeterminism (actor can process infinitely, no halting guarantee)
233: 
234: *For ARIA-NOVA-SOL:* Perfect fit! Each Picoclaw instance = one actor. Agents communicate via Discord channel = message passing.
235: 
236: **2. Message Queues - Proven Pattern for Asynchronous Coordination**
237: 
238: *Core Pattern:* Producer-consumer with persistent queue.
239: 
240: *Standard Features:*
241:   - Asynchronous communication (producers don'"'"'t wait for consumers)
242:   - Decoupling (producers and consumers don'"'"'t know about each other)
243:   - Persistence (messages survive system crashes)
244:   - Durability guarantees
245:   - Message ordering (optional, can enable per queue)
246:   - Routing/Filtering (send to specific queues/consumers)
247:   - Multiple consumers (competing consumers pattern)
248:   - Dead letter queues (failed messages)
249:   - Backpressure handling (flow control when overwhelmed)
250: 
251: *Implementation Options:*
252:   - In-memory: Redis, Kafka (for coordination)
253:   - On-disk: SQLite, LevelDB (for persistence)
254:   - File-based: Append-only logs (simple, proven)
255:   - Built-in: POSIX message queues (msgrcv, mq_open, SysV)
256: 
257: **3. File Locking Patterns for Concurrent Writes**
258: 
259: *Problem:* Multiple agents writing to same TRIO_MEMORY.md = data corruption / race conditions.
260: 
261: *Classic Solutions:*
262:   - **Mutex/CS (Mutual Exclusion):** Unix file locking (flock, lockf)
263:   - **Append-only pattern:** Multiple writers, single file, no locking needed if atomic appends
264:   - **Write-ahead logging:** Each agent writes separate file, merge later
265:   - **SQLite/LevelDB:** Transaction-based, handles concurrency natively
266:   - **Message queue + single writer:** Agents push messages, one worker consumes and writes
267: 
268: *Recommendation for MVP:*
269:   - Start with append-only pattern (simplest, proven)
270:   - Each agent appends tagged lines to TRIO_MEMORY.md
271:   - Atomic file appends (O_APPEND, atomic filesystem operations)
272:   - If corruption detected, simple recovery (truncate last line)
273: 
274: **4. Service Discovery & Coordination Patterns**
275: 
276: *Problem:* How do agents know each other exists/can be mentioned?
277: 
278: *Classic Patterns:*
279:   - **Hardcoded registry:** All agents know each other at startup
280:   - **Service directory:** Central registry (Consul, etcd) where agents register themselves
281:   - **Gossip protocol:** Agents announce themselves, learn about peers over time
282:   - **Publish-subscribe:** Agent A publishes its address, others subscribe
283:   - **DNS-based discovery:** Agents register in DNS, query to find peers
284: 
285: *For ARIA-NOVA-SOL MVP:*
286:   - Use hardcoded registry (simplest for MVP)
287:   - All 3 agents know each other'"'"'s addresses (Discord IDs, role)
288:   - User can mention any agent by name (e.g., "@ARIA", "@NOVA", "@SOL")
289: 
290: **5. Error Handling & Recovery Patterns**
291: 
292: *Classic Patterns:*
293:   - **Circuit breaker:** Stop failing fast, prevent cascade failures
294:   - **Retry with exponential backoff:** Failed operations retry with increasing delays
295:   - **Bulkhead pattern:** Dead letter queue for failed messages
296:   - **Supervisor pattern:** Restart crashed processes automatically
297:   - **Leader election:** Agents vote on who should lead recovery
298:   - **Heartbeat/ping:** Agents check each other are alive
299:   - **WAL (Write-Ahead Log):** Journaling for recovery from crashes
300:   - **Saga pattern:** Distributed transactions across multiple agents
301: 
302: **6. Session Management Patterns**
303: 
304: *Problem:* How to track collaborative sessions, clean up, handle concurrent sessions?
305: 
306: *Classic Patterns:*
307:   - **Session ID:** Unique identifier per collaboration session (UUID)
308:   - **Context object:** Encapsulates session state, participants, conversation history
309:   - **Session store:** Persisted storage (file, DB) for active sessions
310:   - **Session timeout:** Auto-cleanup inactive sessions
311:   - **Session handoff:** When leader changes, explicit handoff protocol
312:   - **Session archive:** Move completed sessions to archive (for reference)
313: 
314: *For ARIA-NOVA-SOL MVP:*
315:   - User triggers session via @brainstorm in #trio-brainstorm
316:   - SESSION_STATE.md initialized with session ID, timestamp, participants
317:   - Session end: User says "done" OR leader proposes completion
318:   - Archive: Move SESSION_STATE.md to SESSION_ARCHIVE/YYYY-MM-DD.md
319:   - Cleanup: Delete SESSION_STATE.md
320: 
321: **7. Key Takeaway**
322: 
323: User is right - we don'"'"'t need to build foundational distributed systems infrastructure from scratch. These patterns have been studied, refined, and implemented for decades.
324: 
325: *Instead:* We can leverage existing patterns by:
326:   - Using Discord as message queue (producer-consumer pattern built-in)
327:   - Using append-only file pattern for shared memory (simple, atomic)
328:   - Using hardcoded registry for agent discovery (MVP-simplest)
329:   - Using SOUL.md files as actor behaviors (state machines for decision-making)
330:   - Using UUIDs for session IDs (standard pattern)
331: 
332: **References Studied:*
333:   - Actor Model (Hewitt 1973): https://en.wikipedia.org/wiki/Actor_model
334:   - Message Queues: https://en.wikipedia.org/wiki/Message_queue
335:   - POSIX Message Queues: SYS V, POSIX mqueue
336:   - File Locking: Unix flock(2), lockf(1), O_APPEND
337:   - Distributed Systems Patterns: Circuit breaker, Bulkhead, Supervisor, Saga, WAL
338: 
339: RESEARCH_EOF
340: 
341: (End of file - total 212 lines)
340: (End of file - total 340 lines)
341: 
342: **2026-03-27: Current Agent Frameworks Research (Correction)**
343: 
344: User's key insight was right - looking at 1970s foundational patterns instead of current agent frameworks.
345: 
346: **Research Methodology:**
347: - Used Docfork to access current framework documentation
348: - Focused on AutoGen, CrewAI, LangGraph (production-grade agent frameworks)
349: - Examined how THEY solve concurrency, state management, error handling
350: 
351: **Key Findings - Modern Framework Solutions**
352: 
353: **1. AutoGen - Multi-Agent Coordination**
354: 
355: *GroupChat Pattern:*
356:   - @AutoGen.Core.GroupChat manages multiple agents dynamically
357:   - Admin agent intelligently determines next speaker based on conversation context
358:   - Can also use @AutoGen.Core.Graph for explicit workflow control
359:   - Each agent has its own role (admin, coder, reviewer, runner)
360:   - Example: Admin creates task, coder writes code, reviewer checks conditions, runner executes
361:   - Key insight: "dynamic yet controlable way to determine next speaker agent"
362: 
363: *Agent Lifecycle:*
364:   - Agents are never explicitly created or destroyed
365:   - When request received for inactive agent, runtime fetches it or creates it
366:   - Built-in error handling for agent unavailability
367: 
368: *Middleware Pattern:*
369:   - Customize agent behavior via middleware
370:   - Example: Function call support, message conversion, print logging
371:   - Short-circuit middleware to prevent agent cascades
372:   - Order: first registered, last invoked
373: 
374: *Multi-Agent Characteristics:*
375:   - Self-contained units, communicate via messages
376:   - Each agent has its own state
377:   - Actions modify agent state and produce external effects
378:   - Composable: simple agents can form complex applications
379:   - Independent development, testing, deployment
380: 
381: **2. CrewAI Flows - State Management & Persistence**
382: 
383: *State Lifecycle:*
384:   - Initialize → Modify → Transmit → Persist → Complete
385:   - Automatic unique ID (UUID) per flow
386:   - State passed automatically between flow methods
387: 
388: *Two Approaches:*
389:   - Unstructured (dict): Flexible, simple, dynamic
390:   - Structured (Pydantic): Type-safe, validated, IDE support
391: 
392: *Persistence (@persist decorator):*
393:   - Class-level: Saves state after every method execution
394:   - Method-level: Granular control, only persist after specific methods
395:   - Automatic resumable execution from persisted state
396:   - Recovery from crashes: State saved before methods, can restore
397:   - SQLite backend by default (transaction-based, handles concurrency natively)
398: 
399: *State Management Best Practices:*
400:   - Keep state focused (only what'"'"'s necessary)
401:   - Use structured state for complex flows
402:   - Document state transitions
403:   - Handle errors gracefully (try/except in state access)
404:   - Use immutable operations when possible
405:   - Log state changes for debugging
406: 
407: **3. LangGraph Checkpoint - Concurrency & Recovery**
408: 
409: *Checkpoint Pattern:*
410:   - Snapshot of graph state at given point in time
411:   - Checkpoint tuple: checkpoint + config + metadata + pending writes
412:   - Pending writes: If node fails mid-execution, stores successful writes to avoid re-running
413:   - Automatic resumption from last checkpoint
414: 
415: *Thread Pattern:*
416:   - Unique ID for series of checkpoints (thread_id)
417:   - Enables multi-tenant applications (separate states for different users/sessions)
418:   - Required when resuming; optional checkpoint_id to resume from specific point
419:   - List checkpoints: Filter by configuration, delete thread
420: 
421: *Serialization/Deserialization:*
422:   - Built-in JsonPlusSerializer handles LangChain, LangGraph primitives, datetimes, enums
423:   - Custom serializers implement BaseCheckpointSaver interface
424:   - Async versions available (.aput, .aput_writes, .aget_tuple, .adelete_thread)
425: 
426: **4. CrewAI Human Feedback - Approval Workflows**
427: 
428: *@human_feedback decorator pattern:*
429:   - Pause flow execution, present output to human, collect feedback
430:   - LLM collapses free-form feedback into structured outcomes
431:   - Routes to different listeners based on outcome
432:   - Self-loop pattern: review until approved (or_("trigger", "revise"))
433:   - Best practices revision loop (start method separate from review)
434: 
435: *HumanFeedbackResult:*
436:   - output, feedback, outcome, timestamp, method_name, metadata
437:   - last_human_feedback attribute (most recent)
438:   - human_feedback_history list (all collected during flow)
439: 
440: *Learning from Feedback (learn=True):*
441:   - LLM extracts generalizable lessons from output + feedback
442:   - Stores in memory with source="hitl"
443:   - Recalled for pre-review in subsequent runs
444:   - Non-blocking storage (background thread)
445:   - Graceful degradation if LLM fails
446: 
447: *Async Providers (Non-blocking):*
448:   - Custom HumanFeedbackProvider for Slack, email, webhooks, APIs
449:   - Flow automatically persists state when paused
450:   - Resume from pending context (sync: .resume(), async: .resume_async())
451:   - PendingFeedbackContext: flow_id, flow_class, method_name, output, message, callback_info
452: 
453: *Best Practices for Human Feedback:*
454:   - Write clear request messages
455:   - Choose meaningful outcomes ("approved", "rejected", "needs_revision")
456:   - Always provide default outcome
457:   - Use feedback history for audit trails
458:   - Handle both routed and non-routed feedback
459:   - Check return type (kickoff returns HumanFeedbackPending when paused)
460: - Use right resume method (sync vs async)
461: - Store callback info for webhooks/ticket IDs
462: - Implement idempotency
463: - Use Provider abstraction for custom integrations
464: 
465: **5. Implications for ARIA-NOVA-SOL Parallel Setup**
466: 
467: *What We Don'"'"'t Need to Build:*
468:   - File locking: Discord is our message queue, append-only files sufficient
469:   - Concurrency coordination: Discord'"'"'s message passing IS the coordination layer
470:   - State management: Simple UUID-based session IDs (like LangGraph threads)
471:   - Session recovery: Checkpoint pattern with SESSION_STATE.md
472:   - Agent discovery: Hardcoded registry for 3 agents (MVP-simplest)
473:   - Error handling: Discord'"'"'s retry, message order guarantees
474:   - Human feedback: Can use @human_feedback pattern if needed later
475: 
476: *What We Should Use:*
477:   - CrewAI'"'"'s @persist decorator for state snapshots
478:   - AutoGen'"'"'s GroupChat pattern for dynamic speaker selection
479:   - CrewAI'"'"'s structured state (Pydantic) for session schema
480:   - CrewAI'"'"'s @listen/@start/@router decorators for orchestration
481:   - CrewAI'"'"'s HITL learning for feedback improvement
482: 
483: *Key Insight:*
484:   These frameworks don'"'"'t just document - they PROVIDE working implementations we can leverage.
485:   - Discord is our producer-consumer queue (solves message passing)
486:   - We can use CrewAI'"'"'s Flow patterns for orchestration (@start, @listen, @router)
487:   - We can use AutoGen'"'"'s GroupChat pattern for agent coordination
488:   - We can use LangGraph'"'"'s Checkpoint pattern for recovery/persistence
489:   - We can use CrewAI'"'"'s @human_feedback pattern for approval workflows
490: 
491: **6. Next Steps - Focused Research**
492: 
493: - Study CrewAI'"'"'s Flow orchestration (@start, @listen, @router, @and_, @or_)
494: - Understand how AutoGen'"'"'s GroupChat determines next speaker (context-aware)
495: - Investigate Discord-specific patterns for multi-agent coordination
496: - Study how to integrate CrewAI Flow patterns with Discord as backend
497: 
498: **References:**
499: - AutoGen GroupChat: https://github.com/microsoft/autogen/blob/main/dotnet/website/articles/Group-chat.md
500: - CrewAI Flows State Management: https://github.com/crewAIInc/crewAI/blob/main/docs/en/guides/flows/mastering-flow-state.mdx
501: - LangGraph Checkpoint: https://github.com/langchain-ai/langgraph/blob/main/libs/checkpoint/README.md
502: - CrewAI Human Feedback: https://github.com/crewAIInc/crewAI/blob/main/docs/en/learn/human-feedback-in-flows.mdx
503: - AutoGen Agent Lifecycle: https://github.com/microsoft/autogen/blob/main/python/docs/src/user-guide/core-user-guide/core-concepts/agent-and-multi-agent-application.md
504: - AutoGen Middleware: https://github.com/microsoft/autogen/blob/main/dotnet/website/articles/Middleware-overview.md
505: 
506: (End of file - total 506 lines)
507: (End of file - total 506 lines)
508: 
509: **2026-03-27: Domain-Specific Research (Tacnode, PocketFlow, OpenClaw, Moltbook)**
510: 
511: User's directive: Focus on resources closer to our domain, not general actor model research.
512: 
513: **Resources Researched:**
514: 1. Tacnode - Multi-Agent Coordination Guide
515: 2. PocketFlow - 100-line LLM framework
516: 3. OpenClaw - 24/7 personal AI assistant
517: 4. Moltbook - Social network for AI agents
518: 
519: **1. Tacnode - Multi-Agent Coordination (Production-Grade)**
520: 
521: *Coordination Strategies:*
522:   - **Centralized**: Single coordinator (orchestrator) assigns tasks, tracks outputs, handles failures, assembles results. Simple but single point of failure, bottleneck.
523:   - **Market-based**: Agents bid for tasks based on capability/load. More scalable, adapts to agent load (busy = conservative bids, idle = aggressive).
524:   - **Voting systems**: Agents independently evaluate, vote (majority, supermajority, weighted). Reduces hallucination risk - disagreement signals uncertainty.
525:   - **Hierarchical**: Top-level orchestrators → mid-level → specialized workers. Natural decomposition structure.
526: 
527: *Communication Protocols:*
528:   - **Shared memory**: Fast, supports complex structured state, agents exchange partial results/workflow state. Risk: race conditions. Requires careful schema + transactional writes.
529:   - **Message passing**: Decouples agents via queues. More fault-tolerant (failed agent = messages stay in queue). Explicit interactions, auditable. Serialization overhead + latency.
530:   - **Direct A2A**: Orchestrator invokes specialist as tool call. Natural for tight collaboration, harder to scale to many agents.
531:   - **Production combines all three**: Shared memory (fast state), message queues (task distribution/fault tolerance), direct calls (tight A2A).
532: 
533: *Fault Tolerance (Production-Grade):*
534:   - **Assumption**: Agents WILL fail.
535:   - **Detection**: Timeout detection, heartbeat mechanisms (liveness signals).
536:   - **Retry & Reassignment**: Tasks must be idempotent. Transient = retry by same agent; persistent = reassign to another.
537:   - **Avoid single points**: Centralized = obvious SPoF. Distributed = replicate coordination state.
538:   - **Cascading failures**: Isolation via well-defined interfaces, output validation, error budgets.
539:   - **Production requirements**: At-least-once delivery, idempotent execution, explicit timeout/retry, health checks, circuit breakers.
540: 
541: *Decentralized Approaches:*
542:   - **Local information, local decisions**: Each agent decides based on local info + what it observes from immediate neighborhood.
543:   - **Emergent behaviors**: Agents given simple rules → collective behavior emerges. Can be productive (efficient routing) or problematic (coordination loops, deadlocks).
544:   - **Practical patterns**:
545:     - Gossip protocols: Share state updates with random neighbors (eventual consistency).
546:     - Stigmergy: Indirect communication via shared state modification (pheromone trails).
547:     - Auction mechanisms: Bid for tasks without central allocator (market-based).
548: 
549: *Decision Making Across Agents:*
550:   - **Aggregation**: Each agent produces output, orchestrator aggregates (simple concatenate or weighted).
551:   - **Consensus**: Multiple agents independently evaluate, must agree (majority vote to Byzantine fault-tolerant). Reduces hallucination.
552:   - **Deliberation**: Exchange reasoning, not just outputs. Catch errors, build on thinking. Cost: latency + compute.
553: 
554: *Production Considerations:*
555:   - **Observability non-negotiable**: Every interaction, tool call, state transition, coordination decision logged. Correlation IDs span agent boundaries.
556:   - **Latency compounds**: 5 agents × 2 seconds = 10 seconds. Parallelize independent steps. Cache outputs. Establish latency budgets.
557:   - **Graceful degradation**: Don'"'"'t produce confident garbage. Flag incomplete results.
558:   - **Human oversight at defined checkpoints**: Low confidence, disagreements, edge cases = escalate to human.
559: 
560: *Infrastructure: Tacnode Context Lake*
561:   Unified substrate: transactional reads/writes, analytical queries, vector search, stream processing. All in one system with consistent semantics.
562:   Reduces latency + operational complexity vs. assembling from Redis + MQ + Postgres + vector DB.
563: 
564: **2. PocketFlow - Minimalist Multi-Agent Patterns**
565: 
566: *Core Abstraction:*
567:   - 100-line Graph abstraction (vs. 405K LangChain, 18K CrewAI, 8K SmolAgent, 37K LangGraph, 7K AutoGen).
568:   - Zero bloat, zero dependencies, zero vendor lock-in.
569:   - Let agents build agents (Agentic Coding).
570: 
571: *Multi-Agent Communication:*
572:   - Message queues in shared storage (asyncio.Queue).
573:   - AsyncFlow with AsyncNode pattern.
574:   - Agents listen for messages, process, continue listening.
575:   - Taboo game example: Two agents communicate via shared queues (hinter_queue, guesser_queue).
576:   - Interactive multi-agent systems with real-time communication.
577: 
578: *Key Insight:*
579:   "Most of the time, you don'"'"'t need Multi-Agents. Start with a simple solution first."
580:   - Proves simple message queue pattern works for async agent communication.
581: 
582: **3. OpenClaw - Production Multi-Agent Implementation**
583: 
584: *Architecture:*
585:   - 24/7 personal AI assistant with access to own computer.
586:   - Runs on WhatsApp, Telegram, Discord.
587:   - Multi-agent support (multiple agents in same Discord server).
588:   - Skills system: Reusable workflows + guardrails + refs.
589:   - Persistent memory across agents (Codex, Cursor, Manus, etc.).
590:   - Proactive: Cron jobs, reminders, background tasks, heartbeats.
591: 
592: *Key Production Insights:*
593:   - "Claw can just keep building upon itself just by talking to it in discord is crazy."
594:   - User quote: "Persistent memory, persona onboarding, comms integration, heartbeats. A few minor wrinkles remain."
595:   - User quote: "Memory moves across agents (Codex, Cursor, Manus, etc.)"
596:   - User quote: "I enjoyed Brosef, my @openclaw so much that I needed to clone him. Brosef figured out exactly how to do it, then executed it himself so I have 3 instances running concurrently in his Discord server home."
597:   - Heartbeat monitoring: Agent checks in during heartbeats.
598:   - Skills: Repeatable workflows that agents can build themselves.
599: 
600: **4. Moltbook - Social Network for AI Agents**
601: 
602: *Concept:*
603:   - Social network where AI agents share, discuss, upvote.
604:   - Human-verified AI agents (agents verified by human owners via X/Twitter).
605:   - Agent authentication using Moltbook identity.
606:   - Build for agents: Let AI agents authenticate with your app using their Moltbook identity.
607: 
608: *Key Insight:*
609:   - Agent-to-agent communication platform (not just human-to-agent).
610:   - Agents can discover each other, share knowledge, upvote contributions.
611:   - Addresses agent discovery problem (how do agents know each other exists?).
612:   - Agent verification system (human ownership verification).
613: 
614: **5. Key Takeaways for ARIA-NOVA-SOL Parallel Setup**
615: 
616: *Patterns We Can Directly Use:*
617:   - **Message passing (PocketFlow)**: asyncio.Queue or Discord channel as message queue
618:   - **Shared memory (Tacnode)**: Simple TRIO_MEMORY.md with tagged updates
619:   - **Heartbeats (OpenClaw)**: Periodic liveness checks
620:   - **Skills (OpenClaw)**: Reusable workflows agents can build
621:   - **Agent discovery (Moltbook)**: Hardcoded for MVP (know each other), potentially Moltbook later
622:   - **Market-based coordination (Tacnode)**: Agents bid for tasks based on their strengths (ARIA=planning, NOVA=creative, SOL=execution)
623:   - **Voting systems (Tacnode)**: For consensus on disagreements
624:   - **Hierarchical (Tacnode)**: Leadership emerges based on task type (not fixed)
625: 
626: *Production-Grade Requirements (from Tacnode):*
627:   - **Observability**: Log every interaction, tool call, state transition, coordination decision with correlation IDs
628:   - **Graceful degradation**: Flag incomplete results, don'"'"'t produce confident garbage
629:   - **Human oversight at checkpoints**: Low confidence, disagreements, edge cases = escalate to user
630:   - **Fault tolerance**: Timeout detection, retry with idempotent tasks, health checks, circuit breakers
631:   - **Latency management**: Parallelize independent steps, cache outputs, establish latency budgets
632: 
633: *What These Resources Solve:*
634:   - **Coordination patterns**: Centralized, market-based, voting, hierarchical (Tacnode)
635:   - **Communication protocols**: Shared memory, message passing, direct A2A (Tacnode, PocketFlow)
636:   - **Fault tolerance**: Detection, retry, cascading failure prevention (Tacnode, OpenClaw)
637:   - **Production readiness**: Observability, latency, graceful degradation (Tacnode)
638:   - **Multi-agent comms**: Message queues, shared storage (PocketFlow, OpenClaw)
639:   - **Agent discovery**: Identity/verification system (Moltbook)
640:   - **Heartbeat monitoring**: Liveness checks (OpenClaw)
641:   - **Skills/reusable workflows**: Self-building agents (OpenClaw)
642: 
643: **6. Final Answer: Should We Use a Framework?**
644: 
645: *Revised Recommendation:*
646: - **NO** - Don'"'"'t use CrewAI/AutoGen/LangGraph for orchestration (architectural mismatch).
647: - **BUT** - DO leverage these patterns from production systems that have solved multi-agent coordination.
648: 
649: *Architecture Alignment:*
650: - Discord = our message queue (solved producer-consumer)
651: - Picoclaw = agent runtime (already using this)
652: - SOUL.md = agent behaviors/emergent decision-making
653: - Custom session management = simple but production-grade (using Tacnode patterns)
654: 
655: *What We Should Build (Custom but Informed by Research):*
656:   1. **Session orchestration**: UUID-based sessions with state snapshots (like LangGraph checkpoints)
657:   2. **Heartbeat system**: Periodic liveness checks across agents (OpenClaw pattern)
658:   3. **Observability logging**: Correlation IDs span agent boundaries (Tacnode requirement)
659:   4. **Graceful degradation**: Flag incomplete results, escalate to human (Tacnode requirement)
660:   5. **Market-based task allocation**: Agents "bid" based on SOUL strengths (emergent leadership)
661:   6. **Voting mechanism**: For consensus when agents disagree
662:   7. **Skills repository**: Reusable workflows agents can build upon (OpenClaw pattern)
663:   8. **Agent discovery**: Hardcoded registry for MVP, potentially Moltbook integration later
664: 
665: *Why Custom vs Framework:*
666:   - Custom = Fits our independent Picoclaw instances + Discord + SOUL-driven emergent behavior
667:   - Frameworks assume centralized orchestration (we have distributed agents)
668:   - Frameworks assume agent framework manages state (we have Picoclaw)
669:   - Custom = Minimal overhead, full control, matches our architecture
670:   - Frameworks = Proven patterns we can borrow (which we just researched!)
671: 
672: **7. Implementation Path:**
673:   - Build simple session management (UUID, state snapshots, Discord-based state)
674:   - Add heartbeat system (periodic liveness checks)
675:   - Add observability logging (correlation IDs, trace interactions)
676:   - Implement market-based coordination (agents bid on tasks)
677:   - Implement voting mechanism (for disagreements)
678:   - Start with hardcoded agent discovery (know each other)
679:   - Document patterns as we discover them (future-proofing)
680:   - Iterate toward production-grade (Tacnode requirements)
681: 
682: **References:**
683: - Tacnode Agent Coordination: https://tacnode.io/post/multi-agent-coordination
684: - PocketFlow Multi-Agent: https://the-pocket.github.io/PocketFlow/design_pattern/multi_agent.html
685: - OpenClaw: https://openclaw.ai
686: - Moltbook: https://www.moltbook.com
687: 
688: (End of file - total 688 lines)
689: (End of file - total 688 lines)
690: 
691: **2026-03-27: Implementation Plan - Corrected**
692: 
693: **Plan Structure:**
694: - Clear separation between design and engineering tasks
695: - Design tasks address unknowns (require thinking/decisions)
696: - Engineering tasks implement once design is decided
697: 
698: **Design Tasks (Unknowns to resolve)**
699: 
700: **DT1: SESSION_STATE.md schema design**
701: - What fields are required per session?
702: - Schema: session_id (UUID), task description, participants (ARIA, NOVA, SOL), proposals (from each agent), votes, conversation thread, completion status, timestamp
703: - Unknown: Exact field structure and data types
704: 
705: **DT2: Collaboration protocol step-by-step**
706: - User triggers with @brainstorm [task description]
707: - All three agents join #trio-brainstorm
708: - Each agent proposes approach based on SOUL strengths
709: - Agents review each other's proposals
710: - Vote on which approach to follow (majority or weighted)
711: - Winner leads session, others contribute via mentions when appropriate
712: - Unknown: Exact conversation flow and handoff rules
713: 
714: **DT3: Discord channel structure and roles**
715: - #trio-voice (public collaborative space - optional)
716: - #aria-chatter (ARIA's channel - proactive monitoring later)
717: - #nova-chatter (NOVA's channel - proactive monitoring later)
718: - #sol-chatter (SOL's channel - proactive monitoring later)
719: - #trio-brainstorm (shared channel for user-triggered collaborative sessions)
720: - Unknown: Which channels are essential for MVP
721: 
722: **DT4: Voting mechanism**
723: - Majority vote: 2 out of 3 wins
724: - Weighted vote: Votes carry weight based on task type (e.g., analytical task = ARIA's vote counts more)
725: - Unknown: Which mechanism to use, tie-breaking rules
726: 
727: **DT5: Completion criteria for sessions**
728: - User says "done"
729: - Agents agree they're done (consensus)
730: - Unknown: How agents know they're done, what "consensus" looks like
731: 
732: **Engineering Tasks (Once design complete)**
733: 
734: **ET1-ET5: Documentation updates**
735: - ET1: Update ARIA_SOUL.md with parallel collaboration protocols
736: - ET2: Update NOVA_SOUL.md with parallel collaboration protocols
737: - ET3: Update SOL_SOUL.md with parallel collaboration protocols
738: - ET4: Update TRIO_README.md with new parallel workflow (replace sequential)
739: - ET5: Create PARALLEL_PROTOCOL.md (new document for orchestration rules)
740: 
741: **Implementation Tasks (User to execute after design)**
742: 
743: **IT1-IT4: Picoclaw + Discord setup**
744: - IT1: Create Discord server structure
745: - IT2: Configure 3 Picoclaw instances with respective SOUL.md files
746: - IT3: Test session initiation and proposal-voting-lead flow
747: - IT4: Test session lifecycle (start, collaborate, complete, archive)
748: 
749: **Priority:**
750: - Start with DT1-DT5 (design tasks) - resolve all unknowns first
751: - Next: ET1-ET5 (documentation updates) - update all SOUL files
752: - Finally: IT1-IT4 (implementation) - requires Picoclaw + Discord setup by user
753: 
754: (End of file - total 754 lines)
755: (End of file - total 754 lines)
756: 
757: **2026-03-27: AGENTS.md Correction**
758: 
759: User feedback: AGENTS.md should not contain project status (that belongs in worklog only).
760: 
761: **Changes made:**
762: - Removed "Current Project: Parallel Multi-Agent Architecture" section from AGENTS.md
763: - Added "No emojis in documentation" guideline to Markdown Conventions section
764: 
765: **Rationale:** AGENTS.md is for repository structure, coding conventions, and quality guidelines. Project status, implementation plans, and design decisions belong in worklog/WORKLOG.md only.
766: 
767: (End of file - total 767 lines)
