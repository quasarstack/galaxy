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
    - Set image source
    - Version
    - Destination directory
    - Guest Template Name
    - CPU
    - Memory
- [user_data](./create-kvm-guest/defaults/main.yml)
    Default [user-data](./create-kvm-guest/files/user-data) file. Cloud-init file to set:
    - Hostname
    - User and password
    - SSH config
    - SSH Auth Keys

- [image_download](./create-kvm-guest/defaults/main.yml)
    - Default is False. Image download will be skipped if already exists
    - Using `-e image_download=true` to force image download

Review [default vars](./create-kvm-guest/defaults/main.yml) before running the role and alter the behavior of the role by overwriting ansible variables using -e.

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

- Run Playbook with custom user_data
```bash
ansible-playbook kvm-guest-template.yml -e user_data='full path of user-data file on KVM host'
```
