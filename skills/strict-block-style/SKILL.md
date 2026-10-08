---
name: strict-block-style
description: Enforce consistent JavaScript and TypeScript control-flow style when creating, editing, refactoring, or reviewing code. Require braces and multiline non-empty blocks, allow intentionally empty inline blocks, and separate independent statements with blank lines. Apply automatically to new code and to the relevant scope of legacy-code changes.
---

# JavaScript/TypeScript Control-Flow Style

Apply these rules whenever creating, modifying, refactoring, or reviewing JavaScript or TypeScript code.

## Scope and change boundaries

- **New code:** Apply all rules to every new JavaScript or TypeScript statement.
- **Existing code:** Apply all rules to code being changed and its directly related surrounding statements. Do not reformat unrelated legacy code unless explicitly requested.
- **Code reviews:** Identify relevant violations, but do not make formatting-only changes unless code edits are requested.
- Preserve program behavior, meaningful comments, and established indentation settings. These rules do not prescribe tabs versus spaces or indentation width.

## Braces and block layout

Always use curly braces for statement bodies of control-flow structures, including:

- `if`, `else if`, `else`
- `for`, `for...of`, `for...in`
- `while`, `do...while`
- `try`, `catch`, `finally`
- `switch` and other applicable statement-based control structures

Every **non-empty** control-flow block must span multiple lines, even if it contains just one statement. Keep its statements on their own indented lines.

Correct:

```typescript
if (condition) {
  doSomething();
}

continueProcessing();
```

Incorrect:

```typescript
if (condition) doSomething();
```

```typescript
if (condition) { doSomething(); }
```

Intentionally **empty** blocks may remain inline as `{}`. This exception applies to `catch`, `finally`, and other control-flow blocks that are legitimately empty. Do not expand them into multiline empty blocks or add placeholder statements solely for formatting.

Correct:

```typescript
try {
  performOperation();
} catch {}

continueProcessing();
```

```typescript
try {
  performOperation();
} finally {}
```

Do not introduce empty blocks or silently discard errors to satisfy the style rules. Preserve existing behavior.

## Switch statements

- Always enclose the `switch` body in braces.
- Do **not** add braces around individual `case` or `default` clauses by default.
- Add a block around a case only when required, for example to create an appropriate lexical scope for `let`, `const`, or `class` declarations.
- Preserve intentional fallthrough and existing control flow.

```typescript
switch (status) {
  case "active":
    activate();
    break;

  case "pending": {
    const pendingResult = calculatePendingResult();
    handle(pendingResult);
    break;
  }

  default:
    reset();
}
```

## Concise expressions

One-line **expressions** remain allowed when easy to read, including ternaries, nullish coalescing, optional chaining, short returns, and concise arrow-function expressions.

```typescript
const label = enabled ? "Enabled" : "Disabled";
const displayName = user.name ?? "Unknown";
const email = user?.contact?.email;
const doubled = values.map((value) => value * 2);
return result ?? fallback;
```

For complex or nested ternaries, prefer clear multiline statements. Permission for concise expressions never permits an unbraced or single-line **non-empty control-flow block**.

## Blank lines around control flow

Separate a control-flow statement from adjacent **independent sibling statements** in the same block with **exactly one blank line**. This applies before and after it, including between consecutive sibling control structures.

```typescript
const level = environment.LOG_LEVEL?.trim() ?? "info";

if (level !== "info" && level !== "debug") {
  throw new Error("Invalid log level.");
}

initializeLogging(level);
```

```typescript
for (const item of items) {
  if (!item.isValid) {
    continue;
  }

  if (item.shouldProcess) {
    process(item);
  }
}

return result;
```

- Do not insert a blank line immediately after a block's opening brace or immediately before its closing brace.
- Do not insert blank lines between connected clauses: `if` / `else if` / `else`, `try` / `catch` / `finally`, or `do` / its terminating `while`.
- Treat a complete connected control-flow structure as one statement for surrounding blank-line decisions.
- The same surrounding blank-line rules apply when a control-flow structure contains an inline empty block.
- Never use more than one consecutive blank line in the code being formatted.

```typescript
if (condition) {
  handleCondition();
} else {
  handleAlternative();
}

continueProcessing();
```

## Comments

- Place a comment that explains a statement **immediately above that statement**, or **inside the relevant block** when it explains internal behavior. Do not place it below the statement it describes.
- Keep a comment attached to the statement it describes; do not insert a blank line between them.
- If a blank line is required before a commented control-flow statement, place it **before the comment**, not between the comment and the statement.
- When editing legacy code, preserve the meaning and placement of unrelated comments; do not move them merely to reformat unrelated lines.

```typescript
prepare();

// Reject invalid inputs before processing.
if (!input.isValid) {
  return;
}

process(input);
```

## Final verification

Before completing JavaScript or TypeScript work, verify that:

1. Every applicable control-flow structure uses braces; every non-empty block is multiline.
2. Intentionally empty blocks may remain inline as `{}`.
3. `switch` cases have no unnecessary block braces.
4. Independent sibling statements and control structures have exactly one separating blank line, without separating connected clauses or attached comments.
5. No unnecessary blank lines occur immediately inside block boundaries or as repeated blank lines.
6. Concise expressions remain readable, comments stay associated with their statements, and program behavior is unchanged.
7. Formatting changes to existing code stay within the requested and directly related scope.
