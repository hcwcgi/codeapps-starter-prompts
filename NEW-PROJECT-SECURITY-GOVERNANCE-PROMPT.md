# New Project Security Governance Prompt

Use this prompt when defining security, permissions, and governance expectations for a generic Code App.

## Copy/Paste Prompt

```md
You are defining the security and governance expectations for a new Power Apps Code App.

Create a design that is safe by default and realistic for enterprise delivery.

Use these rules:

## 1. Platform-aware security

- Assume the app runs inside Power Apps and interacts with Dataverse under platform security rules.
- Respect environment permissions, table permissions, and user context.
- Do not design the UI as if every user can perform every action.

## 2. Permission-aware UX

- Design workflows that can surface permission failures clearly.
- Distinguish between read-only access, editable access, and restricted actions.
- Do not let unauthorized actions fail silently.

## 3. Data safety

- Do not expose more data than the workflow needs.
- Minimize selected and returned fields where possible.
- Keep sensitive data handling deliberate.

## 4. Input and output safety

- Validate and normalize user input before save.
- Render user content safely.
- Avoid unsafe HTML injection or untrusted rendering patterns.

## 5. Operational governance

- Use named solutions.
- Use environment separation.
- Use controlled deployment identities and approvals where appropriate.
- Avoid relying on personal accounts for production deployment workflows.

## 6. Supportability

- Make permission and configuration issues diagnosable.
- Keep error messages safe for users but useful for support.
- Ensure operational ownership is clear.

## 7. Audit-minded design

- Be deliberate about create, update, delete, and reassignment workflows.
- Make risky actions clear and attributable.
- Keep governance in mind when defining bulk actions or destructive actions.

## 8. Deliverables

Provide:

- permission-aware design guidance
- secure input and rendering rules
- data minimization expectations
- deployment governance guidance
- operational support expectations

## 9. Anti-patterns to avoid

- assuming admin-level access for all users
- exposing unnecessary fields or records
- silent permission failures
- insecure rendering of user content
- production deployment controlled by personal user habits instead of governance

## 10. Success criteria

The security and governance plan is complete only if:

- permissions are treated as a first-class design concern
- unsafe data handling is avoided
- deployment governance is defined
- the app remains supportable in an enterprise environment
```
