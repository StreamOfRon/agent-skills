---
name: kubectl-readonly
description: Use when running any kubectl command — read-only operations MUST go through kubectl-readonly (auto-approved); plain kubectl is not auto-approved.
---

# Kubectl Read-Only

**Core rule: every supported (read-only) kubectl operation MUST run through `kubectl-readonly`. Plain `kubectl` is NOT auto-approved — using it for read-only work needlessly prompts the user for permission.**

`kubectl-readonly` is a wrapper that allows only read-only kubectl commands and blocks writes, `exec`, and secret-value output. The user's permission system allowlists it (e.g. `Bash(kubectl-readonly:*)`); plain `kubectl` is not on the allowlist.

## When to Use

Before running ANY kubectl command, classify it against the supported list below. This skill applies even when plain kubectl "would obviously work" — the issue is approval, not correctness.

- Running read-only kubectl commands (`get`, `describe`, `logs`, `top`, …)
- Inspecting or switching kubeconfig (`config view`, `config get-contexts`, `config use-context`, …)
- Checking RBAC or rollout state (`auth can-i`, `rollout status`)
- Any time you are about to run plain `kubectl <cmd>` where `<cmd>` is in the supported list

**Not needed for:** write/mutating commands (`apply`, `delete`, `scale`, `patch`, `edit`, `create`, `exec`, …) — the wrapper blocks those; they must go through plain kubectl with explicit user approval.

## Procedure

### 1. Classify the command

**Supported — MUST use kubectl-readonly:**

| Command | Notes |
|---------|-------|
| `get` | display resources |
| `describe` | detailed resource info |
| `logs` | view container logs (including `-f`) |
| `top` | CPU/memory usage (including `--watch`) |
| `explain` | API documentation for resources |
| `api-resources`, `api-versions` | list available API objects/versions |
| `cluster-info`, `version` | cluster and client info |
| `events` | view cluster events |
| `wait` | wait for a condition (read-only) |
| `diff` | show differences without applying (see Pitfalls) |
| `kustomize` | local manifest rendering (some flags blocked) |

**Commands with specific allowed subcommands:**

| Command | Allowed subcommands |
|---------|---------------------|
| `config` | `view`, `get-contexts`, `get-clusters`, `get-users`, `current-context`, `use-context` |
| `auth` | `can-i`, `whoami` |
| `rollout` | `status`, `history` |
| `krew` | `list`, `search`, `info` |

**Not on the list → plain kubectl, with explicit user approval.** When in doubt, ask the wrapper:

```bash
kubectl-readonly --readonly-check-ok <command>
# OK: This command is allowed by kubectl-readonly
# BLOCKED: This command is not allowed in read-only mode
```

### 2. Locate the kubectl-readonly binary

Check in this order:

1. `command -v kubectl-readonly` — in PATH (release binary or go install)
2. Krew install location: `~/.krew/bin/kubectl-readonly` (Windows: `%USERPROFILE%\.krew\bin`) — may not be on PATH; invoke by full path or via the plugin form `kubectl readonly <args>`
3. Go install location: `$(go env GOPATH)/bin/kubectl-readonly` or `$GOBIN` — often not on PATH
4. Manual release installs: `/usr/local/bin/kubectl-readonly`, `~/bin/kubectl-readonly`

If found, run the **standalone binary** (`kubectl-readonly <args>`), not the plugin form — see Pitfalls.

### 3. If not found — point to installation

Do NOT silently install, and do NOT fall back to plain kubectl for a supported command. Show the user the options from the installation guide (https://github.com/Evaneos/kubectl-readonly#installation):

1. **Krew (recommended):** `kubectl krew install readonly`
2. **Prebuilt binary:** latest release from https://github.com/Evaneos/kubectl-readonly/releases, added to PATH
3. **From source** (Go 1.25+): `go install github.com/Evaneos/kubectl-readonly@latest`

Ask permission before installing (see the `install-missing-tool` skill), then re-run step 2 and verify with `kubectl-readonly --version`.

### 4. Run the command through the wrapper

```bash
kubectl-readonly get pods -A
kubectl-readonly describe deployment nginx -n <ns>
kubectl-readonly logs <pod> -n <ns> --tail=100
```

**Write commands:** if the task requires a mutation, use plain `kubectl`, state exactly what will change, and ask for explicit approval first. Never route writes through kubectl-readonly (it blocks them) and never attempt to circumvent the block.

## Pitfalls

- **Plain kubectl for read-only work** — the main failure this skill exists to prevent. It is correct but not auto-approved; it prompts the user for permission on every call. Always use the wrapper when the command is supported.
- **Plugin-form approval mismatch:** `kubectl readonly get pods` runs the wrapper, but a permission allowlist of `Bash(kubectl-readonly:*)` does NOT match — the command still looks like plain kubectl and gets gated. Prefer invoking the standalone binary; only use the plugin form if the binary is not in PATH and the user has approved `kubectl readonly:*`.
- **Missing binary ≠ permission to use plain kubectl:** if kubectl-readonly is not installed, present the installation options (step 3) and ask. Only fall back to plain kubectl if the user declines installation AND explicitly approves running the command.
- **Secret masking is a feature:** `get secret -o yaml`, `get --raw` on secret paths, and `config view --raw` are blocked by design. If the user explicitly needs secret values, mention the `kubectl-readonly-leaky` profile exists — do not silently switch profiles or work around the block.
- **`diff` with external programs:** if `KUBECTL_EXTERNAL_DIFF` is set in the environment, `kubectl diff` can execute arbitrary programs; prefer `kubectl-readonly-strict` in that case (krew: `kubectl krew install readonly-strict`).
- **`use-context`:** allowed, but it modifies the local kubeconfig. Tell the user which context you switched to.

## Verification

- `command -v kubectl-readonly` resolves (or the full path from step 2 executes)
- `kubectl-readonly --readonly-check-ok <cmd>` prints `OK: This command is allowed` before you run it
- A write command attempted through the wrapper is **blocked** (`This command is not safe for read-only access`) — if a write ever goes through the wrapper, stop and investigate
