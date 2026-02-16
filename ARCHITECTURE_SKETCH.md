# FlowPilot — Architecture Sketch

## System Architecture

```
╔══════════════════════════════════════════════════════════════════════╗
║                        FlowPilot Plugin                             ║
║                    (Actions SDK C# Plugin)                          ║
╠══════════════════════════════════════════════════════════════════════╣
║                                                                      ║
║  ┌─────────────────────────────────────────────────────────────┐    ║
║  │                    LAYER 1: SENSING                          │    ║
║  │                                                              │    ║
║  │  ┌──────────────┐ ┌──────────────┐ ┌──────────────────────┐ │    ║
║  │  │ Window       │ │ Content      │ │ Temporal Context     │ │    ║
║  │  │ Monitor      │ │ Analyzer     │ │                      │ │    ║
║  │  │              │ │              │ │ • System clock       │ │    ║
║  │  │ • Win32 API  │ │ • UI Auto-   │ │ • Calendar (Graph)  │ │    ║
║  │  │ • Active     │ │   mation     │ │ • Meeting detector   │ │    ║
║  │  │   process    │ │ • Title/URL  │ │ • Focus timer        │ │    ║
║  │  │ • Window     │ │   parsing    │ │                      │ │    ║
║  │  │   title      │ │ • File type  │ │                      │ │    ║
║  │  └──────┬───────┘ └──────┬───────┘ └──────────┬───────────┘ │    ║
║  │         └────────────────┼────────────────────┘              │    ║
║  └──────────────────────────┼──────────────────────────────────┘    ║
║                             ▼                                        ║
║  ┌─────────────────────────────────────────────────────────────┐    ║
║  │                    LAYER 2: REASONING                        │    ║
║  │                                                              │    ║
║  │  ┌──────────────────────┐  ┌──────────────────────────────┐ │    ║
║  │  │ Rule Engine          │  │ AI Prediction Engine         │ │    ║
║  │  │                      │  │                              │ │    ║
║  │  │ • App → Profile      │  │ • ONNX Runtime              │ │    ║
║  │  │   mapping (fast)     │  │ • Phi-3-mini (3.8B)         │ │    ║
║  │  │ • User-defined       │  │ • Sequence prediction        │ │    ║
║  │  │   overrides          │  │ • Action ranking             │ │    ║
║  │  │ • Workflow recipes   │  │ • Pattern learning           │ │    ║
║  │  │   (JSON)             │  │                              │ │    ║
║  │  └──────────┬───────────┘  └──────────────┬───────────────┘ │    ║
║  │             └──────────┬──────────────────┘                  │    ║
║  └────────────────────────┼────────────────────────────────────┘    ║
║                           ▼                                          ║
║  ┌─────────────────────────────────────────────────────────────┐    ║
║  │                 LAYER 3: SURFACE MANAGEMENT                  │    ║
║  │                                                              │    ║
║  │  ┌────────────────┐ ┌──────────────┐ ┌────────────────────┐ │    ║
║  │  │ Button Manager │ │ Dial Manager │ │ Haptics Controller │ │    ║
║  │  │                │ │              │ │                    │ │    ║
║  │  │ • Set labels   │ │ • Set mode   │ │ • Confirm pulse   │ │    ║
║  │  │ • Set icons    │ │ • Set range  │ │ • Warning buzz    │ │    ║
║  │  │ • Bind actions │ │ • Set detents│ │ • Scroll detents  │ │    ║
║  │  │ • Multi-state  │ │ • Bind adj.  │ │ • Pattern library │ │    ║
║  │  └────────────────┘ └──────────────┘ └────────────────────┘ │    ║
║  └─────────────────────────────────────────────────────────────┘    ║
║                                                                      ║
║  ┌─────────────────────────────────────────────────────────────┐    ║
║  │                 LAYER 4: EXECUTION                           │    ║
║  │                                                              │    ║
║  │  ┌────────────────┐ ┌──────────────┐ ┌────────────────────┐ │    ║
║  │  │ Single-App     │ │ Cross-App    │ │ System Actions     │ │    ║
║  │  │ Commands       │ │ Sequences    │ │                    │ │    ║
║  │  │                │ │              │ │ • DND toggle       │ │    ║
║  │  │ • Keyboard     │ │ • "Ship It"  │ │ • Volume/mic      │ │    ║
║  │  │   simulation   │ │   pipeline   │ │ • Screenshot       │ │    ║
║  │  │ • API calls    │ │ • "Review"   │ │ • Clipboard mgmt  │ │    ║
║  │  │ • CLI commands │ │   workflow   │ │ • App launcher     │ │    ║
║  │  └────────────────┘ └──────────────┘ └────────────────────┘ │    ║
║  └─────────────────────────────────────────────────────────────┘    ║
╚══════════════════════════════════════════════════════════════════════╝

          ▼                    ▼                    ▼
  ┌──────────────┐   ┌──────────────┐   ┌──────────────────┐
  │ MX Creative  │   │ MX Master4   │   │ Actions Ring     │
  │ Console      │   │              │   │ (On-Screen       │
  │              │   │ • Thumb btn  │   │  Overlay)        │
  │ • 9 buttons  │   │ • Scroll     │   │                  │
  │ • Main dial  │   │ • Gestures   │   │ • Top-3 actions  │
  │ • Touch ring │   │ • Haptics    │   │ • Swipe execute  │
  └──────────────┘   └──────────────┘   └──────────────────┘
```

