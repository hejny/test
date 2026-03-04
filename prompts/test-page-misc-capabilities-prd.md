[ ]

[✨🍉] Testing page on `/test` to showcase misc system capabilities

-   Create a new testing page accessible at route `/test`.
-   Purpose: provide a convenient, developer-facing playground to demonstrate and manually verify miscellaneous capabilities of the system (UI, API connectivity, auth/session, storage, realtime, error handling, etc.).
-   Keep the page non-production-facing (guarded / hidden) to avoid end-user discovery.
-   Keep in mind the DRY _(don't repeat yourself)_ principle.
-   Do a proper analysis of the current functionality before you start implementing.

## Context / Problem

We need a single place to quickly validate that the system is wired correctly and to demo “misc capabilities” without having to create one-off temporary pages and code.

## Goals

-   Provide `/test` route that is easy to access in development/staging.
-   Offer a set of small “widgets” (sections) to exercise common capabilities.
-   Make it easy to add/remove widgets over time.
-   Make it safe: gated behind @@@ (e.g., env flag, auth role, or dev-only).

## Non-goals

-   Not intended as a public-facing feature page.
-   Not a full automated test suite replacement.

## Target users

-   Developers
-   QA / Product (internal)

## Key requirements

### 1) Routing & access control

-   Route available at `/test`.
-   Access control: @@@
    -   Prefer one or more of:
        -   Only enabled when `NODE_ENV !== 'production'` or `ENABLE_TEST_PAGE=true`.
        -   Only accessible to authenticated users with role `admin`/`developer`.
        -   Additionally hide from navigation UI.
-   If not allowed: show 404 or “Not Found” (preferred) or an explicit “Forbidden” page (decide @@@).

### 2) Page structure / layout

-   Title: “System Test / Playground”.
-   Layout: simple list of collapsible cards/accordions (“capability widgets”).
-   Each widget should be isolated in its own component/module to keep code clean.
-   Add a small “About this page” section explaining that it’s internal.

### 3) Capability widgets (initial set)

Implement initial widgets below; if some are not applicable to this codebase, stub them with clear “not supported yet” and TODO.

1.  **Environment & build info**
    -   Display app version/commit SHA if available @@@.
    -   Display environment (dev/staging/prod) and key feature flags relevant to the page.

2.  **API ping / health**
    -   Button: “Ping API”.
    -   Shows latency, status code, and response body (pretty printed JSON).
    -   Endpoint used: @@@ (e.g., `/api/health`). If missing, create minimal health endpoint.

3.  **Auth/session snapshot**
    -   Show whether user is authenticated.
    -   Display safe subset of session/user data (no secrets).

4.  **Notifications / toasts**
    -   Buttons to trigger success/info/warn/error toast.

5.  **Error boundary / logging test**
    -   Button to intentionally throw a UI error to verify error boundary.
    -   Button to trigger a handled error and verify logger pipeline.

6.  **Storage test**
    -   LocalStorage read/write test (key/value).
    -   If app uses server-side persistence, add DB write/read test @@@.

7.  **Realtime / WebSocket test** (optional)
    -   Connect/disconnect.
    -   Send a ping message and show received messages.
    -   If not supported, hide the widget.

8.  **Forms & validation**
    -   A small form demonstrating validation errors and success state.

### 4) Extensibility

-   Add a simple registry/array of widgets so new widgets can be added by appending a component.
-   Each widget should have:
    -   `id`, `title`, `description`, `component`.

### 5) UX details

-   All actions must show loading state.
-   Show errors clearly (error text + optional raw response).
-   Provide “Copy to clipboard” for JSON outputs where relevant.

## Technical notes / open questions

-   What framework/router is used? (Next.js/React Router/etc.) @@@
-   Where to place the page in the repo structure? @@@
-   Existing toast system? @@@
-   Existing API health endpoint? @@@
-   Auth mechanism (cookies/JWT/session provider)? @@@

## Acceptance criteria

-   Visiting `/test` in allowed environment shows the testing page with at least widgets (1)-(5).
-   In production (or when disabled), `/test` is not accessible per access control decision.
-   API ping works and reports result.
-   Toast widget triggers visible notifications.
-   Error test demonstrates error boundary/logging behavior without breaking the whole app (unless intentionally crashing a contained boundary).
-   Code is modular: widgets are separate components and can be easily extended.

## Analytics / logging

-   Log interactions on this page only to dev logs @@@.
-   Do not send end-user analytics events (unless explicitly allowed) @@@.

## QA checklist

-   Verify gating rules.
-   Verify each widget works and has loading/error states.
-   Verify no sensitive info displayed in session output.

## Rollout

-   Ship behind feature flag `ENABLE_TEST_PAGE` defaulting to `false` in prod.
-   Document in README/dev docs @@@.
