# Draft: H200 conventional inflight benchmark and collapse reproduction

The conventional PR2 optimizer and inflight runtime completed1280-start queues
at capacities64/128, but a256-slot campaign aborted after a62-atom candidate
collapsed to coincident atoms and the model returned non-finite outputs.
This branch supplies a standalone conventional-only reproduction setup and
historical results for discussion of production batching and failure handling.

Base: Takuya's `feat/inflight-symmetry-relaxation`, pinned to `e479d16`.
The optimizer, scheduler, symmetry hook and numerical kernels are unchanged.
Additions are Cartesian inputs, the measured admission/neighbor/model settings,
ASE FrechetCellFilter stopping, CUDA workload telemetry, PBS template and reports.
The public fork's current CI workflow is retained for publishing compatibility;
this infrastructure alignment is separate from the numerical implementation.

Stop: max ASE filter atomic/cell generalized-force row norm <0.02,500 updates
per candidate per stage, one observation, no separate stress2e-5 threshold.
The archived A64/A128 times are503.74s/463.67s, with985/1280 converged each.
A256 timing is incomplete and must not be treated as full-queue throughput.

The standalone extraction changes initial-coordinate export and warmup:
CPU float32 Cartesian starts replace runtime chart expansion, warmup is
conventional-only, and four-stage transitions omit the chart audit.
Historical measurements are retained separately. Exact trajectory/failure
reproduction and new GPU timing are not yet verified.

Review focus: admission correctness, common stopping configuration, workload
accounting, and how to isolate non-finite candidates without losing unrelated
work. Validation results are recorded in VALIDATION.json. This is a Draft PR
for reproduction/review, not a completed production-safety fix.
