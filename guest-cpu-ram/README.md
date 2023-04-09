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
1. Review [default vars](./defaults/main.yml) before running the this role. You can overwrite the vars using below playbook example.
   
2. Create a playbook
```yaml
---
- hosts: localhost
  gather_facts: true
  become: true

  vars:
    kvm_deployment:
      net_list: # This var is not used this role
        - br-mgmt
        - br-vxlan 
      virtual_machines:
        - name: ubuntu2204
          cpu: 1
          memory: 1024 # in KB
  roles:
  - role: guest-cpu-ram

```

2. Run playbook
```bash
ansible-playbook pb.yml
``` 