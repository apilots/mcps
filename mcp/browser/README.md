# Browser

This default MCP uses the Talos Rust adapter (`talos --browser-mcp`) and the Worker's private BrowserRuntime/IPC. The adapter ships with Talos. Its host path and private socket are resolved at runtime; this repository never ships a second adapter binary or replaces its authorization bridge with the upstream MCP server.

Desktop's embedded Worker uses the existing native browser host and gains no Python dependency. Docker's Worker uses upstream browser-use as a Python library. `requirements.in` records the pinned upstream dependency; the generated `requirements.lock` pins the complete Linux/Python 3.12 graph with hashes. Runtime and image preinstallation consume the lock. A matching valid preinstallation is reused.

Regenerate the lock from the repository root after validating a deliberate upstream update:

```sh
uv pip compile --python-version 3.12 --python-platform linux --generate-hashes mcp/browser/requirements.in -o mcp/browser/requirements.lock
```

`browser.api_version` must match the Talos adapter's supported host API. Dependency updates incompatible with the product driver require a Talos change and acceptance. A dependency-changing update after backend binding takes effect after restarting that Worker. Existing disabled state is preserved. Browser can be disabled; its existing product policy prevents uninstall.
