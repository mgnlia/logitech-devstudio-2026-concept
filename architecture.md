# AgentDeck — Architecture Sketch

## System Overview

```
┌─────────────────────────────────────────────────────────────┐
│                    USER'S DESKTOP                           │
│                                                             │
│  ┌──────────────┐    ┌──────────────┐    ┌──────────────┐  │
│  │  MX Creative  │    │  MX Master 4  │    │  Logi        │  │
│  │  Console      │    │  + Actions    │    │  Options+    │  │
│  │  (Keys+Dial)  │    │    Ring       │    │  (Host App)  │  │
│  └──────┬───────┘    └──────┬───────┘    └──────┬───────┘  │
│         │                   │                    │          │
│         └───────────┬───────┘                    │          │
│                     │ HID Events                 │          │
│                     ▼                            │          │
│  ┌──────────────────────────────────────────┐    │          │
│  │         AgentDeck Plugin (C#/.NET 8)     │◄───┘          │
│  │                                          │  Actions SDK  │
│  │  ┌────────────┐  ┌───────────────────┐   │              │
│  │  │ Key Manager│  │ Workspace Manager │   │              │
│  │  │ (9 slots)  │  │ (Ring ↔ Profiles) │   │              │
│  │  └─────┬──────┘  └────────┬──────────┘   │              │
│  │        │                  │               │              │
│  │  ┌─────▼──────────────────▼──────────┐   │              │
│  │  │        State Controller           │   │              │
│  │  │  • LED color mapping              │   │              │
│  │  │  • Haptic pattern dispatch        │   │              │
│  │  │  • Dynamic icon updates           │   │              │
│  │  │  • Dial → temperature binding     │   │              │
│  │  └─────────────┬─────────────────────┘   │              │
│  └────────────────┼─────────────────────────┘              │
│                   │ WebSocket (localhost:7890)              │
│                   ▼                                         │
│  ┌──────────────────────────────────────────┐              │
│  │      Agent Bridge Service (Node.js)      │              │
│  │                                          │              │
│  │  ┌──────────┐ ┌──────────┐ ┌──────────┐ │              │
│  │  │ Agent #1 │ │ Agent #2 │ │ Agent #N │ │              │
│  │  │ (Claude)  │ │ (GPT-4)  │ │ (Ollama) │ │              │
│  │  └────┬─────┘ └────┬─────┘ └────┬─────┘ │              │
│  │       │             │            │        │              │
│  └───────┼─────────────┼────────────┼────────┘              │
│          │             │            │                        │
└──────────┼─────────────┼────────────┼────────────────────────┘
           │             │            │
           ▼             ▼            ▼
    ┌──────────┐  ┌──────────┐  ┌──────────┐
    │ Anthropic │  │  OpenAI  │  │  Local   │
    │   API     │  │   API    │  │  LLM     │
    └──────────┘  └──────────┘  └──────────┘
```

## Component Details

### 1. AgentDeck Plugin (C#/.NET 8)

The core Actions SDK plugin running inside Logi Options+.

**Responsibilities:**
- Register 9 key commands (agent slots) + 1 dial adjustment + ring workspace switch
- Maintain agent-to-key assignment state
- Update dynamic button icons (agent name, status indicator, mini progress)
- Dispatch haptic patterns to MX Master 4 on agent state changes
- Map dial rotation to active agent's temperature parameter (0.0–2.0)
- Handle workspace switching via Actions Ring (load/save workspace profiles)

**Key SDK Features Used:**
| SDK Feature | Usage |
|---|---|
| `Command` actions | Key tap, long-press, double-tap per agent slot |
| `Adjustment` actions | Dial rotation → temperature control |
| Dynamic button images | Real-time agent status icons |
| Haptics API | Completion/error feedback on MX Master 4 |
| `DynamicFolder` | Workspace browser for agent configuration |
| Plugin Settings | API keys, default models, workspace configs |
| External Service Login | OAuth for cloud AI providers |
| Multistate actions | Agent status cycling (idle/thinking/done/error) |

