---
name: miniapp-dev
description: Build or extend a WeChat Mini Program from scratch. Use when the user asks to create a new mini program, add pages/components, write WXML/WXSS/JS logic, integrate WeChat APIs, or set up project config. Covers scaffold layout, page lifecycle, component patterns, storage, networking, navigation, and compliance requirements. When an API signature or behavior is uncertain, fetch the latest official WeChat docs via WebFetch.
tools:
  - Bash
  - Read
  - Edit
  - Glob
  - Grep
  - Write
slash_command: /miniapp-dev
---

# Miniapp Development Guide

## Overview

Use this skill when the user wants to **write or scaffold WeChat Mini Program code** from scratch. Reference the split docs in `references/` instead of keeping everything in this file.

## Quick Start

1. Ask user: **app name, target pages, and any WeChat API needs** (login, payment, location, etc.).
2. Read `references/scaffold.md` for project structure rules.
3. Generate the smallest runnable scaffold:
   - `app.js`, `app.json`, `app.wxss`, `project.config.json`
   - One `pages/index/` folder with `.js`, `.wxml`, `.wxss`, `.json`
4. If user asks for specific features, read the matching reference:
   - Components → `references/components.md`
   - APIs (network, storage, media, device, UI, open) → `references/apis.md`
   - WXML/WXSS patterns → `references/wxml-wxss.md`
   - Page lifecycle / custom components → `references/page-component.md`
   - TypeScript → `references/typescript.md`
   - Compliance → `references/compliance.md`
5. For uncertain API signatures, check `references/official-docs-index.md`.

## Core Rules

- **Four-file pages**: every page must have `.js`, `.wxml`, `.wxss`, `.json` with matching base name.
- **App entry**: `app.js` calls `App({ onLaunch() {}, globalData: {} })`.
- **Page entry**: `.js` calls `Page({ data: {}, onLoad() {}, ... })`.
- **Component entry**: `.js` calls `Component({ properties: {}, data: {}, methods: {} })`.
- **JSON configs**: page `.json` uses `"navigationBarTitleText"`; component `.json` uses `"component": true`.
- **Networking**: use `wx.request` with HTTPS only; register domains in MP Admin.
- **Storage**: `wx.getStorageSync` for small config; `wx.getStorage` for larger data.
- **Navigation**: `wx.navigateTo` for push, `wx.switchTab` for tab pages.
- **Images**: prefer CDN or `cloud://`; local `images/` only for static assets.
- **Do not invent APIs**: if an API signature or behavior is uncertain, fetch the latest official docs via WebFetch (see `references/official-docs-index.md` for URLs) or ask the user to paste the relevant section.

## Reference Map

| When user asks about... | Read this reference |
|------------------------|---------------------|
| Project structure, `app.json`, `app.js`, `project.config.json` | `references/scaffold.md` |
| Page lifecycle, `Page()` / `Component()`, custom components | `references/page-component.md` |
| WXML syntax, data binding, list rendering, events | `references/wxml-wxss.md` |
| WXSS rules, rpx, `@import`, selectors | `references/wxml-wxss.md` |
| Built-in components (`view`, `scroll-view`, `swiper`, `button`, `input`, etc.) | `references/components.md` |
| APIs: network, storage, login, user info, payment, location, media, file, device, UI | `references/apis.md` |
| Full API catalog (1,600+ APIs by category) | `references/api-catalog.md` |
| TypeScript setup, `tsconfig.json`, typings | `references/typescript.md` |
| Compliance, HTTPS, ICP, privacy policy | `references/compliance.md` |
| Cloud Development (database, functions, storage) | `references/cloud-development.md` |
| Official doc links for specific APIs | `references/official-docs-index.md` |

## Output Format

When generating code:

1. Show the file tree first.
2. Show core files (`app.json`, `app.js`, first page).
3. If APIs are needed, show the call sequence and required config entries.
4. Tell user what to configure in WeChat DevTools and MP Admin.

## Resources

- `references/scaffold.md` — project structure and global config
- `references/page-component.md` — page lifecycle and component patterns
- `references/wxml-wxss.md` — view layer syntax and styling rules
- `references/components.md` — built-in component quick reference
- `references/apis.md` — common APIs with parameters and code examples
- `references/api-catalog.md` — full catalog of 1,600+ APIs (search with Grep)
- `references/typescript.md` — TypeScript setup guide
- `references/compliance.md` — compliance checklist (HTTPS, ICP, privacy policy, etc.)
- `references/cloud-development.md` — cloud database, functions, storage
- `references/official-docs-index.md` — categorized deep links to WeChat official docs
