# LLM Playbook for Mobile App Project Development
_A practical, copy‑pasteable guide you can use during planning sessions, design reviews, and sprint execution._

**Audience:** Mobile engineers, PMs, designers, and data/ML folks.  
**Format:** Ready-to-present Markdown (use headings as slide sections).  
**Running example:** “AI Photo Finder” (Flutter, on‑device ML, optional LLM for query parsing).

---

## 0) How to use this playbook
- In a meeting: project the headings and run the prompts live; capture outputs back into the doc.
- Solo work: duplicate this file, replace the `{{braces}}` with your project details, and paste the prompts to your LLM.
- Each section contains: purpose → short checklist → a few high‑leverage prompts → acceptance criteria.

> **Tip:** Keep one “Project Card” (Section 1) updated. Paste it at the top of every prompt so the LLM always has the same context.

---

## 1) Project Card (paste this into every prompt)
```text
Project Name: {{Name}}
One‑liner: {{What are we building in one sentence?}}
Problem: {{What pain point is this solving? Who feels it?}}
Primary Users: {{e.g., consumers with large photo libraries}}
Top 3 Goals: {{e.g., natural language search, face grouping, on‑device privacy}}
Non‑Goals (MVP): {{e.g., editing, social sharing, cloud sync}}
Platforms: {{Android + iOS via Flutter}}
Constraints: {{privacy‑first, on‑device inference, budget, API limits}}
Performance Targets: {{e.g., <150 ms query on 10k–30k photos}}
Risks: {{e.g., background indexing power limits, iOS BGTask quotas}}
Key Decisions locked: {{e.g., SQLite + sqlite‑vec, ONNX Runtime, ML Kit}}
```

**Checklist**
- [ ] Goals + Non‑goals fit MVP scope
- [ ] Performance targets are testable
- [ ] Privacy constraints explicit (what never leaves device?)

---

## 2) PRD / Vision (fast “first draft”)
**Purpose:** Get a clean starting document you can iterate on together.

**Prompt — PRD (use with your Project Card)**
```
You are a principal PM. Draft a crisp PRD for {{Project Name}}.
Organize as: Problem, Users, Goals, Non‑Goals, Scenarios/UX flows, Success metrics, Open questions.
Keep under 900 words. Avoid marketing language. Call out assumptions and unknowns.
```

**Acceptance Criteria**
- Clear problem statement, realistic success metrics, explicit unknowns.

---

## 3) Architecture & Tech Stack (mobile‑first)
**Purpose:** Turn goals into a concrete architecture with trade‑offs and mobile constraints.

**Prompt — Architecture (Flutter, on‑device ML, optional cloud)**
```
You are a staff mobile architect. Propose a privacy‑first architecture for {{Project Name}} using Flutter.
Cover: client layers, background indexing/processing, local DB, vector search, model loading, and optional cloud APIs.
Show module boundaries and where platform‑specific code is needed (Android/iOS). List 3 major risks + mitigations.
Return as Markdown with a labeled diagram (ASCII) plus a dependency list.
```

**Prompt — Tech Stack Selection**
```
Given the Project Card, select concrete technologies for:
- UI framework and state management,
- Media access, background work managers,
- On‑device inference runtime and model formats,
- Storage (including vector search),
- Optional cloud APIs (LLM, analytics, crash reporting).
For each choice, explain why it's fit for mobile and note 1 alternative.
```

**Acceptance Criteria**
- On‑device vs cloud concerns are explicit.
- Trade‑offs called out (e.g., sqlite‑vec vs dedicated vector DB, ONNX vs TFLite).

---

## 4) Data Model & Contracts
**Purpose:** Make the app’s vocabulary precise; keep it stable as code evolves.

**Prompt — Database Schema**
```
Design a normalized SQLite schema for {{Project Name}} to support {{key features}}.
Include tables, columns, types, primary/foreign keys, and which ones are virtual/vector indexes.
Add 5 example queries (CRUD + vector search). Note WAL/FTS/pragma considerations for mobile.
```