### 2. Agent Bridge Service (Node.js)

A lightweight local service that manages AI agent lifecycles.

**Responsibilities:**
- Maintain agent pool (create, run, cancel, query status)
- Route prompts to configured AI providers
- Stream responses and emit status events via WebSocket
- Persist conversation history per agent
- Manage workspace-to-agent mappings

**API Endpoints:**
```
POST   /agents              Create a new agent
DELETE /agents/:id           Kill an agent
POST   /agents/:id/prompt   Send prompt to agent
PATCH  /agents/:id/config   Update agent config (temp, model, etc.)
GET    /agents/:id/status   Get agent status
WS     /ws                  Real-time status feed
```

### 3. State Controller

The bridge between the Agent Bridge Service and the hardware.

**State Machine per Agent Slot:**
```
          ┌─────────┐
          │  EMPTY   │ (no agent assigned)
          └────┬─────┘
               │ assign agent
               ▼
          ┌─────────┐
    ┌────►│  IDLE    │ 🟢 Green LED
    │     └────┬─────┘
    │          │ prompt sent
    │          ▼
    │     ┌─────────┐
    │     │ THINKING │ 🟡 Yellow LED (pulsing)
    │     └────┬─────┘
    │          │
    │     ┌────┴────┐
    │     ▼         ▼
    │ ┌───────┐ ┌───────┐
    │ │ DONE  │ │ ERROR │
    │ │ 🔵    │ │ 🔴    │
    │ └───┬───┘ └───┬───┘
    │     │         │
    └─────┴─────────┘ (auto-reset after focus/ack)
```

### 4. Workspace System

Workspaces group agent configurations by task type:

| Workspace | Agents | Use Case |
|---|---|---|
| **Code** | Claude Code, Copilot, Local Llama | Active development |
| **Research** | Perplexity, Claude, GPT-4 | Information gathering |
| **Design** | DALL-E, Midjourney proxy, Claude | Creative work |
| **DevOps** | Claude, GPT-4, Custom scripts | Infrastructure & deployment |

Actions Ring swipe cycles through workspaces. Each workspace remembers its agent assignments, temperatures, and last prompts.

## Data Flow Example

**User rotates dial to set temperature, then presses Key 1:**

1. Dial rotation → Actions SDK `Adjustment` event → Plugin reads delta
2. Plugin updates Agent #1 temperature via Bridge API: `PATCH /agents/1/config {temp: 0.7}`
3. Plugin updates Key 1 icon to show "T:0.7"
4. User presses Key 1 → `Command` event → Plugin sends: `POST /agents/1/prompt`
5. Bridge dispatches to Anthropic API with temp=0.7
6. Agent status → THINKING → WebSocket event → Plugin sets Key 1 LED to yellow pulse
7. Response streams back → Agent status → DONE → WebSocket event
8. Plugin sets Key 1 LED to blue, triggers MX Master 4 haptic pulse
9. Plugin updates Key 1 icon with response preview snippet

## Technology Stack

| Layer | Technology | Rationale |
|---|---|---|
| Plugin | C# / .NET 8 | Required by Actions SDK |
| Bridge Service | Node.js / Express | Fast async I/O, npm ecosystem for AI SDKs |
| Communication | WebSocket | Real-time bidirectional status updates |
| AI Providers | OpenAI, Anthropic, Ollama APIs | Cover cloud + local LLM use cases |
| Config Storage | SQLite (local) | Lightweight, no server dependency |
| Icons | SVG templates + sharp | Dynamic icon generation |

## Security Considerations

- All communication is localhost-only (127.0.0.1)
- API keys stored in OS keychain via `CredentialManager` (Windows) / `Keychain` (macOS)
- No telemetry or external data transmission beyond AI API calls
- Bridge service binds only to loopback interface

---

*Architecture is concept-level. Detailed implementation begins in Semi-finals Phase (if selected for Top 50).*
