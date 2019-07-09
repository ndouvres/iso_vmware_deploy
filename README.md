# iso_vmware_provisioner role


## Role Variables

A list of all the default variables for this role is available in `defaults/main.yml`. The role setups the following facts:

- vmware_provisioner_vms_basic_facts: gathered virtual machines basic facts
- vmware_provisioner_vms_detailed_facts: gathered virtual machines detailed facts
- vmware_provisioner_inventory_vms: virtual machines configs found in the inventory

## Example Playbook

This is an example playbook:

```yaml
---

- hosts: all
  roles:
    - role: iso_vmware_deploy
      vmware_provisioner_hostname: vcenter.iso.com
      vmware_provisioner_username: username
      vmware_provisioner_password: password
      vmware_provisioner_validate_certs: no
      vmware_provisioner_testing_vms:
        - name: "iso_vm"
          annotation: Ansible provisioned vm
          folder: /
          guest_id: ol8_64Guest
          hardware:
            memory_mb: 4096
            num_cpus: 2
            num_cpu_cores_per_socket: 1
          datacenter: iso
          cluster: name_of_cluster
          disk:
            - size_gb: 60
              type: thin
              datastore: name_of_datastore            
          wait_for_ip_address: no          
          force: yes
```

## Testing

To run test you must point the variable `vmware_provisioner_tests_host` to a host that can be managed with ansible and that has access to an existing vCenter/ESXi. provide the following minimim role variables (see `defaults/main.file` for details):

- `vmware_provisioner_hostname`
- `vmware_provisioner_username`
- `vmware_provisioner_password`
- `vmware_provisioner_vm_datacenter`
- `vmware_provisioner_vm_cluster`
- `vmware_provisioner_vm_disk`

One way to provide this information is calling the testing playbook passing the host to use and an additional vault inventory plus the default one provided for testing, as it's show in this example:

```shell
$ cd iso_vmware_deploy/tests
$ ansible-playbook main.yml -e "vmware_provisioner_tests_host=test_host" -i inventory -i ~/mycustominventory.yml --vault-id myvault@prompt
```

