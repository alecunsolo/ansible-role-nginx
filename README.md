[![build status](https://github.com/alecunsolo/ansible-role-nginx/actions/workflows/ci.yml/badge.svg)](https://github.com/alecunsolo/ansible-role-nginx/actions/workflows/ci.yml)
[![pre-commit](https://img.shields.io/badge/pre--commit-enabled-brightgreen?logo=pre-commit)](https://github.com/pre-commit/pre-commit)

Ansible Role: nginx
=========

## DISCLAIMER
After [this](https://www.redhat.com/en/blog/furthering-evolution-centos-stream) announcement I will not test on RHEL anymore.

---------
Ansible role to install and configure nginx. Site definitions are stored in the `/etc/nginx/conf.d` directory and can be managed by this role.

Requirements
------------

None.

Role Variables
--------------
```yaml
nginx_main_config: nginx.{{ ansible_os_family | lower }}.conf
```
The name of the main configuration template file.
```yaml
nginx_static_files:
  - src: proxy_params
    dest: proxy_params
```
List of common files to deploy (dest directory partent is `/etc/nginx`)
```yaml
nginx_manage_selinux: true
```
Whether to enable `httpd_can_network_connect` on `SELinux` enabled systems.
```yaml
nginx_exclusive_sites: true
```
Whether the role should take full controll of the configured sites (deleting undefinded files) or not.
```yaml
nginx_configured_sites: []
# - src: mysite.conf.j2
#   dest: conf.d/mysite.conf
```
List of site definition to be deployed. Any site not listed here will be deleted (minus the default rejection one, if the option is enabled).

Dependencies
------------

The role also need `ansible.posix` for SELinux related operations.

Example Playbook
----------------

```yaml
- name: Converge
  hosts: all
  vars:
    nginx_configured_sites:
      - src: mysite.conf.j2
        dest: conf.d/mysite.conf
  tasks:
    - name: Include nginx.
      ansible.builtin.include_role:
        name: alecunsolo.nginx
```

License
-------

MIT

Notes
-----

Testing with molecule is ~~stolen from~~ heavily inspired by [Jeff Geerling](https://www.jeffgeerling.com/). Watch [his video](https://youtu.be/FaXVZ60o8L8) (and the other ones as well).
