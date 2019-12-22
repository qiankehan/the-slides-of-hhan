# Introduction
It is a personal review about three topics on **KVM Forum 2019**: 
[Never too late to learn new tricks](https://people.redhat.com/berrange/kvm-forum-2019/kvm-forum-2019-libvirt.pdf),
[QEMU Status Report](https://static.sched.com/hosted_files/kvmforum2019/1d/kvmforum19-qemu.pdf),
[Reports of my bloat have been greatly exaggerated](https://static.sched.com/hosted_files/kvmforum2019/c6/kvmforum19-bloat.pdf)

## Libvirt
A toolkit to manage virtulization platforms(**qemu**, **xen**, **esx**,
**vmware**, **lxc**, **openvz**, **bhyve**, **virtualbox**,
**IBM PowerVM hypervisor**, **hyperv**).

## Qemu
A generic and machine emulator and virtualizer.

## Virtualization stack of RedHat
In Redhat downstream(RHEL7&8), libvirt supports virtulization platforms **qemu**
and **esx**. Qemu supports virtualizer **qemu tcg** and **kvm**.  
![virt stack](./virtualiazation-stack.svg)

### Reference
[1] KVM APIs: https://www.kernel.org/doc/Documentation/virtual/kvm/api.txt  
[2] qemu manual: https://qemu.weilnetz.de/doc/qemu-doc.html  
[3] qmp(QEMU Machine Protocol) reference: https://qemu.weilnetz.de/doc/qemu-qmp-ref.html  
[4] qemu gap(QEMU Guest Agent Protocol) reference: https://qemu.weilnetz.de/doc/qemu-ga-ref.pdf  
[5] libvirtd manual: https://libvirt.org/manpages/libvirtd.html  
[6] libvirt APIs reference: https://libvirt.org/html/  


# Workshop
- Check **what virtulization platforms are supported in RedHat downstream libvirt**
- Check **what virtualizers are supported in RedHat downstream qemu**
