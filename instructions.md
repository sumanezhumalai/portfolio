# Skills Interactive Grid — Implementation Instructions

## 1. Purpose

Replace the currently empty Skills/Values section with a minimal interactive skills grid inspired by the supplied reference image.

The result must feel native to the existing portfolio rather than looking like a separate component or a copied technology-logo wall.

The implementation is intentionally static-first:

- no framework
- no new UI library
- no animation library
- no runtime data fetching
- no client-side state
- no click interaction
- CSS-driven hover behavior
- existing SVG assets remain the source of the skill icons
- existing theme variables remain the source of all visual colors

The implementation should preserve the existing portfolio architecture and visual language.

---

## 2. Existing Codebase Context

The current repository is a static HTML/CSS/JavaScript portfolio rather than an Astro application.

Relevant existing structure:

- `index.html` contains the page sections and navigation.
- `styles/about.css` contains the main section/layout styling.
- `styles/color.css` defines the theme system and foreground/background alpha levels.
- `styles/variables.css` defines the global grid, gutters, margins, typography, and responsive grid presets.
- `styles/grid.css` controls the existing developer grid overlay.
- `styles/media-queries.css` changes the main grid from 12 columns to 6, 3, and 2 columns at smaller widths.
- `scripts/script.js` already treats `.section.values` as the Skills destination and scroll target.
- The current `.section.values` exists in the document but its content and section-specific styling were intentionally removed during cleanup.

Do not convert this feature to Astro, React, or another framework.

Do not replace the existing design system.

---

## 3. Section Identity

Use the existing `.section.values` section as the Skills section.

Do not introduce a separate page.

Do not add a visible section title.

Do not add a description, introductory paragraph, category headings, badges, cards, buttons, or explanatory text.

The visual content of the section should be the grid itself.

The existing navigation label `Skills` should continue to scroll to this section.

The existing scroll/navigation logic already targets `.section.values`; preserve that relationship.

---

## 4. Overall Visual Direction

The reference is the structural inspiration only.

Target appearance:

- minimal
- monochrome
- restrained
- editorial
- technical
- spacious
- geometric
- quiet
- aligned with the existing portfolio

Do NOT reproduce the colorful technology-logo-wall appearance from the reference.

The portfolio's active theme must determine the visual colors.

The grid should look like it belongs to the existing page, including its typography, margins, spacing, contrast levels, and theme transitions.

---

## 5. Layout Strategy

Use a CSS Grid-based layout.

The skill area should occupy the existing section content region and align with the site's established page grid rather than introducing an unrelated centered container.

Respect the existing:

- application margin
- application gutter
- column system
- section alignment
- responsive breakpoints

The desktop layout should use a stable number of equal cells per row.

Each skill occupies exactly one grid cell.

Every cell must have identical dimensions within a given viewport.

Icons must be centered inside their cells.

The grid must not depend on individual SVG dimensions to determine cell size.

The cell itself is the sizing authority.

---

## 6. Responsive Behavior

The grid must scale down cleanly as the viewport becomes smaller.

Do not allow a fixed desktop-sized grid to overflow horizontally.

Do not use horizontal scrolling for the Skills grid.

Do not simply shrink the icons while keeping an oversized desktop grid.

Instead, reduce the number of columns as available width decreases.

The implementation should follow the same responsive philosophy already used by the portfolio:

- wide desktop: more columns
- medium width: fewer columns
- tablet/small desktop: fewer columns
- mobile: very few columns

The exact column counts should be selected according to the number of skills and the available width, while preserving equal square cells.

Cell dimensions should remain visually balanced as the viewport changes.

The grid should reflow automatically without JavaScript resize calculations.

Prefer CSS responsive layout over JavaScript measurement.

---

## 7. Cell Geometry

Every skill cell must be equal.

Requirements:

- equal width
- equal height
- consistent internal padding
- consistent icon size
- consistent border/line treatment
- consistent alignment

The icon must never determine the cell size.

The cell should remain square or visually square at all supported viewport sizes.

Do not create larger cells for important skills.

Do not create different icon sizes based on logo type.

---

## 8. Grid Lines

The grid should visually resemble a technical/editorial construction grid.

Use thin lines with low theme-relative opacity.

Grid lines must use the existing foreground color variables rather than a hard-coded gray.

The normal grid-line tone should be subtle, equivalent to the existing low-opacity foreground levels already used by the site.

Do not use a new fixed color palette.

Do not use bright borders.

Do not use rounded card borders.

The grid should feel like a single continuous structure rather than a collection of cards.

Avoid excessive border emphasis around individual cells.

---

## 9. Edge Fade Requirement

The outer ending edges of the grid lines must fade smoothly.

This is important.

The fade should make the grid appear to dissolve into the surrounding page rather than terminate as hard vertical/horizontal lines.

