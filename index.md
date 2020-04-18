## AWS Deeplens Factory Reset

There is a couple of division between Linux and OSx [AWS Deeplens Factory Reset](https://docs.aws.amazon.com/deeplens/latest/dg/deeplens-device-factory-reset-preparation.html)

### Preparing USB flash drive

1. Erase USB drive as `GUID partition map` via Disk Utility GUI
1. Type `diskutil list` via Command line
1. Use `sudo gpt destroy /dev/diskX` to remove all partitions from the external USB drive
1. Create FAT32 2048 MB partition named boot using [Hard Disk Manager](https://www.paragon-software.com/hdm-mac/)
1. Create NTFS partition for the rest of space using the dame software

Check your operations by using `diskutil list`:

```bash
/dev/disk2 (external, physical):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:     FDisk_partition_scheme                        *15.7 GB    disk2
   1:                 DOS_FAT_16                         2.0 GB     disk2s1
   2:               Windows_NTFS                         13.6 GB    disk2s2
```

## Preparing and deploying Recovery Image

1. Download [Recovery Disk](https://s3.amazonaws.com/deeplens-public/factory-restore/Ubuntu-Live-16.04.3-Recovery.iso)
1. Using command-line convert `.iso` to `.dmg` file
- `hdiutil convert -format UDRW -o ~/Downloads/deeplens_recovery.img ~/Downloads/Ubuntu-Live-16.04.3-Recovery.iso`
- `mv ~/Downloads/deeplens_recovery.img.dmg ~/Downloads/deeplens_recovery.img`
1. Umount preparing device using `diskutil umountDisk /dev/diskX`
1. Deploy the image using command `sudo dd if=~/Downloads/deeplens_recovery.img of=/dev/rdisk2s1 bs=1m`

After this you should see the following output:

```bash
1308+1 records in
1308+1 records out
1372379136 bytes transferred in 231.060062 secs (5939491 bytes/sec)
```

## Preparing and deploying factory restore files

1. Download files for [1.1](https://s3.amazonaws.com/deeplens-public/factory-restore/DeepLensFactoryRestore_hw_1_1.zip) or for [1.0](https://s3.amazonaws.com/deeplens-public/factory-restore/DeepLensFactoryRestore.zip)
1. Copy files from zip to `Flash` using [Tuxera](http://tuxera.com/products/tuxera-ntfs-for-mac/)
