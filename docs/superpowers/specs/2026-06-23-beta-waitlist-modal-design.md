# Beta Waitlist Modal — Design Spec

**Date:** 2026-06-23
**Status:** Approved

## Overview

Intercept every CTA click on the octoid.ai landing page and present a "Join the Private Beta" modal that collects the visitor's email via Formspree. The page remains intact as a hype/preview surface; no routes or pages are added.

## Trigger

A single delegated `click` event listener on `document` intercepts any element that:
- Is an `<a>` or `<button>` with the `.btn` class, **or**
- Is a descendant of such an element (e.g. text node inside the button)

The listener calls `event.preventDefault()` and `event.stopPropagation()`, then opens the modal. No `href` values are changed.

**Excluded from interception:** the X close button and the backdrop overlay itself.

## Modal Structure

```
┌─────────────────────────────────────┐
│                                [X]  │
│                                     │
│  Join the Private Beta              │  ← h2, gradient-text on "Private Beta"
│                                     │
│  We're onboarding our first public  │  ← .lede muted text
│  beta users. Drop your email and    │
│  we'll reach out when your spot     │
│  is ready.                          │
│                                     │
│  [  your@email.com            ]     │  ← input, full width
│  [ Secure my spot →           ]     │  ← btn-primary, full width
│                                     │
│  (error message, if any)            │
└─────────────────────────────────────┘
```

**Success state** (replaces form content):
```
│  ✓  You're on the list!             │
│     We'll be in touch soon.         │
```

## Styling

- All CSS written inline in `index.html` within the existing `<style>` block (no new files)
- Uses existing tokens: `--void`, `--graphite`, `--card-border`, `--gradient-brand`, `--stone`, `--muted`, `--font-display`, `--font-body`
- Backdrop: `rgba(0,0,0,0.72)` with `backdrop-filter: blur(6px)`
- Modal panel: `var(--graphite)` background, `border: 1px solid var(--card-border)`, `border-radius: 16px`, max-width `440px`
- Email input styled to match the existing dark card aesthetic
- Submit button uses `.btn-primary` gradient

## Formspree Integration

- Endpoint: `https://formspree.io/f/mqevavnd`
- Method: `fetch` POST with `Content-Type: application/json`, body `{ email }`
- On HTTP 200: show success state
- On any error: show inline error message "Something went wrong — try again or email us at hello@octoid.ai"

## Accessibility

- Modal traps focus while open (`Tab` cycles within modal)
- `Escape` key closes the modal
- `aria-modal="true"`, `role="dialog"`, `aria-labelledby` pointing to the headline
- Backdrop click closes the modal

## Scope

- All changes are contained to `index.html` (CSS additions in `<style>`, HTML for modal before `</body>`, JS additions in the existing `<script>` block)
- No new files, no dependencies, no build step
