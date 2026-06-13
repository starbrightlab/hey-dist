# hey-dist

Public **release channel** for *hey* — the project that revives hands-free Amazon Alexa
("Hey Alexa") on discontinued, locked, unrooted **Meta Portal** devices, and adds wake-word
support for other assistants.

This repo hosts only **downloadable artifacts** (GitHub Releases). It contains **no source**:
the launcher is the public [`immortal`](https://github.com/starbrightlab/immortal) repo, and the
wake-word app + build tooling live in private repos. Private repos can't serve release assets
publicly, so the public-facing binaries are published here.

> Not affiliated with Amazon or Meta. We do **not** redistribute their binaries — see *Legal*.

## What's in a release

| Asset | What it is | Notes |
|---|---|---|
| `falcon.bsdiff` | A **binary diff**, not an app. Applied (`bspatch`) to the **public stock** Amazon Alexa APK from a Portal firmware dump, it reconstructs our patched + re-signed client **byte-for-byte**. | We ship only our *changes*; you fetch the stock APK yourself (URL + SHA-256 in `bridge-manifest.json`). |
| `millennium.apk` | The *hey* wake-word app (free build) that drives the revived Alexa hands-free. | _Coming once release signing is finalized._ |
| `bridge-manifest.json` | Data, not code: where to fetch stock falcon (+ checksums) and the reconstruction target hash. | Read by Immortal's provisioner and the future on-device installer. |

## How it's used

[`immortal`](https://github.com/starbrightlab/immortal)'s provisioner (`provisioning/provision.sh`)
offers an opt-in **"Restore Amazon Alexa?"** step (`./provision.sh --alexa`). It downloads the
public stock APK, applies `falcon.bsdiff` to rebuild our signed client, installs it, applies the
privileged setup, then installs the wake-word app. One Alexa build serves every Portal model.

## Legal

We never host Amazon's binary. `falcon.bsdiff` encodes only the difference between the
long-public Portal firmware dump (data the user already fetches) and our modified build. The
reconstruction is deterministic. This restores original on-device functionality for hardware the
manufacturer discontinued; it is intended for owners reviving their own devices.
