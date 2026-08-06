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
| **[writ-mcp](https://github.com/usewrit/writ-mcp)** | The MCP connector. Your saved workflows become tools an AI assistant can call. Zero dependencies. |

### How it's built

The coordinator is Python and FastAPI on SQLite, and it **never launches a browser itself** — every browsing job goes to a separate Rust agent you run wherever the work should happen. That split is the whole design: the thing holding your data has no browser, and the thing driving browsers holds no data.

Some of what that has meant in practice:

- The agent refuses loopback, RFC1918 and cloud-metadata addresses outright, and normalises IPv4-mapped IPv6 before checking — SSRF protection that can't be argued around with an env var.
- Releases are gated end-to-end across repositories: bring the stack up, create the owner, install the *published* agent release through the same one-liner the UI prints, verify its checksum, record a workflow on a real browser, replay it, and call it over both REST and MCP. A tag isn't cut until that passes.
- AGPL-3.0, with a live `GET /api/about` source offer, because §13 asks for one and network services are exactly where it matters.

### How I got here

I have been writing software since I was eleven, when modding games turned into wanting to understand what I was actually changing. Desktop applications and websites came next. At seventeen I registered as a sole trader doing on-site computer repair, and built brochure sites for local businesses alongside it. All of it self-taught, none of it in a classroom.

Today I manage a bakery. Writ is what happens outside a fifty-hour week.

AI tooling is a real part of how one person ships something this size, and I would rather say so than have you wonder. It compounds what I already knew — full-stack, DevOps, and the operational side of running things that other people depend on — instead of standing in for it. The architecture, the reviews and the verification are mine. That is precisely why the release gate above exists: I don't ship what I haven't watched work end to end.

### Elsewhere

- **[usewrit.app](https://usewrit.app)** — the project site
- **[Documentation](https://github.com/usewrit/writ/wiki)** — setup, deployment, operations
- **[Security policy](https://github.com/usewrit/writ/blob/main/SECURITY.md)** — how to report an issue
