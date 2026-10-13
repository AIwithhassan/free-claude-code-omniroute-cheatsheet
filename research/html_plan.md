# Design Plan for Report HTML: Open-Weight Coding Models vs Paid Agents (2026)

## 1. Audience & Purpose
- **Audience**: Engineering leaders, CTOs, senior software architects, and AI practitioners weighing the trade-offs between hosting open-weight coding models and paying for enterprise SaaS coding assistants.
- **Purpose**: Provide a definitive, data-backed, interactive decision document that cuts through the hype, clarifies the 2026 capability divide, and outlines the emerging dual-tier software engineering architecture.

---

## 2. Content Signals & Domain Character
- **Domain**: Developer tooling, artificial intelligence, software engineering systems, high-performance computing, open-source ecosystems.
- **Visual Metaphors / Textures**: Terminal interfaces, system telemetry graphs, silicon wafer dark mode, IDE code editors, monospaced tabular benchmarks, hardware topology diagrams.
- **Emotional Temperature**: Precision, empirical rigor, engineering realism, analytical clarity (no generic corporate gradients or empty marketing fluff).

---

## 3. Short Design Directions (Brainstorming)

### Direction A: "Terminal Telemetry" (Dark Developer HUD) [SELECTED]
- **Content Link**: Reflects the native environment of software engineers—terminal debuggers, dark-mode IDEs, and GPU telemetry dashboards.
- **Color Token System**:
  - `--bg-canvas`: `#0d1117` (Deep GitHub/Obsidian dark gray)
  - `--bg-surface`: `#161b22` (Subtle elevated panel)
  - `--bg-elevated`: `#21262d` (Interactive hover & card border)
  - `--text-primary`: `#f0f6fc` (Crisp high-contrast off-white)
  - `--text-secondary`: `#8b949e` (Muted technical slate)
  - `--accent-cyan`: `#58a6ff` (Terminal prompt / benchmark highlight)
  - `--accent-emerald`: `#3fb950` (Open-weights victory / parity indicator)
  - `--accent-amber`: `#d29922` (Hardware warning & cost trade-off indicator)
  - `--border-subtle`: `#30363d` (Precise 1px technical border)
- **Type**:
  - Display: `Geist`, `-apple-system`, `BlinkMacSystemFont`, `Segoe UI`, sans-serif
  - Body: `Inter`, `SF Pro Text`, sans-serif
  - Mono / Tabular: `JetBrains Mono`, `SF Mono`, `Menlo`, monospace (`tabular-nums`)
- **Layout**: Sticky technical header with live benchmark selector, dual-pane comparative matrix, interactive hardware TCO calculator widget, and structured collapsible citation drawer.
- **Signature**: Interactive "2026 Model Dual-Tier Selector" showing where each model sits on the Capability vs. Infrastructure Friction spectrum.

### Direction B: "Academic Whitepaper Modern" (Clean Architectural Light Mode)
- **Content Link**: Formal research lab report, reminiscent of DeepMind/Anthropic research papers.
- **Color Token System**: `--bg-canvas: #f8fafc`, `--text-primary: #0f172a`, `--accent: #2563eb`.
- **Reason Not Selected**: Too sterile; misses the vibrant hacker ethos of the GitHub/Hugging Face ecosystem.

---

## 4. Distinctiveness Review
- **Why Direction A Wins**: Perfectly matches the subject matter (coding agents, GPU hardware, Hacker News discussions) while offering superior contrast for tables, code snippets, diff blocks, and benchmark visualizations.
- **Generic Trap Avoidance**: Replaced generic purple/pink gradients with crisp hardware/terminal metrics, functional monospace tables, and semantic badge indicators.

---

## 5. Information Architecture & Section Flow
1. **Hero & Executive Telemetry**: Key verdict banner ("Dual-Tier Hybrid Architecture in 2026"), quick-glance KPI metrics (SWE-bench delta, VRAM cliff, TCO comparison).
2. **Section 1: Research Benchmarks & Quantitative Coding Capability**: SWE-bench Verified bar chart comparison, LiveCodeBench Pass@1 analysis, Aider multi-file diff adherence breakdown.
3. **Section 2: Hugging Face & Open-Source Agent Ecosystem**: Hugging Face download dynamics, scaffolding matrix (Aider, Cline, OpenHands, Continue), local inference runtimes (vLLM, Ollama), hardware/VRAM requirements matrix.
4. **Section 3: Developer Sentiment & Real-World Pragmatics**: Hacker News practitioner consensus, failure modes analysis (diff syntax drift, lost-in-the-middle, API hallucination), enterprise privacy & compliance drivers.
5. **Section 4: Strategic Synthesis & Decision Matrix**: Interactive feature comparison matrix, engineering leadership decision tree, and concluding strategic recommendations.
6. **References & Citation Index**: Filterable numbered references with direct external links.

---

## 6. Design Contract & Technical Specifications
- **Single-file HTML**: Standalone self-contained HTML with embedded styles, SVG iconography, and lightweight vanilla JS for table filtering, tab switching, and citation popovers.
- **Accessibility & Responsiveness**: Clean semantic tags (`<header>`, `<main>`, `<section>`, `<article>`, `<table>`), full mobile responsiveness with scrollable tables, high WCAG AAA contrast ratios.
- **Zero Hallucination / Fact Fidelity**: Every statistic, score, and citation directly maps to `report.md` and `citations.json`.
