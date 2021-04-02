# OztekinGroup
Instructions to reinstall lab workstations

## Creating Installer
1. Download CentOS 7.9 from [Lehigh mirror](http://linux.cc.lehigh.edu/centos/7/isos/x86_64/) for fastest speed, DVD iso is enough.
2. Use [balenaEtcher](https://www.balena.io/etcher/) to write .iso to a flash drive.
3. Boot from the flash drive. Select `UEFI` if available.
4. Install CentOS 7. If you experience graphics issue, press `e` and append `nomodeset` then press ctrl+x to start installer. If still not working, select `Troubleshooting -> CentOS 7 in basic graphics mode`.
5. 
## Options
1. Software Selection: `Development and Creative Workstation` plus `Development Tools`
2. Installation Destination: Choose the SSD, and select `I will configure partitioning`. Delete all existing partitions and select `Standard Partition`. Set up 200 MB for `/boot/efi`, **at least** 60 GB for `/`(use `ext4`) and the rest to `/home`(use `ext4`). No need to have `/swap`
3. Disable Kdump(useless)
4. Network : Enable ethernet if available.
5. Setup root password and user/password. Do not select `make this user administrator`.

## Initialization
First time boot: Log in to `root`. Click the gear and select `GNOME` instead of `GNOME Classic`.
Perform system update
```
yum -y update
```
Goto `Utilities -> Disks` and select HDD. Click `Edit mount options` and switch `Use Session Defaults` to `OFF`

Reboot

## Nvidia Driver
Import ELRepo
```
rpm --import https://www.elrepo.org/RPM-GPG-KEY-elrepo.org
yum -y install https://www.elrepo.org/elrepo-release-7.el7.elrepo.noarch.rpm
```
Install Nvidia **390** driver(Latest version may not work)
```
yum -y install kmod-nvidia-390xx
nvidia-xconfig
```
Reboot

## OpenFOAM
Install pre-compiled OpenFOAM
```
yum -y install epel-release
yum -y update
yum -y install yum-plugin-copr
yum -y copr enable openfoam/openfoam
yum -y install openfoam-selector
yum -y install openfoam2012-default
```
Install legacy Paraview
```
yum -y install paraview paraview-devel
```
Install alternative Paraview binary
```
cd /usr/lib/openfoam/
wget -O ParaView-5.9.0.tar.gz "https://www.paraview.org/paraview-downloads/download.php?submit=Download&version=v5.9&type=binary&os=Linux&downloadFile=ParaView-5.9.0-MPI-Linux-Python3.8-64bit.tar.gz"
tar -xzf ParaView-5.9.0.tar.gz 
mv ParaView-5.9.0-MPI-Linux-Python3.8-64bit/ ParaView-5.9.0
rm ParaView-5.9.0.tar.gz 
```
## Install Ansys
Only CentOS 7 + Ansys 2020 R2 combination works.
```
mkdir -p /share/Apps/ANSYS
yum -y install filezilla libpng12
```
Copy all files from `sol.cc.lehigh.edu:/share/Apps/ANSYS/v202`
Use `chmod +x -R` to fix executable permission.

## Customization (Optional)
If you don't need a Windows-10 like desktop environment, skip this step.
```
yum -y install chrome-gnome-shell
```
Goto https://extensions.gnome.org/ and install the following gnome-extensions

[Dash to Panel](https://extensions.gnome.org/extension/1160/dash-to-panel/)

[TopIcons Plus](https://extensions.gnome.org/extension/1031/topicons/)

[No Topleft Hot Corner](https://extensions.gnome.org/extension/118/no-topleft-hot-corner/)

Start `GNOME Tweaks`

In `Desktop` tab enable icons.

In `Extensions` tab switch on `Application menu`. Goto the setting button of `Dash to panel` and disable `Application button`

In `Windows` tab enable minimize and maximize button.

## Troubleshooting
1. OpenFOAM is not activated for new users.
```
echo "source /usr/lib/openfoam/openfoam2012/etc/bashrc $FOAM_SETTINGS" >> $HOME/.bashrc
```
2. The default paraview version is too old.
```
echo "export PATH=/usr/lib/openfoam/ParaView-5.9.0/bin:$PATH" >> $HOME/.bashrc
```
3. Can't read external hard drive

Install `ntfs-3g`
