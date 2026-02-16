# FlowPilot — Judging Criteria Alignment Map

## DevStudio 2026 Judging Criteria → FlowPilot Response

The Devpost page lists four judging criteria. This document maps each criterion to specific FlowPilot features and talking points to maximize score coverage.

---

### 1. 🆕 NOVELTY

> *"We want an innovative concept that showcases a new way of interacting using a device and an app/service/web that serves as a new proposition on the market."*

**Score Target: HIGH**

| What judges want | How FlowPilot delivers |
|---|---|
| New interaction paradigm | First plugin that **dynamically reconfigures** all device surfaces based on AI-detected context — buttons, dials, ring, and haptics all change together |
| New market proposition | No existing Marketplace plugin does context-aware surface management. Current plugins are static 1:1 app integrations. FlowPilot is a **meta-plugin** |
| Novel use of device | Combines **all three device surfaces** (keypad, dial, Actions Ring) + haptics as a unified intelligent layer, not siloed per-app |
| Innovation signal | Local LLM inference for **predictive** action surfacing — device anticipates what you need before you ask |

**Key talking point for video:** *"Every plugin on the Logitech Marketplace today gives you a fixed set of buttons for one app. FlowPilot makes the entire device surface intelligent — it watches what you're doing and reconfigures itself in real time."*

**Competitive differentiation:**
- vs. Stream Deck: Static macro boards, no context awareness
- vs. Existing Marketplace plugins: 1 plugin = 1 app, no cross-app orchestration
- vs. Keyboard shortcuts: Memorization-dependent, not adaptive

---

### 2. 💡 IMPACT

> *"We look for meaningful solutions to a relevant need that a sizeable number of people face today. The value proposition needs to be relevant to the target audience and with a clear storytelling narrative for marketing purposes."*

**Score Target: HIGH**

| What judges want | How FlowPilot delivers |
|---|---|
| Relevant need | Context-switching costs knowledge workers **23 minutes per switch** (UC Irvine). Average worker switches 6-9 apps per task. This is a $450B/year productivity loss (IDC) |
| Sizeable audience | **28M software developers** (primary), **10M+ designers**, millions of PMs and content creators — all existing or potential Logitech MX customers |
| Clear value prop | "Your device always shows the right controls for right now" — one sentence, instantly understood |
| Marketing narrative | **"FlowPilot: Your Workflow, On Autopilot"** — clean brand story about removing friction between intent and action |

**Key talking point for video:** *"Every knowledge worker loses over an hour a day to context switching. FlowPilot eliminates that friction by making your Logitech devices adapt to you — not the other way around."*

**Impact metrics to cite:**
- 23 min average refocus time per context switch (UC Irvine, Gloria Mark)
- 6-9 app switches per task for average knowledge worker
- 40M+ Logi Options+ users = massive distribution channel via Marketplace

---

### 3. ✅ VIABILITY

> *"We evaluate the technical feasibility and the potential for the concept to be developed into a working product."*

**Score Target: HIGH**

| What judges want | How FlowPilot delivers |
|---|---|
| Technical feasibility | **Every component uses existing, proven technology**: Actions SDK (official), Win32 APIs (decades-old), ONNX Runtime (production-grade), Microsoft Graph (GA) |
| Buildable with SDK | Core plugin uses **standard Actions SDK patterns**: Commands, Adjustments, Dynamic Folders, Plugin Actions, Haptics — all documented |
| Realistic scope | Phase 1 = rule-based context switching (no AI needed). AI prediction is Phase 2 enhancement. MVP is fully buildable in 4 weeks |
| Path to product | Natural fit for Logitech Marketplace distribution. JSON recipe format enables community ecosystem. Freemium model: free core + premium AI features |

**Key talking point for video:** *"FlowPilot isn't science fiction. Every component — window detection, the Actions SDK, local AI inference — is production-ready technology today. We're just combining them in a way nobody has before."*

**Technical proof points:**
- Actions SDK supports dynamic button labels, icon changes, and profile switching (documented features)
- ONNX Runtime runs Phi-3-mini at <500ms latency on consumer CPUs (Microsoft benchmarks)
- Win32 `SetWinEventHook` provides <50ms window change notifications (OS-level guarantee)
- Plugin data storage API enables pattern learning persistence

**Phased delivery plan:**
| Phase | Scope | Timeline |
|---|---|---|
| MVP | Rule-based app detection → static profile switching | 4 weeks |
| v1.0 | Content-aware profiles + dial/haptics integration | +3 weeks |
| v1.5 | Local AI prediction + pattern learning | +4 weeks |
| v2.0 | Community recipe marketplace + cross-app sequences | +4 weeks |

---

### 4. 🎬 PRESENTATION

> *"Quality of the pitch video and text description."*

**Score Target: HIGH**

| What judges want | How FlowPilot delivers |
|---|---|
| ~1 min video | Tight script: Problem (15s) → Solution demo (25s) → Architecture (10s) → Impact (10s) |
| Clear text description | Structured Devpost text with headers, bullet points, and concrete examples |
| Professional quality | Clean screen recordings + architecture diagrams + consistent branding |
| Judges watch ≤1 min | **Hook in first 5 seconds**: show the device surface morphing in real-time as apps switch |

**Video structure (60 seconds):**
```
[0:00-0:05]  HOOK: Split-screen — left: user switches apps, right: MX Console buttons morph in real-time
[0:05-0:15]  PROBLEM: "Knowledge workers lose 1+ hour/day to context switching..."
[0:15-0:40]  DEMO: Walk through 3 context switches (Slack→VS Code→Figma), show device surface adapting each time
[0:40-0:50]  ARCHITECTURE: Quick diagram — Sensing → Reasoning → Surface Management
[0:50-0:60]  CLOSE: "FlowPilot. Your workflow, on autopilot." + team + category
```

---

## Risk Matrix & Mitigations

| Risk | Severity | Mitigation |
|---|---|---|
| "Too complex for Phase 1 concept" | Medium | Emphasize phased delivery; MVP is just rule-based switching (no AI) |
| "Privacy concerns with screen monitoring" | Medium | Stress **local-only** processing, no cloud, no data exfiltration |
| "Actions SDK may not support dynamic relabeling" | Low | SDK docs confirm dynamic button labels, icons, and profile switching |
| "987 participants = crowded field" | High | Differentiate on **meta-plugin** positioning — most entries will be single-app plugins |
| "No working prototype for Phase 1" | None | Phase 1 explicitly requires **concept pitch only**, not code |

---

## Judge Persona Mapping

Based on the listed judges:

| Judge | Role/Background | FlowPilot angle to emphasize |
|---|---|---|
| Paul Fitzsimons | Logitech leadership | Business impact, Marketplace potential, user reach |
| Jean-Michel Chardon | Engineering | Technical feasibility, SDK usage depth, architecture quality |
| Mario Gutierrez | Product | User experience, workflow improvement, market fit |
| Mireno Rossi | Engineering | Performance targets, implementation plan, code architecture |
| Mathieu Meisser | Innovation | Novelty of approach, AI integration, future vision |
| Mary Peppi | Marketing | Storytelling, brand narrative, target audience clarity |
| Felix Hartwigsen | Design | UX of adaptive surfaces, haptic design, visual polish |
| Joanna Similä | Logitech | Ecosystem fit, Marketplace readiness, strategic alignment |

---

*Judging alignment map for DevStudio 2026 Phase 1 concept submission.*
