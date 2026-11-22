# Manifests for the Nokia Lumia 1520 (bandit)

- `lineageos-18.1/bandit_manifest.xml`: local manifest for LineageOS 18.1.

```
repo init -u https://github.com/LineageOS/android.git -b lineage-18.1 --git-lfs --depth=1
mkdir -p .repo/local_manifests
curl -fsSL https://raw.githubusercontent.com/lumia-msm8974/bandit-manifests/lineage-18.1/lineageos-18.1/bandit_manifest.xml \
  -o .repo/local_manifests/bandit.xml
repo sync -c -j8 --no-tags --no-clone-bundle
source build/envsetup.sh && lunch lineage_bandit-userdebug
```

## Framework patches

`lineageos-18.1/patches/` holds the changes to LineageOS projects that have no fork: apply them after
`repo sync`, from a clone of this repository.

```
git clone https://github.com/lumia-msm8974/bandit-manifests
(cd frameworks/base && git am ../../bandit-manifests/lineageos-18.1/patches/frameworks/base/*.patch)
```

- `frameworks/base/0001`: SystemUI builds one `ThumbnailData` per task snapshot (recents).
- `frameworks/base/0002`: `TextView` reads `ro.config.marquee_fade_scrolling`; the msm8974 common tree sets it
  to `false`, so a scrolling marquee has no fading edges (each one costs the Adreno 330 a framebuffer copy in
  the middle of the frame).
- `frameworks/base/0003`: ActivityManager reads its cached process limit from
  `ro.config.max_cached_processes` (32 when unset); the msm8974 common tree sets 64, so the low memory killer,
  not a count, decides how many background apps stay alive.
- `frameworks/base/0004`: SettingsProvider loads a default for the animator duration scale
  (`def_animator_duration_scale`); the msm8974 common tree overlays it, with the window and transition scales, to
  0.5x for new installations.

Credits: LineageOS (msm8974 common, bacon and kernel trees), TheMuppets
(vendor blobs). See the device tree README for the full list.
