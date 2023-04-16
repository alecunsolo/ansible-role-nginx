[![build status](https://github.com/alecunsolo/ansible-role-nginx/actions/workflows/ci.yml/badge.svg)](https://github.com/alecunsolo/ansible-role-nginx/actions/workflows/ci.yml)
[![pre-commit](https://img.shields.io/badge/pre--commit-enabled-brightgreen?logo=pre-commit)](https://github.com/pre-commit/pre-commit)

Ansible Role: nginx
=========

Ansible role to install and configure nginx. Site definitions are stored in the `/etc/conf.d` directory and can be managed by this role.

Requirements
------------

The role depends on `community.crypto` to generate self-signed certifiate used to block direct ip access when the `ssl_reject_handshake` directive is not available (ver < 1.19.4).
The module used depends on the python `cryptography` library.

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
nginx_configure_default_block: true
nginx_default_reject_site: default-reject.conf.j2
```
Wheter to configure of not the default rejecting behaviour and the template to use.
```yaml
nginx_configured_sites: []
# - src: mysite.conf.j2
#   dest: conf.d/mysite.conf
```
List of site definition to be deployed. Any site not listed here will be deleted (minus the default rejection one, if the option is enabled).

Dependencies
------------

The role depends on `community.crypto` for the self-signed certificate generation.

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
