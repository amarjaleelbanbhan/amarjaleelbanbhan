# Debugging & Reliability — Verifiable Public Proof

This page is a compact proof sheet for **bounded software debugging, production-readiness, and reliability work**. It is not a list of client case studies: every example below is public engineering work that can be independently checked on GitHub.

## Merged upstream fixes

### Pydantic AI — preserve AG-UI message ordering
**Merged:** [pydantic/pydantic-ai#5969](https://github.com/pydantic/pydantic-ai/pull/5969)

Fixed an ordering bug where `ToolReturnPart` and `UserPromptPart` could be emitted in the wrong order when replaying AG-UI histories, causing providers that require tool results immediately after tool calls to reject the history.

- Added a focused ordering fix rather than rewriting the adapter.
- Added regression coverage for the failure mode.
- Validated the targeted tests and Ruff checks before merge.

### promptfoo — per-test repeat behavior
**Merged:** [promptfoo/promptfoo#9781](https://github.com/promptfoo/promptfoo/pull/9781)

Added per-test repeat configuration while preserving global behavior, scenario precedence, cache isolation, schemas, and provider prompt behavior.

- Changed evaluator behavior across a mature TypeScript codebase.
- Added regression/cache coverage.
- Validated 467 targeted tests plus TypeScript, formatting, architecture checks, generated schema consistency, docs build, and real CLI runs before merge.

### loop-engineering — MCP cost-estimation bug
**Merged:** [cobusgreyling/loop-engineering#437](https://github.com/cobusgreyling/loop-engineering/pull/437)

Fixed an MCP server cost estimator that ignored `early_exit_required` and could overestimate the realistic cost of early-exit patterns by roughly 3× compared with the canonical library/CLI.

- Removed duplicated estimation logic and reused the canonical implementation.
- Added a regression fixture that failed on the old behavior and passed on the corrected result.

### MCTS — optional toolchain diagnostics
**Merged:** [MCP-Audit/MCTS#233](https://github.com/MCP-Audit/MCTS/pull/233)

Added deeper readiness checks for optional MCP/API extras and command-line tooling while keeping missing optional components as warnings rather than breaking core installs.

- Added tests for missing and present optional toolchains.
- Kept the change focused to the requested diagnostic behavior.

## Relevant builds

### BuildSphere
[Repository](https://github.com/amarjaleelbanbhan/BuildSphere) · [Live demo](https://buildsphere.vercel.app)

A browser-based 3D floor planner built with **Next.js 15, TypeScript, React Three Fiber/Three.js, Zustand, and Vercel**. It includes interactive scene state, object placement, undo/redo, save/load, and browser deployment.

### AutoBugFix / BugFlow AI
[Repository](https://github.com/amarjaleelbanbhan/AutoBugFix)

A full-stack debugging/triage demo built with **React 19, TypeScript, Express, Vite, and AI-assisted diagnostics**. It includes issue triage, crash/log diagnosis, testing helpers, API routes, and deployment configuration. Its README explicitly documents demo limitations instead of presenting them as production guarantees.

## What I can responsibly scope

For an existing React/Next.js/TypeScript or AI-assisted web application, a small rescue engagement can be bounded around one concrete outcome:

1. Reproduce the failure and record the smallest reliable reproduction.
2. Trace the root cause from logs, requests, state, configuration, or code.
3. Make the smallest safe code/configuration change that fixes the issue.
4. Add a regression test or repeatable verification step where practical.
5. Re-test the affected flow and document exactly what changed.
6. Hand back the patch, verification evidence, and any remaining risks.

Good first scopes include broken application flows, regressions, API/integration failures, deployment/runtime errors, state bugs, test failures, and reliability problems in an existing codebase.

## Boundaries

- I do not claim customer results that I cannot verify.
- I do not perform security testing against systems without explicit authorization.
- I do not make live payment or identity-system changes casually; payment/auth work should use the client's authorized test or staging path whenever possible.
- I will say when a reported problem is a platform/vendor issue rather than inventing code work.
- If a task is not genuinely bounded, I will scope it before proposing a fixed-price fix.

## Useful first message

Send the **public URL (if any), stack, exact symptom, reproduction steps, and relevant logs with secrets/private data removed**. That is usually enough to determine whether the problem is a small repair, a QA/reliability pass, or a larger cleanup.