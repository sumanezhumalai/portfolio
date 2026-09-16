---
title: "Building My Portfolio — From Plain HTML to a Deployable Stack"
description: "How I designed and deployed this portfolio from scratch — a minimal design system inspired by great designer portfolios, and a clear path toward Astro, Docker, and a self-hosted VPS."
pubDate: 2026-09-12
tags:
  - development
  - devops
  - portfolio
draft: false
---

Most portfolio tutorials tell you to pick a framework, install twelve dependencies, and deploy to Vercel in thirty seconds. I went the other way. This is the story of building a portfolio that I actually understand end to end — HTML, plain CSS, a handful of lines of JavaScript, and a clear migration path to Astro when the time is right.

## Why start with plain HTML?

The goal of a portfolio is to demonstrate that you can build and ship things. Starting with plain HTML forces you to think about structure before style, and style before interaction. There are no abstractions hiding the decisions from you.

The instruction document I wrote for this project states it clearly: *"Do not add any dependency unless the owner explicitly changes this instruction."* That constraint was deliberate. It keeps the site fast, lightweight, and completely owned by me.

## Design reference

I took heavy inspiration from [Arturo Spatino's portfolio](https://www.arturospatino.com/). What strikes you immediately is the restraint. Near-black background, off-white text, generous whitespace, and a single warm accent colour. No gradients fighting for attention, no cards stacked in a grid, no decorative JavaScript. Just content, well presented.

I extracted a few concrete principles from studying that site:

- The navigation is sparse — name on the left, three or four links on the right.
- Content lives in a narrow column (~720px) centered on the page.
- Typography carries the visual weight. Headings are large, letter-spaced tight.
- Colour is used exactly once per context, never decoratively.

## The design token system

I centralised all design decisions into CSS custom properties at the top of `styles.css`. Colours, spacing, font families, layout widths — everything is a variable:

```css
:root {
  --bg:          #0d0d0d;
  --text:        #e8e8e8;
  --muted:       #777777;
  --accent:      #c9a96e;

  --content-width: 720px;
  --wide-width:    1100px;

  --space-lg: 4rem;
  --space-xl: 8rem;
}
```

Changing the accent colour means changing one line. This is the kind of maintainability that Tailwind utilities can actually make harder, not easier.

## Structural decisions

The site is three HTML pages right now: `index.html`, `resume.html`, and `blog.html`. Each page shares the same navbar and footer. In a plain-HTML project that means duplicating the markup — a known cost that the migration to Astro will eliminate through layout components.

The blog is the most interesting architectural challenge. Right now the post pages are static HTML files in a `blog/` subdirectory. When I migrate to Astro, those files disappear and are replaced by Markdown files in `src/content/blog/`, rendered by a single `[...slug].astro` dynamic route. Adding a new article will require nothing more than writing a Markdown file and pushing to Git.

## What comes next

The immediate next step is the visual polish pass — styling the hero section, skills grid, project cards, and timeline on the landing page. Then the Astro migration, a Dockerfile, Nginx configuration, GitHub Actions CI/CD, and deployment to a VPS behind Traefik.

Building in phases like this — structure first, style second, migration third — keeps the project moving without getting overwhelmed. Each phase produces something shippable. That matters.
