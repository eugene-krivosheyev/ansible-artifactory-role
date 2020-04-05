JFrog Artifactory OSS Ansible Role
==================================
The simpliest Ansible role for JFrog Artifactory OSS 7 that works on Ubuntu 19.10 hosts with init.d.

Role includes all *key Artifactory config files* in [templates/](https://github.com/eugene-krivosheyev/ansible-artifactory-role/tree/master/templates/) so you can widely customize your Artifactory installation.


Role requirements
-----------------
- _Unzip_. This role requires unzip package and installs it automatically by itself. If you don't need it, you can remove it in post_tasks section of your playbook.
- _Production Database_. Artifactory uses simple lightweigth database called Derby by default. If you don't plan to host high-load/fail-over solution with your Artifactory instance, Derby could be enough. If you plan to use production level database, you have to install and cofigure it yourself in your playbook. Already done Ansible Galaxy roles will help, e.g. geerlingguy.postgresql, geerlingguy.mysql, etc. After you describe database installation and configuration with your playbook, [it required to configure Artifactory](https://www.jfrog.com/confluence/display/JFROG/Configuring+the+Database) to work with your database. After you add driver downloading, creating database itself and database user to your playbook, you're required to configure Atrifactory role to work with your just configured database by [setting properties artifactory_database_*](https://github.com/eugene-krivosheyev/ansible-artifactory-role/blob/master/defaults/main.yml) to correct values. As an example you can take a look at the [test playbook with Postgres](https://github.com/eugene-krivosheyev/ansible-artifactory-role/blob/master/tests/test.yml).


Role variables
--------------
Variables that could be customized described in [defaults/main.yml](https://github.com/eugene-krivosheyev/ansible-artifactory-role/blob/master/defaults/main.yml).


Example _requirements.yml_ to add this role to your playbook
------------------------------------------------------------
```yml
- eugene_krivosheyev.artifactory
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

Known issues
------------
If you deploy Artifacory with PostgreSQL, you may face issue with property *become_method* for role _geerlingguy.postgresql_ just like described in [test case](https://github.com/eugene-krivosheyev/ansible-artifactory-role/blob/master/tests/test.yml).

License
-------
BSD
