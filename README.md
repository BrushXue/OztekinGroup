# OztekinGroup
Instructions to reinstall lab workstations

## Install CentOS 7.9
### Installer
1. Download CentOS 7.9 from [Lehigh mirror](http://linux.cc.lehigh.edu/centos/7/isos/x86_64/) for fastest speed, DVD iso is enough.
2. Use [balenaEtcher](https://www.balena.io/etcher/) to write .iso to a flash drive.
3. Boot from the flash drive. Select `UEFI` if available.
4. Install CentOS 7. If you experience graphics issue, press `e` and append `nomodeset` then press ctrl+x to start installer. If still not working, select `Troubleshooting -> CentOS 7 in basic graphics mode`.
### Options
1. Software Selection: `Development and Creative Workstation` plus `Development Tools`
2. Installation Destination: Choose the SSD, and select `I will configure partitioning`. Delete all existing partitions and select `Standard Partition`. Set up 200 MB for `/boot/efi`, **at least** 80 GB for `/`(use `ext4`) and the rest to `/home`(use `ext4`). No need to have `/swap`
3. Disable Kdump(useless)
4. Network : Enable ethernet if available.
5. Setup root password and user/password. Do not select `make this user administrator`.

## Install Nvidia Driver


## Troubleshooting
1. Can't read external hard drive

Install `ntfs-3g`
