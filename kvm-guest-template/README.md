# Create KVM Guest Template
 
## Overview
Create a KVM guest given of your choice using a cloud image or convert an existing KVM guest to a template.


**Supported KVM guests:**
- ubuntu2204(Ubuntu 22.04 LTS)

In [default vars](./defaults/main.yml), `supported_images` contains supported KVM guest

## OS Tweaks
[OS-tuning](../os-tuning/README.md) ansible role is used to make OS configuration related changes.

The following params affects the OS behavior defined in [OS tuning Default vars](../os-tuning/defaults/main.yml).

```yaml
grub_default_param: "quiet splash console=tty0 console=ttyS0,115200 ipv6.disable=1 net.ifnames=0 biosdevname=0"
grub_param: "console=tty0 console=ttyS0,115200 ipv6.disable=1 net.ifnames=0 biosdevname=0"
```

## Create Guest Template Using an Qcow2 Image
Review [From-An-Image](./From-An-Image.md)

## Convert existing KVM Machine to KV Guest Template
Review [From-An-Existing-VM](./From-An-Existing-VM.md)