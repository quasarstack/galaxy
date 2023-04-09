# Attach a Network to a KVM Guest
***
This role provides capability to create clones of a given template

## Required Ansible Collections
***
- community.libvirt.virt

**Install Connections**
```bash
ansible-galaxy collection install community.libvirt
```

## Implementation
***

1. Review [default vars](./defaults/main.yml) before running the this role. You can overwrite the vars using below playbook example.
   
2. Create a playbook
```yaml
---
- hosts: localhost
  gather_facts: true
  become: true

  vars:
    kvm_deployment:
      template: ubuntu2204-vm-template
      net_list:
        - br-mgmt
        - br-vxlan 
      virtual_machines:
        - name: ubuntu2204
          networks: # Update below list to attach interface of the networks
            - br-mgmt
            - br-vxlan
  roles:
  - role: create-vm-clones

```

2. Run playbook
```bash
ansible-playbook pb.yml
```