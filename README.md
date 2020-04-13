JFrog Artifactory OSS Ansible Role
==================================
The simpliest Ansible role for JFrog Artifactory OSS 7 that works on Ubuntu 19.10 hosts with init.d.

Role includes all *key Artifactory config files* in [templates/](https://github.com/eugene-krivosheyev/ansible-artifactory-role/tree/master/templates/) so you can widely customize your Artifactory installation.


Role requirements
-----------------
- _Unzip_. This role requires unzip package and installs it automatically by itself. If you don't need it, you can remove it in post_tasks section of your playbook.
- _Production Database_. Artifactory uses simple lightweigth database called Derby by default. If you don't plan to host high-load/fail-over solution with your Artifactory instance, Derby could be enough. If you plan to use production level database, you have to install and cofigure it yourself in your playbook. Already done Ansible Galaxy roles will help, e.g. geerlingguy.postgresql, geerlingguy.mysql, etc. After you describe database installation and configuration with your playbook, [it required to configure Artifactory](https://www.jfrog.com/confluence/display/JFROG/Configuring+the+Database) to work with your database. After you add driver downloading, creating database itself and database user to your playbook, you're required to configure Atrifactory role to work with your just configured database by [setting properties artifactory_database_*](https://github.com/eugene-krivosheyev/ansible-artifactory-role/blob/master/defaults/main.yml) to correct values. As an example you can take a look at the [test playbook with Postgres](https://github.com/eugene-krivosheyev/ansible-artifactory-role/blob/master/tests/test.yml).


[Role variables](https://github.com/eugene-krivosheyev/ansible-artifactory-role/blob/master/defaults/main.yml)
----------------
Variable 'ansible_become_method' is used to generate service script. It have default value 'sudo' so if your target host don't have sudo command you have to explicitly set it to 'su'. Because it depends on target hosts linux distr better set it on per-host basis in your inventory file. See [example](https://github.com/eugene-krivosheyev/ansible-artifactory-role/blob/master/tests/inventory.yml) of setting 'ansible_become_method' variable for inventory hosts.

Other variables that could be customized described in [defaults/main.yml](https://github.com/eugene-krivosheyev/ansible-artifactory-role/blob/master/defaults/main.yml).


[Example _requirements.yml_](https://github.com/eugene-krivosheyev/ansible-artifactory-role/blob/master/tests/requirements.yml) to add this role to your playbook
------------------------------------------------------------
```yml
- eugene_krivosheyev.artifactory
```


[Example playbook](https://github.com/eugene-krivosheyev/ansible-artifactory-role/blob/master/tests/test.yml)
------------------
```yml
- hosts: ci_server
  roles:
    - role: eugene_krivosheyev.artifactory
      artifactory_port: 8081
      artifactory_ui_port: 8082
      artifactory_username: admin
      artifactory_password: P@ssw0rd
```
Also you can refer [test role usage](https://github.com/eugene-krivosheyev/ansible-artifactory-role/blob/master/tests/test.yml) for example.


Known issues
============
1. You may face issue if target host don't have command 'sudo'
-------------------------------------------------------------- 
In this case just set default priviledge escalation command to 'su'. Because it depends on target hosts linux distr better set it on per-host basis in your inventory file. See [example](https://github.com/eugene-krivosheyev/ansible-artifactory-role/blob/master/tests/inventory.yml) of setting 'ansible_become_method' variable for inventory hosts.

2. You may face issue with 'su' command timeouts if it set as default privilege escalation command for some hosts
-----------------------------------------------------------------------------------------------------------------
```bash
TASK [geerlingguy.postgresql : Ensure PostgreSQL Python libraries are installed.] ********
fatal: [test_host]: FAILED! => {"msg": "Timeout (12s) waiting for privilege escalation prompt: "}
```
In this case set default priviledge escalation command to 'sudo' with 'ansible_become_method' variable explicitely. Better set it on per-host basis in your inventory file. 
Or just skip setting 'ansible_become_method' at all because of its default is 'sudo' already. 

3. Artifactory tries to start but then get permanent error at web interface
---------------------------------------------------------------------------
```json
{
  "errors" : [ {
    "status" : 500,
    "message" : "Artifactory failed to initialize: check Artifactory logs for errors."
  } ]
}
```
And in logs (e.g. */opt/jfrog/artifactory/var/log/console.log*) you have
```log
.
Could not validate router Check-url: http://::1:8082/router/api/v1/system/ping
.
.
.
Registration with router on URL http://localhost:8046 failed with error: UNAVAILABLE: io exception.
Registration with router on URL http://localhost:8046 failed with error: UNAVAILABLE: io exception.
Registration with router on URL http://localhost:8046 failed with error: UNAVAILABLE: io exception.
.
```
This means that one of Artifactory component uses 'localhost' landed to IPv6's "localhost" called '::1'.

To fix you can try [force Java to use IPv4](https://superuser.com/questions/453298/how-to-force-java-to-use-ipv4-instead-ipv6) or 
manually remove binding of IPv6 from 'localhost' at your */etc/hosts*:
```/etc/hosts
127.0.0.1       localhost   
::1             localhost  # <-- Remove any type of such binding "::1 -> localhost" at any string 
::1             ip6-localhost ip6-loopback
ff02::1         ip6-allnodes
ff02::2         ip6-allrouters
```
In case of reproducing issue just [turn IPv6 off](https://www.techrepublic.com/article/how-to-disable-ipv6-through-grub-in-linux/).

License
=======
BSD
