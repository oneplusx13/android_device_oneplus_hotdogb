# Device Tree for OnePlus 7T Pro aka Hotdog for TWRP (this should be unified with 7T, but I cannot test since I do not own that device)
## Disclaimer - Unofficial TWRP!
These are personal test builds of mine. In no way do I hold responsibility if it/you messes up your device.
Proceed at your own risk.

## Setup repo tool
Setup repo tool from here https://source.android.com/setup/develop#installing-repo

## Compile

Sync twrp-10.0 manifest:

```
repo init -u git://github.com/minimal-manifest-twrp/platform_manifest_twrp_omni.git -b twrp-10.0
```

Create a manifest is .repo/local_manifests (name doesn't matter) and this repo in it.
In revision, specify what branch you want to clone. 

```xml
<?xml version="1.0" encoding="UTF-8"?>

<?xml version="1.0" encoding="UTF-8"?>
<manifest>
		<remote name="github" 
		fetch="https://github.com/" />       

		<project path="device/oneplus/hotdog"
			name="systemad/android_device_oneplus_hotdog"
			remote="github"
			revision="a11k" />
</manifest>

```

Sync the sources with

```
repo sync
```

To build, execute these commands in order

```
. build/envsetup.sh
export ALLOW_MISSING_DEPENDENCIES=true
export LC_ALL=C
lunch omni_hotdog-eng (sometimes you may have to cd into device/oneplus/hotdog and lunch there)
mka adbd recoveryimage 
```

To test it:

```
# To temporarily boot it
fastboot boot out/target/product/hotdog/recovery.img 

# Since 7T / Pro has a dedicated recovery parition, you can flash the recovery with
fastboot flash recovery recovery.img
```

#### Working
- [X] Flashing ROMs (AOSP and OOS)
- [X] ADB (+ sideload)
- [X] all important partitions listed in mount/backup lists
- [X] MTP export
- [X] decrypt /data (Custom ROM only)
- [X] Backup to internal/microSD (Custom ROM only)
- [X] Restore from internal/microSD (Custom ROM only)
- [X] F2FS/EXT4 Support, exFAT/NTFS where supported
- [X] backup/restore to/from external (USB-OTG) storage
- [X] update.zip sideload
- [X] backup/restore to/from adb (https://gerrit.omnirom.org/#/c/15943/)
- [ ] format data (untested)

#### Not working - OxygenOS specific
- [ ] decrypt /data (OxygenOS, because encryption is implemented differently)
- [ ] Backup to internal/microSD
- [ ] Restore from internal/microSD
- [ ] MTP export

##### Credits
- CaptainThrowback for original trees.
- mauronofrio for original trees.
- TWRP team and everyone involved for their amazing work.
