# AGENTS.md

## Project
Room Booker: a React + TypeScript single-page app built with Vite.

## Commands
- `npm run dev` starts the dev server
- `npm test` runs all tests once
- `npm run lint` lints the code
- `npm run build` type-checks and builds

## Where things go
- `docs/specs/` says what the product must do. Read the relevant spec before any change.
- `docs/design/` lists the screens and states for each spec.
- `src/domain/` holds business rules as pure functions. No React and no browser APIs here.
- `src/components/` holds React components. They call `src/domain` and never re-implement a rule.

## Rules
- Every change maps to a rule (R1, R2, ...) or an acceptance criterion (AC1, AC2, ...) in a spec. If it does not, stop and ask.
- If the spec is wrong or silent, propose a spec edit first. Do not guess in code.
- Write or update the test for an acceptance criterion before the code that satisfies it. Name the test after the criterion.
- One slice per pull request. Do not touch files outside the slice.
- Do not add a dependency without asking.
- Before you say you are done, run `npm test`, `npm run lint` and `npm run build`, and report the results.