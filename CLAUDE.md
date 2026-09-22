# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

The marketing site for the Small Business Fleet Alliance (SBFA), live at https://smallbusinessfleetalliance.com. SBFA is a membership group that gives independent box truck, Sprinter, cargo van, gig delivery, and field-service operators group buying power.

The site is plain static HTML. It has no build step, package manager, framework, linter, or tests. Each `.html` file at the repo root is a complete page, and its CSS lives in its own inline `<style>` block. To preview the site, open a file in a browser or serve the root, for example with `python3 -m http.server`.

## Architecture and conventions

- **No shared CSS or partials.** Every page repeats the same design tokens (`:root` vars such as `--bg-dark`, `--orange`, `--text-muted`), the header/logo SVG, the footer, and the card/button patterns. A change to the nav, footer, or a token has to be made by hand in every page that has it. Before you call such a change done, grep across `*.html`.
- **Two header variants.** `index.html`, `benefits.html`, and `preferred-partners.html` have the full `<nav>` (About, Benefits, Pricing, Partners, Preferred Partners, Start a Chapter, plus the "Join Today" CTA). The current page's link carries `class="active"`. `join.html`, `chapter.html`, `partner.html`, and `thank-you.html` use a simpler header with no nav. When you add a nav link, update all three full-nav pages.
- **Reuse existing components.** Past changes deliberately introduced no new colors, fonts, or components. They copied existing patterns, such as the numbered-step component from `join.html` or the card grids. Card grids that can end on a partial row (`.perks-grid`, the "Who This Is For" grid) use flexbox with centered wrap and a card max-width, not CSS grid, so a last card doesn't sit alone on one side.
- **Forms are third-party.** Join, chapter, and partner applications are Tally embeds (`data-tally-src="https://tally.so/embed/..."` plus the Tally loader script). `join.html` and `chapter.html` share form `ODKeZa`, and `partner.html` uses `7ROrqz`. The referral form on `thank-you.html` posts to FormSubmit (`formsubmit.co`) and redirects back through a hidden `_next` field.
- **Affiliate perks** in the `index.html` perks grid (Parts Geek, NTB) use real affiliate links plus required 1×1 tracking-pixel `<img>` tags. Keep the pixels when you edit those cards.
- `fo-verify.html` is a domain-ownership verification file. Leave it alone.
- Partner logos go in `assets/partners/`.

## Content rules (business and legal)

- **Tiers and pricing:** Commercial Pro is $150/yr (founding rate, $50 off the standard $200). Gig Tier is $60/yr ("$5/month equivalent"). Prices appear on `index.html`, `benefits.html`, and `join.html` and must stay consistent across them.
- **The $10,000 Group AD&D coverage is Commercial Pro only.** Any mention of it must be scoped that way, and the standard "*Insurance Disclosure" paragraph (the same text on every page that has it) must stay intact. Earlier commits fixed copy that implied all members got coverage.
- Perk cards are tagged by tier: "both tiers" or "Commercial Pro only". Preferred Local Partners and AD&D are Commercial Pro only.
- **Don't publish member-only value on public pages.** Partner discount specifics, live discount codes such as the Mudflap/Upside fuel codes, and redemption links stay paywalled. `preferred-partners.html` and `benefits.html` use generic descriptions only. Public affiliate links (Parts Geek, NTB) are the exception, because exposing them costs members nothing.
- Referral and chapter residual figures appear in both `join.html` and `chapter.html`. Keep them in sync.

## Commits

Commit messages use an imperative summary line and a body that explains the *why*: the business or legal reasoning, or the layout bug being fixed.
