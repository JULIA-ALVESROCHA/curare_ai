# Curare.AI — Interface & User Experience Core (Frontend)

**Owner:** Júlia Alves — *Strategic Design & Product Co-Lead*
**Code focus:** JavaScript / TypeScript / React — Frontend Development & Data Visualization

This module is the layer the researcher interacts with directly. It translates Curare.AI's visual identity and high-fidelity prototypes into a real, responsive, interactive web application — including the dynamic dashboard where the researcher visualizes connection networks and cross-cultural convergence charts.

---

## Scope of this deliverable

This part of the project covers **everything the user sees and manipulates** on the platform:

- The responsive web interface (React).
- The design system (visual identity applied as real components).
- The interactive discovery-graph visualization.
- The convergence charts and hypothesis readouts.
- The end-to-end usage experience (navigation, search, light/dark theme, loading and error states).

It does not cover the data/back-end layer or the scientific hypothesis-generation logic, which belong to other tracks of the team.

---

## Responsibilities & practical tasks

### 1. Translate visual identity and prototypes into real components
- Design system implemented in code: color tokens, typography (Fraunces for headings, Inter for body), radii, shadows, and standardized spacing via CSS custom properties.
- Reusable component library: buttons, cards, type/quality chips, segmented control, form fields, avatar, switches, page headers, and empty states.
- A **real, working light/dark theme**, applied across the entire surface (including the graph and the command palette) through a `data-theme` attribute on the root.

### 2. Build the responsive web interface (React)
- Single-page React application with navigation across screens: Dashboard, Discovery graph, Hypotheses, Research assistant, Provenance/Evidence, Dossiers, Workspace, and Settings.
- Responsive layout: on narrow screens the sidebar becomes a drawer, the graph's side panel becomes an overlay, and spacing compacts.
- Command palette (`⌘K` / `Ctrl+K`) with search across pages and nodes, and keyboard navigation.
- Per-user client-side persistence: bookmarks, assistant threads, graph state, last route, and preferences are restored on the next session.

### 3. Code the dynamic data-visualization dashboard
**The heart of this deliverable.** This is where the researcher sees the connection networks and cross-cultural convergence.

- **Interactive discovery graph** — a layered map (culture → species → compound → target → disease, with evidence beneath). Supports pan, zoom-to-cursor, node dragging, search, type filtering, and a selection that lights up the entire connected chain while the rest dims.
- **Custom force simulation** (a resumable `requestAnimationFrame` loop with link springs, repulsion, and lane gravity) that "reheats" on interaction.
- **Relationship-typed edges** — established, predicted, and evidence-backed, each with a distinct visual style.
- **Convergence charts** — a convergence score per hypothesis, with per-factor breakdown bars and grouping of the nodes that support each hypothesis.
- **Traceable citations** — the assistant references evidence as `[ev-N]` pills that link straight to the provenance record.

> Technical note: the current version implements the visualization with a custom SVG force simulation (no external graph dependency). The architecture is ready to incorporate graphics libraries such as **D3.js** should the graph's scale require it — the layout layer is already isolated and driven by the measured container size.

---

## Tech stack

- **React** (hooks) — single component, no mandatory build framework in the artifact.
- **lucide-react** — icons.
- **SVG + custom force simulation** — graph rendering and layout.
- **CSS custom properties** — light/dark theming.
- **Fraunces + Inter** — typography (Google Fonts).
- **TypeScript** — evolution target for typing the node/edge model and component props.

---

## How to run

It's a single-file React component (`Curare.jsx`). The simplest way to run it is to paste it into a Claude.ai artifact, where it renders directly.

To run it in your own React project (Vite or CRA):

1. Drop `Curare.jsx` into the project.
2. Install the one runtime dependency:
   ```bash
   npm install lucide-react
   ```
3. Import and render the default export:
   ```jsx
   import App from "./Curare.jsx";

   export default function Root() {
     return <App />;
   }
   ```

---

## Keyboard shortcuts

- `⌘K` / `Ctrl+K` — open command palette
- `Esc` — close palette / sidebar drawer
- `Enter` — submit in login and assistant inputs

---

## Frontend decisions worth recording

- **Login without a native `<form>`** — sandboxed iframes block native submit, so authentication uses `onClick` handlers (this was the root cause of the old "the buttons don't work").
- **Graph SVG fills its container via absolute positioning** — the SVG has no `viewBox` (it uses screen-space pan/zoom transforms); resolving `height: 100%` on a replaced element was collapsing to its default intrinsic height. Absolute positioning fixes it, and the `ResizeObserver` then measures the real height.
- **Theme applied at the root** — so the entire surface, including the graph and palette, switches theme together.

---

## Status

Under active development for **CICSIC 2026**. The dataset is illustrative and curated for demonstration; the interface layer is designed to scale to a larger ethnopharmacology graph.
