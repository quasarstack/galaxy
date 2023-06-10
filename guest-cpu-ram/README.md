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

2. Review variable files in `vars` directory for variables input example

3. Create a playbook
```yaml

---
# Check files in guest-cpu-ram/vars detailed info on vars
- hosts: kvm_host
  gather_facts: true
  become: true

  vars:
    kvm_guest_machines:
      - name: ubuntu2204
        cpu: 1
        memory: 1024 # in KB
  
  tasks:
  - name: Include role guest-cpu-ram
    ansible.builtin.include_role:
      name: guest-cpu-ram
```

4. Run playbook
```bash
$ ansible-playbook -i <kvm host inventory file> pb.yml
``` 
