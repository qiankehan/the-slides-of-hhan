# Workshop: customize your libvirt and QEMU
## General steps
1. Install dependencies
2. Download the code
3. Do configurations(for qemu you may edit Kconifg files to customize your
boards and devices)
4. Make
5. Install

## Check RedHat(CentOs) downstream configurations
1. Go to RedHat(CentOs) [build site](https://koji.mbox.centos.org/koji)
2. Install the src rpm file
3. Build the rpm pkgs by rpm-build with -v option then check the log of
configurations stage.

## Build libvirt
### Install dependencies
For RHEL8/Centos8, Fedora 22 or higher:
```sh
dnf builddep libvirt -y
```

### Download code
```
git clone https://github.com/libvirt/libvirt
```

### Configuration
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

### Building
```
make
```

### Installation
```
make install
```

## Build qemu
### Install dependencies
For RHEL8/Centos8:
```sh
dnf builddep qemu-kvm -y
```
For Fedora 22 or higher:
```sh
dnf builddep qemu -y
```

### Download code
```
git clone https://git.qemu.org/git/qemu.git
```

### Configuration
Make configuration by **./configure**
For example, the qemu configuration of RHEL8:
```sh
./configure \
# Installation dir
--prefix=/usr \
--libdir=/usr/lib64 \
--sysconfdir=/etc \
--interp-prefix=/usr/qemu-%M \
--localstatedir=/var \
--docdir=/usr/share/doc/qemu-kvm \
--libexecdir=/usr/libexec \
--with-pkgversion=qemu-kvm-4.2.0-1.el8 \
--with-confsuffix=/qemu-kvm \
--firmwarepath=/usr/share/qemu-firmware \
# CFLAGS and LDFLAGS
'--extra-ldflags=-Wl,--build-id -Wl,-z,relro -Wl,-z,now' \
'--extra-cflags=-O2 -g -pipe -Wall -Werror=format-security -Wp,-D_FORTIFY_SOURCE=2 -Wp,-D_GLIBCXX_ASSERTIONS -fexceptions -fstack-protector-strong -grecord-gcc-switches -specs=/usr/lib/rpm/redhat/redhat-hardened-cc1 -specs=/usr/lib/rpm/redhat/redhat-annobin-cc1 -m64 -mtune=generic -fasynchronous-unwind-tables -fstack-clash-protection -fcf-protection' \
# Library dependencies list
--disable-fdt \
--enable-glusterfs \
--enable-guest-agent \
--enable-numa \
--enable-rbd \
--enable-rdma \
--disable-pvrdma \
--enable-seccomp \
--enable-spice \
--enable-smartcard \
--enable-virglrenderer \
--enable-opengl \
--enable-usb-redir \
--disable-tcmalloc \
--enable-libpmem \
--enable-vhost-user \
--enable-avx2 \
--python=/usr/libexec/platform-python \
# Emualtion target
--target-list=x86_64-softmmu \
# block backend list
--block-drv-rw-whitelist=qcow2,raw,file,host_device,nbd,iscsi,rbd,blkdebug,luks,null-co,nvme,copy-on-read,throttle,gluster \
--block-drv-ro-whitelist=vmdk,vhdx,vpc,https,ssh \
--disable-bochs \
--disable-cloop \
--disable-dmg \
--disable-qcow1 \
--disable-vdi \
--disable-vvfat \
--disable-qed \
--disable-parallels \
--disable-sheepdog \
--audio-drv-list= \
--with-coroutine=ucontext \
--tls-priority=NORMAL \
--disable-bluez \
--disable-brlapi \
--disable-cap-ng \
--enable-coroutine-pool \
--enable-curl \
--disable-curses \
--disable-debug-tcg \
--enable-docs \
--disable-gtk \
--enable-kvm \
--enable-libiscsi \
--disable-libnfs \
--enable-libssh \
--enable-libusb \
--disable-bzip2 \
--enable-linux-aio \
--disable-live-block-migration \
--enable-lzo \
--enable-pie \
--disable-qom-cast-debug \
--disable-sdl \
--enable-snappy \
--disable-sparse \
--disable-strip \
--enable-tpm \
--enable-trace-backend=dtrace \
--disable-vde \
--disable-vhost-scsi \
--disable-vxhs \
--disable-virtfs \
--disable-vnc-jpeg \
--disable-vte \
--enable-vnc-png \
--enable-vnc-sasl \
--enable-werror \
--disable-xen \
--disable-xfsctl \
--enable-gnutls \
--enable-gcrypt \
--disable-nettle \
--enable-attr \
--disable-bsd-user \
--disable-cocoa \
--enable-debug-info \
--disable-guest-agent-msi \
--disable-hax \
--disable-jemalloc \
--disable-linux-user \
--enable-modules \
--disable-netmap \
--disable-replication \
--enable-system \
--enable-tools \
--disable-user \
--enable-vhost-net \
--enable-vhost-vsock \
--enable-vnc \
--enable-mpath \
--disable-xen-pci-passthrough \
--enable-tcg \
--with-git=git \
--disable-sanitizers \
--disable-hvf \
--disable-whpx \
--enable-malloc-trim \
--disable-membarrier \
--disable-vhost-crypto \
--disable-libxml2 \
--enable-capstone \
--disable-git-update \
--disable-crypto-afalg \
--disable-debug-mutex \
--disable-auth-pam \
--enable-iconv \
--disable-lzfse \
--enable-vhost-kernel \
--with-default-devices
```

#### Configuration for boards or devices
Kconfig files are store in **default-configs/** divided by **target name**(see
--target-list option in ./configure).  
Echo config option are started with **CONFIG\_**  
Use **--without-default-devices** to disable default devices and make a cleaner
device or boards list.

### Building
```
make
```

### Installation
```
make install
```

# Examples
## Build qemu with only MircoVM,  virtio devices, x86_64 cpu arch, kvm enabled
Configuration:
```
./configure --target-list=x86_64-softmmu --enable-kvm --disable-werror
```
Edit **default-configs/x86_64-softmmu.mak**:
```
include i386-softmmu.mak
CONFIG_VIRTIO_BALLOON=y
CONFIG_VIRTIO_BLK=y
CONFIG_VIRTIO_NET=y
CONFIG_VIRTIO_RNG=y
CONFIG_VIRTIO_SCSI=y
CONFIG_VIRTIO_SERIAL=y
CONFIG_MICROVM=y
```
