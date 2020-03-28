JFrog Artifactory Ansible Role
==============================

The simpliest Ansible role for JFrog Artifactory 7 that works on Ubuntu 19.10 hosts with init.d.

Role includes all key Artifactory config files in [templates/](templates/) so you can widely customize your Artifactory installation.


Role Variables
--------------

Variables that could be customized described in [defaults/main.yml](defaults/main.yml).


Example _requirements.yml_
--------------------------
```yml
- src: https://github.com/eugene-krivosheyev/ansible-artifactory-role
  scm: git
  name: ekr.artifactory
  version: master
```


Example Playbook
----------------
```yml
- hosts: servers
  roles:
    - role: ekr.artifactory
      artifactory_port: 8081
      artifactory_ui_port: 8082
      artifactory_username: admin
      artifactory_password: P@ssw0rd
```


License
-------

BSD
