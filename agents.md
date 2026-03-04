# AGENTS.md

Repository execution guide for AI/code agents working in this project.

## 1) Project Intent

This repository hosts a **React SPA frontend** and a **.NET backend** using **Clean Architecture**.

Current baseline:
- Frontend: Vite + React + TypeScript
- Backend: .NET 10 solution with layered projects
- Repository style: single root with `frontend/` and `backend/`

## 2) Source of Truth: Architecture

The backend follows strict Clean Architecture boundaries:

- `Domain` (core business model)
  - Entities, value objects, domain rules
  - No dependencies on other project layers
- `Application` (use cases)
  - Orchestrates domain behavior
  - Depends only on `Domain`
  - Defines interfaces for external services/persistence
- `Infrastructure` (external concerns)
  - DB access, external services, file systems, integrations
  - Implements interfaces defined in `Application`
  - Depends on `Application` and `Domain`
- `WebApi` (delivery)
  - ASP.NET Core API, DI wiring, request/response contracts
  - Depends on `Application` and `Infrastructure`

Dependency rule (must never be violated):
`WebApi -> Application -> Domain`
`WebApi -> Infrastructure -> (Application, Domain)`

No other direction is allowed.

## 3) Repository Layout

```text
.
├── agents.md
├── README.md
├── backend/
│   ├── AIAgentDemo.slnx
│   ├── src/
│   │   ├── AIAgentDemo.Domain/
│   │   ├── AIAgentDemo.Application/
│   │   ├── AIAgentDemo.Infrastructure/
│   │   └── AIAgentDemo.WebApi/
│   └── tests/
│       ├── AIAgentDemo.Domain.Tests/
│       └── AIAgentDemo.Application.Tests/
└── frontend/
    ├── src/
    ├── public/
    └── package.json
```

## 4) Agent Operating Rules

1. Keep changes minimal and scoped to user ask.
2. Respect layer boundaries; do not add cross-layer shortcuts.
3. Do not introduce backend framework references into `Domain`.
4. Prefer constructor injection and interfaces in `Application`.
5. Keep API contracts in `WebApi`; do not leak infrastructure models to API responses.
6. Create tests in matching test projects when adding business logic.
7. Avoid unrelated refactors while implementing requested work.
8. Prefer explicit names over abbreviations.

## 5) Backend Conventions

### Naming
- Namespaces and projects use `AIAgentDemo.*`
- Interface names start with `I`
- Asynchronous methods end with `Async`

### Project responsibilities
- `AIAgentDemo.Domain`: domain entities/value objects/domain events
- `AIAgentDemo.Application`: use cases, DTOs, interfaces, validation
- `AIAgentDemo.Infrastructure`: persistence adapters + external service adapters
- `AIAgentDemo.WebApi`: controllers/endpoints, middleware, DI composition root

### DI pattern
- Add extension classes named `DependencyInjection` in each composition layer where needed.
- `WebApi` is the composition root and wires all services.

## 6) Frontend Conventions

- Tech stack: React + TypeScript + Vite
- Keep UI components in `frontend/src/components`
- Keep app pages/features in `frontend/src/features` (create when first feature is added)
- Keep API clients in `frontend/src/services`
- Keep shared types in `frontend/src/types`
- Use environment variables via `VITE_*`

Integration rule:
- Frontend communicates with backend via HTTP only (no shared runtime model dependencies).

## 7) Bootstrap & Run Commands

Run from repo root unless stated otherwise.

### Backend

```bash
cd backend
dotnet restore
dotnet build AIAgentDemo.slnx
dotnet test AIAgentDemo.slnx
dotnet run --project src/AIAgentDemo.WebApi
```

### Frontend

```bash
cd frontend
npm install
npm run dev
```

## 8) Quality Gates (Minimum)

Before marking work complete, run:

```bash
# backend
cd backend
dotnet build AIAgentDemo.slnx
dotnet test AIAgentDemo.slnx

# frontend
cd ../frontend
npm run build
```

If linting is configured for changed files/features, run lint too.

## 9) Definition of Done for Agent Tasks

A task is complete only if:
- Code compiles
- Existing tests pass
- New logic includes tests where appropriate
- Architecture boundaries remain intact
- Documentation updated when behavior or setup changes

## 10) Preferred Delivery Pattern (Vertical Slice)

For new features, implement in this order:
1. Domain model/rules (`Domain`)
2. Use case/contracts (`Application`)
3. Infrastructure adapter (`Infrastructure`)
4. API endpoint (`WebApi`)
5. Frontend integration (`frontend/src/services` + UI)
6. Tests for domain/application behavior

## 11) What Not To Do

- Do not move fast by placing business rules in controllers.
- Do not let `Infrastructure` types leak into `Domain`.
- Do not add shared mutable global state in frontend.
- Do not bypass `Application` layer from API to persistence.
- Do not add major tooling changes (state managers, ORMs, frameworks) unless requested.

## 12) Environment Notes

Target runtime is .NET 10 and current environment has .NET 10 SDK installed.
If a contributor does not have .NET 10 available, align the entire solution consistently before changing target frameworks.
