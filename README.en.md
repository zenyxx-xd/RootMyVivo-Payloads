# RootMyVivo Payloads

The payload catalog for [RootMyVivo](https://github.com/zenyxx-xd/RootMyVivo).
The app ships no exploits — it downloads binaries described in
[`catalog/devices.json`](catalog/devices.json).

[Русский](README.md) · [中文](README.zh.md)

## Structure

```
catalog/devices.json        the single catalog manifest (schemaVersion 5)
bin/                        canonical .so files named by kernel build git-id
                            (bin/g1f71897ac249.so); builds without a git suffix
                            use a readable id (pd2405-ap3a.so)
support/targets-vivo.json   LEGACY v4 catalog for old app versions.
                            Frozen — do not edit; update the app instead.
```

## devices.json schema (v5)

- `builds` — one-entry-per-KERNEL-BUILD table keyed by GKI git-id. One build =
  one Image = one payload binary. Fields: `match` (uname patterns; full GKI
  string is stricter than a short version, `.*` = prefix), `exploit`, `status`,
  `file` ({name,url,mirrors,sha256,size}), `env`, `matchCondition` (provenance
  and evidence).
- `devices` — one entry per physical body: `marketName` + `code` (V-code), alias
  lists `models` (Build.DEVICE) / `names` (Build.MODEL), and `kernels` — every
  known kernel build of the body as `{build, note}`.
- Build `status`: `ready` | `off` | `patched` | `unsupported`.
- Matching: device by `models`/`names`, kernel by `match` — a short version must
  equal the uname release exactly, full strings match as substrings (this tells
  gf2c960562dc8 and gb57af212129c apart). No "any payload of this model"
  fallback.

## Adding a device

1. Obtain the exact kernel Image (full OTA → boot → uname/kallsyms, confirm
   unpatched remove_waiter).
2. Build the .so, put it in `bin/` named by git-id, record sha256 and size.
3. Extend `builds` and `devices` (note = data source: user form reports,
   upstream release, on-device test). Never invent model codes or statuses.

## Mirrors

`url` — GitHub release asset; `mirrors` — jsDelivr and raw repo (the asset CDN
is unreachable for some users while the catalog host works).

Older app versions read the frozen `support/targets-vivo.json` (v4).
