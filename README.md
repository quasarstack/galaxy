# Galaxy Repo
***
- [Overview](#overview)
- [Create KVM Guest Template](#create-kvm-guest-template)
  - [Reinstall Grub in Image](#reinstall-grub-in-image)
- [Create KVM Guest with Custom Config](#create-kvm-guest-with-custom-config)
- [Tweak KVM Guest](#tweak-kvm-guest)
  - [Enable Serial Console](#enable-serial-console)
## Overview
***
Each ansible role provides different capability and can be consumed in different projects.

## Create KVM Guest Template
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
```

- Successful grub reinstall msg would be like below:
```
Installing for i386-pc platform.
Installation finished. No error reported.
```

- Ctrl + d -> Twrice to exit from rescue mode


## Create KVM Guest with Custom Config
***
- Create a yaml with and define guest VM config and pass to playbook using `custom_guest_input` extra var.

```yaml
$ cat /tmp/os-ctrl1.yml
guest_os: ubuntu2204
image_type:
  ubuntu2204:
    source: "https://cloud-images.ubuntu.com/jammy/current/jammy-server-cloudimg-amd64.img"
    image: jammy-server-cloudimg-amd64.img
    user: root
    password: redhat
    version: '22.04'
    local_path: /var/lib/libvirt/images
    os_variant: 'ubuntu22.04'
    template:
      name: openstack-ctrl1
      network: default
      cpu: 2
      memory: 4096
      disk: openstack-ctrl1.img
      size: 20G
```
- If you wish to set custom username/password, create your own cloudinit file and pass to playbook using `user_data` extra var. Default [cloudinit](./create-kvm-guest/files/cloud-init/user-data.yml)

**Run Playbook with custom user_data**
```bash
ansible-playbook kvm-guest-template.yml -e custom_guest_input=/tmp/os-ctrl1.yml [-e user_data=/tmp/user-data.yml]
```

## Tweak KVM Guest
***
### Enable Serial Console


