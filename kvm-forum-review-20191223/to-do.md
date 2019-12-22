# Install dependencies
For RHEL7/Centos7:
```sh
yum builddep libvirt -y
```
For RHEL8/Centos8, Fedora 22 or higher:
```sh
dnf builddep libvirt -y
```
For Debian/Ubuntu:
```sh
apt-get build-dep libvirt0 -y
```

# Download code
```
git clone https://github.com/libvirt/libvirt
```

# Configuration
Libvirt uses the GNU Autotools build system, so in general can be built and
installed with the usual commands, however, we mandate to have the build
directory different than the source directory. The configure script is to set
configurations for feature enabling or disabling, complitation, installation.
```sh
cd libvirt
mkdir build && cd build
../configure
```
See config options:
```sh
../configure --help
```
RHEL8 configuration example:
```sh
./configure \
# Installation options:
--build=x86_64-redhat-linux-gnu \
--host=x86_64-redhat-linux-gnu \
--program-prefix= \
--disable-dependency-tracking \
--prefix=/usr \
--exec-prefix=/usr \
--bindir=/usr/bin \
--sbindir=/usr/sbin \
--sysconfdir=/etc \
--datadir=/usr/share \
--includedir=/usr/include \
--libdir=/usr/lib64 \
--libexecdir=/usr/libexec \
--localstatedir=/var \
--sharedstatedir=/var/lib \
--mandir=/usr/share/man \
--infodir=/usr/share/info \
--with-runstatedir=/run \

# Compilation options
--enable-werror \
--enable-expensive-tests \

# Feature options
## hypervisors
--without-phyp \
--with-esx \
--without-hyperv \
--without-vmware \
--without-vz \
--without-bhyve
--with-qemu \
--without-openvz \
--without-lxc \
--without-vbox \
--without-libxl \

## Authorization and security
--with-sasl \
--with-polkit \
--with-selinux \
--with-selinux-mount=/sys/fs/selinux \
--without-apparmor \

## Storage pools
--with-storage-fs \
--with-storage-lvm \
--with-storage-iscsi \
--with-storage-iscsi-direct \
--with-storage-scsi \
--with-storage-disk \
--with-storage-mpath \
--with-storage-rbd \
--without-storage-sheepdog \
--with-storage-gluster \
--without-storage-zfs \
--without-storage-vstorage \

## Dependency configuration
--with-numactl \
--with-numad \
--with-capng \
--without-fuse \
--with-netcf \
--without-hal \
--with-udev \
--with-yajl \
--with-sanlock \
--with-libpcap \
--with-macvtap \
--with-audit \
--with-dtrace \
--with-driver-modules \
--with-firewalld \
--with-firewalld-zone \
--without-wireshark-dissector \
--without-pm-utils \
--with-nss-plugin \
--without-login-shell \

## Misc features or settings
--with-libvirtd \
--with-remote-default-mode=legacy \
--with-interface \
--with-network \

# Build info
'--with-packager=Unknown,2019-12-09-09:40:33,10.66.85.241' \
--with-packager-version=4.el8 \

# Default runtime settings
--with-qemu-user=qemu \
--with-qemu-group=qemu \
--with-tls-priority=@LIBVIRT,SYSTEM \
--with-init-script=systemd \
```
