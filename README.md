# BlockWerk plugin for Claude

Build, run and analyse block-diagram simulations from Claude — PID loops,
transfer functions, state-space models, step responses, Bode and FFT — on a
live [BlockWerk](https://blockwerk.tech) canvas.

## What it contains

| Part                          | Purpose                                                       |
| ----------------------------- | ------------------------------------------------------------- |
| `.mcp.json`                   | The remote BlockWerk MCP server (streamable HTTP, no sign-in) |
| `skills/blockwerk-simulation` | How to build, run, read and tune a diagram with those tools   |
| `commands/simulate.md`        | `/blockwerk:simulate <system>` — model a system in one go     |

Full setup guide: <https://blockwerk.tech/connect-ai>

## Install

In Claude Code:

```bash
claude plugin install blockwerk@claude-plugins-official
```

Or add the server alone as a custom connector in Claude (web, desktop, Cowork):
**Settings → Connectors → Add custom connector**, URL
`https://blockwerk-mcp-worker.blockwerk.workers.dev/`.

## Use your own canvas

Without pairing, the server keeps a private scratch canvas: you can build and
inspect diagrams and get a share link, but simulation needs the browser. To let
Claude run simulations in your own tab, open https://blockwerk.tech, click
**Connect AI**, and use the connector URL shown there instead.

## Example prompts

- "Build a PID speed controller for a DC motor and tune it for less than 5 % overshoot."
- "Model a mass-spring-damper, simulate a step input and give me the settling time."
- "Show the Bode plot of 1/(s^2 + 0.4s + 1) and tell me the resonance peak."

## Privacy Policy

BlockWerk's privacy policy is at <https://blockwerk.tech/privacy>. In short:

- **Collected:** the diagram and tool arguments you send, processed to answer the
  call. No account, name or email is required.
- **Storage and retention:** an unpaired scratch canvas is stored by the server
  and deleted after 24 hours without a call. A paired session is relayed
  end-to-end encrypted to your own browser tab; the server keeps only the
  short-lived pairing record, not your diagram.
- **Sharing:** no data is sold or shared with third parties. Hosting runs on
  Cloudflare Workers.
- **Contact:** blockwerk.tech@outlook.com

## Licence

MIT — see [LICENSE](LICENSE).
