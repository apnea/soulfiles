### 2026-03-27: 12-00-00-Foundational-Pattern-Research

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
