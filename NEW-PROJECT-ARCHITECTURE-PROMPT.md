# New Project Architecture Prompt

Use this prompt to define the internal structure of a generic Code App before feature work begins.

## Copy/Paste Prompt

```md
You are designing the architecture for a new Power Apps Code App.

Create a clean architecture that keeps the app maintainable as it grows.

Use these rules:

## 1. Layering model

Design the app with clear layers:

- presentation layer for components and pages
- orchestration layer for hooks/controllers
- domain layer for app types and concepts
- service layer for Dataverse and platform integration
- utility layer for pure reusable logic

## 2. Responsibilities

Apply strict boundaries:

- components render UI and capture user actions
- hooks/controllers manage view state, async workflows, and user messages
- services own external reads, writes, mapping, validation handoff, and error normalization
- domain models represent business concepts independent of the UI framework
- utilities contain pure functions only

## 3. Component design

- Keep components focused and composable.
- Split list, detail, toolbar, search, filter, and summary areas into separate components where appropriate.
- Avoid giant page files that own all state and all logic.

## 4. Hook/controller design

- Use hooks/controllers as the main coordination point for screen-level behavior.
- Keep async action orchestration out of presentation components.
- Centralize loading, success, and error messaging per workflow.

## 5. Service design

- Define a service boundary early, even if the first version is mock-backed.
- Make the UI depend on service contracts, not implementation details.
- Allow services to evolve from mock data to real Dataverse without forcing a large UI rewrite.

## 6. Domain-first thinking

- Model records, filters, workflows, and editable drafts as domain types.
- Separate display models from write payloads.
- Do not let Dataverse wire models become the app's domain language.

## 7. State design

- Keep local UI state local.
- Keep screen workflow state in hooks/controllers.
- Compute derived views from source state using pure helpers.
- Avoid duplicating the same derived data across multiple places.

## 8. File design

Prefer files that each have a single clear job:

- page or screen components
- reusable UI components
- orchestration hooks
- service implementations
- mapping functions
- utility functions
- domain models

## 9. Anti-patterns to avoid

- one file owning UI, CRUD, mapping, validation, and error handling
- React components directly calling platform APIs
- generated Dataverse models leaking into the entire UI
- screen logic duplicated across multiple components
- unstructured state with no clear owner

## 10. Deliverables

Produce:

- architecture summary
- folder layout
- responsibility boundaries
- example screen flow from component to hook to service
- naming guidance for files and modules

## 11. Success criteria

The architecture is only complete if:

- each layer has a clear purpose
- React components stay thin
- service logic is isolated
- domain models are explicit
- the app can scale without a rewrite of core boundaries
```

