# Ansible Role: manage-filesysten
This ansible role can handel repartitioning (LVM) with filesystem resizing support. For example, you can simply expand the LVM after enlarging the vhd.

## Example playbooks to use this role

<details><summary>Install this role on your ansible host</summary>

```
cat <<EOF > /tmp/requirements.yaml
- src: git@codehub.sva.de:Lab/stuttgart-things/ansible/manage-filesysten.git
  scm: git
EOF
ansible-galaxy install -r /tmp/requirements.yaml --force
```

</details>


<details><summary>Example playbook for an nfs client installation with mount</summary>

```
- hosts: "fs"
  gather_facts: true
  become: true
  vars:
    lvm_device: /dev/sda
    lvm_device_part_number: 3
    lvm_vg: vg0
    lvm_home_sizing: 10%
    lvm_root_sizing: 60%
    lvm_var_sizing: 30%

  roles:
    - manage-filesystem
```

**Note: This role requires become yes**
</details>

<details><summary>Example inventory</summary>

```
[fs]
foo.bar.example.com ansible_user=foobar
```
</details>

<details><summary>Execute playbook</summary>

```
ansible-playbook -i inventory manage-filesystem.yml
```
</details>


## Role TODOs
- Full list visible at https://codehub.sva.de/Lab/stuttgart-things/ansible/manage-filesystem/-/issues



## Variables

This role need variables

1. requiered vars:
    - lvm_device: /dev/sda 
    - lvm_device_part_number: 3
    - lvm_vg: vg0                       #LVM volume group name
    - lvm_home_sizing: 10%
    - lvm_root_sizing: 60%
    - lvm_var_sizing: 30%

2. defaults: You don't need to change or add this variables.
    - part_sizing: 100%                 #Size claimed from the vhd by LVM
    - lv_home_name: home
    - lv_var_name: var
    - lv_root_name: root

**Note: You can easy check your partition number via fdisk or parted**

**Note: The enlargement of the file system is automatically calculated from the new free space**
    
## Requirements and Dependencies:
Server and client:
- Ubuntu 20.04
- Ubuntu 18.04
- CentOS 8
- CentOS 7

## Version:
```
DATE         WHO       		  WHAT
20200810     Marcel Zapf  	  First Release
```

License
-------

BSD

Author Information
------------------

Marcel Zapf; 08/2020; SVA GmbH