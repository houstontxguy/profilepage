# Profile Page - Design Document

## Overview

Personal profile/resume single-page site for Aaron Voges, styled as a dark-themed resume page with a light mode toggle. Features a CSS sunglasses overlay on the headshot that animates in when switching to light mode.

## Tech Stack

- Plain HTML + CSS + vanilla JavaScript
- No frameworks, no build tools
- Single `index.html`, `styles.css`, `script.js`, and `assets/` folder

## File Structure

```
profilepage/
  index.html        # Full page markup + inline FOUC-prevention script
  styles.css        # Theming, layout, sunglasses animation
  script.js         # Theme toggle logic
  assets/
    headshot.jpg    # Profile photo
  docs/
    design.md       # This file
```

## Layout

Two-column CSS Grid (`300px` sidebar | `1fr` content area):

### Left Sidebar (sticky)

- Circular profile photo (180px) with sunglasses SVG overlay
- Name: Aaron Voges
- Title: macOS Platform Architect
- Contact links: email, LinkedIn, location (Katy, TX)
- Dark/light toggle button (sun/moon icons)

### Right Content Sections

1. **About** - Summary paragraph
2. **Experience** - 7 roles from ExxonMobil (Contractor, current) through AT&T Wireless (1998). Detailed bullet points for ExxonMobil (Contractor) and MD Anderson; brief summaries for earlier roles.
3. **Skills** - Tag pills (Jamf Pro, Intune, Entra ID, Zero Trust, Bash, Python, PowerShell, etc.)
4. **Certifications** - Tag pills (MCITP-EA, ACTC, Jamf 400, ITIL v3, CCNA, F5 BIG-IP)
5. **Education** - WGU MS (2024), Texas A&M Commerce BAAS, Pierpont AAS

## Theming

| Property       | Dark (default)     | Light              |
|----------------|--------------------|--------------------|
| Background     | `#0d1117`          | `#ffffff`          |
| Text           | `#e6edf3`          | `#1f2328`          |
| Secondary text | `#8b949e`          | `#656d76`          |
| Accent         | `#58a6ff`          | `#0969da`          |
| Border         | `#30363d`          | `#d0d7de`          |
| Tag background | `#1f2937`          | `#ddf4ff`          |

- CSS custom properties on `[data-theme]` attribute on `<html>`
- Smooth 0.3s transitions on background, color, border
- Inline `<script>` in `<head>` reads `localStorage` to prevent FOUC

## Sunglasses Trick

- `<div class="sunglasses-overlay">` positioned absolutely over the profile photo
- Contains inline SVG of aviator-style sunglasses silhouette
- **Dark mode:** `opacity: 0; transform: translateY(-20px)` (hidden, shifted up)
- **Light mode:** `opacity: 1; transform: translateY(0)` (visible, slides down into place)
- 0.4s ease transition with 0.15s delay (background changes first, then glasses drop in)
- `pointer-events: none` so overlay doesn't block interaction

## Theme Toggle Logic (`script.js`)

1. On page load: inline `<head>` script reads `localStorage.getItem('theme')`
2. Falls back to `prefers-color-scheme` media query
3. Sets `data-theme` attribute on `<html>` before first paint
4. Toggle button click swaps the attribute and persists to `localStorage`

## Responsive Breakpoints

| Breakpoint | Behavior |
|------------|----------|
| > 768px    | Two-column grid, sticky sidebar |
| <= 768px   | Single column, sidebar becomes centered top banner |
| <= 480px   | Tighter padding, smaller photo (120px), stacked header |

## Data Source

All content populated from Aaron's LinkedIn profile PDF export.
