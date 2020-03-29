TESTING ROLE
============

INSTALL ROLE UPDATE FOR TESTING
-------------------------------
```bash
git commit
ansible-galaxy install -r tests/requirements.yml --force
```

START DOCKER TEST ENVIRONMENT
-----------------------------
```bash
docker-compose --file tests/inventory-docker-compose.yml up --detach
```

RUN TEST ROLE DEPLOY
--------------------
Reset ssh key [if needed]:
```bash
ssh-keygen -R 127.0.0.1
```

Ansible connection smoke test [if needed]:
```bash
ansible -i tests/inventory.yml -m shell -a 'uname -a' all
```

Run test role applying:
```bash
ansible-playbook -i tests/inventory.yml tests/test.yml --skip-tags not_for_docker_test_env
```

Ssh connect to test docker host [if needed]:
```bash
docker exec -it test_host /bin/bash
```

SHUTDOWN DOCKER TEST ENVIRONMENT
--------------------------------
```bash
docker-compose --file tests/inventory-docker-compose.yml down
```
