# New Project Power Config Prompt

Use this prompt when setting up `power.config.json` for a new Power Apps Code App so the Power Apps runtime can host, load, and connect to the app correctly.

## Copy/Paste Prompt

```md
You are setting up `power.config.json` for a new Power Apps Code App.

Treat this file as a runtime contract between the built frontend and Power Apps.

Set it up so the Power Apps host can:

- identify the app correctly
- target the right environment
- load the correct build output
- use the right app entry point
- support local development
- bind Dataverse and other platform resources correctly

Use these rules:

## 1. Purpose of power.config.json

`power.config.json` tells Power Apps how to work with the app.

It is not just a convenience file.
It defines the key runtime assumptions that allow the Power Apps host and CLI tooling to:

- know which app is being updated
- know which environment it belongs to
- know which local build output to publish
- know which local dev URL to use
- know which Dataverse resources are available to the app

If this file is wrong, the app may build locally but fail to work correctly in Power Apps.

## 2. Required mindset

Set up `power.config.json` with the same care as an API contract.

Do not:

- guess field values
- assume defaults will be safe
- treat build settings and datasource settings as optional

## 3. Core fields to define correctly

Make sure the file includes and correctly sets:

- `appId`
- `appDisplayName`
- `description`
- `environmentId`
- `buildPath`
- `buildEntryPoint`
- `localAppUrl`
- `connectionReferences`
- `databaseReferences`

## 4. appId

- `appId` identifies the Code App record in Power Platform.
- It must point at the correct app.
- Reuse it when updating the same app.
- Do not accidentally overwrite or mix ids across different projects.

If `appId` is wrong, pushes and runtime behavior can target the wrong app or fail unexpectedly.

## 5. environmentId

- `environmentId` tells the tooling which Power Platform environment the app belongs to.
- It must match the intended target environment for the current configuration.
- Be explicit about environment strategy for Dev, Test, and Prod.

Do not hard-code assumptions that make the same config unsafe to reuse blindly across environments.

## 6. buildPath

- `buildPath` must point to the actual compiled frontend output folder.
- For a Vite-based Code App, this should normally be `./dist`.
- Power Apps will publish what is in this folder, not your source files.

If `buildPath` is wrong, the wrong artifact or no usable artifact will be published.

## 7. buildEntryPoint

- `buildEntryPoint` must point to the real app entry file inside the build output.
- For a Vite SPA, this should normally be `index.html`.
- This must match the artifact structure actually produced by the build.

If this is wrong, Power Apps may host an incomplete or broken app shell.

## 8. localAppUrl

- `localAppUrl` is used during local development.
- It should point to the correct dev server URL, for example `http://localhost:5173/` for Vite.
- Keep it aligned with the actual local dev setup.

This helps ensure the local runtime path matches the expected development workflow.

## 9. appDisplayName and description

- `appDisplayName` should be clear and user-friendly.
- `description` should explain what the app is for at a high level.
- Keep both meaningful so the app is understandable inside Power Platform and deployment tooling.

## 10. connectionReferences

- Use `connectionReferences` when the app depends on platform-managed connections.
- Keep them explicit rather than relying on undocumented assumptions.
- Plan them as environment-specific configuration where needed.

Do not leave connection expectations vague if the app depends on them.

## 11. databaseReferences

- `databaseReferences` define the Dataverse connection context for the app.
- This area tells the runtime which Dataverse environment and data sources are configured.
- It is critical for Dataverse-backed Code Apps.

Make sure the configuration correctly includes:

- the Dataverse instance URL
- API version
- data source list
- logical names
- entity set names

## 12. Datasource binding expectations

For Dataverse-backed apps:

- only bound data sources are available to the runtime
- a table existing in Dataverse is not enough by itself
- the app must be configured to know about that table

This means:

- verify the required tables are present in the metadata
- verify logical names and entity set names are correct
- verify naming matches what the runtime-generated app metadata expects

## 13. Correct setup for Power Apps runtime

Set up the project so the Power Apps runtime can use the app correctly:

1. The frontend build must produce the correct static artifact.
2. `power.config.json` must point at that artifact with the correct path and entry point.
3. The app must target the correct environment and app id.
4. The runtime metadata must include the required bound data sources.
5. The service layer must resolve datasource names from app metadata rather than guessing them.
6. The app must assume that hosted runtime access is controlled by what Power Apps has configured, not by what exists locally.

## 14. Common mistakes to prevent

Prevent these mistakes from the start:

- wrong `buildPath`
- wrong `buildEntryPoint`
- stale or incorrect `appId`
- pointing at the wrong `environmentId`
- assuming local success means hosted success
- missing Dataverse bindings
- mismatched table names, logical names, or entity set names
- treating datasource metadata as optional

## 15. Relationship to code architecture

The runtime config and the code architecture must agree.

That means:

- the build tool must output where `power.config.json` says it does
- the service layer must use the datasource names the runtime metadata exposes
- Dataverse integration code must be written around actual bindings, not guessed strings
- the project structure must support hosted runtime assumptions from the start

## 16. Deliverables

When setting up a new project, produce:

- a correct `power.config.json`
- matching build configuration
- matching local dev URL
- correct Dataverse references where applicable
- datasource-aware service setup
- documented assumptions for environment targeting

## 17. Anti-patterns to avoid

- treating `power.config.json` as a minor config file
- copying ids from another project without checking them
- using build output settings that do not match the real frontend build
- assuming Dataverse tables are automatically available
- guessing datasource names in services
- leaving runtime configuration implicit

## 18. Success criteria

The runtime setup is only complete if:

- Power Apps can identify the correct app
- the correct environment is targeted
- the correct built artifact is published
- local development points to the right dev server
- required data sources are bound and discoverable
- the service layer can use runtime metadata safely

Build and validate the project accordingly.
```

## Why This Prompt Exists

This exists because a Code App can look correct as a frontend project while still being misconfigured for the Power Apps runtime.

The biggest risks usually come from:

- build output mismatch
- wrong environment or app targeting
- incomplete Dataverse bindings
- runtime metadata and service code falling out of sync

## Short Version

If you want the one-line version:

Set up `power.config.json` so Power Apps knows exactly which app to host, which environment to use, which built files to publish, and which platform data sources the runtime is allowed to access.