**Prompt — API/JSON Contracts**
```
Define stable JSON contracts for any inter‑module calls (e.g., query parser → client filters).
Return a concise JSON Schema per contract and 2 positive + 1 negative example each.
```

**Acceptance Criteria**
- Schema captures core entities and future‑proofs optional fields.
- Example queries and contracts are executable/valid JSON.

---

## 5) Milestones, Estimates, Risks
**Purpose:** Create a doable plan you can track.

**Prompt — Milestones & Estimates**
```
Draft 6–12 milestones for {{Project Name}} from “hello grid view” to “beta”.
For each: objective, exit criteria, testable demo, and t‑shirt size (S/M/L). Include a parallelizable path.
```

**Prompt — Risk Register**
```
List top 10 risks (tech, product, compliance). For each, probability/impact (H/M/L), early warning signal, and mitigation.
```

**Acceptance Criteria**
- Exit criteria are demo‑able; risks have concrete mitigations.

---

## 6) Engineering Prompts (Flutter + mobile constraints)
**Purpose:** Get scaffolds that are close to production and respect platform realities.

**Prompt — Flutter Scaffolding**
```
Generate idiomatic Flutter scaffolding for a 3‑tab app: Search, People, Tags.
Use Riverpod or Provider for state. Add a grid view optimized for 10k+ items (builder pattern, cached thumbnails).
Include a basic DI setup and a repository interface for DB access.
```

**Prompt — Background Indexer (cross‑platform)**
```
Provide a production‑ready background indexing strategy for Flutter using workmanager.
Explain Android WorkManager constraints and iOS BGTask scheduling limits.
Show how to batch work, pause/resume, and surface progress to the UI without jank.
Provide Dart code for registering/scheduling tasks and a stub task handler.
```

**Prompt — On‑Device Inference (ONNX Runtime)**
```
Show how to bundle quantized ONNX models in Flutter and initialize sessions once.
Give a helper for embedImage()/embedText(). Include memory/perf notes and warm‑up strategy.
```

**Prompt — Vector Search with SQLite**
```
Show how to load a prebuilt sqlite-vec extension in Flutter (FFI), create virtual tables, and run MATCH queries.
Include a safe fallback if the extension fails to load on iOS, and a tiny benchmark harness.
```

**Acceptance Criteria**
- Code compiles with minimal edits; includes platform caveats and error handling.

---

## 7) Privacy, Security, and Compliance
**Purpose:** Bake in privacy early; avoid last‑minute fire drills.

**Prompt — Privacy Checklist**
```
Given the Project Card, produce a privacy and security checklist.
Cover: data minimization, on‑device processing, consent screens, “opt‑in” cloud features, data reset/export, and app lock.
Propose wording for user‑facing toggles and a brief in‑app privacy explainer.
```

**Prompt — Threat Modeling (lite)**
```
Run a lightweight STRIDE‑style review for {{Project Name}}.
List plausible threats, affected components, and simple mitigations appropriate for a mobile‑only MVP.
```

**Acceptance Criteria**
- Opt‑in semantics are plain‑language; sensitive flows have controls and logs.

---

## 8) Evaluation & QA
**Purpose:** Verify the system is fast, relevant, and robust.

**Prompt — Perf Test Plan**
```
Create a test plan to measure indexing throughput and query latency on mid‑range devices.
Include synthetic data generation, test harness setup, and pass/fail thresholds.
```

**Prompt — Relevance & Quality Checks**
```
Propose a relevance evaluation method for semantic search: hold‑out queries, human annotation rubric, and acceptance gates.
```

**Prompt — Accessibility & UX QA**
```
Checklist for mobile a11y (contrast, focus order, semantics), empty/error/loading states, and large library scroll perf.
```

**Acceptance Criteria**
- Clear pass/fail thresholds; repeatable harness and datasets.

---

## 9) Red Teaming & Design Critique
**Purpose:** Pressure‑test the plan before you write lots of code.

