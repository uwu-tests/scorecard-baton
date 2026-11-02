# scorecard-baton (demo)

This repository shows [baton](https://github.com/uwu-tools/baton)
tracking OpenSSF Scorecard's follow-up work against live data, set up
the way a project's `<project>-baton` repository would be.

It isn't affiliated with or endorsed by OpenSSF, and its grace periods
aren't Scorecard project policy. baton reads the real Scorecard
repositories but writes issues only to the `uwu-tests` organization,
whose repositories stand in for the real components:

| Component               | Stand-in                     |
| ----------------------- | ---------------------------- |
| `ossf/scorecard-baton`  | `uwu-tests/scorecard-baton`  |
| `ossf/scorecard-action` | `uwu-tests/scorecard-action` |
| `ossf/scorecard-infra`  | `uwu-tests/scorecard-infra`  |
| `ossf/scorecard-webapp` | `uwu-tests/scorecard-webapp` |
| `ossf/allstar`          | `uwu-tests/allstar`          |

Each rollout has a parent issue here with a checklist; each pending
or overdue obligation has a child issue in the stand-in that owes it.

- `baton.yml` is a copy of baton's
  [Scorecard example](https://github.com/uwu-tools/baton/blob/main/examples/scorecard/baton.yml).
- `.github/workflows/baton.yml` reconciles daily, on manual dispatch,
  and on `repository_dispatch` (`baton-reconcile`).
- `.github/workflows/validate.yml` checks `baton.yml` on pull requests.

Both workflows are adapted from baton's
[templates](https://github.com/uwu-tools/baton/tree/main/templates/workflows)
and pin `BATON_VERSION`.

Try an unreleased baton (a branch or commit):

```sh
gh workflow run baton.yml --repo uwu-tests/scorecard-baton -f version=main
```

Trigger a run the way a component's release workflow would:

```sh
gh api repos/uwu-tests/scorecard-baton/dispatches \
  -f event_type=baton-reconcile \
  -F 'client_payload[source]=ossf/scorecard' \
  -F 'client_payload[version]=v5.6.0'
```

Read-only checks from a checkout:

```sh
go run github.com/uwu-tools/baton/cmd/baton@v0.1.0 validate baton.yml
go run github.com/uwu-tools/baton/cmd/baton@v0.1.0 plan baton.yml
```
