# FlowPilot — AI-Powered Context-Aware Workflow Orchestrator

## Concept Brief — DevStudio 2026 by Logitech (Phase 1)

**Category:** MX Creative Console + MX Master4 & Actions Ring (Actions SDK)

---

## The Problem

Knowledge workers context-switch between 6–9 applications per task. Each switch costs 23 minutes of refocus time (UC Irvine research). Existing keyboard shortcuts and macro tools are **static** — they don't adapt to what the user is actually doing right now. The MX Creative Console's buttons sit idle with the same fixed assignments regardless of whether you're reviewing a PR, writing a design doc, or triaging Slack messages.

## The Solution: FlowPilot

**FlowPilot** is an Actions SDK plugin that turns the MX Creative Console and MX Master4 Actions Ring into an **AI-powered, context-aware command surface** that dynamically reconfigures itself based on:

1. **Active application** (VS Code, Figma, Slack, browser, terminal)
2. **Content on screen** (code review → show approve/comment/request-changes; design file → show export/share/version)
3. **Workflow state** (morning standup routine → meeting controls; deep coding → test/build/deploy)
4. **Calendar awareness** (meeting in 5 min → surface "join call" + "set DND" on keypad)

### How It Works

```
┌─────────────────────────────────────────────────────┐
│                   FlowPilot Engine                   │
│                                                      │
│  ┌──────────┐  ┌──────────┐  ┌───────────────────┐  │
│  │ Context   │  │ AI       │  │ Actions SDK       │  │
│  │ Detector  │→ │ Reasoner │→ │ Surface Manager   │  │
│  │           │  │ (Local   │  │                   │  │
│  │ • Window  │  │  LLM /   │  │ • Button labels   │  │
│  │ • Content │  │  Rules)  │  │ • Dial mappings   │  │
│  │ • Time    │  │          │  │ • Ring actions     │  │
│  │ • Calendar│  │          │  │ • Haptic feedback  │  │
│  └──────────┘  └──────────┘  └───────────────────┘  │
│                                                      │
│  ┌──────────────────────────────────────────────┐    │
│  │         Dynamic Profile Switcher              │    │
│  │  Learns user patterns → pre-loads surfaces    │    │
│  └──────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────┘
```

### Core Interactions

| Device Surface | FlowPilot Behavior |
|---|---|
| **MX Creative Console Keypad** | 9 buttons dynamically relabeled per context (e.g., in VS Code during PR review: Approve, Comment, Request Changes, Next File, Toggle Diff, Copy SHA, Open in Browser, Run Tests, Merge) |
| **MX Creative Console Dial** | Scroll through files/tabs/layers contextually; haptic detents at section boundaries |
| **MX Master4 Actions Ring** | Quick-access overlay with top-3 predicted next actions; swipe to execute |
| **Haptic Feedback** | Confirmation pulse on action execution; warning buzz on destructive actions (delete, force-push) |

### Key Differentiators

1. **Predictive, not reactive:** FlowPilot learns your workflow patterns and pre-loads the right surface *before* you need it
2. **Privacy-first AI:** Runs a local small language model (e.g., Phi-3-mini via ONNX) — no data leaves the machine
3. **Workflow recipes:** Community-shareable JSON workflow definitions (e.g., "DevOps Pipeline", "Design Sprint", "Content Creation")
4. **Cross-app orchestration:** A single dial turn can trigger a multi-step sequence across applications (e.g., "Ship It" = save all → git commit → push → open PR → post to Slack)

## Target Audience

- **Software developers** (primary) — 28M worldwide, heavy multi-app workflows
- **Designers** — Figma/Adobe users who already own MX Creative Console
- **Product managers** — Jira/Notion/Slack power users drowning in context switches
- **Content creators** — Video editors, writers, streamers with complex tool chains

## Technical Approach

- **Actions SDK (C#)** for plugin core, with Node.js SDK (alpha) as stretch goal
- **Local LLM inference** via ONNX Runtime (Phi-3-mini, 3.8B params, runs on CPU)
- **Windows accessibility APIs** for active window/content detection
- **Microsoft Graph API** (optional) for calendar integration
- **Plugin data storage** via Actions SDK local storage for learned patterns
- **Haptics API** for tactile feedback on all MX Master4 interactions

## Market Positioning

No existing Logitech Marketplace plugin offers AI-driven dynamic surface reconfiguration. Current plugins are static 1:1 app integrations. FlowPilot is the first **meta-plugin** — it orchestrates the entire device surface as a unified, intelligent command layer.

---

*Concept prepared for DevStudio 2026 Phase 1 submission. Feb 2026.*