The fade must be theme-aware.

Do not use a hard-coded white or black overlay.

Preferred implementation strategy:

- keep the skill/icon content separate from the decorative grid-line layer
- apply the fade only to the grid-line layer
- use a CSS mask/gradient-based edge fade
- use the existing theme background/transparent color variables as the fallback/visual basis
- preserve the icons at full visibility near the edges

The fade should be subtle, not a large glow.

The edge fade should exist on desktop and mobile.

If the implementation uses a CSS mask, include the appropriate browser-compatible mask form for modern browsers.

Do not implement the fade with JavaScript.

Do not animate the fade.

---

## 10. Skill Asset Source

Use the existing SVG skill assets under:

`/assets/svg/`

Do not replace them with an icon package.

Do not download a new icon library.

Do not embed a large icon font.

Do not inline a large collection of SVG paths into the page.

Each skill should reference its corresponding existing SVG file.

The filename is the source of truth for the skill identity.

There should be no separate manually maintained display-name field unless an exceptional filename requires normalization.

---

## 11. Skill Name Rule

The displayed skill name must come from the SVG filename itself.

For example, conceptually:

- SVG filename -> skill identity
- SVG asset -> icon
- no duplicated manual skill-name definition

The implementation should strip only the file extension and apply minimal presentation normalization if needed.

Do not maintain two independent lists where the filename and displayed name can drift apart.

If a filename needs a human-readable presentation, derive it systematically from the filename rather than manually duplicating the name.

---

## 12. Monochrome SVG Treatment

All skill icons must visually use the same monochrome language.

Preferred approach:

1. Keep the original SVG files intact where possible.
2. Apply a CSS monochrome treatment to externally referenced SVG images when the SVG structure does not reliably expose its internal fills/strokes.
3. If a particular SVG cannot be reliably normalized through CSS because of its internal color structure, edit that SVG once to make its artwork monochrome.
4. Do not maintain separate light/dark copies of every icon.

The monochrome icon appearance should normally use a theme-derived foreground color with reduced opacity.

Avoid manually assigning different colors to different technologies.

The hover state should increase contrast/opacity using the existing foreground color system.

---

## 13. Performance Preference for SVGs

Prefer CSS treatment over editing every SVG when the existing assets can be normalized reliably.

The goal is to avoid duplicating assets and avoid adding runtime image-processing work.

Do not convert SVGs into PNGs.

Do not rasterize the logos.

Do not introduce an SVG processing dependency only for this section.

SVG should remain the native asset format.

If some SVGs contain hard-coded fills that prevent reliable monochrome rendering, normalize those specific files once rather than adding a runtime transformation system.

---

## 14. Icon Sizing

Every icon must use one consistent visual box.

The icon image itself should have a fixed maximum visual size relative to the cell.

Do not let intrinsic SVG dimensions create different apparent icon scales.

Use a consistent icon box and contain the SVG within it.

Preserve the SVG aspect ratio.

Do not stretch logos.

Do not use individual per-icon size overrides unless a specific asset is objectively malformed.

If a logo has unusual whitespace inside its SVG viewBox, correct the asset itself rather than compensating with a one-off CSS size.

---

## 15. Normal State

Default skill icon appearance:

- monochrome
- subdued
- low-to-medium foreground opacity
- visually secondary to the page's main typography
- no shadow
- no glow
- no colored background
- no card background
- no badge

The grid lines should remain subtle behind/around the icons.

The overall section should initially look almost like a quiet technical diagram.

---

## 16. Hover Interaction

Hovering a skill should highlight only that skill.

The intended response:

- icon becomes more visible
- icon opacity increases toward the active foreground color
- optional very small scale increase is acceptable if it remains subtle
- the corresponding cell can slightly increase visual contrast
- no large movement
- no tooltip
- no label expansion
- no color explosion
- no glow

The interaction should be CSS-only.

Use transitions based on opacity/color/transform where appropriate.

Keep the transition short and understated.

---

## 17. Click Behavior

Clicking a skill must do nothing.

Skills are informational visual elements, not navigation controls.

Do not add:

- links
- anchors
- modals
- popovers
- project pages
- filters
- expandable panels
- click-selected state

The user explicitly requested hover interaction only.

---

## 18. Touch Devices

There is no click/tap action.

On touch devices, the component should remain usable and visually stable.

Do not emulate hover with a persistent JavaScript-selected state.

Do not introduce touch-specific interaction logic.

The normal icon presentation is acceptable on touch screens.

---

## 19. Skill-to-Project Relationship

Keep skill membership and skill/project relationships modular.

Do not hard-code project relationships into the visual grid markup.

The visual component should know:

- which skill assets exist
- which SVG belongs to each skill

It should not need to know the full project presentation logic.

Maintain the relationship separately so that future work can associate:

- a skill with one or more projects
- a project with multiple skills

without restructuring the grid component.

The relationship should be keyed by the skill's filename-derived identity.

Do not duplicate skill names in several unrelated data structures.

For the initial implementation, the relationship can exist as metadata/data even if it is not yet exposed as an interaction.

Do not implement a project-filter interaction now.

---

## 20. Modularity Requirement

The skills list must be easy to change.

Adding a skill should conceptually require:

1. adding the SVG asset to the existing skill asset directory
2. registering/including it in the skill source of truth if the current implementation requires explicit registration
3. optionally associating it with projects in the relationship metadata

Removing a skill should not require redesigning the component.

Reordering skills should not require rewriting layout markup.

The grid component should not contain one-off presentation markup for every individual skill.

---

## 21. Static Implementation Requirement

Keep the generated page static.

Do not fetch a skill list from a server.

Do not fetch project relationships at runtime.

Do not use a database.

Do not add an API.

Do not add a framework island.

Do not use a client-side rendering library.

Do not calculate grid dimensions with JavaScript.

The browser should receive ordinary HTML/CSS and static SVG assets.

---

## 22. JavaScript Policy

Do not add JavaScript specifically for:

- grid layout
- icon hover
- icon highlighting
- responsive resizing
- edge fading
- monochrome processing
- click handling

Existing site JavaScript may continue to control section navigation and active navigation state.

The new Skills grid should not require new runtime behavior.

If the current page already uses JavaScript to update the active `Skills` navigation item based on scroll position, preserve that behavior.

---

## 23. Existing Theme Integration

The current theme system defines:

- foreground at multiple opacity levels
- background at multiple opacity levels
- multiple selectable themes

Use those variables directly.

The grid must respond automatically when the user changes theme.

The following visual properties should be theme-derived:

- grid lines
- normal icon color
- hover icon color
- hover contrast
- edge fade
- any subtle cell highlight

Do not introduce a new independent accent color.

Do not hard-code `#fff`, `#000`, or a fixed gray for the component.

The component should work correctly on both light and dark themes and across the existing theme spectrum.

---

## 24. Relationship to Existing Global Grid

The repository already contains a developer grid overlay controlled by `styles/grid.css`.

Do not confuse that global/debug grid with the new Skills grid.

The Skills grid is permanent visual content.

The existing `.app-grid-overlay` remains an independent debugging/design overlay.

Do not modify its behavior merely to implement the Skills grid.

Do not reuse the overlay DOM as the Skills grid because the overlay is fixed to the viewport and is controlled by the existing grid toggle.

The Skills grid must live inside the Skills/Values section.

---

## 25. Section Positioning

Use the existing section grid architecture.

The current section system normally places content starting at a later grid column on large screens and expands it across the available width at smaller breakpoints.

Follow that behavior unless visual testing shows that the Skills grid needs a deliberate full-width exception.

If a full-width grid is necessary, make the decision explicit and preserve alignment with the existing page margins.

Do not create a second independent page-width system.

---

## 26. Section Spacing

The Skills section should have enough vertical breathing room to feel like a real section, but it should not become a giant empty block.

There is no heading, so spacing must visually establish the section boundary.

Use responsive vertical spacing.

Avoid excessive fixed heights.

The section height should be determined naturally by:

- grid cell size
- number of rows
- controlled section padding

Do not force a full viewport height unless the existing page composition clearly requires it.

---

## 27. Grid Density

The grid should visually resemble the reference without becoming cramped.

Desktop:

- enough columns to create a broad technical matrix
- enough rows to accommodate the current skill count
- equal square cells

Mobile:

- fewer columns
- larger relative cell area so icons remain recognizable
- more rows as a natural consequence of reflow

Do not preserve a fixed number of columns at every width.

---

## 28. Skill Ordering

Use a deterministic order.

The initial order should follow the existing intended skill sequence if one already exists.

Do not sort alphabetically unless the current content clearly establishes that as the intended behavior.

The order should be controlled by the skill source of truth, not by DOM insertion scattered across unrelated markup.

---

## 29. Accessibility

Even though the skills are non-interactive, they must remain understandable.

Each SVG should have appropriate accessible text derived from the skill name.

Because clicking does nothing, do not give the skill items button/link semantics.

The hover effect must not be the only way to understand the content.

Do not rely on color alone for meaning.

Maintain sufficient contrast between the normal icon state and the background.

---

## 30. Reduced Motion

The hover transition must respect the site's reduced-motion preference.

For users requesting reduced motion:

- remove/reduce transform movement
- retain only a simple opacity/color transition or make it effectively immediate

Do not introduce continuous animation.

No pulsing icons.

No floating logos.

No animated grid movement.

---

## 31. Browser/Rendering Considerations

