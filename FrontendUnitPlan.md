## Frontend Architecture Blueprint

### Tech stack

| Layer | Tool | Reason |
|-------|---------|-----------|
| **Framework** | **React 18 with TypeScript** or **Next.js** (for static sites) | 1) Huge community and number of developers (easy to hire); 2) huge ecosystem; 3) SSR/CSR flexibility |
| **State** |  **Zustand + React Query** or **Redux Toolkit + RTK Query** | 1) Separate responsibility (React Query for server state, Zustand for client state); 2) scale easily; 3) performance and size (both libs are not big)  |
| **Styles Lib**   |  **Tailwind** (For prototyping), Custom Component Library build on the top of **MUI** or **React Aria** from Adobe (for production)     | 1) Development speed; 2) Community    |
| **Design System**   |  **Storybook**  | 1) Better communication between design and FE teams; 2) Visual regression for QA   |
|  **Form / Validation**    | **React Hook Form + Zod**  | 1) Easy schema validation; 2) Faster create complex forms; |
|  **Build**  | **Vite** + ESBuild   | 1) Super fast; 2) Optimized for production   |
| **Testing**  | **Vitetest** (for unit testing) + React Testing Lib (for unit and integration testing) **Cypress** (or **Playwright**) (for e2e testing)   | 1) Speed; 2) Utils; 3) Full 3-level Testing Pyramid  |

### Folder structure

```
app/
├─ .storybook/.   # Storybook stuff
├─ public/.   # Assets
├─ src/
│  ├─ app/
│  │  ├─ hooks.ts   # Re‑exports of typed Zustand + React‑Query hooks
│  │  ├─ queryClient.ts   # createQueryClient + config
│  │  ├─ router.tsx   # Router for  TanStackRouter or React‑Router
│  │  ├─ store.ts    # Zustand store
│  │  └─ index.tsx    # ReactDOM.createRoot + <QueryClientProvider/>
│  │
│  ├─ features/.   # **domain (business) feature**
│  │  ├─ auth/
│  │  │  ├─ api.ts   # React‑Query fetchers
│  │  │  ├─ hooks.ts   # login, sessions
│  │  │  ├─ models.ts   # zod schemas
│  │  │  ├─ AuthPage.tsx
│  │  │  └─ __tests__/
│  │  ├─ dashboard/
│  │  │  ├─ components/
│  │  │  ├─ charts/
│  │  │  ├─ DashboardPage.tsx
│  │  │  └─ …
│  │  └─ …
│  │
│  ├─ components/.   # shared UI components
│  │  ├─ Button/
│  │  │  ├─ Button.tsx
│  │  │  ├─ Button.stories.tsx
│  │  │  └─ Button.test.tsx
│  │  └─ …
│  │
│  ├─ lib/.   # helpers (date, i18n, clsx, etc.)
│  ├─ hooks/.    # cross‑feature hooks
│  ├─ icons/.   # SVGs icons
│  ├─ styles/.   # global CSS
│  ├─ assets/.   # Images, fonts
│  └─ types/.   # *.d.ts
│
├─ tests/.   # Cypress e2e
├─ vitest.setup.ts
├─ vite.config.ts
├─ tsconfig.json
└─ package.json
```

### Reusability and scalability

* ADR repository (or separate folder) for each important architecture decision
* Any component methodology, for example **Atomic → Molecule → Organism**
* Module separation for big project (no internal importing between modules, modules should independent = for future it helps to migrate into microfrontend architecture)
* Using Typescript (IntelliSense + compiler catch issues)

### DevOps and CI/CD thoughts

* Cache via Turborepo (less cache size)
* Standard git workflow (main for devs, feature branches, release branches)
* Check flow = lint -> type-check -> unit tests
* E2E testing on the stage or production env (also local e2e testing possibility)
* Error login (Sentry or Rollbar)
* Video screen catching for better production testing (Hotjar (doesn't work for React-native apps) or/and LogRocket)

## Team Setup & Role Definition

* Lead Dev (1 person per team): Architecture, hiring, code review monitoring
* Senior Dev (1-2 persons per team): Critical code section, guarantee feature delivery, code review
* Middle Dev (2-3 per team): Feature delivery (need mentoring from senior or lead), code review, test coverage
* Junior Dev (2-3 per team): Small utils and components (need mentoring from middle and senior devs)
* Design team (1-2 per team)
* QA (1 manual QA for 2-4 devs)

## Best Practices

* ESLint + Prettier (Code should looks like written one persons)
* Convention for commits
* Strict mode code
* Minimum 2 approves for code review (at least 1 should be from senior or lead)
* All core components should be in the Storybook
* Readme (*.md) files for modules or/and pages
* PerfectPixel Chrome extension for pixel perfect tasks

## Workflow

* Standard 2-3 weeks sprints
* Daily meats (10-15 mins) with planning, demo and retro for each sprint
* Poker points for estimating tasks (or ideal days)
* Architecture meats every month
* Dev meats every second week (2 per month)
* One to one meats for each team member each month

## Pre-sales Collaboration Example

* Clearly understanding business goals (propose couple different solutions to clear understand real goals)
* Functional and Non-Functional requirements (SEO, testability, security, speed, main devices, dependencies from 3-d party tools, APIs or companies, caching, authorisation, real‑time updates, internationalization, scalability, usability, availability)
* Budget limitation
* Sorting back log with tasks for 3 months
* Splitting critical (100% delivery features) and nice to have features to minimizing Risk
* Build Storybook components library before designers start working with feature pages (should be UI convention between Design (and/or Product) and FE teams)
* Collaborate with product and/or design team on early development stages for better delivery and estimation, propose faster solution in UI

