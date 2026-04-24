# New Project Forms Validation Prompt

Use this prompt when designing create and edit flows for a generic Code App.

## Copy/Paste Prompt

```md
You are designing forms and save workflows for a new Power Apps Code App.

Build the form architecture so inputs are safe, understandable, and compatible with the backend service layer from the start.

Use these rules:

## 1. Form model

- Treat forms as workflow surfaces, not just field collections.
- Separate raw input, normalized values, and final service payloads.
- Use domain draft types for editable state.

## 2. Validation layers

- Validate in the UI for immediate user feedback.
- Validate again in the service layer before save.
- Keep the service layer as the final guardrail.

## 3. Input normalization

- Normalize dates, numbers, booleans, option values, and trimmed strings before save.
- Do not pass raw form input straight into backend calls.
- Keep normalization logic explicit and reusable.

## 4. Save workflow

- Disable duplicate submits while saving.
- Show progress clearly.
- Surface success and failure intentionally.
- Refresh or read back the saved record safely after submit.

## 5. Field design

- Group related fields logically.
- Mark required fields clearly.
- Keep helper text concise and useful.
- Use appropriate controls for the data type.

## 6. Error display

- Show validation errors near the relevant field when possible.
- Show form-level errors for workflow failures.
- Avoid generic silent failure behavior.

## 7. Edit safety

- Be deliberate about partial updates versus full saves.
- Track which fields are editable.
- Avoid accidentally submitting display-only values.

## 8. Deliverables

Provide:

- form state model
- validation plan
- normalization plan
- save workflow design
- field grouping rules
- user feedback rules

## 9. Anti-patterns to avoid

- raw UI values passed directly to service calls
- validation existing in only one layer
- duplicate submits
- form pages that do not indicate saving state
- success paths that do not safely refresh the saved data

## 10. Success criteria

The form design is complete only if:

- users know what is required
- invalid input is caught early
- backend-safe values are submitted
- save outcomes are obvious
- post-save state stays stable and trustworthy
```

