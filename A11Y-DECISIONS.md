# A11Y Decisions Log (Pattern Memory)

> **Purpose:** cross-turn memory of choices between **equally conformant alternatives**. Two implementations can both pass `A11Y.md` and axe and still diverge (different `role`, focus pattern, announcement wording) — twenty compliant modals, zero coherence. This file prevents that.

## Rules for the AI

1. **Record only what is not derivable** from `A11Y.md` rules or from the code itself. Landmarks, headings, and alt presence are machine-verifiable — they do **NOT** belong here.
2. **Index by pattern, never by screen.** ✅ *"Destructive confirmation modal → `alertdialog`"* · ❌ *"The dispute modal does X"*.
3. **One line per decision:** pattern → choice → short why.
4. **Read before building:** before generating any interactive component, check this log and reuse the recorded pattern (see *Component Reuse* in the AI Behavior Contract).
5. **Never fork silently:** if a new requirement contradicts a recorded decision, ask the user — do not create a parallel variant.
6. **Stay lean:** tens of lines, not hundreds. This file shares the context budget with Lazy Loading; if it grows past ~40 entries, consolidate.
7. **Versioned, never gitignored:** this is *shared* memory — across turns, agents and developers. A local-only copy per developer forks the patterns and defeats the file's entire purpose.

## Decisions

<!-- Append entries below. Format: - **Pattern** → choice — why. (date) -->

- **Entry animation (hero, above the fold)** → animate **transform only, never opacity**. These run with `animation-fill-mode: both`, which holds the from-state whenever the animation does not advance (throttled background tab, print render, engine that skips animation). With opacity in the keyframes that state is `opacity: 0` and the hero copy and primary CTA are simply absent — the §6 "Content Held Hostage" failure in CSS rather than JS. Sliding cannot hide anything. Observed live during implementation, not theorised. *(2026-08-19)*
- **Scroll reveal (below the fold)** → opacity fade **is** allowed here, because the hidden state only exists once an inline pre-paint script has set `html.js-reveal`, i.e. only when something is guaranteed to be able to undo it. Plus two backstops: a viewport sweep on `load` and on `visibilitychange`, and a hard release if the IntersectionObserver has delivered nothing within 2s. Satisfies guide-loading-skeleton.md's own test ("JS enabled but throttled: never blank while content sits in the DOM"). *(2026-08-19)*
- **Reduced-motion authoring direction** → all **new** motion is written reduced-first: no animation at default, motion added only inside `@media (prefers-reduced-motion: no-preference)`. Pre-existing Desker motion keeps the inverted form (a global `*{animation:none!important}` inside a `reduce` block) — a **House Rule† relaxation, not an SC deviation**: SC 2.3.3 is satisfied either way, and that blunt global reset is in practice harder to miss than per-rule inversion. Recorded here rather than in `EXCEPTIONS.md` because no WCAG SC is being waived. Revisit only if that file is substantially refactored. *(2026-08-19)*
- **Smooth-scroll library** → **Lenis, self-hosted** (`/lenis.min.js`), never from a CDN. Both pages otherwise make zero third-party requests, and Desker's entire pitch is "no telemetry, no accounts, no third parties" — loading a CDN script on its own marketing page would contradict the product's claim and leak visitor IPs to another host. *(2026-08-19)*
- **Smooth scroll vs. assistive interaction** → when Lenis is active it owns `scrollTop`, which silently breaks two things the browser does for free. Both are restored explicitly: (a) `focusin` scrolls an off-screen control into view, or the focus ring lands outside the viewport (SC 2.4.7); (b) in-page anchors move **focus** as well as scroll, or keyboard users are scrolled somewhere their focus never went — this is also what keeps the skip link working. Any future scroll-hijacking library inherits these two obligations. *(2026-08-19)*
- **`prefers-reduced-motion` and smooth scroll** → Lenis starts **off** and is enabled only under `no-preference`, and it is destroyed live if the user flips the preference mid-session (`matchMedia` change listener), rather than only being read once at load. Scroll-driven motion is a vestibular trigger the user never asked for — guide-media.md §5. *(2026-08-19)*
