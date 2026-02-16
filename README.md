# FlowPilot — AI-Powered Context-Aware Workflow Orchestrator

> **DevStudio 2026 by Logitech** | Category: MX Creative Console + MX Master4 & Actions Ring (Actions SDK)
>
> 🏆 [Devpost Submission](https://devstudiologitech2026.devpost.com/) | 🎥 [Video Pitch](VIDEO_URL_PLACEHOLDER) | 📄 [Concept Brief](CONCEPT_BRIEF.md)

---

## 🧠 What is FlowPilot?

FlowPilot is an Actions SDK plugin that transforms the MX Creative Console and MX Master4 into an **AI-powered, context-aware command surface**. It dynamically reconfigures every button, dial mapping, Actions Ring shortcut, and haptic response based on your current activity.

**The first meta-plugin for Logitech devices** — it doesn't add controls for one app, it makes your entire device surface intelligent across all apps.

## ✨ Key Features

- **🔄 Context-Aware Surfaces** — Buttons, dials, and Actions Ring reconfigure automatically when you switch apps or tasks
- **🤖 Predictive Actions** — Local AI learns your workflow patterns and pre-loads the right controls before you need them
- **⚡ Cross-App Sequences** — One button triggers multi-step workflows across applications ("Ship It" = save → commit → push → PR → notify)
- **📳 Intelligent Haptics** — Confirmation pulses, warning buzzes, and scroll detents provide tactile workflow feedback
- **🔒 Privacy-First** — All AI inference runs locally via ONNX Runtime (Phi-3-mini). No data leaves your machine
- **📦 Workflow Recipes** — Shareable JSON configurations for different roles (Developer, Designer, PM, Creator)

## 🏗️ Architecture

```
Sensing → Reasoning → Surface Management → Device Execution
  │           │              │                    │
  ├ Win32 API ├ Rule Engine  ├ Button Manager     ├ MX Creative Console
  ├ UI Auto   ├ Local LLM   ├ Dial Manager       ├ MX Master4
  └ Calendar  └ Patterns    └ Haptics Controller └ Actions Ring
```

See [ARCHITECTURE_SKETCH.md](ARCHITECTURE_SKETCH.md) for detailed system design.

## 📋 Submission Package

This repository contains the Phase 1 concept submission materials:

| Document | Description |
|---|---|
| [CONCEPT_BRIEF.md](CONCEPT_BRIEF.md) | Full concept description with problem statement, solution, and technical approach |
| [ARCHITECTURE_SKETCH.md](ARCHITECTURE_SKETCH.md) | System architecture diagram, data flow, technology stack, and file structure |
| [JUDGING_ALIGNMENT.md](JUDGING_ALIGNMENT.md) | Criterion-by-criterion alignment map with talking points and risk matrix |
| [DEVPOST_SUBMISSION_DRAFT.md](DEVPOST_SUBMISSION_DRAFT.md) | Paste-ready Devpost submission text |
| [VIDEO_SCRIPT.md](VIDEO_SCRIPT.md) | 60-second video pitch script with cut plan and production notes |

## 🎯 Target Audience

- **Software Developers** (28M worldwide) — Multi-tool workflows with VS Code, terminal, browser, Slack
- **Designers** — Figma/Adobe users who already own MX Creative Console
- **Product Managers** — Jira/Notion/Slack power users
- **Content Creators** — Complex tool chains across editing, publishing, and social platforms

## 🛠️ Technical Stack

| Component | Technology |
|---|---|
| Plugin Core | C# / .NET 8 / Logitech Actions SDK |
| AI Inference | ONNX Runtime + Phi-3-mini (3.8B, local) |
| Window Detection | Win32 API (P/Invoke) |
| Calendar | Microsoft Graph API (optional) |
| Workflow Recipes | JSON schema |
| Haptics | Actions SDK Haptics API |

## 📅 Timeline

| Phase | Scope | Status |
|---|---|---|
| **Phase 1** | Concept pitch (this submission) | ✅ Submitted |
| Semi-finals | Working MVP — rule-based context switching | 🔜 If selected (Mar 4) |
| Finals | Full AI prediction + recipe marketplace | 🔜 If selected (Apr 8) |

## 📊 Key Metrics

- **< 50ms** context switch detection (Win32 event hooks)
- **< 100ms** surface reconfiguration (pre-computed profiles)
- **< 500ms** AI prediction latency (ONNX quantized)
- **< 200MB** memory footprint

## 🔗 Links

- [DevStudio 2026 Challenge Page](https://devstudiologitech2026.devpost.com/)
- [Logitech Actions SDK Documentation](https://logitech.github.io/actions-sdk-docs/)
- [Node.js SDK (Alpha)](https://logitech.github.io/actions-sdk-docs/nodejs/overview/)
- [Logitech Marketplace](https://www.logitech.com/en-us/software/marketplace/developer.html)

---

*Built for DevStudio 2026 by Logitech. February 2026.*
