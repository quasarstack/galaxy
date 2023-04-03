# Attach a Network to a KVM Guest
***
This role provides capability to attach a kvm virtual network to an existing KVM guest.

Currently you can attach to one guest at a time.

## Required Extra vars
***
- **guest_name:** Name of a KVM guest
- **net_list:** List of networks you want to attach to KVM guest

## Implementation
***
Networks will be attached to VM in the specified order. If guest machine already has an interface attached to it from given net_list. Playbook will be stopped. Since interface order may impact OS/App functionality, so this will ensure we are attaching network in correct order.

Modify net_list and re-run playbook.

1. Create a playbook
```yaml
---
- hosts: localhost
  gather_facts: true
  become: true

  vars:
    net_list:
      - br-mgmt
      - br-vxlan
  roles:
  - role: attach-network-to-guest

```

2. Run playbook
```bash
ansible-playbook pb.yml -e guest_name=u2204-dh
```

**Note:** You can overwrite `net_list` using `-e '{"net_list":[br-mgmt, br-vxlan,]}'`