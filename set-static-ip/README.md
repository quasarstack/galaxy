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

## Implementation
***
1. Review [default vars](./defaults/main.yml) before running the this role. You can overwrite the vars using below playbook example.

2. `kvm_guest_user` and `kvm_guest_password` must be exported before running playbook. 

3. Run Playboook

```bash
ansible-playbook playbooks/static-ip-config.yml
```