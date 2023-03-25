# Galaxy Repo
***
- [Overview](#overview)
- [Create KVM Guest](#create-kvm-guest)
  - [Usage]()

## Overview
***
Each ansible role provides different capability and can be consumed in different projects.

## Create KVM Guest
***
Create a KVM guest given of your choice. Check `supported_images` in [default vars](./create-kvm-guest/defaults/main.yml).

Supported KVM guests:
- ubuntu2204(Ubuntu 22.04 LTS)

**Variables:**
- [image_type](./create-kvm-guest/defaults/main.yml)
- [user_data](./create-kvm-guest/defaults/main.yml)

Review [default vars](./create-kvm-guest/defaults/main.yml) before running the role and alter the behavior of the role by overwriting ansible variables using --extra_vars.

### Playbook example to create KVM guest

- Create a playbook `kvm-guest-template.yml`
```yaml
---
- hosts: localhost
  gather_facts: true
  become: yes
  vars:
    guest_os: ubuntu2204
  roles:
  - role: create-kvm-guest
```

- Run Playbook
```bash
ansible-playbook kvm-guest-template.yml
```
