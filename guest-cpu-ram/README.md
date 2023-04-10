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

  vars_files:
    - playbooks/vars/main.yml
  
  roles:
  - role: guest-cpu-ram

```

2. Run playbook
```bash
$ ansible-playbook pb.yml

# To force clone -> This will remove existing VM
$ ansible-playbook pb.yml -e force_clone=true
``` 
