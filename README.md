ansible-systemd-docker  [![Build Status](https://travis-ci.org/nfaction/ansible-systemd-docker.svg?branch=master)](https://travis-ci.org/nfaction/ansible-systemd-docker)
=========

Run `docker` containers as a service via `systemd`.

Requirements
------------

Any Linux OS running `systemd` as well as Ansible 1.9+

Role Variables
--------------

By default this role installs `nginx` as a `docker` container. You can set up as many containers as you wish by adding to the `docker_apps` subelement.

```
# Example Docker App. https://hub.docker.com/_/nginx/
docker_apps:
  - name: nginx
    description: Nginx
    container: nginx
    volumes:
      - { src: '/web', mount: '/usr/share/nginx/html', ro: true }
    ports:
      - { src: '80', dest: '8443' }
    extra_args:
      - 'PUID=0'
      - 'PGID=0'
```

Dependencies
------------

N/A

Example Playbook
----------------

Create variable file to override default `docker` containers. Make it look something like this:

```
# docker_apps.yml
---
# Docker App: https://hub.docker.com/_/httpd/
docker_apps:
  - name: apache
    description: Apache httpd
    container: httpd:2.4
    volumes:
      - { src: '/web', mount: '/usr/local/apache2/htdocs/', ro: true }
    ports:
      - { src: '80', dest: '8444' }
    extra_args:
      - 'PUID=0'
      - 'PGID=0'
```

Include the new `docker` apps to the `playbook`:

```
# install_docker_apps.yml
---
- hosts: all
  remote_user: root
  pre_tasks:
    - include_vars: docker_apps.yml
  roles:
    - ansible-systemd-docker
```

License
-------

GPL

Author Information
------------------

Matt DePorter
