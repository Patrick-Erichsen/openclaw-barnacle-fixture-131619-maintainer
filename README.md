# OpenClaw Barnacle live fixture for PR #131619

This disposable public repository exercises the Barnacle auto-response script from
OpenClaw PR [#131619](https://github.com/openclaw/openclaw/pull/131619) against real
GitHub pull requests and the real GitHub REST API.

- Source head: `3eb3ae4d879bdca46ee82bd7aae90023556fda3e`
- Source blob: `8b5c35a50f9fbb125e4b49b8b73ac0c48b14c31d`
- Source SHA-256: `a834ef6718fe511771165549122bf2fc8078074def1afa63cd3294d9c428d06b`
- Production script: copied byte-for-byte; no test seam or fixture branch was added.
- Credentials: only the disposable repository's scoped `GITHUB_TOKEN`; no user,
  upstream, or GitHub App secrets.

## Cases

1. A core runtime change receives an automatic `r: skill` label. Barnacle must
   remove the label and leave the pull request open.
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
