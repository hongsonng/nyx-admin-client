---
name: nyx-reusable-components
description: Use when adding or changing a React/Next.js component in the Nyx project and reuse, placement, duplication, exports, or component documentation is uncertain.
---

# Nyx Reusable Components

## Overview

Search before creating. Keep a component close to its feature until a second real consumer proves that it belongs in the shared Nyx library. Reuse the existing contract instead of creating a visually similar duplicate.

## Decision workflow

1. **Search the repository first.** Inspect `src/components/nyx`, route-local `_components` folders, and existing imports:

   ```bash
   rg --files src | rg 'components|ui|layout|form|feedback|navigation'
   rg -n "ComponentName|similar semantic role" src
   ```

2. **Classify the need.** Reuse the existing component when behavior and semantics match. Extend it with a focused prop only when the new variation preserves the same responsibility. Create a new component when the responsibility, accessibility contract, or data shape is genuinely different.

3. **Choose placement by consumers.**

   | Consumers | Location |
   | --- | --- |
   | One route or feature | Near that feature, such as `src/app/(admin)/users/_components/` |
   | Two or more unrelated features | `src/components/nyx/<category>/` |
   | Domain-specific but shared | `src/components/nyx/<domain>/` with a documented contract |

   Do not promote a component because it might be reused someday. Do not duplicate a component merely because importing it feels inconvenient.

4. **Use the Nyx library structure.** Put shared components in the category that matches their responsibility:

   ```text
   src/components/nyx/
   ├── ui/            # Small branded primitives
   ├── layout/        # Shells, panels, and page composition
   ├── forms/         # Inputs, field groups, and form actions
   ├── feedback/      # Alerts, status, loading, and empty states
   ├── navigation/   # Menus, tabs, breadcrumbs, and pagination
   ├── data-display/  # Tables, lists, cards, and summaries
   ├── providers/    # Shared React context providers
   └── index.ts       # Explicit public exports
   ```

   Keep the `nyx` brand in the shared path and use semantic exported names such as `NyxStatusBadge` or `NyxDataTable`. Avoid generic names that collide with unrelated libraries.

5. **Define a clear public contract.** Use named exports, typed props, accessible states, and the smallest API that serves current consumers. Add the component to the nearest barrel only when it is intended for reuse. Add a short README beside non-obvious components with purpose, props, states, dependencies, and one usage example.

6. **Verify before handoff.** Search call sites to confirm reuse, run `pnpm lint`, run `pnpm build` for application changes, and inspect the final diff for accidental generated files or unrelated edits.

## Example

If a status badge is needed on a second admin route, search first. If `NyxStatusBadge` already exists, import its named export from the Nyx library and reuse its states; do not create `UserStatus`, `AccountStatus`, and `MemberStatus` copies with different colors.

## Stop conditions and common mistakes

- **Only one consumer:** keep the component feature-local.
- **Similar but different responsibility:** create a focused component instead of a prop-heavy catch-all component.
- **Existing component is hard to use:** improve its public API or add a focused adapter; do not copy it silently.
- **Duplicate names or styles:** search imports and shared categories before adding files.
- **Missing export or usage docs:** fix the public contract before calling the component reusable.
- **Unclear ownership or accessibility behavior:** stop and ask; do not promote it to `src/components/nyx`.

**Red flag:** a new shared component cannot name two current consumers, or its props describe several unrelated responsibilities. Keep it local or redesign the boundary.
