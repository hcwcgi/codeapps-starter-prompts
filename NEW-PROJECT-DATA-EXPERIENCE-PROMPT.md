# New Project Data Experience Prompt

Use this prompt when designing list, detail, search, and record-navigation experiences for a generic Code App.

## Copy/Paste Prompt

```md
You are designing the data experience for a new Power Apps Code App.

Create reusable patterns for how users browse, inspect, filter, search, and act on records.

Use these rules:

## 1. Core experience model

Most business apps need a combination of:

- record list or board view
- search and filters
- summary indicators
- detail panel or detail page
- create and edit actions

Design these as first-class patterns, not afterthoughts.

## 2. Record browsing

- Make it easy to scan records quickly.
- Support sort, filter, and search where useful.
- Use grouping only when it genuinely helps understanding.
- Keep record cards or rows consistent in structure.

## 3. Detail experience

- Provide a reliable way to inspect one record in context.
- Keep high-value fields easy to read.
- Separate read-only information from editable information where needed.
- Allow users to return to the broader data view without losing context.

## 4. Search and filter rules

- Put search and filters near the data they affect.
- Make active filters visible.
- Provide a clear reset path.
- Do not hide why a result set is empty.

## 5. Summary and insight layer

- Add summary metrics only if they help users orient or prioritize.
- Keep summaries tied to the visible data or workflow.
- Avoid vanity metrics with no action value.

## 6. Data freshness and feedback

- Make reload or refresh behavior understandable.
- After edits, ensure the user sees updated information clearly.
- Do not leave stale data on screen without explanation.

## 7. Selection and context

- Define how record selection works.
- Keep selected-state handling predictable.
- Make sure actions apply to the correct record.

## 8. Deliverables

Provide:

- list or board view guidance
- detail view guidance
- search and filter guidance
- summary pattern guidance
- state and refresh expectations

## 9. Anti-patterns to avoid

- data-dense screens with no hierarchy
- detail views that detach users from context
- filters that are invisible once applied
- search that returns ambiguous states with no explanation
- stale data after updates

## 10. Success criteria

The data experience is complete only if users can:

- find records quickly
- understand what they are looking at
- open a record confidently
- take action without losing context
- trust what the screen is showing
```

