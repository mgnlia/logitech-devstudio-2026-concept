# AgentDeck — Concept Brief

## One-Liner
AgentDeck turns the Logitech MX Creative Console and MX Master 4 into a physical AI agent orchestration surface — assign, monitor, and control multiple AI agents with tactile hardware instead of switching between chat windows.

---

## Problem Statement

The rise of AI coding agents (Claude Code, GitHub Copilot, ChatGPT, local LLMs) has created a new workflow paradigm: developers now **manage multiple concurrent AI workers**. Yet the interface for this is entirely screen-based — scattered across browser tabs, terminal panes, and IDE sidebars. This leads to:

- **Context-switching fatigue** — Constantly alt-tabbing between agent outputs
- **Loss of situational awareness** — No at-a-glance view of which agents are busy, done, or errored
- **No tactile control** — Adjusting parameters (temperature, model, context window) requires navigating nested menus
- **Notification overload** — No way to *feel* when an agent finishes without watching the screen

## Solution: AgentDeck

A Logitech Actions SDK plugin that maps AI agent orchestration onto MX hardware:

### Key Mappings (MX Creative Console)
- **Keys 1–9**: Each key is bound to an AI agent slot. Dynamic icons show agent name + status.
  - *Tap* → Focus/bring agent output to foreground
  - *Long-press* → Cancel/kill running agent task
  - *Double-tap* → Re-run last prompt
- **LED Colors**: 🟢 Idle | 🟡 Thinking | 🔵 Done | 🔴 Error
- **Dial**: Rotate to adjust the active agent's temperature (0.0–2.0). Press dial to send/confirm prompt.

### MX Master 4 Integration
- **Actions Ring**: Swipe to cycle between workspaces (Code / Research / Design / DevOps). Each workspace loads a different set of agent assignments to the Console keys.
- **Haptic Feedback**: Short pulse = agent completed successfully. Double-pulse = agent errored. Long rumble = all agents in workspace finished.

### Software Architecture
- **Plugin (C#/.NET 8)**: Runs as an Actions SDK plugin, communicating with Logi Options+ via the standard plugin API.
- **Agent Bridge Service**: A lightweight local HTTP service that connects to AI providers (OpenAI, Anthropic, Ollama, LM Studio) via their APIs.
- **WebSocket Status Feed**: Real-time status updates from agents → plugin → hardware LED/haptic state.

## Target Users

| Segment | Size | Pain Point Solved |
|---|---|---|
| AI-assisted developers | 30M+ | Physical control over agent workflows |
| Creative professionals using AI | 10M+ | Tactile temperature/style control for generative AI |
| DevOps/SRE teams | 5M+ | Multi-agent monitoring for infrastructure AI |

## Business Model

1. **Free tier**: Plugin with 3 agent slots, supports OpenAI + Ollama
2. **Premium ($9.99/mo)**: Unlimited agent slots, all providers, workspace sync across devices, team sharing
3. **Enterprise**: SSO, audit logs, custom agent connectors

## Differentiation from Existing Submissions

| Existing Concept | AgentDeck Difference |
|---|---|
| FlowState (context-aware profiles) | We don't just detect context — we *control AI agents* as the primary interaction |
| Luxbin (AI command center) | Luxbin maps buttons to single AI actions; AgentDeck orchestrates *multiple concurrent agents* with status feedback |
| Generic app integrations | We target the emerging "AI agent" paradigm, not traditional app automation |

## Key Metrics (Projected)

- **Time saved**: ~45 min/day for developers managing 3+ AI agents
- **Context switches reduced**: 60% fewer alt-tabs during AI-assisted workflows
- **Agent error detection**: 3x faster via haptic + LED feedback vs. screen-only

---

*Submitted for DevStudio 2026 Phase 1 — Concept Only*