Prefer standard CSS features already appropriate for a modern static portfolio.

Do not use experimental layout mechanisms when normal CSS Grid can solve the problem.

For the edge fade, use a broadly supported CSS masking/gradient approach with the necessary compatibility form.

Do not depend on JavaScript to compensate for browser layout differences.

SVG rendering must preserve aspect ratio and remain crisp at all viewport sizes.

---

## 32. Files/Areas Expected to Change

Keep the implementation localized.

Likely areas:

- `index.html` — populate the existing `.section.values` content.
- `styles/about.css` or a dedicated Skills stylesheet — add the Skills grid presentation.
- `styles/media-queries.css` — only if existing responsive rules need a small Skills-specific adjustment.
- `assets/svg/` — normalize only SVGs that cannot be reliably rendered monochrome through CSS.
- a small skill metadata/source file may be introduced if needed for modularity, but it must remain static and lightweight.

Do not rewrite unrelated sections.

Do not refactor the whole stylesheet.

Do not replace the existing theme architecture.

Do not migrate the project to a framework.

---

## 33. Recommended Component Boundaries

Because this is a plain static site, do not create a large component architecture.

Conceptually separate the feature into:

- Skills section container
- grid structure
- individual skill cell
- skill asset source/metadata
- optional skill/project relationship metadata

These can remain simple static markup/data structures.

Do not introduce a generic component system just for this feature.

---

## 34. Implementation Sequence

Implement in this order:

### Step 1 — establish the section

Populate the existing Skills/Values section with the grid structure.

### Step 2 — establish the equal-cell layout

Create the responsive CSS Grid and ensure every cell is equal and square.

### Step 3 — add the SVG assets

Use the existing `/assets/svg/` files.

Derive the skill identity from each filename.

### Step 4 — normalize icon appearance

Make all icons monochrome using the least invasive method that works reliably.

### Step 5 — integrate theme colors

Replace all fixed icon/grid colors with existing theme foreground/background variables.

### Step 6 — add grid lines

Create the thin structural lines using low-opacity theme foreground colors.

### Step 7 — add edge fading

Fade the decorative grid-line layer at the outer edges without fading the icon content.

### Step 8 — add hover

Use CSS-only opacity/contrast and optionally a very small transform.

### Step 9 — validate responsive behavior

Test the layout at desktop, tablet, narrow mobile, and very narrow mobile widths.

### Step 10 — validate theme switching

Test the same grid across the site's light, dark, and intermediate themes.

### Step 11 — validate navigation

Ensure the existing Skills navigation item still scrolls to the populated Values section and remains correctly highlighted.

---

## 35. What Not to Build

Do not add:

- React
- Vue
- Svelte
- Astro
- Tailwind
- icon libraries
- SVG icon libraries
- animation libraries
- GSAP
- Framer Motion
- client-side state
- API calls
- database storage
- tooltips
- modals
- project filtering
- click interactions
- colored technology logos
- individual skill cards
- rounded cards
- gradients used as decorative backgrounds
- glow effects
- particle effects
- scroll-based skill animations

The feature should remain a small amount of HTML/CSS plus existing assets.

---

## 36. Visual Acceptance Criteria

The implementation is correct only if all of the following are true:

- The Skills section has no title.
- The Skills section has no description.
- The main visual is an equal-cell grid.
- The grid aligns with the existing portfolio page margins/grid.
- Every skill icon uses the same visual sizing box.
- SVGs remain SVGs.
- Skill identity comes from SVG filenames.
- Icons are monochrome.
- Colors come from the existing theme variables.
- No technology-specific colors are visible.
- Grid lines are subtle.
- Grid edges fade smoothly.
- Edge fading affects the decorative grid lines rather than making edge icons disappear.
- Hovering highlights only the hovered skill.
- Clicking does nothing.
- No new runtime JavaScript is required for the feature.
- The number of columns changes responsively.
- The cells remain equal and visually square.
- No horizontal overflow is introduced.
- The section works across all existing theme variants.
- Existing Skills navigation continues to work.
- The existing global/debug grid overlay continues to work independently.
- No unrelated page section is visually or behaviorally changed.

---

## 37. Quality Standard

The final result should look deliberately designed, not like a collection of logos placed into CSS Grid.

The key visual idea is:

**quiet grid + monochrome icons + theme-aware contrast + subtle hover + fading boundaries**

The reference image should be treated as inspiration for the structural concept only.

The portfolio's existing typography, theme system, spacing, and grid language are the authoritative design system.

When forced to choose between visual complexity and consistency with the existing site, choose consistency.

When forced to choose between JavaScript and CSS for this feature, choose CSS.

When forced to choose between a new dependency and an existing browser capability, choose the browser capability.

When forced to choose between a clever abstraction and a simple static implementation, choose the simple static implementation.
