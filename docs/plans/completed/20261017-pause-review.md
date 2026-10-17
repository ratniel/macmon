Done: 2026-10-17, PR #3 (ratniel/macmon)

# Pause and toggle saved filters (roadmap item 10)

Combined plan for upstream [PR #108](https://github.com/vladkens/macmon/pull/108), including
review fixes in a separate PR to ratniel/macmon main. The person reviews,
tests locally and merges the fork PR before any upstream update. Reuse ProcInfo rows and
existing sort functions; no new dependency, settings format, JSON, Prometheus or library API.

Historical fork implementation plans remain in
[20261016-pause-order.md](20261016-pause-order.md) and
[20261016-filter-switch.md](20261016-filter-switch.md). This plan defines the
combined behavior for upstream; their old lifecycle and sort decisions are superseded.

## Behavior

- Space holds the existing process membership and automatic order; values and system metrics
  stay live. New processes appear after resuming normal sampling, not while paused.
- If Space precedes the first sample, that sample becomes a normally sorted paused baseline.
- Explicit sorting by header, s or S reorders held processes and filter totals once while
  keeping pause enabled and selection attached to process identity.
- Exited held processes remain in place without a name prefix; dim their rows, keep unavailable
  values as dashes and preserve selection visibility and existing safe-action restrictions.
- Show paused in bold terminal yellow on the process bottom border, including narrow layouts.
- In the file/socket overlay, Space controls the same pause; the current snapshot stays fixed.
  Space remains text while editing a filter. Hiding the process panel clears pause.
- f turns the current process or file filter off/on without discarding the saved query. The
  disabled text stays faint; / enables it for editing. Empty filters make f a no-op. Filter
  syntax, group totals and selection handling follow the existing rules.
- Simplify keyboard help and document f in the README. Keep the PID/start-time safety checks;
  other users' processes still lack start times, so PID reuse cannot be detected for them.

## Tasks

- [x] Reproduce the startup and explicit-sort issues with focused regression tests.
- [x] Update paused membership, initialization and explicit sorting using existing row structs.
- [x] Update exited styling and pause indicator with rendering and safe-action coverage.
- [x] Update help, README, roadmap and combined upstream item 10 plan.
- [x] Run make check, make test and production-line measurement; independently review.
- [x] Open a PR against the fork for the person's local testing and merge.

## Result

Fork [PR #3](https://github.com/ratniel/macmon/pull/3) implements these review fixes using
existing row storage and sorting, with five fewer production lines. `make check` and all
202 tests passed, including startup, explicit sorts, membership, PID reuse and rendering.
Independent code review found no actionable issues; the additional Codex CLI review could
not run because its configured model was rejected for this account. Live TUI testing and
merge are left to the person. Upstream PR #108 remains unchanged pending their confirmation.
