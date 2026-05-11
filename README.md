# ChatGPT Smart Code Collapse

![ChatGPT](https://img.shields.io/badge/ChatGPT-Supported-10a37f)

A lightweight Tampermonkey script that automatically collapses long code blocks (>20 lines) in ChatGPT.

## Features

- Auto-collapses blocks over 20 lines
- Shows preview of first 10 lines with fade effect
- Clean toggle button with chevron icon
- Works with streaming responses and dynamically loaded content

## Installation

1. Install [Tampermonkey](https://www.tampermonkey.net/) browser extension
2. Click install or create a new script and paste the code
3. Visit [chatgpt.com](https://chatgpt.com/)

**Direct install:**  
https://github.com/rs4t/chatgpt-smart-code-collapse/raw/main/smart-collapse.js

## Configuration

Edit these values at the top of the script:

| Variable | Default | What it does |
|----------|---------|---------------|
| `COLLAPSE_AFTER_LINES` | 20 | Collapse code blocks longer than this |
| `PREVIEW_LINES` | 10 | Lines to show when collapsed |

## Publishing on Greasy Fork

When uploading to [Greasy Fork](https://greasyfork.org/), fill in:

| Field | Value |
|-------|-------|
| Name | ChatGPT Smart Code Collapse |
| Description | Collapses long code blocks (>20 lines) in ChatGPT with a smooth fade preview |
| Category | Utilities |
| Compatible with | chrome, firefox, edge, opera, safari |

**Greasy Fork install URL:**  
`https://greasyfork.org/scripts/[your-script-id]/chatgpt-smart-code-collapse.user.js`

---

Made by [rs4t](https://github.com/rs4t)
