# Dashboard Visual Research

Access date for reviewed references: 2026-07-03.

Purpose: capture interaction and design patterns from strong data visualization work before changing the BayesTransitEquity dashboard. This is not a requirements spec. It is a design memory file for future dashboard decisions.

## References Reviewed

| Reference | URL | Why it matters here |
|---|---|---|
| Native Land Digital | https://native-land.ca/ | Caveat-forward GIS; overlapping geographies; address search and simple layer controls. |
| Mapping Inequality | https://dsl.richmond.edu/panorama/redlining/ | Map as archival index; selected areas link to documentary evidence and context. |
| Opportunity Atlas | https://www.opportunityatlas.org/ | Dense policy dashboard with place, metric, subgroup, comparison, and download controls. |
| The Pudding Happy Map | https://pudding.cool/2026/02/happy-map/ | Human prompt before exploration; guided story with skip path and methodology panel. |
| The Pudding archive | https://pudding.cool/ | Bespoke data stories that start from human-scale questions, not chart types. |
| Bloomberg, What's Really Warming the World | https://www.bloomberg.com/graphics/2015-whats-warming-the-world/ | Fixed visual frame that tests explanations one by one. |
| NYT You Draw It | https://www.nytimes.com/interactive/2015/05/28/upshot/you-draw-it-how-family-income-affects-childrens-college-chances.html | Reader commits to a belief before the chart reveals reality. |
| NYT How the Virus Won | https://www.nytimes.com/interactive/2020/us/coronavirus-spread.html | Layered evidence types build one causal story. |
| NYT Tulsa Race Massacre reconstruction | https://www.nytimes.com/interactive/2021/05/24/us/tulsa-race-massacre.html | Reconstruction makes loss legible before showing destruction. |
| Reuters Graphics | https://www.reuters.com/graphics/ | Compact visual-question archive with dated cards and direct premises. |
| Parable of the Polygons | https://ncase.me/polygons/ | Playable model: manual action, simulation, sliders, sandbox. |
| The Evolution of Trust | https://ncase.me/trust/ | Game first, theory second; named strategies make model behavior memorable. |
| Gapminder Tools | https://www.gapminder.org/tools/ | Expert cockpit with linked views, time playback, trails, search, and data doubts. |
| Observable D3 Gallery | https://observablehq.com/@d3/gallery | Small reusable interaction patterns with visible provenance and resettable interactions. |
| Distill Feature Visualization | https://distill.pub/2017/feature-visualization/ | Technical explainability through progressive visual examples, alternatives, and caveats. |
| We Feel Fine | http://wefeelfine.org/ | Data as human utterance first, metric second; emotional browsing and filtering. |
| Information Is Beautiful Awards | https://www.informationisbeautifulawards.com/showcase | Broad pattern library for civic, artistic, editorial, and analytical visual forms. |

## Core Design Thesis

BayesTransitEquity should be map-first, caveat-forward, and uncertainty-native.

The dashboard should not present a single deterministic equity surface as the truth. Its strongest product promise is to help users inspect where decisions are stable, where threshold classifications are ambiguous, and which interventions are likely to move places across policy-relevant boundaries.

The simplest winning shape is:

- One persistent map.
- One control panel for `where`, `what`, and `who`.
- One detail panel for selected tract evidence, uncertainty, and intervention response.
- Three modes max: `Explore`, `Compare`, `Explain`.

## Patterns To Steal

### 1. Put Caveats Beside The Map

Native Land Digital is valuable because the caveat is part of the map experience. It does not pretend mapped boundaries are neutral facts.

For this dashboard:

- Show "model estimate", "data vintage", and "posterior uncertainty" in the map chrome.
- Add a visible data/model doubts panel, inspired by Gapminder, for sparse data, assumptions, and fragile estimates.
- Put tract-level caveats in the selected-tract panel, not only in a methodology page.

### 2. Use `Where / What / Who`

Opportunity Atlas has the right policy-dashboard grammar.

For this dashboard:

