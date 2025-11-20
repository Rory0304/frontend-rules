## General Principles (The Golden Rules)
1) Do not over-engineer or make things unnecessarily complex.
→ Choose the simplest and most straightforward solution.

2) Do not proceed if a request is ambiguous or incomplete.
→ Stop and ask the user for confirmation before starting.

3) Do not implement everything from scratch by default.
→ Prefer stable, widely adopted libraries first, as long as they allow commercial use and do not impose source code disclosure obligations.

4) Do not generate code blocks until the user explicitly says it allows direct code modification.
→ Provide suggestions, conceptual improvements, or pseudocode only

5) Translate the explanation into Korean

## Commit Message Rules
Follow this format for all commit messages (maximum 2 lines):

Format: ${type}: ${message}

Available types:
- feature: New functionality or feature addition
- fix: Bug fixes or error corrections
- refactor: Code restructuring without changing functionality
- chore: Maintenance tasks, dependency updates, configuration changes

Guidelines:
- Use imperative mood: "add" not "added" or "adds"
- Be concise and descriptive

Examples:
- feature: add user authentication with JWT
- fix: resolve memory leak in data processor
- refactor: simplify payment validation logic
- chore: update dependencies to latest versions

## Reducing Eye Movement (Colocating Simple Logic)

**Rule:** Colocate simple, localized logic or use inline definitions to reduce context switching.

**Reasoning:**
- Allows top-to-bottom reading and faster comprehension.
- Reduces cognitive load from context switching (eye movement).

#### Recommended Pattern A: Inline `switch`

```tsx
function Page() {
  const user = useUser();

  // Logic is directly visible here
  switch (user.role) {
    case "admin":
      return (
        <div>
          <Button disabled={false}>Invite</Button>
          <Button disabled={false}>View</Button>
        </div>
      );
    case "viewer":
      return (
        <div>
          <Button disabled={true}>Invite</Button> {/* Example for viewer */}
          <Button disabled={false}>View</Button>
        </div>
      );
    default:
      return null;
  }
}
```

#### Recommended Pattern B: Colocated simple policy object

```tsx
function Page() {
  const user = useUser();
  // Simple policy defined right here, easy to see
  const policy = {
    admin: { canInvite: true, canView: true },
    viewer: { canInvite: false, canView: true },
  }[user.role];

  // Ensure policy exists before accessing properties if role might not match
  if (!policy) return null;

  return (
    <div>
      <Button disabled={!policy.canInvite}>Invite</Button>
      <Button disabled={!policy.canView}>View</Button>
    </div>
  );
}
```
