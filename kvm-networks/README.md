# Configuring Virtual Machine Network Connections
***

- [Configuring Virtual Machine Network Connections](#configuring-virtual-machine-network-connections)
  - [Overview](#overview)
  - [Purpose](#purpose)
  - [Requirements](#requirements)
  - [Install Requirements](#install-requirements)
  - [Implementation](#implementation)

## Overview
***
For your virtual machines (VMs) to connect over a network to your host, to other VMs on your host, and to locations on an external network, the VM networking must be configured accordingly.

Review following articles for detailed infomation on KVM Networks:
- [RHEL 8 KVM Networks](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html/configuring_and_managing_virtualization/configuring-virtual-machine-network-connections_configuring-and-managing-virtualization)

## Purpose
Using `kvm-networks` ansible role, you can create/delete/update kvm networks on hypervisor. You need root priviledges to manage kvm networks.

## Requirements

- ansible v2.9+
- Ansible Collections:
  - community.libvirt

## Install Requirements

1. Ansible Collections:

```bash
ansible-galaxy collection install community.libvirt
```

## Implementation
By default `kvm-networks` create virtual network on KVM hypervisor, if you want to remove any existing networking overwrite the behaviour using extra_vars `-e operation=create`.

Operation on all networks is restricted. You need to perform specific operation on specific network at a time.

1. Review [Default Vars](./defaults/main.yml) before running the playbook. Overwrite default vars using extra_vars `-e @vars_file.yml`

`kvm_networks` var store all network you want to manage.

2. Required extra vars
```yaml
openration
net_name
cleanup(optional)
```

3. Create a playbook kvm-networks.yml
```yaml
---
- hosts: localhost
  gather_facts: true
  become: true

  roles:
  - role: kvm-networks
```

3. Run Playbook
```
ansible-playbook kvm-networks.yml -e operation=create -e net_name=br-vxlan
```