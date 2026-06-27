# Python sandbox

A browser-based Python sandbox — write code, run it for real, and explore it interactively in a terminal-style console. No installs, no signup, no server.

**Live at:** https://debneilroy.github.io/python-sandbox/

## What it is

A code editor and console split-pane UI that runs genuine CPython 3.12 entirely in your browser, powered by [Pyodide](https://pyodide.org) (CPython compiled to WebAssembly). Nothing is sent to a server — execution happens locally on your machine.

## Features

**Editor**
- Real Python syntax highlighting, auto-indent, bracket matching, and auto-closing brackets via [CodeMirror](https://codemirror.net/5/)
- Indentation guides — faint vertical lines tracing each nesting level, so deeply nested loops/conditionals are easy to follow
- Python-aware autocomplete — keywords, builtins, and any identifiers you've already typed
- `Tab` inserts 4 spaces; `Cmd/Ctrl + Enter` runs the buffer; `Ctrl + Space` opens suggestions manually

**Console**
- A real interactive REPL: type any Python statement at the `>>>` prompt and it runs immediately against the same interpreter session as the editor — build objects, call methods, inspect values, all live
- The prompt behaves like a real terminal: each command becomes a permanent line in the transcript, and the live `>>>` always stays at the bottom as the history grows
- Autocomplete in the console too, pulling from the editor's identifiers
- Paste multiple lines at once (e.g. several tree-construction statements) and each runs in sequence automatically
- `assert` statements get a clean ✓ / ✗ result instead of a raw traceback

**Workflow**
- Autosave — code and console state persist in the browser
- Named snippets — save and reload multiple pieces of code via "Save as…"
- Built-in timer — useful for timing yourself on a problem
- Fully offline-capable — Pyodide's runtime files are bundled directly in this repo (see `pyodide/`), so the page makes no external network calls once loaded

## Why the `pyodide/` folder is here

This project vendors the Pyodide WebAssembly runtime (`pyodide.js`, `pyodide.asm.js`, `pyodide.asm.wasm`, `python_stdlib.zip`, `pyodide-lock.json`) instead of loading them from a CDN. That removes any dependency on a third-party CDN staying online — the sandbox keeps working as long as this repo and GitHub Pages exist.

Runtime version: **Pyodide 0.26.2** (Python 3.12)

## Limitations

- Standard library only — no `numpy`, `pandas`, or other third-party packages preinstalled
- Saved snippets and console history live in the browser's `localStorage`, so they don't sync across devices or browsers
- No file I/O or network access from within the sandboxed Python environment
- `input()` calls inside scripts don't pause and wait for typed input (a separate, browser-limited Pyodide feature) — use the console to interact with objects instead

## Local development

This is a single static `index.html` file plus the `pyodide/` runtime folder — no build step. To run it locally, serve the folder with any static file server (it won't work opened directly via `file://` due to browser module-loading restrictions):

```bash
python3 -m http.server 8000
```

Then open `http://localhost:8000`.
