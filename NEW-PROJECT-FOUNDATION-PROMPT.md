# New Project Foundation Prompt

Use this prompt when starting a brand-new Power Apps Code App so the project is scaffolded with the right assumptions from day one.

## Copy/Paste Prompt

```md
You are creating a new Power Apps Code App project.

Set up the project with production-ready defaults rather than demo-only shortcuts.

Use these baseline rules:

## 1. Core stack

- Use `Vite`, `React`, and `TypeScript`.
- Build as a static frontend suitable for Power Apps Code Apps hosting.
- Use `dist/` as the publish artifact.
- Assume the app will run inside the Power Apps host, not as a standalone backend-driven web app.

## 2. Project structure

Create a maintainable structure like:

- `src/components`
- `src/hooks`
- `src/domain`
- `src/services`
- `src/utils`
- `src/generated`
- `.power/appschemas`

## 3. Power Apps configuration

- Include `power.config.json`.
- Set `buildPath` to `./dist`.
- Set `buildEntryPoint` to `index.html`.
- Make local development and hosted runtime assumptions explicit.
- Treat `.power/appschemas` as runtime metadata, not handwritten source.

## 4. Architecture defaults

- UI components render and capture input.
- Hooks or controllers coordinate state and actions.
- Services own platform integration logic.
- Domain models represent app-level concepts.
- Utilities handle pure transformation and formatting logic.

## 5. Development workflow

Set up scripts for:

- local development
- type checking
- testing
- production build

The project should support a workflow like:

- `npm run dev`
- `npm run typecheck`
- `npm test`
- `npm run build`

## 6. Hosting assumptions

- The app will be hosted inside Power Apps.
- Platform integration should go through the Power Apps runtime, not ad hoc browser calls.
- Build for compatibility with the Power Apps hosting model from the start.

## 7. Non-negotiables

- Use TypeScript strictly.
- Separate UI logic from service logic.
- Do not let components become the place where integration complexity lives.
- Do not assume the first version is throwaway.
- Make the app easy to test, refactor, and deploy.

## 8. Deliverables

Generate:

- initial folder structure
- package scripts
- starter app shell
- base styling entry point
- Power Apps configuration file
- placeholder service boundary
- placeholder domain model area
- placeholder hook/controller area

## 9. Anti-patterns to avoid

- Building the whole app in a single component
- Treating the project as a plain web app with no Power Apps runtime concerns
- Mixing Dataverse or Power Platform logic directly into UI components
- Skipping testing and typecheck scripts in the initial scaffold
- Leaving deployment shape to be figured out later

## 10. Success criteria

The project is only complete if it already looks like a real Code App codebase:

- correct stack
- correct build output shape
- correct folder boundaries
- correct Power Apps assumptions
- correct scripts for dev, quality, and build
```