## Data Flow — Context Switch Example

```
User switches from Slack → VS Code (file: auth.ts)
         │
         ▼
[1] Window Monitor detects: process="Code.exe", title="auth.ts - myproject"
         │
         ▼
[2] Content Analyzer: language=TypeScript, git_status=modified, branch=feature/auth
         │
         ▼
[3] Rule Engine: VS Code + TypeScript → "Coding" profile
    AI Engine: user pattern = "after Slack, usually runs tests first" → boost "Run Tests"
         │
         ▼
[4] Surface Manager reconfigures in <100ms:
    • Button 1: Run Tests (★ predicted)    • Button 6: Toggle Terminal
    • Button 2: Git Commit                  • Button 7: Open PR
    • Button 3: Git Push                    • Button 8: Format Document
    • Button 4: Toggle Sidebar              • Button 9: Command Palette
    • Button 5: Split Editor
    • Dial: Scroll through open tabs (haptic detent per tab)
    • Actions Ring: [Run Tests] [Git Commit] [Toggle Terminal]
         │
         ▼
[5] Haptics: Single soft pulse confirms surface loaded
```

## Technology Stack

| Component | Technology | Rationale |
|---|---|---|
| Plugin Core | C# / .NET 8 | Actions SDK primary language, mature ecosystem |
| AI Inference | ONNX Runtime + Phi-3-mini | Local-only, no cloud dependency, CPU-capable |
| Window Detection | Win32 API (P/Invoke) | Native, zero-latency, no dependencies |
| Content Analysis | UI Automation + regex | Lightweight, no screen capture needed |
| Calendar | Microsoft Graph API | Optional, OAuth2 consent flow |
| Workflow Recipes | JSON schema | Human-readable, shareable, versionable |
| Pattern Storage | Actions SDK LocalStorage | Persists across sessions, per-user |
| Icons | SVG via Actions SDK Icon API | Dynamic generation per context |

## File Structure (Planned)

```
FlowPilot/
├── FlowPilot.sln
├── src/
│   ├── FlowPilot.Plugin/           # Actions SDK plugin entry
│   │   ├── FlowPilotPlugin.cs      # Plugin lifecycle
│   │   ├── Actions/
│   │   │   ├── DynamicCommand.cs    # Context-aware button action
│   │   │   └── DynamicAdjustment.cs # Context-aware dial action
│   │   └── manifest.json           # Plugin manifest
│   ├── FlowPilot.Context/          # Sensing layer
│   │   ├── WindowMonitor.cs
│   │   ├── ContentAnalyzer.cs
│   │   └── TemporalContext.cs
│   ├── FlowPilot.Reasoning/        # AI + rules layer
│   │   ├── RuleEngine.cs
│   │   ├── AIPredictionEngine.cs
│   │   └── WorkflowRecipe.cs
│   └── FlowPilot.Surfaces/         # Device surface management
│       ├── ButtonManager.cs
│       ├── DialManager.cs
│       └── HapticsController.cs
├── recipes/                         # Community workflow recipes
│   ├── developer-fullstack.json
│   ├── designer-figma.json
│   └── pm-agile.json
├── models/                          # Local AI model (ONNX)
│   └── phi3-mini-context.onnx
└── tests/
    └── FlowPilot.Tests/
```

## Performance Targets

| Metric | Target | Method |
|---|---|---|
| Context switch detection | < 50ms | Win32 event hooks |
| Surface reconfiguration | < 100ms | Pre-computed profiles |
| AI prediction latency | < 500ms | ONNX quantized model |
| Memory footprint | < 200MB | Model quantization + lazy loading |
| Battery impact | Negligible | Event-driven, not polling |

---

*Architecture sketch for DevStudio 2026 Phase 1 concept submission.*
