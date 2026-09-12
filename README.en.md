# RootMyVivo Payloads

The payload catalog for [RootMyVivo](https://github.com/zenyxx-xd/RootMyVivo). The app ships empty-handed — every exploit is downloaded from here.

[Русский](README.md) · [中文](README.zh.md)

## What's in here

- `support/targets-vivo.json` — the catalog: which devices are supported, by which exploit, where the binary lives
- Releases — the payloads themselves. One build per device; offsets don't carry over between kernels

## How the app picks a payload

Model first (`models` / `marketNames`), then kernel. A `kernelVersions` entry comes in three shapes:

```json
"6.6.89"                              // short: any 6.6.89
"6.6.89-android15-8-gb57af212129c"    // full: one specific kernel build (matched against uname)
"6.6.89-android15-8-*"                // prefix
```

The full form matters when one model ships on different kernel builds — e.g. the Neo10 Pro exists on both `gf2c960562dc8` and `b57af212129c` (shared with the X200). A short entry would grab the wrong binary and the slide simply won't converge.

## Builds

| Release | Contents |
|---|---|
| [v0.4.0-ports](https://github.com/zenyxx-xd/RootMyVivo-Payloads/releases/tag/v0.4.0-ports) | RMV-engine ports: iQOO 13 India (I2401), Neo10 Pro (PD2426), X200 (PD2415), X200 Pro (PD2405) |
| [v0.4.0-neo11](https://github.com/zenyxx-xd/RootMyVivo-Payloads/releases/tag/v0.4.0-neo11) | Neo 11, kernel 6.6.89 |
| [v0.4.0-6.6.127-neo11](https://github.com/zenyxx-xd/RootMyVivo-Payloads/releases/tag/v0.4.0-6.6.127-neo11) | Neo 11, kernel 6.6.127 |
| [v0.2.1-neo11](https://github.com/zenyxx-xd/RootMyVivo-Payloads/releases/tag/v0.2.1-neo11) | Neo 11, early build |

Offsets for the ports come from the public repos of the respective device authors (AmarnathCJD, sgswzglwlx, xiaohj233, CyberMeowfia) — credit lives in each catalog entry's `verifiedBy` field.

## Adding your device

You need your firmware's `boot.img` (an OTA zip is enough, no root required) or a kallsyms dump. Then:

1. Generate target.h from kallsyms+BTF — see [RootMyVivo-Exploit](https://github.com/zenyxx-xd/RootMyVivo-Exploit), the generator is there
2. Build preload.so and check the chain at least gets past the slide
3. PR with an entry in `targets-vivo.json`: model, full kernel string, binary URL, sha256

Third-party payloads are welcome too, as long as they don't soft-reboot the phone mid-install — that's the whole point of this setup.

## Disclaimer

Your own devices only. The authors are not responsible for the consequences.
