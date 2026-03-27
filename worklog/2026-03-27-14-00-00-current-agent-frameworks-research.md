### 2026-03-27: 14-00-00-Current-Agent-Frameworks-Research

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
