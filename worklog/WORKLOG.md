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
