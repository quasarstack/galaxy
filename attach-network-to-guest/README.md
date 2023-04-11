# Attach a Network to a KVM Guest
***
This role provides capability to attach a kvm virtual network to an existing KVM guest.

## Required Ansible Collections
***
- community.libvirt.virt

**Install Connections**
```bash
ansible-galaxy collection install community.libvirt
```

## Implementation
***
Networks will be attached to VM in the specified order. If guest machine already has an interface attached to it from given net_list. Playbook will be stopped. Since interface order may impact OS/App functionality, so this will ensure we are attaching network in correct order.

1. Review [default vars](./defaults/main.yml) before running the this role. You can overwrite the vars using below playbook example.
   
2. Create a playbook
```yaml
---
- hosts: localhost
  gather_facts: true
  become: true

  vars_files:
    - playbooks/vars/main.yml
  
  roles:
  - role: attach-network-to-guest

```

2. Run playbook
```bash
ansible-playbook pb.yml
```