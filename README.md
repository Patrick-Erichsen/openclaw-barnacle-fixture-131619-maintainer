# OpenClaw Barnacle live fixture for PR #131619

This disposable public repository exercises the Barnacle auto-response script from
OpenClaw PR [#131619](https://github.com/openclaw/openclaw/pull/131619) against real
GitHub pull requests and the real GitHub REST API.

- Source head: `6f2ba484538838ae8ea792c2d5d290a40d7c90c6`
- Source blob: `df931a742b3c6caa44d64d086538ebcdb97036d4`
- Source SHA-256: `6de49b8cb1adc61eb1e0ecdf6e2b7a4d21cf8fac9f16de85343f668430843150`
- Production script and its static import closure: copied byte-for-byte; no test
  seam or fixture branch was added.
- Import closure SHA-256:
  - `scripts/github/real-behavior-proof-policy.mjs`: `37063a0366114a160797a7df0606fc7e425ddd3000fe90bbf687b277c9424803`
  - `scripts/lib/bounded-response.mjs`: `f7250e2e6d1d0d9416a846bb304e2346c46ee5f5e182a770be5144e826e8b41d`
  - `scripts/lib/regexp.mjs`: `966a949fc75bb560219ffac105068fb74bdc9b039e586d6093315987f571f17f`
- Credentials: only the disposable repository's scoped `GITHUB_TOKEN`; no user,
  upstream, or GitHub App secrets.

## Cases

1. A mixed standalone-skill and core-runtime change receives an automatic
   `r: skill` label. Barnacle must remove the label and leave the pull request open.
2. A newly added standalone `skills/<name>/SKILL.md` receives the same label.
   Barnacle must retain the label, post its normal response, and close the pull
   request.

GitHub intentionally prevents a label written with `GITHUB_TOKEN` from starting a
second workflow run. The fixture therefore records the real bot-authored label
event from the pull-request timeline and replays that event context inside the
same real `pull_request_target` run before invoking the exact production script.
All label, comment, and pull-request state mutations are real GitHub API calls.

The run summary and a marker-backed comment on each fixture pull request contain
the source hashes, recorded event identity, before/after state, labels, comments,
and assertion result.
