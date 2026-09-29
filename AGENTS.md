# AGENTS.md — plugin-ssh

Standalone plugin repo for the externalized `charly ssh tunnel` command
(`command:ssh`). The plugin is a Go module at `candy/plugin-ssh/` (module path
`github.com/opencharly/plugin-ssh/candy/plugin-ssh`); the root `charly.yml` only
declares `discover: candy` so the repo is a project and its candy is scanned.

Canonical files:

- `candy/plugin-ssh/charly.yml` — the `plugin-ssh:` candy entity (`plugin:`
  block, `plan:` check).
- `candy/plugin-ssh/command.go` / `provider.go` — the `charly ssh` command
  grammar and the provider.
- `candy/plugin-ssh/tunnel.go` — the SSH-forwarded SPICE/VNC endpoint.
- `candy/plugin-ssh/schema/ssh.cue` — the self-contained schema.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — user overview only; never agent guidance.

## Load these skills first (R0)

- `/charly-internals:plugin` — the plugin authoring reference: the `plugin:`
  block, the unified Provider model, command-class dispatch, the per-plugin
  CUE-schema contract, placement. Load before touching the provider or schema.
- `/charly-core:ssh` — SSH tunnel access to remote SPICE and VNC endpoints.
- `/charly-internals:git-workflow` — before any git/PR action.

## Build / validate / test

- `go build ./...` in `candy/plugin-ssh/` — compile the plugin module.
- `go test ./...` in `candy/plugin-ssh/` — the plugin's Go tests
  (`schema_serve_test.go`).
- `charly box validate` at the repo root — the structural check (the candy +
  `plugin:` block, CUE schema).
- The merge gate is the **org-wide** `charly/pr-validator` (required check
  `validate / validate`, defined in `opencharly/.github`); this repo has **no**
  per-repo candy gate.
- The live tunnel needs a remote libvirt VM + interactive SIGINT, so it is
  exercised manually / by the vm bed roster, not a deterministic check.

## Modify this repo

- Edit the `plugin-ssh:` candy entity, the Go source, and `schema/ssh.cue`
  **together** — the schema is the single source for the generated types.
- The plugin is **compiled-in** because its `Invoke(OpRun)` reaches
  `verb:libvirt` over the in-proc reverse channel; it cannot run out-of-process.

## Landing

- PR-only. Every change lands through a pull request; the org-required
  `charly/pr-validator` validates the diff and body and arms native auto-merge on
  PASS. Direct pushes to `main` are blocked.
- History lives in `CHANGELOG/` (written by `tag-on-merge` at merge time); the PR
  body IS the changelog.
- The authoritative rulebook is the umbrella `AGENTS.md` in
  `opencharly/opencharly` and `charly/AGENTS.md` in the charly repo. Do not
  restate its rules here.
