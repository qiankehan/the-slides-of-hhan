# Libvirt releases
Libvirt releases a major version every month.
Recent [major releases](https://libvirt.org/news.html):
- v5.10.0 (2019-12-02)
- v5.9.0 (2019-11-05)
- v5.8.0 (2019-10-05)

# Current libvirt architecture
- Libvirtd: Libvirt is typical C/S architecture. The server part is libvirtd
daemon. Client(like virsh) Communication to libvirtd is by RPC call.
- Languages:
    - core codes: C
    - build system: shell, sed, GNU autotools
    - docs: XSL, HTML, Markdown
    - Other scripts: perl, python
- handle OOM(out of memory): Libvirt handles OOM in its code.
- Low level library: GNULIB
- Code review: mail list

# Changes in virt useage
## Historical usage
- Datacenter virt, like oVirt
- Public/private cloud, like OpenStack
- Desktop virtualization, like Gnome Box or VirtManager

## New usage
- KVM inside containers, like KubeVirt
![KubeVirt arch](https://raw.githubusercontent.com/kubevirt/kubevirt/master/docs/architecture.png)
- KVM outside containers, like Kata Containers
![Kata Containers arch](https://katacontainers.io/images/kata-explained1@2x.png)
- Short-lived VMs for FaaS([function as a service](https://en.wikipedia.org/wiki/Function_as_a_service))
like [Firecracker](https://firecracker-microvm.github.io/)


# Tends of libvirt architecture
- libvirtd --> [modular daemons](Split the libvirtd daemon into per-driver daemons): 
Finished. Modular daemons will be changed to default in 2020.
    - For virt platforms:
        - virtlibxld
        - virtlxcd
        - virtqemud
        - virtvboxd
        - virtvzd
    - For other resources:
        - virtinterfaced
        - virtnetworkd
        - virtnodedevd
        - virtnwfilterd
        - virtsecretd
        - virtstoraged
- Languages:
    - docs: RST([reStructuredText](https://en.wikipedia.org/wiki/ReStructuredText)). Not finished.
    - build scripts: Change to Meson. Not finished.
    - Others: Python. Not finished.
- handle OOM: Use abort() instead. Not finished.
- Low level library: Glib(so-called glib2 in RHEL). Finished.

## Other revolutionary changes in future
- Embedded QEMU driver
    - No libvirtd involved
    - Run inside app provcess
    - Invisible to other libvirt clients
    - Isolated to private directory subtree
- Core codes: May be changed to memory safe languages like Rust or Golang.
- Code review: Web based code review, maybe github or gitlab

# Purposes for these changes
- Attractive work/challenges for **new contributors**
- Attractive feature for **app developers**
- Less work on **recreating wheels**
- Focus on features that **matter to apps**
