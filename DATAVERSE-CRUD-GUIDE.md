# Dataverse CRUD Guide For This Code App

## Purpose

This document explains how this repository should read and write Dataverse records from TypeScript when running as a Power Apps Code App.

It is based on the live-tested patterns now used in:

- [src/services/dataverse-sales-service.ts](C:/Users/harvey.williams/dev/codeapps/cgi%20code%20app/codeappsdemo/src/services/dataverse-sales-service.ts)
- [src/services/dataverse-search-service.ts](C:/Users/harvey.williams/dev/codeapps/cgi%20code%20app/codeappsdemo/src/services/dataverse-search-service.ts)
- [src/components/data-page.tsx](C:/Users/harvey.williams/dev/codeapps/cgi%20code%20app/codeappsdemo/src/components/data-page.tsx)

The short version:

- use the Power Apps runtime Dataverse client
- use bound table names from app metadata
- read with generated services or `retrieveMultipleRecordsAsync`
- write with minimal payloads only
- use `@odata.bind` for lookups
- validate and normalize in TypeScript before submit
- do not send display-only or system-managed fields back to Dataverse

## The Runtime Model

Once the app is published, the browser is not calling your local code or a custom backend. The TypeScript bundle runs inside the Power Apps host and talks to Dataverse through the Power Apps runtime SDK.

That means CRUD operations should be built around:

- `getClient(dataSourcesInfo)` from `@microsoft/power-apps/data`
- table bindings defined in `.power/appschemas/dataSourcesInfo.ts`
- table names resolved through [resolve-dataverse-data-source.ts](C:/Users/harvey.williams/dev/codeapps/cgi%20code%20app/codeappsdemo/src/services/resolve-dataverse-data-source.ts)

If a Dataverse table exists in the environment but is not bound in the Code App metadata, the app cannot use it.

## Core Rule

Treat Dataverse reads and writes differently.

Reads can be broad:

- select display fields
- use generated model types
- map Dataverse rows into UI/domain models

Writes must be narrow:

- send only fields that Dataverse actually accepts for create or update
- use the real wire format Dataverse expects
- never assume the generated TypeScript model is a safe write contract

This is the main lesson from the submission issues we worked through.

## Read Pattern

There are two acceptable read patterns in this repo.

### 1. Generated service classes

Used by the pipeline flow:

- [OpportunitiesService.ts](C:/Users/harvey.williams/dev/codeapps/cgi%20code%20app/codeappsdemo/src/generated/services/OpportunitiesService.ts)
- [SystemusersService.ts](C:/Users/harvey.williams/dev/codeapps/cgi%20code%20app/codeappsdemo/src/generated/services/SystemusersService.ts)

Good for:

- `get`
- `getAll`
- straightforward table reads

### 2. Direct runtime client reads

Used by the Search page:

- [dataverse-search-service.ts](C:/Users/harvey.williams/dev/codeapps/cgi%20code%20app/codeappsdemo/src/services/dataverse-search-service.ts)

Good for:

- direct `retrieveMultipleRecordsAsync(...)`
- cross-table read helpers
- custom lookup resolution logic

Example shape:

```ts
const client = getClient(dataSourcesInfo);
const result = await client.retrieveMultipleRecordsAsync<TableRow>(
  dataSourceName,
  {
    select: ["field1", "field2"],
    filter: "contains(name, 'Contoso')",
    top: 10,
  }
);
```

## Create Pattern

For create operations, Dataverse wants a minimal payload.

Do:

- send only writable primitive fields
- send lookup relationships via `@odata.bind`
- normalize types before submit

Do not:

- send `...name`, `...type`, or `...yominame` fields
- send read-only/system-managed fields
- send UI labels where Dataverse expects option-set integers
- send full timestamps where the field expects a date-only value

### The Current Opportunity Create Pattern

This now lives in [dataverse-sales-service.ts](C:/Users/harvey.williams/dev/codeapps/cgi%20code%20app/codeappsdemo/src/services/dataverse-sales-service.ts).

The payload is shaped like this:

```ts
type DataverseOpportunityWritePayload = {
  name?: string;
  estimatedvalue?: number;
  closeprobability?: number;
  estimatedclosedate?: string;
  description?: string;
  customerneed?: string;
  currentsituation?: string;
  salesstage?: number;
  "customerid_account@odata.bind"?: string;
  "customerid_contact@odata.bind"?: string;
  "transactioncurrencyid@odata.bind"?: string;
  "ownerid@odata.bind"?: string;
};
```

Example create payload:

```ts
{
  name: "Test opp 1",
  estimatedvalue: 2323,
  closeprobability: 44,
  estimatedclosedate: "2026-05-31",
  description: "Create a test opportunity against an existing account.",
  customerneed: "Initial qualification call booked",
  currentsituation: "Testing Code App Dataverse create flow",
  salesstage: 0,
  "customerid_account@odata.bind": "/accounts(<guid>)",
  "transactioncurrencyid@odata.bind": "/transactioncurrencies(<guid>)"
}
```

### Why This Matters

The generated `Opportunities` model includes fields that are useful for reads but unsafe for writes, for example:

- `customeridname`
- `customeridtype`
- `owneridname`
- `owneridtype`
- `transactioncurrencyidname`

Those fields should not be echoed back in create/update payloads.

## Update Pattern

Use the same rule as create:

- only send changed fields
- keep the payload minimal
- keep lookup updates in `@odata.bind` format

Current update logic is in:

- [buildOpportunityPatch(...)](C:/Users/harvey.williams/dev/codeapps/cgi%20code%20app/codeappsdemo/src/services/dataverse-sales-service.ts)
- [updateOpportunityRecord(...)](C:/Users/harvey.williams/dev/codeapps/cgi%20code%20app/codeappsdemo/src/services/dataverse-sales-service.ts)

Examples:

```ts
{
  estimatedvalue: 50000,
  closeprobability: 60,
  estimatedclosedate: "2026-06-30"
}
```

```ts
{
  "ownerid@odata.bind": "/systemusers(<guid>)"
}
```

## Delete Pattern

Delete is the simplest operation:

- resolve the bound table name
- call `deleteRecordAsync(dataSourceName, id)`

If this repo adds delete UI later, it should follow the same service-boundary pattern as create/update rather than calling the client directly from components.

## Lookup Rules

Lookups are the biggest source of Dataverse write errors.

For this app, use these patterns:

- account customer: `"customerid_account@odata.bind": "/accounts(<guid>)"`
- contact customer: `"customerid_contact@odata.bind": "/contacts(<guid>)"`
- transaction currency: `"transactioncurrencyid@odata.bind": "/transactioncurrencies(<guid>)"`
- owner: `"ownerid@odata.bind": "/systemusers(<guid>)"`

Do not send:

- `ownerid: "<guid>"` for general create/update payloads
- `customeridtype: "account"`
- `customeridname: "Contoso"`

Those values may exist on reads, but they are not the correct write shape.

## Type Normalization Rules

Dataverse is strict. The app should normalize values before submit.

### Dates

If Dataverse expects a date-only field, send:

```ts
"2026-05-31"
```

Do not send:

```ts
"2026-05-31T00:00:00.000Z"
```

### Option sets / picklists

If Dataverse expects a picklist, send the integer value, not the label.

For the current opportunity stage mapping in this app:

- `Prospecting -> 0`
- `Qualification -> 1`
- `Proposal -> 2`
- `Negotiation -> 3`

`Won` and `Lost` are treated as close states, not normal create-time stage values.

### Numbers

Validate in TypeScript before submit:

- money values must be finite and non-negative
- probability must be an integer between `0` and `100`

### Strings

Use schema-aligned max lengths where known:

- opportunity `name`: `300`
- `description`: `2000`
- `customerneed`: `2000`
- `currentsituation`: `2000`

Trim values before submit.

## Validation Boundary

This app now validates in two places, on purpose.

### 1. UI-level validation

In [data-page.tsx](C:/Users/harvey.williams/dev/codeapps/cgi%20code%20app/codeappsdemo/src/components/data-page.tsx):

- catches obvious input issues early
- gives form-friendly messages
- blocks invalid stage/date/probability submissions

### 2. Service-level validation

In [dataverse-sales-service.ts](C:/Users/harvey.williams/dev/codeapps/cgi%20code%20app/codeappsdemo/src/services/dataverse-sales-service.ts):

- re-validates before constructing the Dataverse payload
- protects create/update even if another UI path is added later

The service layer must remain the final guardrail.

## Customer And Currency Resolution

This app supports text input for customer and currency, then resolves records before create.

Current behavior:

- account/contact name is looked up first
- if not found, the app creates the account or contact
- currency text is resolved to an existing `transactioncurrency` record
- the final opportunity create uses the resolved GUIDs via lookup binds

That logic belongs in the service layer, not the UI.

## Recommended Service Design

For this repo, keep the layering like this:

1. UI components collect plain form input
2. hooks orchestrate user actions and messages
3. service layer resolves lookups, validates, normalizes, and builds Dataverse payloads
4. runtime client performs CRUD
5. mapping functions convert Dataverse rows back into domain models

That keeps Power Apps and Dataverse specifics out of most of the UI.

## Anti-Patterns To Avoid

Avoid these patterns in future changes:

- using generated model interfaces directly as create/update payload contracts
- sending read-only `...name` fields back to Dataverse
- mixing UI labels with Dataverse wire values
- building raw Dataverse payloads in React components
- skipping service-level validation because the form already validated
- assuming a table can be queried just because it exists in the environment

## Practical Checklist

Before adding a new Dataverse CRUD action in this repo:

1. Confirm the table is bound in the Code App.
2. Resolve the runtime datasource name, not just the display name.
3. Identify the true writable fields.
4. Normalize all values before submit.
5. Use `@odata.bind` for lookups.
6. Keep create/update payloads minimal.
7. Map Dataverse rows into domain models after the call returns.
8. Surface friendly errors in the hook/UI, but keep technical validation in the service.

## Current File References

- Main Dataverse write logic:
  [src/services/dataverse-sales-service.ts](C:/Users/harvey.williams/dev/codeapps/cgi%20code%20app/codeappsdemo/src/services/dataverse-sales-service.ts)
- Search/read example:
  [src/services/dataverse-search-service.ts](C:/Users/harvey.williams/dev/codeapps/cgi%20code%20app/codeappsdemo/src/services/dataverse-search-service.ts)
- Form validation example:
  [src/components/data-page.tsx](C:/Users/harvey.williams/dev/codeapps/cgi%20code%20app/codeappsdemo/src/components/data-page.tsx)
- Domain types:
  [src/domain/sales.ts](C:/Users/harvey.williams/dev/codeapps/cgi%20code%20app/codeappsdemo/src/domain/sales.ts)

## Working Rule For Future CRUD

If Dataverse can calculate, infer, or resolve a field itself, do not send it unless the API explicitly requires it.

In practice, this repo should prefer:

- small payloads
- explicit lookup binds
- validated primitives
- service-layer normalization

That is the safest pattern for live Power Apps Code App CRUD in TypeScript.
