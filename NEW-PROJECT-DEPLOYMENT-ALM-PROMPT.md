# New Project Deployment ALM Prompt

Use this prompt when planning how a generic Code App moves from local development into governed Power Platform environments.

## Copy/Paste Prompt

```md
You are defining the deployment and ALM approach for a new Power Apps Code App.

Set up the app so deployment is governed from the beginning, not improvised later.

Use these rules:

## 1. Environment model

Use a standard environment flow:

- Dev
- Test
- Prod

All target environments should have Dataverse enabled where required.

## 2. Solution model

- Use a named non-default solution.
- Reuse the same solution over time.
- Do not create a new solution for every release.

## 3. Deployment path

- Direct push is allowed only into Dev.
- Test and Prod should receive managed promotions through Power Platform Pipelines.
- Do not use direct source-to-Prod deployment as the normal path.

## 4. Build and publish model

- Build the app into `dist/`.
- Publish the built Code App through the Power Platform CLI.
- Treat the code repository as the source of truth for code.
- Treat the Power Platform solution as the governed deployment container.

## 5. Configuration planning

- Separate environment-specific configuration from hard-coded app logic.
- Plan for connection references and environment variables where relevant.
- Do not embed environment assumptions deeply in the UI.

## 6. CI and CD expectations

CI should validate:

- install
- typecheck
- tests
- production build

CD to Dev should:

- authenticate
- build
- publish to the Dev solution

Promotion to Test and Prod should use platform pipelines and approvals.

## 7. Data considerations

- Know that solution promotion does not automatically move business data.
- Plan reference data, seed data, or migration data separately.

## 8. Deliverables

Provide:

- environment model
- solution model
- CI/CD flow
- publish workflow
- promotion workflow
- configuration handling guidance
- data movement caveats

## 9. Anti-patterns to avoid

- deploying from the default solution
- direct source pushes to Prod
- no Test stage
- assuming business data moves with solution deployment
- leaving environment configuration embedded in source code

## 10. Success criteria

The ALM plan is complete only if:

- Dev, Test, and Prod are clearly defined
- the solution strategy is clear
- direct publish is limited to Dev
- governed promotion is defined for later stages
- code and deployment responsibilities are separated cleanly
```

