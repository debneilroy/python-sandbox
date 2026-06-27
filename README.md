# Python sandbox

A browser-based Python sandbox — write code, run it for real, and see the output instantly. No installs, no signup, no server.

**Live at:** https://debneilroy.github.io/python-sandbox/

## What it is

A code editor and console split-pane UI that runs genuine CPython 3.12 entirely in your browser, powered by [Pyodide](https://pyodide.org) (CPython compiled to WebAssembly). Nothing is sent to a server — execution happens locally on your machine.

## Features

- **Real Python execution** — `print()`, exceptions, loops, recursion, the full standard library
- **Autosave** — your code is saved to the browser as you type
- **Named snippets** — save and reload multiple pieces of code via "Save as…"
- **Keyboard shortcut** — `Cmd/Ctrl + Enter` to run
- **Fully offline-capable** — Pyodide's runtime files are bundled directly in this repo (see `pyodide/`), so the page makes no external network calls once loaded

## Why the `pyodide/` folder is here

This project vendors the Pyodide WebAssembly runtime (`pyodide.js`, `pyodide.asm.js`, `pyodide.asm.wasm`, `python_stdlib.zip`, `pyodide-lock.json`) instead of loading them from a CDN. That removes any dependency on a third-party CDN staying online — the sandbox keeps working as long as this repo and GitHub Pages exist.

Runtime version: **Pyodide 0.26.2** (Python 3.12)

## Limitations

- Standard library only — no `numpy`, `pandas`, or other third-party packages preinstalled
- Saved snippets live in the browser's `localStorage`, so they don't sync across devices or browsers
- No file I/O or network access from within the sandboxed Python environment

## Local development

This is a single static `index.html` file plus the `pyodide/` runtime folder — no build step. To run it locally, serve the folder with any static file server (it won't work opened directly via `file://` due to browser module-loading restrictions):

```bash
python3 -m http.server 8000
```

Then open `http://localhost:8000`.
