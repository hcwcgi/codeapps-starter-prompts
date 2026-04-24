# Code Apps Demo Talk Track

## Goal

Use this as a short presenter script to explain what a Code App is, how we build it, how it runs in Power Platform, and how we move it to deployment.

## Demo Flow

### 1. Setup

- We start with a standard web project, then connect it to Power Apps as a Code App.
- This app uses `Vite`, `React`, and `TypeScript` for the frontend.
- Power Platform connection details live in `power.config.json`.
- Dataverse tables are bound into the app so the runtime knows what data sources it can use.

Prompt:
This starts as a normal modern web app, but it is configured so Power Platform can host it and give it access to Dataverse safely through the Power Apps runtime.

### 2. Local development

- We build and iterate locally first.
- The team works in a normal developer workflow with source files, components, hooks, services, and tests.
- Local development is where we shape the UI, user experience, and business logic quickly.

Prompt:
We develop this like any other frontend application, which means we keep a fast local feedback loop before we publish anything to Power Platform.

### 3. App structure

- The UI is split into pages, components, hooks, domain models, and services.
- The UI layer is separated from the data layer.
- That separation makes it easier to move from mock data to Dataverse-backed data without rewriting the whole app.

Prompt:
The important design choice is separation of concerns, so the interface, business logic, and Dataverse integration can evolve independently.

### 4. Data and Dataverse

- Locally, we can use mock data to move quickly.
- In the hosted app, the code runs inside Power Apps and talks to Dataverse through the Power Apps runtime SDK.
- The app only uses data sources that are explicitly bound into the Code App metadata.

Prompt:
The browser is not calling a custom backend here; once deployed, the app runs inside the Power Apps host and accesses Dataverse through the platform runtime.

### 5. Quality checks

- Before deployment, we run type checks, tests, and a production build.
- This catches contract issues, logic regressions, and packaging problems early.
- The goal is to make the deployment artifact predictable.

Prompt:
Before anything is published, we validate the code, prove the important logic still works, and build the exact artifact that Power Platform will host.

### 6. Build

- The app is built into a static output in `dist/`.
- That build is the publishable artifact for this Code App.
- This project uses a Vite SPA output because it matches the Power Apps hosting model more reliably.

Prompt:
The build stage turns the source code into a deployment-ready frontend package that Power Apps can host directly.

### 7. Publish to Power Platform

- We use the Power Platform CLI to push the built app into an environment.
- The app is published into a named solution, not treated as a one-off manual upload.
- In Dev, we can push updates directly from source after validation.

Prompt:
Publishing is where the web build becomes a hosted Code App inside Power Platform, connected to the environment and its Dataverse configuration.

### 8. How it is hosted

- The app is hosted inside Power Apps.
- Power Platform provides the hosting surface, app context, identity boundary, and Dataverse access model.
- The Code App is one component inside a broader solution-based platform architecture.

Prompt:
Power Platform is not where we author the web code, but it is where we host, secure, and run the app as part of the business platform.

### 9. Deployment model

- The recommended path is `Dev -> Test -> Prod`.
- Dev receives direct updates from the codebase.
- Test and Prod should be promoted through Power Platform Pipelines as managed deployments.
- We keep one named solution and promote that solution forward.

Prompt:
Our operating model is code into Dev, then governed promotion through the platform into Test and Prod.

### 10. Outcome

- The result is a custom business app with modern frontend development practices and Power Platform hosting and governance.
- We get flexibility for developers and control for platform teams.

Prompt:
This is the value of Code Apps: modern engineering practices on the build side, with Power Platform governance and Dataverse integration on the runtime side.

## One-Line Stage Summary

1. Setup the app and connect it to Power Platform.
2. Build locally with React, TypeScript, and a normal developer workflow.
3. Keep UI, logic, and data services separated.
4. Bind Dataverse so the app can use platform data safely.
5. Run checks before publishing.
6. Build the static deployment artifact.
7. Push the app into the Dev environment.
8. Host and run it inside Power Apps.
9. Promote through Test and Prod with pipelines.

## Common Questions

### What is a Code App?

Short answer:
A Code App is a custom web app built with standard code and hosted inside Power Apps.

### Is this low-code or pro-code?

Short answer:
It is pro-code development hosted within the Power Platform ecosystem.

### Do we still use normal web engineering practices?

Short answer:
Yes, we still use source code, components, services, testing, builds, and deployment processes.

### Where does the app actually run?

Short answer:
It runs inside the Power Apps host in the target Power Platform environment.

### How does it talk to data?

Short answer:
It talks to Dataverse through the Power Apps runtime SDK and bound data sources.

### Can we use mock data first?

Short answer:
Yes, mock services are useful for early UI and workflow development before live Dataverse wiring is finished.

### What gets deployed?

Short answer:
The built frontend artifact and its solution component get deployed; Dataverse table data itself is handled separately.

### Do pipelines move the data too?

Short answer:
No, pipelines promote solution components and configuration, but business data must be migrated separately.

### Why use a named solution?

Short answer:
Because the solution is the governed deployment container that supports proper ALM across environments.

### Why not push straight to production?

Short answer:
Because Dev should be the direct publish environment, while Test and Prod should use controlled promotion and approvals.

### What is the main benefit for the business?

Short answer:
It gives us a tailored user experience without losing Power Platform hosting, security, and Dataverse integration.

## Closing Prompt

We build Code Apps with the same discipline as any modern web application, then let Power Platform host, secure, and operationalise that experience across environments.
