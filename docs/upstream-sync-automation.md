# Official melonDS upstream sync and desktop snapshot releases

**Canonical upstream:** https://github.com/melonDS-emu/melonDS (master).

**Workflow:** [`.github/workflows/upstream-sync-release.yml`](../.github/workflows/upstream-sync-release.yml)

## Preserve existing development history

This GitHub repository has historical source branches including `arm9cache2`, `master` and experimental branches. The automation is deliberately isolated:

- **`arm9cache2`**: existing default branch, stores automation config and these docs.
- **`upstream-master`**: exact official upstream master mirror. Only this branch gets fast-forwarded.
- All other branches and existing releases are untouched. No force-push or automatic merge into `master` or `arm9cache2`.

## Schedule and release conditions

A GitHub-hosted scheduled job checks approximately every 10 minutes, depending on GitHub scheduling. A new upstream commit is fetched, checked and pushed to `upstream-master`. If the tag `melonds-upstream-<12-digit-SHA>` does not exist, the same run builds Windows, Linux and macOS from the mirror SHA. Only if all three builds pass is one prerelease published, with SHA256SUMS.txt.

If upstream has not moved, scheduled checks do not repeat builds for a prior failed revision. Use the **Run workflow** button in Actions to retry the current revision manually. A new upstream commit starts a new build automatically.

## Build outputs

- Windows x64 ZIP: official CMake/vcpkg preset, executable and license.
- Linux x64 TAR.GZ: Ubuntu 22.04 build and installed files; requires system libraries and is **not an AppImage**.
- macOS Apple Silicon ZIP: official CMake/vcpkg preset, `.app`, ad-hoc signed but not notarized.

This is an unofficial community build. Source is licensed according to the upstream repository; preserve license and attribution. No game ROMs, BIOS or firmware files are bundled.

## Run/diagnostics

Actions → **Sync official melonDS and release desktop snapshots** → Run workflow. The workflow needs a GitHub Actions token with repository Contents write permission (the default GITHUB_TOKEN can be granted this through workflow job permissions).

A failed build does not prevent the isolated source branch from staying synchronized, but it does prevent incomplete snapshot releases.
