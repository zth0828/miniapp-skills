# Miniapp Fix Playbook

## Diagnostic Flow

When the user reports a DevTools or build problem, run through this flow in order.

### Step 1: Inspect Repo State

Check:

- `git status`
- repository root versus miniapp code root
- `project.config.json` and `miniprogramRoot`
- `app.json` pages match real files
- `project.private.config.json` (local-only, may hold stale compile conditions)

### Step 2: Confirm CLI Availability

Run the official DevTools CLI with `--help` to prove it is callable.

Typical commands:

```powershell
& '<cli-path>' open --help
& '<cli-path>' preview --help
```

### Step 3: Establish IDE Connectivity

Run `open` to get the live service port and confirm the session is healthy.

```powershell
& '<cli-path>' open --project '<project-root>' --port 9000
```

### Step 4: Compile Check With `preview`

Use `preview` as the primary compile check:

```powershell
& '<cli-path>' preview --project '<project-root>' --port '<live-port>'
```

Collect exit code, stdout, stderr, and any reported file/line information.

### Step 5: Classify The Failure

| Observation | Class | Action |
|-------------|-------|--------|
| `preview` reports repo syntax/compile error | repo-scoped | patch locally, rerun `preview` |
| `preview` reports TS recognition or config drift | repo-scoped | patch `project.config.json`/`tsconfig.json`, rerun |
| repo has DevTools-generated residue | repo-scoped | restore tracked files, delete residue |
| CLI starts but does not expose the problem | host/session | ask for GUI evidence |
| `preview` green but runtime still fails | runtime | stop CLI loop, ask for runtime evidence |
| host/login/AppID error | host blocker | tell user to fix on host |

## Pollution Cleanup

When DevTools has generated files in the wrong place:

1. Close the polluted project in DevTools.
2. Restore tracked files from git before deleting anything.
3. Delete only generated residue: nested code roots, template pages, extra config files.
4. Re-import the intended repository root.
5. Clear compile cache if the old shape was different.

## Success Criteria

Claim fix success only when:

1. the repo state matches the intended tracked shape, or
2. the CLI `preview` command exits successfully and the reported problem is gone.
