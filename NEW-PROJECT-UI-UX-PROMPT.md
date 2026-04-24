# New Project UI UX Prompt

Use this prompt to shape the user interface of a generic Code App in a way that feels product-quality rather than like a rough internal tool.

## Copy/Paste Prompt

```md
You are designing the UI and UX for a new Power Apps Code App.

Create a polished business application experience that feels intentional, clear, and reliable.

Use these rules:

## 1. Overall UX goals

- Design for clarity, speed, and confidence.
- Prioritize workflows that help users scan, understand, and act.
- Make the app feel like a real product, not just a technical wrapper around data.

## 2. Layout approach

- Use a clear app shell.
- Create strong hierarchy between navigation, page header, summary area, data area, and detail area.
- Support desktop first, but ensure the layout remains usable on smaller screens.

## 3. Generic business app patterns

Design reusable patterns for:

- page headers
- summary cards or metrics
- filters and search
- list or board views
- detail panels
- create and edit flows
- empty states
- loading states
- error states

## 4. Interaction design

- Keep primary actions obvious.
- Make destructive or risky actions deliberate.
- Surface system status clearly during loading and save operations.
- Do not make users guess whether an action succeeded.

## 5. State visibility

- Show loading intentionally, not with random spinners everywhere.
- Show empty states with guidance.
- Show inline validation where appropriate.
- Show save and refresh outcomes in a way users can understand.

## 6. Accessibility and usability

- Use semantic HTML.
- Ensure keyboard navigation works.
- Ensure color contrast is acceptable.
- Use descriptive labels and section headings.
- Avoid relying on color alone to communicate status.

## 7. Visual quality

- Use a coherent design system.
- Prefer strong spacing, hierarchy, and readability over decorative complexity.
- Use consistent patterns for cards, inputs, buttons, and panels.
- Keep the interface calm and credible.

## 8. Power Apps context

- Remember this is a hosted Code App inside Power Apps.
- Keep the app shell self-contained and resilient in the hosted surface.
- Avoid relying on browser assumptions that may not hold in the host.

## 9. Deliverables

Provide:

- UI structure for core screens
- layout rules
- component pattern guidance
- state feedback rules
- accessibility expectations

## 10. Anti-patterns to avoid

- flat pages with no visual hierarchy
- giant forms with no grouping
- hidden primary actions
- no empty-state guidance
- no visible feedback during save or load operations
- inconsistent controls across screens

## 11. Success criteria

The UI plan is complete only if the app would feel:

- understandable to a first-time user
- efficient for a repeat user
- polished enough for a real business demo
- consistent across pages and workflows
```

