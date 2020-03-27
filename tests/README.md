TESTING ROLE
============

INSTALL ROLE UPDATE FOR TESTING
-------------------------------
```bash
ansible-galaxy install -r requirements.yml
```

START DOCKER TEST ENVIRONMENT
-----------------------------
```bash
docker-compose --file tests/inventory-docker-compose.yml up --detach
```

RUN TEST ROLE DEPLOY
--------------------
```bash
ssh-keygen -R 127.0.0.1
ansible -i tests/inventory.yml -m shell -a 'uname -a' all
ansible-playbook -i tests/inventory.yml tests/test.yml --skip-tags not_for_docker_test_env
```

SHUTDOWN DOCKER TEST ENVIRONMENT
--------------------------------
```bash
docker-compose --file tests/inventory-docker-compose.yml down
```
