# stuttgart-things/manage-filesystem

handle repartitioning (LVM) with filesystem resizing support. For example, you can simply expand the LVM after enlarging the vhd. 
The role automatically detects the LVM on the system. The partitions and the device id are also recognized automatically.

**Note: You can easy check your partition number via fdisk or parted**

**Note: The enlargement of the file system is automatically calculated from the new free space**

**Note: This role requires become yes**

**Note: Logical volumes only ever grow.** `root` and `home` are sized with `lvm_*_sizing` as a share of the PVs (`%PVS`); if that share is smaller than or equal to the current LV size, the LV is left as it is (task reports `ok`). The filesystem grow (`xfs_growfs` / `resize2fs`) only runs when the LV was actually extended. `var` takes the rest of the VG (`+100%FREE`). Regression test: `tests/test-lvm-never-shrink.yml` (destructive, scratch host only).

<details><summary>REQUIREMENTS AND DEPENDENCIES</summary>

Server and client:
- Ubuntu 24.04
- Ubuntu 22.04
- Rocky 9

</details>

<details><summary>VARIABLES</summary>

1. requiered vars:
    - lvm_home_sizing: 10%
    - lvm_root_sizing: 60%
    - lvm_var_sizing: 30%

2. defaults: You don't need to change or add this variables.(Unless you want to overwrite it with other values)
    - part_sizing: 100%                 #Size claimed from the vhd by LVM
    - lv_home_name: home
    - lv_var_name: var
    - lv_root_name: root

</details>

<details><summary>ROLE INSTALLATION</summary>

```bash
cat <<EOF > /tmp/requirements.yaml
- src: https://github.com/stuttgart-things/manage-filesystem.git
  version: 2024.05.15
  scm: git
EOF

ansible-galaxy install -r /tmp/requirements.yaml --force
```

</details>

<details><summary>EXAMPLE INVENTORY</summary>

```bash
cat <<EOF > inventory
[appserver]
1.2.3.4 ansible_user=sthings
EOF
```

</details>

<details><summary>EXAMPLE PLAYBOOK</summary>

```yaml
cat <<EOF > manage-filesystem.yaml
---
- hosts: "{{ target_host | default('all') }}"
  gather_facts: true
  become: true
  vars:
    lvm_home_sizing: 10%
    lvm_root_sizing: 60%
    lvm_var_sizing: 30%

  roles:
    - manage-filesystem
EOF
```

</details>

<details><summary>EXAMPLE EXECUTION</summary>

```bash
ansible-playbook -i inventory manage-filesystem.yaml -vv 
```

</details>


## License
<details><summary>LICENSE</summary>

Copyright 2020 patrick hermann.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

</details>

Role history
----------------
| date  | who | changelog |
|---|---|---|
|2026-11-05  | Patrick Hermann | LVs never shrink: lvol `shrink: false`, no `force`, fs grow only when the LV changed (#6)
|2024-05-14  | Andre Ebert | Added Ansible-lint and Yamllint with skip rules and testing
|2020-29-09  | Marcel Zapf | Added logic to autodetect part id, device and vg name
|2020-16-09  | Marcel Zapf | Added logic to check partition is too small
|2020-10-08  | Marcel Zapf | First Release

Author Information
------------------

```yaml
Andre Ebert (andre.ebert@sva.de); 05/2024

Marcel Zapf; 08/2020; Stuttgart-Things
```
