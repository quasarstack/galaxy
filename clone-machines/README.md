# KVM Guest Machine Clones

Create and manage KVM virtual machines clones. Review [Default vars](./defaults/main.yml) before using this role.

## How it works
- It checks for `source_machine` on KVM host
- Checks for `target_machines` on KVM host and stops if a guest machine already available on KVM host
- Clone and create `target_machines`

## Implementation

- Setup inventory
- Create playbook as below

```yaml
- hosts: kvm_host
  gather_facts: true
  become: yes

  tasks:
  - name: include role clone-machines
    ansible.builtin.include_role:
      name: clone-machines
```

- Run playbook
```bash
ansible-playbook -i kvm-hosts pb.yml
```