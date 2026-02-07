---
name: implementation-plans
description: Ensures implementation plans include interface signatures and type-level test cases — no any escapes
---

# Implementation Plans — Type-Complete Specifications

Implementation plans bridge design docs and code. When the plan is vague, the implementer fills gaps with shortcuts (`any`, `unknown`, loose types). Those shortcuts survive forever because nothing forces them to be revisited.

## The Rule

Every implementation plan phase that introduces a **new type, interface, or API** must include:

1. **The target interface signature** — the exact TypeScript interface with generics and constraints
2. **Type-level test cases** — `@ts-expect-error` tests that reject wrong shapes

## Why This Matters

Fields typed as `any` because "nothing consumes them yet" tend to stay `any` forever. The consumer that would catch the loose type never arrives because the field already "works." The only reliable way to prevent this is to specify the types **in the plan** and test them **before implementation**.

**The pattern to watch for:** If a field stores a value for later consumption (schemas, configs, metadata), it MUST have a proper type constraint in the plan — even if the consumer doesn't exist yet. `any` is only acceptable for genuinely untyped data (user-provided JSON, third-party payloads).

## What a Good Phase Looks Like

### Bad — behavioral description only

```markdown
### Phase 7: Router Factories

- `createRouterDef()`: captures prefix, inject, route registrations
```

This tells the implementer *what* to build but not *how to type it*. The implementer will use `any` for every field that doesn't have an immediate consumer.

### Good — includes interface and type tests

```markdown
### Phase 7: Router Factories

**Target interface:**

\`\`\`typescript
interface RouteConfig<
  TParams = unknown,
  TBody = unknown,
  TQuery = unknown,
  THeaders = unknown,
  TResponse = unknown,
> {
  params?: Schema<TParams>;
  body?: Schema<TBody>;
  query?: Schema<TQuery>;
  headers?: Schema<THeaders>;
  response?: Schema<TResponse>;
  middlewares?: NamedMiddlewareDef[];
  handler: (ctx: Ctx<TParams, TBody, TQuery, THeaders>) => TResponse | Promise<TResponse>;
}
\`\`\`

**Type-level tests (RED):**

- `@ts-expect-error` — `params: 'not a schema'` should be rejected
- `@ts-expect-error` — `handler` return type must match `response` schema type
- `@ts-expect-error` — `middlewares: [42]` should be rejected
```

## Checklist for Plan Authors

When writing or reviewing an implementation plan phase:

- [ ] Every new `interface` or `type` is shown with its full signature
- [ ] Generic type parameters have proper constraints (not defaulting to `any`)
- [ ] Fields that accept schemas use `Schema<T>`, not `any`
- [ ] Fields that accept specific shapes use those shapes, not `Record<string, unknown>`
- [ ] Type-level test cases are listed — at minimum, one `@ts-expect-error` per typed field
- [ ] The handler/callback signatures show how generic params flow through (e.g., `Schema<TParams>` → `ctx.params` is `TParams`)

## When Reviewing Existing Plans

If you're about to implement a phase and the plan doesn't include interface signatures:

1. **Stop.** Don't implement with `any` placeholders.
2. Check the design doc for the intended types.
3. Write the interface signature yourself and confirm with the user before proceeding.
4. Add type-level test cases to the phase before starting TDD.

## Relationship to TDD

This rule is upstream of TDD. TDD says "if it's not tested, it doesn't exist." This rule says "if the type isn't specified in the plan, it will be implemented as `any`." Together:

- **Plan** specifies the interface with types → **RED** writes `@ts-expect-error` tests → **GREEN** implements the properly typed interface

Without the plan specifying types, the TDD cycle has nothing to test against — you can't write a `@ts-expect-error` test for a constraint that was never specified.
