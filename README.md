<div align="center">
  <img src="https://raw.githubusercontent.com/usewrit/writ/main/assets/banner.svg" alt="writ — record a web task once, replay it forever, on hardware you own" width="100%">
</div>

## Benjamin Garcia

I build **[Writ](https://github.com/usewrit/writ)** — self-hosted browser automation that runs on hardware you own.

Record a repeatable web task once, in a real browser. Replay it forever as a workflow, a REST endpoint, or a tool your AI assistant can call. Recording uses a model once; **replay never does**, so a task that runs hourly costs nothing per run and doesn't get slower, more expensive, or less predictable as you scale it.

Everything stays on your machine: one SQLite file, no cloud dependency, no telemetry, credentials encrypted at rest.

### The project

| | |
|---|---|
| **[writ](https://github.com/usewrit/writ)** | The self-hosted coordinator — API, web UI, scheduler, document/OCR extraction. `docker compose up`. |
| **[writ-agent](https://github.com/usewrit/writ-agent)** | The Rust fleet agent that drives the browsers. Dials out over WebSocket; no inbound ports. |
| **[writ-sdks](https://github.com/usewrit/writ-sdks)** | Client SDKs for TypeScript, Python, Go and Rust — four languages, one contract. |
| **[writ-mcp](https://github.com/usewrit/writ-mcp)** | The MCP connector. Your saved workflows become tools an AI assistant can run. Zero dependencies. |

### How it's built

The coordinator is Python and FastAPI on SQLite, and it **never launches a browser itself** — every browsing job goes to a separate Rust agent you run wherever the work should happen. That split is the whole design: the thing holding your data has no browser, and the thing driving browsers holds no data.

Some of what that has meant in practice:

- The agent refuses loopback, RFC1918 and cloud-metadata addresses outright, and normalises IPv4-mapped IPv6 before checking — SSRF protection that can't be argued around with an env var.
- Releases are gated end-to-end across repositories: bring the stack up, create the owner, install the *published* agent release through the same one-liner the UI prints, verify its checksum, record a workflow on a real browser, replay it, and call it over both REST and MCP. A tag isn't cut until that passes.
- AGPL-3.0, with a live `GET /api/about` source offer, because §13 asks for one and network services are exactly where it matters.

### Elsewhere

- **[usewrit.app](https://usewrit.app)** — the project site
- **[Documentation](https://github.com/usewrit/writ/wiki)** — setup, deployment, operations
- **[Security policy](https://github.com/usewrit/writ/blob/main/SECURITY.md)** — how to report an issue
