<div align="center" style="text-align: center;">

<h1>privatepilot</h1>

<p>
  <b>openpilot, with data uploads and telemetry off by default.</b>
  <br>
  A privacy-focused fork of <a href="https://github.com/commaai/openpilot">commaai/openpilot</a>.
</p>

</div>

privatepilot tracks upstream openpilot closely and changes as little as possible: your driving logs, dashcam footage, and telemetry stay on your device unless you explicitly opt in.

### What's different from upstream

Upstream openpilot uploads logs, crash data, and telemetry to comma's servers. privatepilot flips that default:

- **No uploads by default.** The `uploader` and `athenad` (telemetry) processes don't run. All data collection still happens locally, so debugging, dashcam, and replay work normally.
- **No device registration.** privatepilot never contacts comma's registration server; the device identity is read from local storage only.
- **Opt-in toggle.** "Enable Data Uploads" in settings (under Toggles) restores stock upload behavior at any time, no restart needed.

What still contacts the network (only if you use the feature): OTA updates (from this repo), navigation, GPS assistance, and any comma services you explicitly enable along with data uploads.

What's gone while uploads are off: [comma connect](https://connect.comma.ai/) route viewer, prime, firehose, and comma-side crash reporting.

Everything else is stock openpilot. See upstream's [supported cars](docs/CARS.md) (300+), [safety model](docs/SAFETY.md), and [documentation](https://docs.comma.ai).

### Using privatepilot in a car

To run it, you need a supported device (comma four), a [supported car](docs/CARS.md), and a [car harness](https://comma.ai/shop/car-harness). Detailed hardware setup instructions are at [comma.ai/setup](https://comma.ai/setup).

Install options:
1. **Installer:** use the `privatepilot` installer built from this repo (points at `github.com/perryh/privatepilot`, branch `nightly-dev`).
2. **SSH:** clone this repo to `/data/openpilot` and check out the `nightly-dev` branch.

Updates install automatically from the `nightly-dev` branch of this repo, same as stock OTA.

### For maintainers

`master` tracks `commaai/openpilot:master` plus a small patch series:
- `EnableUploads` param (off by default) + process gating in `system/manager/process_config.py`
- early-exit guards in `system/athena/athenad.py` and `system/loggerd/uploader.py`
- registration without network in `system/athena/registration.py`
- settings toggle in both UIs, custom installer, this README

Releases are built with comma's own tooling on-device: `RELEASE_BRANCH=nightly-dev PANDA_DEBUG_BUILD=1 tools/release/build_release.sh`, then force-pushed to the `nightly-dev` branch here. To update the fork: rebase the patch series on upstream `master`, rebuild, push — devices OTA on their own.

### Credits

This is a fork of [openpilot](https://github.com/commaai/openpilot), developed by [comma](https://comma.ai/) and its community. All upstream features, supported cars, and docs are theirs — thank you. Nothing here would exist without them.

<details>
<summary>MIT Licensed</summary>

openpilot is released under the MIT license. Some parts of the software are released under other licenses as specified.

Any user of this software shall indemnify and hold harmless Comma.ai, Inc. and its directors, officers, employees, agents, stockholders, affiliates, subcontractors and customers from and against all allegations, claims, actions, suits, demands, damages, liabilities, obligations, losses, settlements, judgments, costs and expenses (including without limitation attorneys’ fees and costs) which arise out of, relate to or result from any use of this software by user.

**THIS IS ALPHA QUALITY SOFTWARE FOR RESEARCH PURPOSES ONLY. THIS IS NOT A PRODUCT.
YOU ARE RESPONSIBLE FOR COMPLYING WITH LOCAL LAWS AND REGULATIONS.
NO WARRANTY EXPRESSED OR IMPLIED.**
</details>
