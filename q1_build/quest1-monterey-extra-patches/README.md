# Quest 1 (monterey) extra patches

Two additional patches for the Quest 1 kernel, built on top of the
`q1_build/` harness in this repo (toolchain, `kernel.config`,
`oculus-kernel-fixes.patch`, KGDB-over-USB — all unchanged, not
duplicated here). Apply after that harness's own fixes, against the same
base commit noted in `q1_build/README-REPRODUCE.md`
(`oculus-linux-kernel` `6929f734`, "Re-sync with internal repository
(#235)").

- **`0001-Dual-boot-recovery-*.patch`** — standard Android-10
  recovery-as-boot handoff (`skip_initramfs` →
  `androidboot.force_normal_boot`, the same technique Moto G7, Essential,
  Razer, and Pixel 2 use), replacing this device's stock BCB-based
  recovery detection. Lets one `boot.img` boot stock Android normally and
  a custom recovery on a button combo, with no separate recovery
  partition and no BCB/hardware-key polling.
- **`0002-True-90Hz-panel-timing-*.patch`** — retimed DSI panel refresh
  rate/porch/clock-post/clock-pre values in
  `dsi-panel-sdc-lightman-video.dtsi` for the display's genuine 90Hz
  target (derived by diffing a third-party 90Hz patch's kernel DTB
  against this device's stock 72Hz baseline; timing values are absolute
  per-target-rate electrical parameters, not deltas, so they carry over
  directly). Covers all 14 board revisions built from this tree, which
  share this one panel-timing source file.

## Apply

```
cd oculus-linux-kernel   # after applying this repo's own oculus-kernel-fixes.patch
git am /path/to/q1_build/quest1-monterey-extra-patches/*.patch
```

## Related

[quest1-90hz-orangefox](https://github.com/TheCez/quest1-90hz-orangefox) —
prebuilt boot.img/flashable zips built from a kernel with these patches
applied, plus an install guide.
