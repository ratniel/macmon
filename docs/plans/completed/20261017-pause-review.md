Done: 2026-10-17, PR #3 (ratniel/macmon)

# Pause and toggle saved filters (roadmap item 10)

This plan covers upstream [PR #108](https://github.com/vladkens/macmon/pull/108).
Fork [PR #3](https://github.com/ratniel/macmon/pull/3) contains the review fixes.
The author tests and merges the fork PR before the upstream update.
The code uses the existing process structs and sort functions.
JSON output, Prometheus output, settings, and the library API stay compatible.

The earlier fork plans remain in
[20261016-pause-order.md](20261016-pause-order.md) and
[20261016-filter-switch.md](20261016-filter-switch.md).
This combined plan replaces their earlier pause and sort decisions.

## Behavior

- Press `Space` to pause the process list. The app keeps the same processes and row order.
  Process values and system metrics continue to update.
- If you pause before the first sample, the app sorts that sample when it arrives.
- Click a column header to sort by that column. Press `s` to select the next visible sort column.
  Press `S` to reverse the sort direction. Each action sorts once and keeps the list paused.
  Selection stays on the same process.
- Keep exited rows in place. Use dim text and `-` for unavailable values.
  Show a selected exited row's PID in dim red. Keep the rest of its selection highlight readable.
  Allow copying. Block actions that require a live process.
- Show `paused` in bold yellow on the bottom border. Keep it visible in narrow windows.
- Stop file and socket updates while paused. `Space` controls the same pause in both views.
  Hiding the process list ends the pause.
- Press `f` outside filter input to turn the current filter off or on.
  Keep its text. Show the text dim when the filter is off.
  The process list and file list have separate filters.
- Press `/` to turn the filter on and edit its text.
  While you edit a filter, `Space` and `f` enter text.
  With no filter text, `f` has no effect.
- Keep the existing process identity checks. Use the PID and available start time.
  The app cannot detect PID reuse when the start time is unavailable.

## Tasks

- [x] Reproduce the startup and sort failures with automated tests.
- [x] Fix paused process updates, startup sorting, and explicit sorting.
- [x] Update exited row styles and the pause label. Test display and safe actions.
- [x] Update help, README, roadmap, and this combined plan.
- [x] Run make check and make test. Measure production lines. Review the changes.
- [x] Open the fork PR for local tests and merge.

## Result

Fork [PR #3](https://github.com/ratniel/macmon/pull/3) contains the review fixes.
The author confirmed these manual tests:

- Sort with `s`, `S`, and column clicks while paused.
- Sort processes owned by different users.
- Keep selection on the same process after a sort.
- Show new processes only after resume.
- Keep exited rows dim while paused.
- Combine pause, comma-separated filters, `f`, and sorting.
- Resize the window. Hide and show the process list to end pause.

The startup pause case passed an automated test. It does not need a timed manual key press.
The selected exited PID color is a later fix. It still needs a manual display test.
Manual tests of file-list pause and filter input remain unconfirmed.
Computer use could build and start macmon in VS Code. It could not verify the controls.

`make check` and all 202 tests passed.
Code review found no blocking issue. The extra Codex CLI review could not start because of a model error.
The fork merge and upstream update remain pending.
