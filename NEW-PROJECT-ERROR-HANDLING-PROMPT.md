# New Project Error Handling Prompt

Use this prompt to define how a generic Code App should detect, normalize, surface, and recover from failures.

## Copy/Paste Prompt

```md
You are defining the error-handling strategy for a new Power Apps Code App.

Create an approach that keeps technical failures understandable to users and manageable for developers.

Use these rules:

## 1. Error ownership

- Services normalize platform and backend errors.
- Hooks/controllers decide how errors affect the current workflow.
- Components render clear user-facing messages.

## 2. Error categories

Design for at least these categories:

- missing configuration
- datasource binding failure
- validation failure
- lookup resolution failure
- save failure
- post-save read-back failure
- permission failure
- unexpected runtime error

## 3. Normalization

- Convert raw platform errors into consistent application errors.
- Preserve technical detail for debugging where useful.
- Avoid pushing raw low-level messages straight into the UI.

## 4. User messaging

- Show user-friendly messages first.
- Tell users what happened and what they can do next.
- Avoid vague messages like "something went wrong" unless absolutely necessary.

## 5. Recovery design

- Decide whether a workflow should retry, refetch, partially recover, or stop.
- Build resilient save flows so successful writes do not crash the page if read-back fails.
- Keep the UI usable after recoverable failures.

## 6. Error boundaries and stability

- Prevent one broken workflow from taking down the whole app where possible.
- Handle unexpected rendering or async failures gracefully.
- Keep the app state consistent after failure.

## 7. Logging and developer usefulness

- Keep enough detail for debugging.
- Identify the failed workflow and the affected record or datasource where relevant.
- Make recurring failure patterns easier to diagnose.

## 8. Deliverables

Provide:

- error taxonomy
- normalization strategy
- user-facing messaging approach
- recovery rules
- save-flow failure handling
- unexpected runtime stability plan

## 9. Anti-patterns to avoid

- raw API errors shown directly to users
- swallowed failures with no visible feedback
- save success followed by crash on record read-back
- inconsistent error wording across screens
- workflows that leave the app in an ambiguous state

## 10. Success criteria

The error strategy is complete only if:

- failures are categorized clearly
- user messages are understandable
- technical detail remains available for debugging
- the app survives recoverable failures gracefully
```

