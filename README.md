# udon-twrp — TWRP for OnePlus 11R, cloud-built on GitHub Actions

Builds **TWRP recovery** for the OnePlus 11R (udon / CPH2487) directly on **free GitHub
Actions runners** — no build farm needed.

- Base: [minimal-manifest-twrp](https://github.com/minimal-manifest-twrp/platform_manifest_twrp_aosp) (`twrp-12.1` by default; `twrp-14` / `twrp-14.1` selectable)
- Device tree: [Gokulgethu/udon](https://github.com/Gokulgethu/udon) (fork of
  [shadowjynxs/udon](https://github.com/shadowjynxs/udon), with the missing
  `prebuilt/dtb.img` restored so `TARGET_PREBUILT_DTB` resolves)
- Kernel: prebuilt `Image.gz` + `dtb` + `dtbo.img` shipped in the device tree
- udon has a **dedicated recovery partition** (`BOARD_USES_RECOVERY_AS_BOOT = false`),
  so the output is a real `recovery.img`

## Run a build

Actions tab → **Build TWRP for udon** → **Run workflow** → Run.

Takes roughly 2–4 h on the free 4-vCPU runner (≈30 min source sync + compile).
When it finishes, grab `twrp-udon.img` from the **Releases** page or the run's
artifacts.

## Flash

```bash
# from bootloader/fastboot mode
fastboot flash recovery_a twrp-udon.img
fastboot flash recovery_b twrp-udon.img   # keep both slots consistent
# boot straight into recovery (do NOT let the OS boot first on a locked setup)
fastboot reboot recovery
```

Booting the ROM after flashing is fine — udon's recovery partition is not
overwritten by the OS.

## Notes

- Free runners have a 6-hour job cap; this build fits within it.
- The device tree enables FBE metadata decryption, NTFS-3G, fastbootd, and ADB
  as root — standard TWRP feature set for this device.
- To try a newer TWRP base: run the workflow with `twrp_branch` = `twrp-14` or
  `twrp-14.1` (tree may need small flag adjustments on newer bases).
