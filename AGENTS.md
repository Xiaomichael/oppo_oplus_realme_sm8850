# Repository Guidelines

## Project Structure & Module Organization
This repository provides automation for building Android 16 Linux 6.12 kernels for OPPO, OnePlus, and Realme SM8850/MT6993 devices. Keep top-level files focused on project documentation and release assets.

- `.github/workflows/`: GitHub Actions kernel build workflows, currently organized by kernel version such as `fastbuild_6.12.58.yml`.
- `local/`: interactive local build scripts, named by kernel version and target family, for example `builder_6.12.58.sh` and `builder_6.12.58_mtk.sh`.
- `droidspaces_patch/`, `other_patch/`, `zram_patch/`: patch sets grouped by feature or subsystem.
- `lib/`: bundled helper binaries and shared libraries used by build workflows.
- `README.md`: user-facing overview, feature list, and acknowledgements.

## Build, Test, and Development Commands
Use the local scripts from the repository root or invoke GitHub Actions manually.

- `bash local/builder_6.12.58.sh`: run the local SM8850 Linux 6.12.58 build flow and answer feature prompts.
- `bash local/builder_6.12.58_mtk.sh`: run the MT6993 variant of the local build flow.
- `bash -n local/*.sh`: syntax-check shell scripts without executing build steps.
- GitHub Actions `workflow_dispatch`: preferred for reproducible remote builds with selectable KSU, susfs, ccache, networking, and patch options.

## Coding Style & Naming Conventions
Shell scripts use Bash with `set -e`, uppercase environment/configuration variables, and clear Chinese status messages. Preserve existing prompt style and option letters (`y/n`, `r/y/n/k/l`) when extending scripts. Name new builders as `builder_<kernel-version>[_gki|_mtk].sh`; name patches descriptively with `.patch` suffix and place them in the most specific patch directory.

## Testing Guidelines
There is no standalone unit test suite. Validate changes with `bash -n` for scripts and, when practical, one full local or Actions build for the affected kernel version. For patch changes, confirm they apply cleanly in the matching workflow or builder path and do not alter unrelated device variants.

## Commit & Pull Request Guidelines
Recent history uses Conventional Commit prefixes such as `feat:`, `fix:`, and `chore:`. Keep commits focused, for example `fix: update 6.12.58 unicode patch path`. Pull requests should describe the target kernel/device family, changed feature flags or patches, validation performed, and any expected build-output differences. Include logs or screenshots for workflow failures or packaging changes.

## Security & Configuration Tips
Do not commit private signing keys, device credentials, or local cache contents. Treat bundled binaries in `lib/` as build inputs: replace them deliberately, document the source/version, and verify executable permissions before release.