- `where`: tract, region, route shed, or selected geography.
- `what`: accessibility, exceedance probability, credible interval width, Wasserstein/divergence, intervention delta.
- `who`: population group, disadvantage stratum, vehicle access group, race/ethnicity lens where data supports it.

Scenario toggles should sit near these filters. They should not feel like a separate app.

### 3. Make Uncertainty Playable

Nicky Case and Gapminder both make models understandable by letting users manipulate assumptions and watch the system move.

For this dashboard:

- Add threshold controls for 20, 30, 45, and 60 minutes.
- Animate or scrub selected tracts across baseline and intervention scenarios.
- Show probability of threshold crossing, not just before/after class labels.
- Consider a small "predict before reveal" interaction for story mode: ask where a headway improvement will help most, then reveal posterior gain and uncertainty.

### 4. Keep A Stable Visual Frame

Bloomberg's climate explainer works by keeping the same chart frame while testing one explanation at a time.

For this dashboard:

- Do not make every story step change layout.
- Reveal baseline access, demographic exposure, posterior uncertainty, threshold sensitivity, and intervention delta in the same spatial frame.
- Write annotations as testable questions: "Does this intervention close the 30-minute access gap?"

### 5. Link Every Claim To Evidence

Mapping Inequality succeeds because the map points into source records, not just colored areas.

For this dashboard:

- A selected tract should show observed inputs, posterior summaries, model run id, and exported artifact links.
- Add download/share state early.
- Let users inspect why a tract changed rank or classification.

### 6. Humanize Before Abstracting

The Pudding, We Feel Fine, and the Tulsa reconstruction all begin from lived or reconstructed experience before abstraction.

For this dashboard:

- Start story mode with reachable opportunities: jobs, clinics, groceries, schools.
- Use concise tract narratives such as "this tract is near the threshold; small assumptions change the classification."
- Avoid starting public-facing views with model jargon.

### 7. Make Technical Work Inspectable

Distill and Observable show that complex technical work feels trustworthy when examples, alternatives, and methods are adjacent to the claim.

For this dashboard:

- Add a collapsible "How this was computed" panel.
- Link map, scatter, rank, and table selections with brushing.
- Provide reset buttons for zoom/brush/filter state.
- Show posterior samples or interval bands where practical, especially in the detail panel.

## Proposed Dashboard Moves

Near-term changes should bias toward a practical dashboard, not a full scrollytelling production.

1. Add a top-level segmented mode control: `Explore`, `Compare`, `Explain`.
2. Build a persistent selected-tract drawer with:
   - deterministic accessibility,
   - posterior exceedance probability,
   - 95% interval or uncertainty band,
   - threshold-crossing probability,
   - scenario delta and probability of improvement,
   - data/model caveats,
   - provenance links.
3. Add `where / what / who` controls:
   - geography search and tract selection,
   - metric selector,
   - population/equity lens selector,
   - color-scale mode: regional, on-screen, posterior-relative.
4. Add scenario playback:
   - baseline,
   - Bayesian targeting,
   - deterministic targeting,
   - selected intervention strength.
5. Add a compact story gallery:
   - "Who falls below 30-minute access?"
   - "Which intervention closes the gap?"
   - "Where is uncertainty too high to act confidently?"
   - "Which tracts are near the policy threshold?"
6. Add linked secondary views:
   - map,
   - tract rank list,
   - uncertainty distribution,
   - intervention comparison.

## Things To Avoid

- Hiding caveats in a footer or methodology-only route.
- Too many equal-weight controls.
- Hover-only interactions.
- Canvas-only text without accessible alternatives.
- Blank or fragile loading states.
- Asset-heavy narrative shells before the core dashboard is useful.
- Treating animation as decoration rather than as a way to reveal posterior/scenario change.
- Letting color-scale changes look like data changes.

## Useful First Implementation Target

The first dashboard revision should probably ship the smallest coherent version:

- `Explore`: map plus tract drawer.
- `Compare`: baseline vs scenario, with threshold-crossing and selected-tract delta.
- `Explain`: selected tract evidence, uncertainty, assumptions, and provenance.

This gives the project a clear design spine while leaving room for later public-facing storytelling.
