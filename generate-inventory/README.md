# Generate Inventory

This role is used along with `set-static-ip` role. Check README.md of `set-static-ip` role.


## pb exmaple
```yaml
---
- hosts: localhost
  gather_facts: true
  become: true
  
  vars_files:
    - playbooks/vars/main.yml

  roles:
  - role: generate-inventory
```