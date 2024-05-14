# stuttgart-things/manage-filesystem

handle repartitioning (LVM) with filesystem resizing support. For example, you can simply expand the LVM after enlarging the vhd. 
The role automatically detects the LVM on the system. The partitions and the device id are also recognized automatically.

**Note: You can easy check your partition number via fdisk or parted**

**Note: The enlargement of the file system is automatically calculated from the new free space**

**Note: This role requires become yes**

<details><summary>REQUIREMENTS AND DEPENDENCIES</summary>

Server and client:
- Ubuntu 20.04
- Ubuntu 18.04
- CentOS 8
- CentOS 7

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
- src: https://github.com/stuttgart-things/install-configure-podman.git
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
