JFrog Artifactory OSS Ansible Role
==================================

The simpliest Ansible role for JFrog Artifactory OSS 7 that works on Ubuntu 19.10 hosts with init.d.

Role includes all *key Artifactory config files* in [templates/](templates/) so you can widely customize your Artifactory installation.


Role variables
--------------

Variables that could be customized described in [defaults/main.yml](defaults/main.yml).


Example _requirements.yml_ to add this role to your playbook
------------------------------------------------------------
```yml
- src: eugene_krivosheyev.artifactory
```


Example playbook
----------------
```yml
- hosts: ci_server
  roles:
    - role: eugene_krivosheyev.artifactory
      artifactory_port: 8081
      artifactory_ui_port: 8082
      artifactory_username: admin
      artifactory_password: P@ssw0rd
```


License
-------

BSD
