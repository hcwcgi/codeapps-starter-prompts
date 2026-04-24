# New Project Dataverse Prompt

Use this prompt when starting a new Power Apps Code App so the app is built with the same Dataverse architecture and guardrails that this project needed.

## Copy/Paste Prompt

```md
You are building a new Power Apps Code App with a Dataverse-backed frontend.

Build it using a frontend and Dataverse architecture similar to this pattern:

- UI components collect and display user input only.
- Hooks or controllers orchestrate loading, saving, messages, and async actions.
- A dedicated service layer owns all Dataverse reads, writes, lookup resolution, normalization, validation, and error handling.
- Mapping functions convert Dataverse rows into app domain models.
- The UI must never build raw Dataverse payloads directly.

The app must be designed from the start to avoid the Dataverse problems we previously hit:

1. Wrong table binding names
2. Lookup fields sent in the wrong shape
3. Primitive values sent with the wrong type
4. UI values being submitted in a format Dataverse does not accept
5. Create or update succeeds, but the app crashes while reading the saved record back

Follow these non-negotiable rules:

## 1. Dataverse binding and datasource rules

- Use bound datasource names from Code App metadata, not guessed table names.
- Read datasource bindings from `.power/appschemas/dataSourcesInfo.ts`.
- Add a small helper like `resolveDataverseDataSourceName(...)` to resolve the real bound datasource name safely.
- Fail early with a clear error if a required table is not bound in the Code App.
- Do not assume a Dataverse table is usable just because it exists in the environment.

## 2. Service-layer architecture

- Create a dedicated Dataverse service module for the app.
- Keep all CRUD operations inside the service layer.
- Put lookup resolution, payload shaping, validation, and Dataverse-specific logic in the service layer.
- Keep React components and pages free of Dataverse wire-format details.
- Use domain models for UI state and separate write payload types for Dataverse operations.

## 3. Read rules

- Use generated services or the Power Apps runtime client for reads.
- Select only the fields actually needed.
- Map Dataverse rows into clean domain types before they reach the UI.
- Never expose raw Dataverse records directly to presentation components unless there is a strong reason.

## 4. Write rules

- Never use generated read models as create or update payload contracts.
- Build explicit minimal write payload types for each entity.
- Send only fields Dataverse can actually accept.
- Never send display-only fields, `...name` helper fields, `...type` helper fields, or system-managed fields back to Dataverse.
- Treat create and update payloads as separate wire contracts, even if they share fields.

## 5. Lookup field rules

- Always handle Dataverse lookups explicitly.
- Use `@odata.bind` for lookup writes.
- Resolve related records before submitting the main record.
- Keep helper functions for common bind paths such as:
  - account lookups
  - contact lookups
  - owner lookups
  - currency lookups
- Do not send plain GUID strings where Dataverse expects a lookup binding path.
- Do not send lookup display names as if they were writable values.

## 6. Type normalization rules

- Normalize all write values before submit.
- Convert date-only fields to `YYYY-MM-DD` if that is what Dataverse expects.
- Send option sets as integer values, not labels.
- Validate money, number, and probability fields before submit.
- Trim strings and enforce safe length limits.
- Do not let the UI submit raw values straight through to Dataverse without normalization.

## 7. Validation boundary

- Validate in the UI for fast user feedback.
- Validate again in the service layer before building the Dataverse payload.
- The service layer must be the final guardrail even if the UI already validated.
- Return friendly messages to the UI, but keep technical validation rules in the service.

## 8. Safe save and read-back flow

For create and update flows:

- Submit the minimal Dataverse payload.
- If Dataverse returns the saved record in a usable shape, map it and return it.
- If the returned record is incomplete, refetch by id.
- If refetch by id fails temporarily, try a safe fallback query where appropriate.
- If read-back still fails, surface a controlled error or fallback result.
- Do not let the page crash just because the write succeeded but the read-back failed.

Design the service so save operations are resilient to partial or delayed Dataverse responses.

## 9. Error handling

- Normalize Power Apps and Dataverse errors in one shared helper.
- Translate common failures into helpful messages:
  - missing table binding
  - ambiguous lookup matches
  - invalid type or date values
  - unsupported fields
  - failed post-save read-back
- Avoid leaking confusing raw API errors into the UI where possible.

## 10. Recommended project shape

Use a structure like:

- `src/components` for UI
- `src/hooks` for orchestration
- `src/domain` for domain models
- `src/services` for Dataverse integration
- `src/generated` for generated models and services
- `.power/appschemas` for datasource metadata

## 11. Implementation steps

Build the app in this order:

1. Define the domain models used by the UI.
2. Confirm the Dataverse tables are bound in the Code App metadata.
3. Add datasource-name resolution helpers.
4. Build read services first and map records into domain models.
5. Build explicit create/update payload types for each writable entity.
6. Add lookup-resolution helpers before wiring create forms.
7. Add normalization and validation helpers for every writable field type.
8. Implement create and update methods with minimal payloads only.
9. Implement safe post-save read-back logic.
10. Wire hooks/controllers to the service layer.
11. Only then connect forms and buttons in the UI.

## 12. Required deliverables

When generating this app, include:

- datasource resolution helper
- Dataverse service layer
- domain-to-Dataverse mapping functions
- explicit write payload types
- lookup bind helpers
- validation and normalization helpers
- safe save/read-back handling
- user-friendly error handling

## 13. Things to avoid

- Direct CRUD calls from React components
- Guessing datasource names
- Using read models as write contracts
- Sending display fields back to Dataverse
- Sending labels instead of wire values
- Sending raw UI form values straight to Dataverse
- Assuming the saved record can always be read back immediately
- Crashing the page after a successful submit because the follow-up read failed

## 14. Success criteria

The app is only complete if:

- all required tables are bound and resolved safely
- lookup writes use the correct Dataverse bind format
- writes use minimal validated payloads
- UI values are normalized into Dataverse-safe wire values
- save flows handle post-submit read-back safely
- the UI is insulated from most Dataverse-specific complexity

Build the code and folder structure accordingly from the start.
```

## Why This Prompt Exists

This is aimed at the exact issues we hit before:

- datasource names did not always match what the app needed at runtime
- lookup fields needed explicit `@odata.bind` handling
- some values needed normalization before submit
- the UI could not safely send raw form values straight to Dataverse
- successful submits still needed defensive read-back handling

## Short Version

If you want the one-sentence version:

Build the app so the UI talks to domain models, the service layer owns all Dataverse rules, and every create or update is normalized, minimal, lookup-safe, and resilient to read-back failures.
