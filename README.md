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

Credits: LineageOS (msm8974 common, bacon and kernel trees), TheMuppets
(vendor blobs). See the device tree README for the full list.
