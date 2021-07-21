nfs
===

Ansible role to configure [NFS server exports](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html/managing_file_systems/exporting-nfs-shares_managing-file-systems). It uses [lineinfile](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/lineinfile_module.html) instead of [template](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/template_module.html) so manually edited entries are not overwritten or modified.

Requirements
------------

* [ansible.builtin](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/index.html)
* [ansible.posix](https://docs.ansible.com/ansible/latest/collections/ansible/posix/index.html)
* [community.general](https://docs.ansible.com/ansible/latest/collections/community/general/)

Role Variables
--------------

* defaults

  ```yaml
  nfs_firewalld: {}     # firewalld settings

  nfs_selinux_fcontext: public_content_t  
                        # SELinux fcontext for all exported directories

  nfs_selinux_bools:    # SELinux booleans
    nfs_export_all_ro: true
    nfs_export_all_rw: true
    virt_use_nfs: true

  nfs_exports: []          # list of exports entries
    - path:                # path to export
      state:               # add or remove export entry
      options:
        - clients: []      # list of clients
          permissions: []  # list of export permissions
  ```

* vars

  ```yaml
  nfs_pkgs:
    - name: []          # list of nfs server software packages to install

  nfs_exports_file:     # exports file attributes
  ```

Dependencies
------------

* [service](https://github.com/mario-slowinski/service)
  * [software](https://github.com/mario-slowinski/software)

Tags
----

* nfs.export - *Manage NFS exports*
* nfs.selinux - *Manage SELinux for NFS server*
  * nfs.selinux.sebool - *Enable nfs SELinux properties*
  * nfs.selinux.fcontext - *Set SELinux fcontext on exports*

Example Playbook
----------------

* `requirements.yml`

  ```yaml
  - name: nfs
    src: https://github.com/mario-slowinski/nfs
  ```

* playbook usage

  ```yaml
  - hosts: servers
    gather_facts: true  # to get ansible_os_family
    roles:
      - role: nfs
  ```

License
-------

[GPL-3.0](https://www.gnu.org/licenses/gpl-3.0.html)

Author Information
------------------

[mario.slowinski@gmail.com](mailto:mario.slowinski@gmail.com)
