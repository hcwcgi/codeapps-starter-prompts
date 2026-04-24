# New Project Testing Quality Prompt

Use this prompt to set the quality bar for a generic Code App before the feature set gets large.

## Copy/Paste Prompt

```md
You are defining the testing and quality strategy for a new Power Apps Code App.

Build a quality approach that proves the important behavior without creating unnecessary test overhead.

Use these rules:

## 1. Quality gates

At minimum, the project should support:

- type checking
- automated tests
- production build verification

The default workflow should include:

- `npm run typecheck`
- `npm test`
- `npm run build`

## 2. Test priorities

Prioritize testing:

- pure utility logic
- mapping functions
- validation logic
- service-layer behavior
- critical orchestration flows

UI tests should focus on important user workflows, not every visual detail.

## 3. Architecture for testability

- Keep pure logic in utilities where possible.
- Keep mapping and normalization functions independent and testable.
- Keep service boundaries explicit.
- Avoid burying important logic inside large UI components.

## 4. Types as quality tooling

- Use TypeScript strictly.
- Treat type errors as broken contracts, not optional warnings.
- Use domain types and payload types intentionally.

## 5. Verification expectations

Before a feature is considered complete, verify:

- valid data path
- invalid data path
- error path
- save path
- read-back or refresh behavior

## 6. Build confidence

- Make production build success part of the standard check, not an afterthought.
- Assume hosted deployment will reveal issues if build assumptions are weak.

## 7. Deliverables

Provide:

- testing strategy
- test prioritization rules
- example targets for unit and workflow tests
- type-safety rules
- pre-deployment verification checklist

## 8. Anti-patterns to avoid

- relying on manual testing only
- testing only the happy path
- skipping mapping and validation tests
- placing logic in components where it is hard to verify
- treating build success as separate from quality

## 9. Success criteria

The quality strategy is complete only if the project can prove:

- contracts are type-safe
- important logic is tested
- critical workflows are verified
- production builds remain stable
```

