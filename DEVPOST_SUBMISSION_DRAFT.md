# FlowPilot — Devpost Submission Draft

> **Category:** MX Creative Console + MX Master4 & Actions Ring (Actions SDK)
> **Video:** [VIDEO_URL_PLACEHOLDER]

---

## What it does

FlowPilot is an Actions SDK plugin that transforms the MX Creative Console and MX Master4 into an **AI-powered, context-aware command surface**. Instead of static button assignments, FlowPilot dynamically reconfigures every button, dial mapping, Actions Ring shortcut, and haptic response based on what you're actually doing right now.

Switch from Slack to VS Code? Your keypad instantly shows Run Tests, Git Commit, Toggle Terminal. Jump to Figma? It becomes Export, Share, Version History. A meeting starts in 5 minutes? "Join Call" and "Set DND" appear automatically.

FlowPilot is the first **meta-plugin** for Logitech devices — it doesn't add controls for one app, it makes your entire device surface intelligent across all apps.

## How it works

FlowPilot operates through four layers:

**1. Sensing** — Monitors the active window, content type, file context, and calendar events using Win32 APIs and Microsoft Graph. No screen capture, no cloud — everything stays local.

**2. Reasoning** — A rule engine handles fast app-to-profile mapping, while a local LLM (Phi-3-mini via ONNX Runtime) predicts your most likely next actions based on learned workflow patterns.

**3. Surface Management** — Reconfigures MX Creative Console buttons (labels + icons), dial behavior (scroll mode, detent spacing), and Actions Ring overlay in under 100ms using the Actions SDK.

**4. Execution** — Handles single-app commands (keyboard simulation, API calls) and cross-app sequences. A "Ship It" macro can save all files → git commit → push → open PR → notify Slack — triggered by a single button press.

Haptic feedback confirms every action: a soft pulse for success, a warning buzz for destructive operations like force-push or file deletion.

## What makes it novel

Every plugin on the Logitech Marketplace today is a **static, single-app integration**. FlowPilot breaks that paradigm:

- **Predictive:** It learns your patterns and pre-loads the right surface before you need it
- **Cross-app:** One button can orchestrate actions across multiple applications
- **Unified:** Keypad, dial, Actions Ring, and haptics all reconfigure together as one intelligent layer
- **Community-driven:** Shareable JSON "workflow recipes" let users exchange configurations (DevOps Pipeline, Design Sprint, Content Creation)

## Who it's for

- **Software developers** (28M worldwide) — multi-tool workflows with VS Code, terminal, browser, Slack
- **Designers** — Figma/Adobe users who already own MX Creative Console
- **Product managers** — Jira/Notion/Slack power users drowning in context switches
- **Content creators** — Complex tool chains across editing, publishing, and social platforms

Context-switching costs knowledge workers an average of 23 minutes of refocus time per switch (UC Irvine research). FlowPilot eliminates that friction.

## Technical approach

- **Actions SDK (C#/.NET 8)** for the plugin core — Commands, Adjustments, Dynamic Folders, Haptics
- **ONNX Runtime + Phi-3-mini** for local AI inference (no cloud, <500ms latency on CPU)
- **Win32 event hooks** for <50ms window change detection
- **JSON workflow recipes** for community-shareable configurations
- **Phased delivery:** MVP (rule-based switching) → v1.0 (content-aware) → v1.5 (AI prediction) → v2.0 (recipe marketplace)

## Built with

C#, .NET 8, Logitech Actions SDK, ONNX Runtime, Win32 API, Microsoft Graph API

## Challenges we anticipate

- Balancing AI inference latency with surface reconfiguration speed (solution: pre-compute likely next states)
- Ensuring privacy-first architecture while maintaining prediction quality (solution: all processing local, no telemetry)
- Supporting the breadth of applications users work with (solution: community recipe system scales beyond our own definitions)

## What's next

If selected for semi-finals, we plan to deliver a working MVP with rule-based context switching for the top 10 most popular developer tools, plus a recipe editor UI for custom workflow definitions.

---

*FlowPilot: Your Workflow, On Autopilot.*
