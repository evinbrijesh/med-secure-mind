# CLAUDE.md

## Stack
- **Language:** TypeScript
- **Runtime / Tooling:** Node.js, Vite 5
- **Frontend:** React 18, React Router 6
- **UI System:** Tailwind CSS, shadcn/ui (Radix primitives), Lucide icons
- **State / Data:** React local state, TanStack Query provider scaffolded (no remote queries yet)
- **AI Library:** `@huggingface/transformers` loaded client-side (best-effort)
- **Linting:** ESLint 9 + TypeScript ESLint

## Structure
```text
.
├── src/
│   ├── components/
│   │   ├── HealthAssessment.tsx      # Core assessment flow and scoring logic
│   │   ├── Header.tsx                # Top navigation/header branding
│   │   └── ui/                       # shadcn/ui component library
│   ├── hooks/                        # Shared UI hooks (toast, mobile checks)
│   ├── lib/
│   │   └── utils.ts                  # Shared utility helpers (class merging)
│   ├── pages/
│   │   ├── Index.tsx                 # Main landing + assessment page
│   │   └── NotFound.tsx              # 404 route
│   ├── App.tsx                       # Global providers and route table
│   ├── main.tsx                      # React bootstrap
│   └── index.css                     # Global tokens/theme and base styles
├── public/                           # Static assets
├── README.md                         # End-user and developer documentation
├── CLAUDE.md                         # Architecture context for AI/code agents
├── components.json                   # shadcn/ui config + path aliases
├── tailwind.config.ts                # Tailwind tokens/extensions
├── vite.config.ts                    # Vite config, aliasing, dev plugin
└── med-secure-mind-main/             # Duplicate project snapshot (non-primary)
```

## Conventions
- **File naming:**
  - Components/pages: `PascalCase.tsx`
  - Utilities/hooks: `kebab-case.ts` / `kebab-case.tsx`
- **Imports:** Use `@/` alias for `src/*` paths.
- **Component style:** Function components with hooks; colocate logic in feature component unless reused.
- **Error handling:**
  - UI-layer try/catch for async actions.
  - Fail open for AI model loading (fallback to rule-based logic).
- **Logging:** Console logging only (`console.error`/`console.log`) at present; no centralized logger.
- **Routing:** Declare concrete routes above the wildcard fallback in `App.tsx`.

## Known gotchas
- The Hugging Face pipeline is initialized but current assessment output is generated from deterministic rule-based logic; model load failure does not block app behavior.
- `device: 'webgpu'` may fail depending on browser/hardware support; this is expected and handled.
- TypeScript strictness is relaxed in `tsconfig.json` (`noImplicitAny: false`, `strictNullChecks: false`).
- Repository currently includes a nested duplicate folder (`med-secure-mind-main/`) with similar contents; root project is the active workspace.

## Module map
- **`src/components/HealthAssessment.tsx`**
  - Owns form state, symptom parsing, risk scoring, urgency classification, recommendations, and result rendering.
- **`src/pages/Index.tsx`**
  - Owns page composition: hero text, assessment component placement, footer disclaimer.
- **`src/components/Header.tsx`**
  - Owns branded header and static trust/risk cues.
- **`src/App.tsx`**
  - Owns app-level providers (`QueryClientProvider`, tooltips, toasts) and router config.
- **`src/index.css` + `tailwind.config.ts`**
  - Own design tokens and utility mappings for the medical theme.
