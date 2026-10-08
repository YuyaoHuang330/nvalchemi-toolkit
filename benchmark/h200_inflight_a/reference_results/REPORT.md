# Historical H200 conventional pilot results

These are A-only views of the completed production short tests on2026-10-08.
They are not measurements produced by the standalone reproduction driver.
Conventional package source matches Takuya PR2 `e479d16`; the experiment driver
supplies the production stopping, admission, neighbor and model settings.

| Capacity | Starts | Runtime status | Wall (s) | Converged | Budget exhausted |
| ---: | ---: | --- | ---: | ---: | ---: |
| 64 | 1280 | PASS | 503.74 | 985 | 295 |
| 128 | 1280 | PASS | 463.67 | 985 | 295 |
| 256 | 1280 | FAIL | Incomplete | Not assigned | Not assigned |

Capacity128 used7.95% less wall time than64 in this one-round measurement.
No complete throughput is assigned to256. The probe at startup passed but did
not certify the long trajectory. No hardware capacity ceiling was established.

The160-start conventional four-stage pilot completed in93.60s including
transitions. Per-stage convergence was160/160,158/160,160/160,136/160; final kept
endpoint convergence was136/160. Stage4 was accepted for76 starts. This is a
transition/cost check, not target-recovery accuracy.

## Localized collapse

The A256 failure was reproduced in a separate diagnostic run and localized to
g22/trial9, candidate229,62 atoms, at its446th update. Before the failed cell
update, positions and cell were finite but energy, force and stress were already
non-finite; minimum periodic pair distance was0 Angstrom. The next update also
made cell/positions non-finite, and FixSymmetryHook raised its guard error.
The original closest pair was0.475648 Angstrom.

The same candidate reached500 updates without convergence at capacities64/128.
Its finite endpoint distances were0.000455/0.000144 Angstrom, with production
fmax0.414/1.246. Thus a finite queue PASS does not establish physical quality of
every endpoint. Exact batch-dependent causes have not been established; neither
the stress threshold nor GPU memory exhaustion was proved to cause the collapse.

`a256_failure_state.pt.gz` preserves the before/after tensor snapshot.
`a256_case229_analysis.json` contains only conventional endpoint comparisons.
The JSON reference exports remove cross-method descriptive metadata and retain
the measured conventional run metrics, with original-file hashes recorded.
No cross-method optimizer implementation or original combined driver is supplied.
