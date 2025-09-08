# Malleable software is a dead end. Granular software is the answer.

## What is Malleable Software

Malleable software adapts to your process instead of forcing you to adapt to it. In the AI era, you define the problem (the what) and LLMs handle the how — assembling components, mapping flows, and producing a first version to iterate on.

- Turns complexity into an advantage: setup becomes a fast, conversational loop from idea to prototype.
- Enables ongoing change: as needs evolve, the tool bends without high switching costs or rigid walls.
- Contrasts with rigid, opinionated tools where AI can only shave seconds off tasks; flexible platforms let AI design and adapt core workflows.

Why now

- AI collapses weeks of setup into a few prompts, shifting focus from solution design to problem definition.
- Customization becomes fast and easy, so teams stop accepting rigid defaults.

Trajectory (from Dubakov)

- 2025–2027: AI removes steep learning curves; migrations accelerate as processes change.
- 2028–2030: Buying criteria shift from "start fast" to "change easily".
- 2030–2035: Setup feels like a conversation; switching costs collapse; rigid vertical SaaS becomes niche.

Source: Michael Dubakov, “Malleable Software Will Eat the SaaS World” ([link](https://www.mdubakov.me/malleable-software-will-eat-the-saas-world/)).

### Hacker News discussion: support and objections

Support

- AI collapses setup/customization via natural‑language prompts, generating workflows, schemas, and automations; complexity becomes an advantage.
- Process fit beats rigid defaults; malleable tools adapt without waiting on vendor roadmaps.
- Lower switching costs as teams reshape products when needs evolve.
- Power‑user leverage: ops/analyst builders ship internal tooling faster.
- Precedent: spreadsheets and flexible canvases (e.g., Notion/Fibery/Retool) succeed when bespoke processes matter.
- Ecosystem potential with open schemas/components enabling reuse and composability.

Objections

- Customization tax: most teams want sane defaults; extreme flexibility invites yak‑shaving and inconsistency.
- Governance/compliance risk: shadow IT, access misconfigurations, and audit gaps from non‑experts “building”.
- Data integrity/drift: ad‑hoc models break analytics, integrations, and migrations; schema versioning is hard.
- UX fragmentation: DIY components lead to inconsistent experiences and design debt.
- Debuggability/reproducibility: LLM‑generated configs/flows are hard to diff, test, and roll back.
- Safety/security: prompt injection, over‑permissioned automations, and unsafe actions risk production.
- Performance/SLAs: malleable layers can be slower and harder to harden for mission‑critical use.
- Economics: vendors rely on opinionated scope; extreme malleability increases support costs.

Reference: [Hacker News discussion](https://news.ycombinator.com/item?id=45036754).

### Nuances: LLMs and end‑user programming (Geoffrey Litt)

- End‑user programming expands: LLMs turn fuzzy intent into small bits of executable code, letting non‑developers author one‑off solutions and modify existing tools.
- New patterns: one‑off scripts and GUIs, more “build don’t buy,” modding/extensions of existing apps, and recombining parts of apps into bespoke hybrids.
- Interaction models: chat alone is insufficient; many tasks need direct manipulation. Hybrid flows where AI helps construct or adapt UIs on‑the‑fly are promising.
- Programming bottleneck: historical limits (e.g., formulas, scraping) soften as LLMs generate glue code and teach users; AI acts as a “local developer” and tutor.
- Agency over automation: design for user empowerment so reliance on AI decreases over time as users learn the medium (e.g., understanding generated formulas).
- Architectural implications: favor extensible, composable computational media and shared data substrates; plan for iteration, versioning, testing, and explainability of AI‑generated artifacts.

Source: Geoffrey Litt, “Malleable software in the age of LLMs” ([link](https://www.geoffreylitt.com/2023/03/25/llm-end-user-programming)).

## Anatomy of (good) projects

## What is granular

