# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This is a single-file personal website for Samuel (HR Analyst), delivered entirely as `website.html`. There is no build system, no `package.json`, and no separate source files — everything is self-contained in one ~7MB HTML file.

## Architecture

The file uses a custom inline bundler architecture:

1. **Loader script** (lines ~30–175): Runs on `DOMContentLoaded`, reads three custom script tags, decompresses assets using the browser's `DecompressionStream` API (gzip), resolves blob URLs, then replaces the document with the unpacked app.

2. **`<script type="__bundler/manifest">`**: JSON map of UUID → `{ mime, compressed, data }` — base64-encoded (gzip-compressed) assets such as fonts, PDFs, images.

3. **`<script type="__bundler/ext_resources">`**: External CDN resources (React, ReactDOM, Babel standalone) that are injected as `<script src>` tags before the app boots.

4. **`<script type="__bundler/template">`**: The full HTML document of the actual app (base64/gzip compressed), including inline `text/babel` JSX scripts that Babel standalone transforms at runtime.

The unpacked app is a React SPA written in JSX, transformed client-side by Babel standalone — there is no pre-compilation step.

## Working with this file

- **To view the site**: Open `website.html` directly in a browser (no server needed; the bundler runs client-side).
- **To edit app content or UI**: The actual React/JSX source lives compressed inside the `__bundler/template` script tag — it cannot be edited in-place as plain text. Any meaningful edits require re-bundling via the tool that originally produced this file.
- **The 7MB size** is expected — it includes embedded fonts, a PDF, and all app assets encoded inline.
