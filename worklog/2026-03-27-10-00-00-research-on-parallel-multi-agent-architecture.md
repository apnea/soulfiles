### 2026-03-27: 10-00-00-Research-On-Parallel-Multi-Agent-Architecture

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
