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
- The prompt behaves like a real terminal: each command becomes a permanent line in the transcript
- Autocomplete in the console too, pulling from the editor's identifiers
- Paste multiple lines at once (e.g. several tree-construction statements) and each runs in sequence automatically

**Workflow**
- Autosave — code and console state persist in the browser
- Named snippets — save and reload multiple pieces of code via "Save as…"
- Built-in timer — useful for timing yourself on a problem
- Clear output button — wipes the console transcript
- Fully offline-capable — Pyodide's runtime files are bundled directly in this repo (see `pyodide/`), so the page makes no external network calls once loaded

**AI assistant**
- Click `✦ Ask AI` to open a side panel and ask questions about your code
- Supports two providers: **Anthropic (Claude)** and **Google Gemini**
- Your API key is stored only in your browser's `localStorage` and sent directly to the provider — it never touches any other server
- The AI has context of whatever is in the editor when you ask

## Why the `pyodide/` folder is here

This project vendors the Pyodide WebAssembly runtime (`pyodide.js`, `pyodide.asm.js`, `pyodide.asm.wasm`, `python_stdlib.zip`, `pyodide-lock.json`) instead of loading them from a CDN. That removes any dependency on a third-party CDN staying online — the sandbox keeps working as long as this repo and GitHub Pages exist.

Runtime version: **Pyodide 0.26.2** (Python 3.12)

## Limitations

- Standard library only — no `numpy`, `pandas`, or other third-party packages preinstalled
- Saved snippets and console history live in the browser's `localStorage`, so they don't sync across devices or browsers
- No file I/O or network access from within the sandboxed Python environment
- `input()` calls inside scripts don't pause and wait for typed input (a browser-level Pyodide limitation) — use the console to interact with objects instead

_AI assistant:_
- **Gemini free tier** — has rate limits (requests per minute and per day). If you hit them you'll need to wait before sending another question. No cost, but throttled.
- **Anthropic (Claude)** — paid API. New accounts get a $5 free credit, after which you're billed per token. Costs are low for short Q&A but can add up with long conversations.
- The AI can only see what's in the editor at the time you ask — it has no access to your console history or runtime state.
- There is no conversation memory between sessions — each page reload starts a fresh chat.
- The AI panel doesn't execute code — it only answers questions. Run the code yourself in the editor/console.

## Local development

This is a single static `index.html` file plus the `pyodide/` runtime folder — no build step. To run it locally, serve the folder with any static file server (it won't work opened directly via `file://` due to browser module-loading restrictions):

```bash
python3 -m http.server 8000
```

Then open `http://localhost:8000`.
