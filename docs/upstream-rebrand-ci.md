# Upstream sync and rebrand automation

`.github/workflows/upstream-rebrand.yml` checks `OpenDrone-hw/OpenRX-Gemini:main`
weekly and on manual dispatch. If upstream has moved ahead, the job merges it
into `main`, runs the repository's PCB rebranding skill in a temporary Boat
sandbox, and opens or updates `automation/upstream-sync-rebrand` as a pull
request. The workflow never merges that pull request.

## One-time GitHub setup

1. Add repository secret `BOAT_API_KEY`. Create it with Boat's `ci` API-key
   preset; it needs permission to create and delete sandboxes and run commands.
2. Add repository secret `BOAT_CI_SNAPSHOT_OPENRX_GEMINI` with the name of a Boat named
   snapshot containing KiCad 10, its `pcbnew` Python bindings, a working GUI
   environment for stackup edits, and the fonts used by the board. Claude Code
   must also be available to the Boat account through its Agents & Proxying
   settings. Boat must be able to run `claude` prompts.
3. Ensure the repository's Actions settings allow `GITHUB_TOKEN` to create
   pull requests and write repository contents.

The job uses Boat's documented CI flow: `boat login --key-stdin`,
`boat new --from <snapshot>`, `boat prompt --provider claude`, and `boat delete`.
It copies the exact synchronized source tree into the sandbox and retrieves the
result before creating a pull request. The sandbox is deleted even if a later
workflow step fails.

## Prepare or refresh the Boat snapshot

Create a Boat sandbox, install and configure the required KiCad 10 build,
`pcbnew` Python bindings, X/GUI support, and board fonts, then save it as a
named snapshot. Set `BOAT_CI_SNAPSHOT_OPENRX_GEMINI` to that name. Refresh the named snapshot
when the supported KiCad build or fonts change. The skill deliberately stops
without editing if it cannot use the supported KiCad route.

## Review

The scheduled job only opens or updates a pull request. Check upstream changes,
all `.kicad_*` diffs, the report in the pull request body, and refreshed board
renders before merging. A merge conflict with upstream fails the run for manual
resolution. `workflow_dispatch` runs the same process on demand; set
`force_rebrand` to true to exercise Boat when upstream has no new commits.
