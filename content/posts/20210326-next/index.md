---
title: "Ansible NEXT server"
author: "phoenix"
date: 2021-03-26T09:54:20+01:00
---
The [geekoops-next](https://github.com/GeekOops/geekoops-next) ansible role makes it easy to setup your own PXE boot server. It configures `dnsmasq` to run a DHCP and TFTP server, from which your PXE clients can boot from. You can turn the DNS functionality of `dnsmasq` off, so that it is possible to run this PXE server next to another DNS server on the same host.

This role also extracts a minimum `syslinux` installation into your tftp directory. So, after deploying this role, your clients should be able to boot from this PXE server, but it appears empty.

![Boot sequence demo](/img/boot-sequence.gif)

Currently this role only supports legacy boot.

## Example

The following playbook installs this PXE boot server on `jellyfish`. However we want to use another dhcp server for handing out the leases, so that we configure `dnsmasq` to act as proxy. This is useful if you want to run your PXE server alongside another DHCP server, like for example in a network configuration where your Fritzbox takes care of the DHCP. Our example is in a `libvirt` environment, so we let the default `libvirt` DHCP server (192.168.122.1) handle dhcp:

    - hosts: jellyfish
      roles:
         - role: geekoops-next
           vars:
             config_firewall: true
             firewall_zone: "public"
             dhcp_range: "192.168.122.1,proxy,255.255.255.0"
             prompt: "My awesome network boot server"

### Testing

If `jellyfish` runs as virtual machine in your default `libvirt` domain, you can now create a new virtual machine that boots from the network.

![Virt-manager demo](/img/configure-client.gif)

In `virt-manager`, create a new Virtual machine, with "Manual Boot". Use any Linux system, even "Generic OS" just works fine. We don't need a disk for booting, so that one can be deselected. Now, if you boot the machine, you will see the boot menu from the PXE server

![Boot screen](/img/BootScreen.png)