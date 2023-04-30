# Convert an existing KVM VM to template

Along with configuring OS, we are using [virt-sysprep](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/virtualization_deployment_and_administration_guide/sect-guest_virtual_machine_disk_access_with_offline_tools-using_virt_sysprep) utility to convert an existing KVM VM to template.

After following the instructions, you will have an customized image that you can use to create new VMs.

**Requirements:**
- KVM VM must be using dhcp IP
    - Run `virsh domifaddr <vm_name>` on KVM host to validate

## Implementation
1. Create an inventory file for KVM VM, example: /tmp/kvm-vm

```ini
192.168.10.236

[all:vars]
ansible_user=ubuntu
ansible_password=redhat
ansible_become_password=redhat
```
 
2. Test Connectivity using ansible
```bash
$ ansible all -i /tmp/kvm-vm -m ping
```

3. Optional -> Overwrite Variables

Create new variable file and provide input for following params to overwrite default values

```yaml
kvm_vm: ubuntu2204-vm # VM that you want to use to create an image
image_dirs: /var/lib/libvirtd/images
customized_image: "{{image_dirs}}/ubuntu2202-template-image.qcow2"
grub_default_param: "quiet splash console=tty0 console=ttyS0,115200 ipv6.disable=1 net.ifnames=0 biosdevname=0"
grub_param: "console=tty0 console=ttyS0,115200 ipv6.disable=1 net.ifnames=0 biosdevname=0"

```

4. Run playbook to convert existing KVM VM to template and provide image
If you need to overwrite default variables as instructed in step.3, pass vars file using `-e @<vars_file_path>`

```bash
$ ansible-playbook -i /tmp/kvm-vm playbooks/kvm-template/kvm-guest-template-using-existing-vm.yml
```

**FIXME:**
- Playbook failed on below task:
```
Copy {{_vm_disk.stdout}} to {{customized_image}}
```


