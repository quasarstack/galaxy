# Set Static IP on KVM VMs
***
This role provides capability to set static ips in KVM vms.

## Required Ansible Collections
***
- community.libvirt.virt

**Install Connections**
```bash
ansible-galaxy collection install community.libvirt
```

## Pre-requisites
- Machines must be using dhcp IP

## Implementation
***
1. Review [default vars](./defaults/main.yml) before running the this role. You can overwrite the vars using below playbook example.

2. By default, playbooks reads `kvm_deployment` var from `playbooks/vars/main.yml`. Overwrite this var by providing custom var file using `-e @<full_path_of_var_file>`.
   
3. Provide KVM VMs user and password using extra vars `-e kvm_guest_user=` and `-e kvm_guest_password=`.

4. Run Playbook

```bash
ansible-playbook playbooks/static-ip-config.yml -e kvm_guest_user=xxx -e kvm_guest_password=yyy
```