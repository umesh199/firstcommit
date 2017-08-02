Role Name
=========

vSphereVM is a role to help an vSphere administrators to manage your virtual machines

Requirements
------------

[pyvmomi](https://pypi.python.org/pypi/pyvmomi/) is required

For install:

```shell
$ pip install --upgrade pyvmomi
```

Role Variables
--------------

Variables required to connect to vCenter is defined on vars/main.yml:

```yaml
vc_hostname: 192.168.6.10
vc_username: administrator@vsphere.local
vc_password: Secret123!

vm_template: miquel-template-W2012R2-Std-ES

dc_domain: ncoraformacion.local
dc_domainadmin: administrador@ncoraformacion.local
dc_domainadminpass: Secret123!

localadminpass: Secret123!
```

For security reasons is recomended encrypt this file with [ansible-vault](https://miquelmariano.github.io/2017/06/ansible-vault/)

Variables for each VMs to deploy, is defined in the end of file deploy-customize-win.yml

```yaml
##
##Insert down the variables for each VM to deploy
##
  with_items:
    - { vm_name: vm-demo, vm_ip: 10.0.0.100, vc_datacenter: VDC, vc_folder: /MIQUEL, vc_cluster: Cluster-EVC, vc_note: Created by Ansible, vm_datastore: FORM01_R5_LUN75, vm_networkportgroup: VLAN6_Formacion, vm_networkdns1: 192.168.6.100, vm_networkdns2: 192.168.6.101, vm_networkmask: 255.255.255.0, vm_networkgw: 192.168.6.1, vm_memory: 4096, vm_cpu: 4, vm_disksize: 20 }
 

```

Dependencies
------------

No dependencies

Example Playbook
----------------

```yaml
---
###vSphereVM.yml

- hosts: ansible
  user: root
  tasks:
     - name: Ensure that role are up to date
       command: ansible-galaxy install --force {{ item }}
       with_items:
          - miquelMariano.vSphereVM
       when:
          - update_mode | default(False)
       tags: update
       ignore_errors: yes

- hosts: ansible
  user: root
  roles:
     - role: miquelMariano.vSphereVM
```

Execute playbook
----------------

```ssh
ansible-playbook playbooks/vSphereVM.yml -i inventory/servers -e "update_mode=true" --tags=update
```

License
-------

BSD

Author Information
------------------

[miquelMariano.github.io](https://miquelMariano.github.io) | [Twitter](https://twitter.com/miquelMariano)
