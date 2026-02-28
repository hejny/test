# PRD: Improve Mobile Menu UI/UX

- **Product/Repo:** `hejny/test`
- **Document type:** Product Requirements Document (PRD)
- **Author:** AI (TypeScript full‑stack dev + Product Owner)
- **Date:** 2026-02-28
- **Status:** Draft (needs product context)

## 1) Summary
The goal is to improve the usability and discoverability of mobile navigation (menu), reduce friction to reach key pages, and improve navigation interaction metrics.

This PRD defines goals, scope, UX/UI options, analytics tracking, and acceptance criteria for improving the mobile menu. The repository currently contains a simple website (`index.html`), so the proposal is intentionally generic and applicable to responsive web/PWA.

## 2) Context & Problem
### Observed / typical issues (hypotheses to validate)
Because no concrete pain points were provided yet, this PRD starts from common mobile menu problems:
- Users cannot find primary actions (too deep in the menu / unclear hierarchy).
- The menu is long, without grouping and visual separators.
- Tap targets are too small and/or contrast is insufficient.
- The menu closes “unexpectedly” (scroll, outside tap) and users lose context.
- Missing “active page” state and unclear navigation context.

### Why now
- Mobile traffic is typically the majority of sessions.
- Navigation is a key driver to core flows and activation.

## 3) Goals & Non-goals
### Goals
1. Reduce time and taps needed to reach key sections (better IA, fewer steps).
2. Increase navigation usage and reduce drop-off after opening the menu.
3. Ensure accessibility and operability (WCAG 2.2 AA, a11y best practices).

### Non-goals
- Full visual identity redesign.
- Re-architecting the whole site information architecture beyond the menu (unless explicitly approved).

## 4) User Scenarios (JTBD)
- **As a visitor**, I want to quickly find the main sections so I can complete my task without searching.
- **As a returning user**, I want quick access to frequent or recent destinations.
- **As a user with accessibility needs** (motor/assistive tech), I want to operate the menu comfortably.

## 5) Proposed Solution (UX/UI)
> The final choice depends on your content and menu type (hamburger drawer vs bottom navigation).

### Option A: Hamburger (drawer) + highlighted primary actions
- Hamburger icon in the header (+ optional label “Menu”).
- Drawer covering ~80–90% width with a background scrim.
- **Sections:**
  - Primary items (Top 4–6)
  - Secondary (Settings, Help, Contact)
  - Account (Sign in/Profile/Sign out)
- Active item clearly highlighted.
- Add in-menu search if items > ~8–10.

### Option B: Bottom navigation for top tasks + “More” drawer
- 3–5 tabs for the most frequent destinations.
- Last tab “More” opens a drawer with the remaining items.
- Benefit: thumb-friendly, less need to open the menu.

### Heuristic recommendation
- If you have **≤5 key destinations**, prefer bottom navigation.
- If you have **many sections and hierarchy**, prefer a drawer + grouping/search.

## 6) Information Architecture (IA)
### Minimal structure (template)
1. **Home**
2. **Primary section A**
3. **Primary section B**
4. **Primary CTA**
5. **Contact / Support**

Secondary:
- Settings
- About
- Terms / Privacy

> Replace this with your real menu items (Top 5) and role-specific variants (guest vs logged-in).

## 7) Interaction Details
- Open via tapping the menu trigger.
- Close via:
  - tapping the scrim,
  - a dedicated “Close” button (for a11y),
  - ESC key (for keyboard users).
- Preserve page scroll position while the drawer is open.
- Focus management:
  - focus trap inside the drawer,
  - return focus to the trigger when closing.

## 8) Accessibility (WCAG)
- Minimum tap target size: **44×44 px**.
- Text contrast at least **4.5:1** (AA).
- `aria-expanded`, `aria-controls` on the trigger.
- Drawer implemented as `role="dialog"` or `nav` (depending on approach).
- Visible focus outline for keyboard navigation.
- Screen reader support: meaningful labels for icons.

## 9) Telemetry / Analytics (Event plan)
> Event naming is a proposal; adapt to GA4/Mixpanel/Amplitude conventions.

### Events
1. `menu_open`
   - props: `source` (hamburger/bottom_more), `page`, `viewport`
2. `menu_close`
   - props: `method` (scrim/close_btn/escape/navigate)
3. `menu_item_click`
   - props: `item_id`, `item_label`, `item_level`, `from_page`
4. `bottom_nav_click` (if bottom nav)
   - props: `tab_id`, `from_page`

### KPIs (proposal)
- CTR on top menu items.
- % sessions with menu opened.
- Drop-off after `menu_open` (close without navigating).
- Time to first navigation after `menu_open`.

## 10) Acceptance Criteria
### Functional
- [ ] Menu is available and works on mobile breakpoints (e.g., ≤ 768px).
- [ ] Users can open and close the menu via at least 3 methods (scrim, close button, ESC).
- [ ] Clicking a menu item navigates and closes the menu.
- [ ] The active page is visually indicated.

### UX
- [ ] Primary items are visible without scrolling on common mobile heights (~700–800px), or scrolling is clearly indicated.
- [ ] Tap targets meet 44×44px.

### Accessibility
- [ ] Trigger includes `aria-expanded` and `aria-controls`.
- [ ] Drawer has focus trap and returns focus on close.
- [ ] Text contrast meets WCAG AA.

### Analytics
- [ ] Events `menu_open`, `menu_close`, `menu_item_click` are emitted with defined properties.

## 11) Edge Cases
- Long item labels (wrapping / ellipsis).
- Large number of items (grouping + sticky header within drawer).
- Logged-in vs logged-out states.

## 12) Open Questions (required to finalize)
Please answer so we can turn this template into a concrete PRD:
1. Is this **web / PWA / native app**?
2. Menu type: **hamburger drawer**, **bottom navigation**, or a combination?
3. What are the current concrete problems (3–5 bullets)?
4. What are your **Top 5 menu items** + secondary links (help/settings/logout, etc.)?
5. Which design system (MUI/Tailwind/custom) and analytics (GA4/Amplitude/Mixpanel) do you use?

## 13) Rollout Plan (proposal)
- Phase 1: Implementation + a11y + baseline events.
- Phase 2: A/B test (Option A vs B) depending on traffic.
- Phase 3: Iterate based on data (shorten menu, reorder, sticky CTA).
