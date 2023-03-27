# Galaxy Repo
***
- [Overview](#overview)
- [Create KVM Guest](#create-kvm-guest)
  - [Reinstall Grub in Image](#reinstall-grub-in-image)
- [Tweak KVM Guest](#tweak-kvm-guest)
  - [Enable Serial Console](#enable-serial-console)

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
    - Disk Size (default 20G, change based on your requirment)
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

### Implementation

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

**NOTE:** 
  - Playbook will pause for 30 minutes and ask to reinstall grub in the image.
  - Follow [Reinstall Grub in Image](#reinstall-grub-in-image) to reinstall grub in image
  - Post reinstall grub, continue playbook with Ctrl + C , and C

### Reinstall Grub in Image
```bash
cd /var/lib/libvirtd/images

virt-rescue "Guest VM Disk"

mount /dev/sda3 /mnt
mount --bind /dev /mnt/dev
mount --bind /proc /mnt/proc
mount --bind /sys /mnt/sys
chroot /mnt
grub-install /dev/sda

Ctrl + d -> to exit from rescue mode
```

## Tweak KVM Guest
***
### Enable Serial Console


