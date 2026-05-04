# Med Secure Mind (HealthAssess)

AI-assisted health risk assessment web application built with React, TypeScript, and Tailwind CSS.

> ⚠️ **Medical Disclaimer:** This software is for informational/educational use only. It is **not** a diagnostic tool and must not replace professional medical advice, diagnosis, or treatment.

---

## Overview

Med Secure Mind provides a guided health assessment form where users input:

- personal profile details (name, age, gender)
- symptom description
- symptom duration and severity
- optional medical history

The app generates:

- a **risk score** (0–100)
- an **urgency tier** (`low`, `medium`, `high`, `emergency`)
- a **disease category** guess
- **possible condition statements**
- prioritized **recommendations**

An in-browser Hugging Face model initialization is attempted (`Bio_ClinicalBERT` via `@huggingface/transformers`), but current result generation is deterministic/rule-based and designed to fail open if model loading is unavailable.

---

## Tech Stack

- **Language:** TypeScript
- **Framework:** React 18
- **Build tool:** Vite 5
- **Routing:** React Router 6
- **Styling:** Tailwind CSS + CSS variables
- **UI primitives/components:** shadcn/ui + Radix UI + Lucide icons
- **State/data:** React local state, TanStack Query provider scaffolded
- **Linting:** ESLint 9 + TypeScript ESLint

---

## Project Structure

```text
.
├── src/
│   ├── components/
│   │   ├── HealthAssessment.tsx   # Assessment UI + scoring and recommendation logic
│   │   ├── Header.tsx             # App header/branding
│   │   └── ui/                    # shadcn/ui component set
│   ├── hooks/                     # Shared hooks (toast/mobile)
│   ├── lib/
│   │   └── utils.ts               # Utility helpers (class merge)
│   ├── pages/
│   │   ├── Index.tsx              # Main page composition
│   │   └── NotFound.tsx           # 404 route
│   ├── App.tsx                    # Providers + route table
│   ├── main.tsx                   # App bootstrap
│   └── index.css                  # Global design tokens + base styles
├── public/                        # Static assets
├── CLAUDE.md                      # Architecture context for AI agents
├── README.md                      # This documentation
├── components.json                # shadcn/ui configuration + aliases
├── tailwind.config.ts             # Tailwind theme/extensions
├── vite.config.ts                 # Vite setup + aliasing
└── med-secure-mind-main/          # Duplicate snapshot (non-primary, not active root)
```

---

## Core Assessment Logic (Current Behavior)

The assessment pipeline in `src/components/HealthAssessment.tsx` follows this sequence:

1. Parse and normalize user input.
2. Match symptom text against condition keyword sets.
3. Infer disease category from best matches.
4. Compute risk score using weighted factors:
   - age
   - severity
   - duration
   - emergency/critical keywords
   - medical history presence
5. Map score to urgency:
   - `<25` → `low`
   - `25–44` → `medium`
   - `45–64` → `high`
   - `>=65` → `emergency`
6. Produce recommendation list and summary text.

If model initialization fails (for example, WebGPU/browser constraints), the application continues normally with rule-based analysis.

---

## Getting Started

### Prerequisites

- Node.js 18+ recommended
- npm 9+ (or compatible)

### Install

```sh
npm install
```

### Run development server

```sh
npm run dev
```

Default Vite dev server is configured on port **8080**.

### Build for production

```sh
npm run build
```

### Preview production build

```sh
npm run preview
```

### Lint

```sh
npm run lint
```

---

## Configuration Notes

- Path alias `@/*` resolves to `src/*` (configured in `tsconfig.json` and `vite.config.ts`).
- TypeScript strictness is currently relaxed:
  - `noImplicitAny: false`
  - `strictNullChecks: false`
- Tailwind theme includes medical-oriented design tokens and custom gradients/shadows.

---

## Routing

Defined in `src/App.tsx`:

- `/` → `Index` page
- `*` → `NotFound` fallback

Keep concrete routes above wildcard catch-all.

---

## Security & Privacy Notes

- No backend API or database is currently present in this workspace.
- Assessment processing is client-side.
- There is no persisted auth/session model in the current implementation.
- Do not interpret UI “secure/private” language as regulatory compliance by default.

---

## Known Gotchas

1. **WebGPU/model compatibility:** `device: 'webgpu'` may fail based on browser/hardware support.
2. **Fail-open AI path:** The app still returns assessments if model loading fails.
3. **Duplicate project folder:** `med-secure-mind-main/` exists as a snapshot copy; root folder is the active project.

---

## Suggested Next Improvements (Documentation Roadmap)

- Add automated tests for scoring and urgency thresholds.
- Move medical heuristics to a dedicated domain module for easier validation/versioning.
- Add a backend boundary if persistence, analytics, or audit trails are required.
- Add structured logging and error telemetry.

---

## License

No explicit license file is currently present in this repository.
If open-source distribution is intended, add a `LICENSE` file and update this section.