**Prompt — Design Critique**
```
As a skeptical staff engineer, critique the architecture and call out brittle parts, hidden costs, or scaling traps.
Offer safer alternatives with trade‑offs. Keep it blunt but constructive.
```

**Prompt — Assumption Hunt**
```
List all assumptions in the PRD/architecture. Tag each as validated/unvalidated and suggest the fastest validation.
```

**Acceptance Criteria**
- A handful of changes that reduce risk or scope without killing the vision.

---

## 10) Meeting Agenda Template (60–90 min)
1. 5’ — Goal & success criteria for session  
2. 10’ — Paste Project Card; agree on constraints and targets  
3. 25’ — Run 1–2 core prompts (PRD or Architecture); capture outputs  
4. 10’ — Risks & unknowns (prompt from Section 5)  
5. 10’ — Next steps & owners (convert outputs into issues)  
6. 5’ — Feedback: what to try differently next time

> **Facilitation tips:** Timebox, keep edits in the doc, @‑assign owners immediately.

---

## 11) Reusable “Master Prompt” (spin up a full plan)
```
You are a cross‑functional trio (PM, Staff Mobile Engineer, UX Lead). Using the Project Card below,
produce a concise but detailed plan for an MVP mobile app in Flutter that is privacy‑first and performant.

Include:
- PRD (Problem, Users, Goals, Non‑Goals, 3 key flows)
- Architecture (client layers, background work, data model, on‑device vs cloud)
- Tech Stack choices with 1 alternative each and why
- Database schema (tables + 3 example queries)
- 8–10 milestones with exit criteria
- Top 8 risks + mitigations
- A privacy & consent section users will actually read

Constraints: no hand‑waving, no invented APIs; reference platform constraints where relevant.
Return Markdown with clear headings and bullet points.
[Paste Project Card here]
```

---

## 12) Example prompts tailored to “AI Photo Finder”
Use these when working on a gallery search app with on‑device ML + optional LLM parsing.

**Query Parser Contract**
```
Design a compact JSON schema for natural‑language photo queries.
Fields: {people[], tags[], date_range[], location?, has_ocr?, query_text}.
Return 3 examples and validation rules (types, ranges).
```

**Background Indexing Strategy**
```
Propose a staged indexing pipeline for 30k photos with pause/resume and charger/Wi‑Fi constraints.
Explain iOS BGTask time limits and Android WorkManager retries. Include telemetry we should log.
```

**People Clustering Approach**
```
Given face embeddings on‑device, propose a robust clustering and labeling workflow (incl. merge/split).
Add UX fail‑safes and how to avoid irreversible merges.
```

**Vector Search Tuning**
```
Suggest a scoring function that blends vector similarity with filter boosts (tags/people/recency).
Show a small numeric example and a tunable set of weights.
```

---

## 13) Anti‑patterns & Pitfalls
- Vague inputs → vague outputs. Always paste the Project Card.
- Letting the LLM drift the scope; keep Non‑Goals visible.
- Asking for giant dumps of code; prefer small, testable scaffolds.
- Ignoring mobile constraints (background limits, memory, binary sizes).
- Prompting for “step‑by‑step chain of thought” vs asking for **concise rationale and assumptions**.

---

## 14) Minimal Prompt Hygiene
- Pin style: “Return Markdown with headings/bullets only.”
- Ask for **assumptions + unknowns** explicitly.
- Set constraints: privacy, on‑device first, performance budgets.
- Keep a **Prompt Log** in your repo (copy outputs; diff changes across iterations).
- Never paste secrets or personal data. Rotate keys used in demos.

---

## 15) Done‑Definition for Project Docs
- [ ] PRD reviewed by PM/Eng/Design
- [ ] Architecture reviewed by two senior engineers
- [ ] Schema & contracts validated with sample data
- [ ] Milestones have owners + dates
- [ ] Risk register exists and is tracked
- [ ] Privacy copy approved
```

# End of playbook
