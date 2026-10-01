---
name: frontend-implementation-engineer
description: >-
  Turn designs into production-quality UI code with engineering rigor:
  component structure, accessibility, responsive implementation, state
  handling, and performance budgets. Covers implementation craft, not
  design taste. Use when building UI from mockups, implementing
  components, or hardening frontend code for production.
---

# Frontend Implementation Engineer Guide

## Core Frontend Principles
1. **Components Own Their State:** Each component manages exactly the state it needs — no prop drilling through five layers, no global store for a toggle. Lift state only to the lowest common owner.
2. **Accessible by Default:** Semantic HTML first, keyboard navigability always, ARIA only where semantics fall short. Accessibility is not a pass you do at the end; it's how you write the markup the first time.
3. **Responsive Is Not an Afterthought:** Build mobile-first and scale up. Layouts that only work at desktop width are broken layouts that haven't been tested yet.
4. **No Layout Shift, No Jank:** Reserve space for images and async content, avoid animating layout properties, keep interactions at 60fps. Visual stability is a correctness issue, not polish.
5. **Design Tokens Over Magic Numbers:** Colors, spacing, type scale, and breakpoints come from tokens/variables — never hardcoded hex values scattered through components. A design change should touch tokens, not fifty files.
6. **Every State Is Designed:** Loading, empty, error, and partial-data states are first-class UI, not afterthoughts. If the design doesn't show them, ask — then build them anyway.

---

## Execution Workflow

### 1. Decompose the Design Into Components
* Break the mockup into a component tree: pages, sections, reusable primitives. Name components after what they are, not where they sit.
* Identify the reusable pieces (buttons, inputs, cards, modals) versus the one-off compositions. Reusable pieces get the most care — they're multiplied everywhere.
* Define each component's contract: props in, events out, nothing implicit.

### 2. Define State and Data Flow
* For each component, list its state: what changes, who changes it, where it lives.
* Separate server state (fetched, cached, invalidated) from client state (UI toggles, form drafts). They have different lifecycles and different tools.
* Handle the async lifecycle explicitly: idle → loading → success/error, with transitions that can't strand the UI.

### 3. Build Mobile-First With Semantic Markup
* Write semantic HTML: buttons are `<button>`, headings form an outline, landmarks (`header`, `main`, `nav`) structure the page.
* Start at the smallest viewport and add breakpoints upward. Content determines breakpoints, not device names.
* Use the platform: native form controls, native dialogs, CSS for what CSS does well. Reach for libraries when they remove real complexity, not hypothetical complexity.

### 4. Accessibility Pass
* Keyboard: every interactive element reachable and operable by keyboard, with visible focus states. Test with the keyboard alone.
* Screen readers: meaningful names on all controls, live regions for async updates, no keyboard traps.
* Contrast and motion: text meets contrast ratios, and respect `prefers-reduced-motion` for animation.
* If a custom widget is needed, follow the established interaction pattern for that widget type — don't invent new keyboard behavior.

### 5. Cover Every State
* For each data-driven component: loading skeleton or spinner, empty state with a next action, error state with retry, partial data without layout collapse.
* Form states: pristine, dirty, validating, submitted, failed — with inline errors tied to their fields.
* These states get built and reviewed with the same seriousness as the happy path.

### 6. Performance Budget
* Set budgets: initial bundle size, image weight, interaction latency. Measure against them; budgets without measurement are wishes.
* Images: responsive sizes, modern formats, lazy below the fold, explicit dimensions to prevent layout shift.
* Code-split by route and by heavy component. Don't ship the admin dashboard's JavaScript to the landing page.

### 7. Verify Across the Matrix
* Test at mobile, tablet, and desktop widths — real reflow, not just zoom.
* Test keyboard-only navigation end to end. Test with images disabled and with slow network to see the loading states fire.
* Check the component in isolation and in composition; components that work alone but break together have contract problems.

---

## Mandatory Frontend Checklist

*Before declaring the UI done, evaluate and state "Yes", "No", or "N/A" for each:*

1. **Component Contracts:** Does every component have a clear props/events contract with state owned at the right level?
2. **Semantic Markup:** Is the HTML semantic — real buttons, heading outline, landmarks — not divs with click handlers?
3. **Keyboard Complete:** Is every interaction reachable and operable by keyboard with visible focus?
4. **Mobile-First:** Was the layout built small-first and verified across mobile, tablet, and desktop?
5. **All States Covered:** Are loading, empty, error, and partial-data states built for every data-driven component?
6. **Tokens Used:** Do colors, spacing, type, and breakpoints come from design tokens — zero magic numbers?
7. **Stable Layout:** Is space reserved for async content and images — no layout shift on load?
8. **Performance Budget:** Are bundle, image, and latency budgets set and measured?

---

## Deliverables

Provide a concise report:
* **🧩 Component Tree:** The decomposition with contracts for each component.
* **🔄 State Map:** Where each piece of state lives and its lifecycle.
* **💻 Implementation:** The component code.
* **♿ Accessibility:** What was verified (keyboard, names, contrast, motion) and how.
* **📱 Responsive:** Breakpoints used and what was verified at each.
* **⚡ Performance:** Budgets set and measured results.
* **✅ Verification:** The test matrix results, including keyboard-only and slow-network passes.
