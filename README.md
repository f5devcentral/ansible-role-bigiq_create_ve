# Ansible Role: bigiq_create_ve

Performs a series of steps needed to create a BIG-IP VE (Virtual Edition) from BIG-IQ in AWS, Azure or VMware.

This role is perfect to use along with [F5 automation tool chain (ATC) deploy declaration](https://galaxy.ansible.com/f5devcentral/atc_deploy) galaxy role used 
to onboard the VE using DO with BIG-IQ.

# Prerequisites

- *Cloud Provider* and *Cloud Environment* need to be created on BIG-IQ, more details:
  - [AWS](https://techdocs.f5.com/en-us/bigiq-7-1-0/add-configure-big-ip-ve-in-aws-cloud.html)
  - [Azure](https://techdocs.f5.com/en-us/bigiq-7-1-0/add-configure-big-ip-ve-in-azure-cloud.html)
  - [VMware](https://techdocs.f5.com/en-us/bigiq-7-1-0/add-configure-big-ip-ve-in-vmware-environment.html)

## Role Variables

Available variables are listed below. For their default values, see `defaults/main.yml`.

Establishes initial connection to your BIG-IQ. These values are substituted into
your ``provider`` module parameter. These values should be the connection parameters
for the **CM BIG-IQ** device.

        provider:
          user: admin
          server: 10.1.1.4
          server_port: 443
          password: secret
          auth_provider: tmos
          validate_certs: false

Define where you want to create your VE based on the ``cloud_environment`` created in BIG-IQ.

    task_name: "Ansible Task"

    cloud_environment: "Cloud Environment Name in BIG-IQ"
    ve_name: bigipvm

    ip_pool_name: management-pool # Vmware only
    ip_pool_alias_name: management-alias-pool # Vmware only
    
    azure_admin_password: myPassword # Azure only

## Example Playbooks

**VMware**

    ---
    - hosts: all
      connection: local
      vars:
        provider:
          user: admin
          server: "{{ ansible_host }}"
          server_port: 443
          password: secret
          auth_provider: tmos
          validate_certs: false

      tasks:
          - name: Create a VE in VMware
            include_role:
              name: f5devcentral.bigiq_create_ve
            vars:
              cloud_environment: "VMware vSphere Data Center 1"
              ve_name: bigipvm-vmware
              ip_pool_name: management-pool
            register: status

          - name: Get VMware BIG-IP VE IP address (port 443)
            debug:
              msg: "{{ ve_ip_Address }}"

**AWS**

   ---
    - hosts: all
      connection: local
      vars:
        provider:
          user: admin
          server: "{{ ansible_host }}"
          server_port: 443
          password: secret
          auth_provider: tmos
          validate_certs: false

      tasks:
          - name: Create a VE in AWS
            include_role:
              name: f5devcentral.bigiq_create_ve
            vars:
              cloud_environment: "AWS US East N. Virginia"
              ve_name: bigipvm-aws
            register: status

          - name: Get AWS BIG-IP VE IP address (port 8443)
            debug:
              msg: "{{ ve_ip_Address }}"

          - name: Get AWS BIG-IP VE private Key Filename
            debug:
              msg: "{{ private_key_filename }}"

**Azure**

   ---
    - hosts: all
      connection: local
      vars:
        provider:
          user: admin
          server: "{{ ansible_host }}"
          server_port: 443
          password: secret
          auth_provider: tmos
          validate_certs: false

      tasks:
          - name: Create a VE in Azure 
            include_role:
              name: f5devcentral.bigiq_create_ve
            vars:
              cloud_environment: "Azure East US"
              ve_name: bigipvm-azure
              azure_admin_password: myPassword
            register: status

          - name: Get Azure BIG-IP VE IP address (port 8443)
            debug:
              msg: "{{ ve_ip_Address }}"


## License

Apache

## Author Information

This role was created in 2020 by [Romain Jouhannet](https://github.com/rjouhann).

[1]: https://galaxy.ansible.com/f5devcentral/bigiq_pinning_deploy_objects

