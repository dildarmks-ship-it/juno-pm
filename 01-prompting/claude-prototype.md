# Juno PM — Project Summary

**Live prototype:** https://claude.ai/code/artifact/2debaccf-2de8-41d0-975f-dbbccb6b8524

## Concept

Juno is an AI Associate PM at RocketShip. Its job is to take the raw, messy inputs a product manager collects — interview transcripts, support tickets, executive emails — and turn them into a structured, evidence-backed PRD draft, without the PM manually jumping between Slack, Notion, and Jira to piece it together.

This prototype is a clickable, three-column dashboard that demonstrates that workflow end to end in the browser, with no backend: paste raw text, click one button, and watch it become tagged insights and a rendered Opportunity Brief.

## Layout

The dashboard is a single-screen, three-column workspace with no sidebar or top navigation, and no settings, config, or login flows — straight to the tool, as scoped for V1.

- **Left — Raw User Transcripts.** A large textarea for pasting interviews, tickets, and emails. Ships with a realistic sample (a customer interview, a support ticket, and an exec email) preloaded so the flow is demoable immediately. Includes "Load sample" and "Clear" controls and a live character count.
- **Center gutter — Process Transcript.** A persistent action button anchored between the left and center columns in its own fixed-width strip, vertically centered and pinned via `position: sticky`. It never scrolls out of view regardless of how far either column is scrolled.
- **Middle — Structured Insights.** Cards generated from the raw input, each tagged with a Priority (High / Medium / Low) and a Sentiment (Positive / Neutral / Negative / Frustrated), plus a guessed source type (User Interview / Support Ticket / Executive Email) and a quoted snippet.
- **Right — Draft PRD.** A live-rendered Opportunity Brief in the standard PRD shape: Problem Statement, Supporting Evidence (citing the specific insights behind it), Proposed Direction, Success Metrics, and Open Questions. A "Copy markdown" button lets a PM drop the raw markdown straight into Notion or Jira.

## How "Process Transcript" works

Clicking the button shows a 1.5-second loading state, then runs a small rule-based synthesis engine entirely in the browser:

1. **Chunking** — the raw input is split into paragraphs (or sentences, if it's unparagraphed).
2. **Classification** — each chunk is scored against keyword lists for sentiment (positive/negative language), priority (urgency signals vs. "nice to have" phrasing, with basic negation-handling so phrases like "not a blocker" don't get misread as urgent), and topic (pricing, reliability, onboarding, notifications, integrations, mobile, collaboration, or reporting).
3. **Card generation** — each classified chunk becomes an Insight card with a human-readable label, its tags, its likely source, and a quoted excerpt.
4. **Brief generation** — the tagged insights are aggregated into a markdown Opportunity Brief: the dominant theme becomes the title, the evidence section cites specific insights, and the proposed direction is pulled from a small set of theme-specific suggestions.

This is a heuristic, not a real LLM call — it's meant to demonstrate the *interaction pattern* of an AI PM assistant, not to be production NLP.

## Design system

The visual style has gone through three passes (see Revision history below); this reflects the current one, modeled on Linear's product UI.

- **Palette:** near-black base (`#08090a`), flat card surfaces (`#131316`), and hairline borders at `rgba(255,255,255,0.08)` — no gradients, no glow/drop-shadow chrome. A single interactive accent, a deep teal (`#0f8a93`, matched to the user's company brand from a reference screenshot), is used consistently for the Process Transcript button, focus rings, links, and priority indicators. Small semantic colors (muted red/green) are used only for sentiment and status dots, kept deliberately restrained rather than as colored backgrounds — this mirrors how Linear itself pairs one brand hue with a tiny functional status palette rather than staying strictly monochrome.
- **Typography:** Inter throughout (headings and body), with tight negative letter-spacing and Inter's `cv11`/`ss01` stylistic-set features enabled — the same typographic choices Linear's own app makes. JetBrains Mono is kept for a few data-like elements (character count, card metadata).
- **Priority indicator:** Linear's actual "signal bars" pattern — three ascending bars, filled left-to-right by priority level (Low = 1 bar, Medium = 2, High = 3) — instead of colored text pills.
- **Sentiment indicator:** a small colored dot plus label, rather than a pill background.
- **Spacing & radius:** denser paddings and smaller corner radii (6–8px) than a typical "SaaS glow" dashboard, for the flatter, information-dense feel of a tool people live in all day.
- **No page-level scroll:** the three columns each scroll independently within a fixed `100vh` frame, which is what keeps the Process Transcript button permanently visible.

## Revision history

1. **V1 — dark mode, single violet accent (`#7c6cf6`).** Original build to the brief: three equal columns, no nav/settings, gradient-and-glow SaaS aesthetic.
2. **Brand color pass.** Accent swapped to a deep teal matched from a screenshot of the user's company site (Accredo/Evernorth), split into two shades (a darker fill for white-text buttons, a brighter one for text/glow on the dark background) to keep WCAG contrast in both directions.
3. **Linear-style refresh (current).** Full pass on palette, typography, borders, and spacing to match Linear's product UI specifically — flat surfaces, hairline borders, tighter Inter type, and Linear's signal-bar priority icon in place of colored tags. Three-column layout and the Process Transcript button were kept intact throughout, per the brief.

## Constraints honored

- Three equal-width columns, no reflow at 1280px and above.
- No sidebar, top navigation, settings, configuration, or auth flows.
- Process Transcript button always visible, never hidden behind scroll.

## Format note

This prototype lives as a single self-contained HTML file (inline CSS/JS, no external dependencies besides Google Fonts), published as a Claude Artifact rather than delivered as a static file — that's what gives it a persistent URL that updates in place and can be reopened or shared without this conversation.

## What it demonstrates

This prototype proves that raw, unstructured customer feedback — interviews, tickets, and exec emails — can be analyzed accurately enough to surface what should be prioritized first, without a PM manually triaging every input by hand.

_____

## Debrief

- **What worked:** Claude came to the rescue — instead of building the prototype in Lovable, Claude built and iterated on the whole thing directly: the initial clickable three-column dashboard, then two full rounds of visual refinement (matching the company's brand color, then a Linear-style redesign), all without touching a design tool or writing code by hand.
- **What broke / felt like a toy:** When the prompt was loaded into Lovable separately, the UI rendered fine but the transcript analysis itself didn't actually run — clicking through produced the shell without the processed output. Not clear yet whether that was a caching issue or something deeper in how that build handled the logic.
- **What I'd change next pass:** Add working links on the top-right integration row — right now Slack, Notion, and Jira are just decorative status dots; next pass they'd actually link out to the connected workspace, page, or board.
