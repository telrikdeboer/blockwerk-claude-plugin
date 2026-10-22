---
name: blockwerk-simulation
description: Use when the user wants to model, simulate or tune a dynamic system — control loops, PID tuning, transfer functions, state-space models, step responses, Bode or FFT analysis — or asks for a block diagram. Drives the BlockWerk canvas through the bundled blockwerk MCP server.
---

# Simulating with BlockWerk

BlockWerk is a browser-based block-diagram simulator. The `blockwerk` MCP server
bundled with this plugin edits a canvas and, when the user's own blockwerk.tech
tab is paired, runs the simulation there.

## Two modes

- **Unpaired (default).** The server keeps a private scratch canvas. These tools
  work there: `get_engine_status`, `get_available_blocks`, `describe_canvas`,
  `load_diagram`, `add_block`, `update_params`, `connect_blocks`, `delete_block`,
  `batch_commands`, `analyze_topology`, `export_diagram`, `get_share_link` and
  `clear_canvas`. Every other tool needs the simulation engine, which runs in the
  user's browser tab: simulation, signal and analysis tools, optimisation, jobs,
  `tidy_layout`, `generate_code`, undo/redo and architect memory. Unpaired, those
  return an error that says no tab is paired. That is expected: call
  `get_share_link`, give the link to the user and ask them to open it and press
  Run.
- **Paired.** The user opens https://blockwerk.tech, clicks **Connect AI** and
  copies the connector URL. Every tool then drives their live canvas, including
  simulation and results.

## Inline canvas (MCP App)

In clients that support MCP Apps, `load_diagram`, `load_template`, `run_simulation`
and `get_share_link` also show the diagram inline in the chat as a running canvas,
with pause and restart. It plays in the chat itself, so it works without a paired
tab too. It is a view of the diagram, not a source of numbers: read results from the
tools, not from what the canvas appears to show.

Tell the user which mode they are in when a tool reports it; do not pretend a
simulation ran when it returned a link.

## Building a diagram

1. Call `get_available_blocks` first. Never guess block types or port ids.
2. For a whole diagram at once, use `load_diagram` — the fastest and most
   reliable route. Every connection needs its own unique `id`.
3. For small edits: `add_block` → `connect_blocks` → `update_params`.
4. Paired only: `tidy_layout`. Then `get_share_link` so the user can open it.

`clear_canvas`, `delete_block` and `load_diagram` ask the user for an approval
click in their tab when paired. Prefer one `load_diagram` over many deletes.

## Reading results

After `run_simulation`, read real values with `get_signal_data`,
`calculate_metrics` or `evaluate_spec_mask`. If something looks wrong, call
`explain_simulation` before changing parameters. Report numbers from these
tools, not from expectation.

## Tuning

- One parameter: `optimize_parameters`. Several (e.g. Kp, Ki, Kd):
  `optimize_multi_parameter`. No plant knowledge: `auto_tune_pid`.
- Plant identification from a step response: `identify_fopdt`.
- Long runs: `start_optimization`, then poll `get_job_status`.

## Code export

Paired only: `generate_code` emits C++/Arduino, Python or Rust from the diagram. Use InPort
and OutPort blocks to mark the hardware I/O boundary.
