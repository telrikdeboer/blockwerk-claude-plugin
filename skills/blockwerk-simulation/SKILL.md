---
name: blockwerk-simulation
description: Use when the user wants to model, simulate or tune a dynamic system — control loops, PID tuning, transfer functions, state-space models, step responses, Bode or FFT analysis — or asks for a block diagram. Drives the BlockWerk canvas through the bundled blockwerk MCP server.
---

# Simulating with BlockWerk

BlockWerk is a browser-based block-diagram simulator. The `blockwerk` MCP server
bundled with this plugin edits a canvas and, when the user's own blockwerk.tech
tab is paired, runs the simulation there.

## Two modes

- **Unpaired (default).** The server keeps a private scratch canvas. You can list
  blocks, build and edit a diagram, inspect it and export it. Anything that needs
  the solver — `run_simulation`, reading signals, optimisation, jobs — returns a
  share link instead. Give that link to the user and ask them to open it and
  press Run.
- **Paired.** The user opens https://blockwerk.tech, clicks **Connect AI** and
  copies the connector URL. Every tool then drives their live canvas, including
  simulation and results.

Tell the user which mode they are in when a tool reports it; do not pretend a
simulation ran when it returned a link.

## Building a diagram

1. Call `get_available_blocks` first. Never guess block types or port ids.
2. For a whole diagram at once, use `load_diagram` — the fastest and most
   reliable route. Every connection needs its own unique `id`.
3. For small edits: `add_block` → `connect_blocks` → `update_params`.
4. Finish with `tidy_layout`, then `get_share_link` so the user can open it.

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

`generate_code` emits C++/Arduino, Python or Rust from the diagram. Use InPort
and OutPort blocks to mark the hardware I/O boundary.
