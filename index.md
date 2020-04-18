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

## Prepering and deploying factory restore files

1. Download files for [1.1](https://s3.amazonaws.com/deeplens-public/factory-restore/DeepLensFactoryRestore_hw_1_1.zip) or for [1.0](https://s3.amazonaws.com/deeplens-public/factory-restore/DeepLensFactoryRestore.zip)
1. Copy files from zip to `Flash` named image using [ntfs-3g](https://www.howtogeek.com/236055/how-to-write-to-ntfs-drives-on-a-mac/)

You can use the [editor on GitHub](https://github.com/eustimenko/deeplens/edit/master/index.md) to maintain and preview the content for your website in Markdown files.

Whenever you commit to this repository, GitHub Pages will run [Jekyll](https://jekyllrb.com/) to rebuild the pages in your site, from the content in your Markdown files.

### Markdown

Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/eustimenko/deeplens/settings). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://help.github.com/categories/github-pages-basics/) or [contact support](https://github.com/contact) and we’ll help you sort it out.
