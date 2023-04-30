# Attach a Network to a KVM Guest
***
This role provides capability to create virtual machines using an qcow2 image.

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

2. `../playbooks/vars` contains variables files to create VM with customized configuration. Replace `vars_files` as per your requirement.

3. Optional -> To force clean-up of existing machines pass extra var `-e cleanup=true`   

4. Create a playbook
```yaml
---
- hosts: localhost
  gather_facts: true
  become: true
  
  vars_files:
    - playbooks/vars/main.yml

  roles:
  - role: create-kvm-machines
```

2. Run playbook
```bash
ansible-playbook pb.yml
```