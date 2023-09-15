# Домашнее задание к занятию 5 «Тестирование roles»»

## Molecule

Добавляю для проверки несколько дистрибутивов:
```bash
ivan@ubuntutest$ cat  molecule/default/molecule.yml
---
dependency:
  name: galaxy
driver:
  name: docker
lint: |
  ansible-lint .
  yamllint .
platforms:
  - name: centos7
    image: docker.io/pycontribs/centos:7
    pre_build_image: true
  - name: centos8
    image: docker.io/pycontribs/centos:8
    pre_build_image: true
  - name: ubuntu
    image: docker.io/pycontribs/ubuntu:latest
    pre_build_image: true
provisioner:
  name: ansible
verifier:
  name: ansible
```

Выполняю команду `ivan@ubuntutest$ molecule test`

В результате выполняется тест, но с ошибками, как их исправить я не могу понять.

```bash
INFO     default scenario test matrix: dependency, lint, cleanup, destroy, syntax, create, prepare, converge, idempotence, side_effect, verify, cleanup, destroy
INFO     Performing prerun...
INFO     Set ANSIBLE_LIBRARY=/home/ivan/.cache/ansible-compat/f5bcd7/modules:/home/ivan/.ansible/plugins/modules:/usr/share/ansible/plugins/modules
INFO     Set ANSIBLE_COLLECTIONS_PATH=/home/ivan/.cache/ansible-compat/f5bcd7/collections:/home/ivan/.ansible/collections:/usr/share/ansible/collections
INFO     Set ANSIBLE_ROLES_PATH=/home/ivan/.cache/ansible-compat/f5bcd7/roles:/home/ivan/.ansible/roles:/usr/share/ansible/roles:/etc/ansible/roles
INFO     Running default > dependency
WARNING  Skipping, missing the requirements file.
WARNING  Skipping, missing the requirements file.
INFO     Running default > lint
COMMAND: ansible-lint .
yamllint .

ERROR    Computed fully qualified role name of vector-role does not follow current galaxy requirements.
Please edit meta/main.yml and assure we can correctly determine full role name:

galaxy_info:
role_name: my_name  # if absent directory name hosting role is used instead
namespace: my_galaxy_namespace  # if absent, author is used instead

Namespace: https://galaxy.ansible.com/docs/contributing/namespaces.html#galaxy-namespace-limitations
Role: https://galaxy.ansible.com/docs/contributing/creating_role.html#role-names

As an alternative, you can add 'role-name' to either skip_list or warn_list.

Computed fully qualified role name of vector-role does not follow current galaxy requirements.
Please edit meta/main.yml and assure we can correctly determine full role name:

galaxy_info:
role_name: my_name  # if absent directory name hosting role is used instead
namespace: my_galaxy_namespace  # if absent, author is used instead

Namespace: https://galaxy.ansible.com/docs/contributing/namespaces.html#galaxy-namespace-limitations
Role: https://galaxy.ansible.com/docs/contributing/creating_role.html#role-names

As an alternative, you can add 'role-name' to either skip_list or warn_list.

INFO     Running default > cleanup
WARNING  Skipping, cleanup playbook not configured.
INFO     Running default > destroy
INFO     Sanity checks: 'docker'

PLAY [Destroy] *****************************************************************

TASK [Set async_dir for HOME env] **********************************************
ok: [localhost]

TASK [Destroy molecule instance(s)] ********************************************
changed: [localhost] => (item=centos7)
changed: [localhost] => (item=centos8)
changed: [localhost] => (item=ubuntu)

TASK [Wait for instance(s) deletion to complete] *******************************
ok: [localhost] => (item=centos7)
ok: [localhost] => (item=centos8)
ok: [localhost] => (item=ubuntu)

TASK [Delete docker networks(s)] ***********************************************
skipping: [localhost]

PLAY RECAP *********************************************************************
localhost                  : ok=3    changed=1    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0

INFO     Running default > syntax

playbook: /home/ivan/05/devops-netology-ansible/08-ansible-05-testing/playbook/roles/vector-role/molecule/default/converge.yml
INFO     Running default > create
[WARNING]: Collection ansible.posix does not support Ansible version 2.15.4

PLAY [Create] ******************************************************************

TASK [Set async_dir for HOME env] **********************************************
ok: [localhost]

TASK [Log into a Docker registry] **********************************************
skipping: [localhost] => (item=None)
skipping: [localhost] => (item=None)
skipping: [localhost] => (item=None)
skipping: [localhost]

TASK [Check presence of custom Dockerfiles] ************************************
ok: [localhost] => (item={'image': 'docker.io/pycontribs/centos:7', 'name': 'centos7', 'pre_build_image': True})
ok: [localhost] => (item={'image': 'docker.io/pycontribs/centos:8', 'name': 'centos8', 'pre_build_image': True})
ok: [localhost] => (item={'image': 'docker.io/pycontribs/ubuntu:latest', 'name': 'ubuntu', 'pre_build_image': True})

TASK [Create Dockerfiles from image names] *************************************
skipping: [localhost] => (item={'image': 'docker.io/pycontribs/centos:7', 'name': 'centos7', 'pre_build_image': True})
skipping: [localhost] => (item={'image': 'docker.io/pycontribs/centos:8', 'name': 'centos8', 'pre_build_image': True})
skipping: [localhost] => (item={'image': 'docker.io/pycontribs/ubuntu:latest', 'name': 'ubuntu', 'pre_build_image': True})
skipping: [localhost]

TASK [Synchronization the context] *********************************************
skipping: [localhost] => (item={'image': 'docker.io/pycontribs/centos:7', 'name': 'centos7', 'pre_build_image': True})
skipping: [localhost] => (item={'image': 'docker.io/pycontribs/centos:8', 'name': 'centos8', 'pre_build_image': True})
skipping: [localhost] => (item={'image': 'docker.io/pycontribs/ubuntu:latest', 'name': 'ubuntu', 'pre_build_image': True})
skipping: [localhost]

TASK [Discover local Docker images] ********************************************
ok: [localhost] => (item={'changed': False, 'skipped': True, 'skip_reason': 'Conditional result was False', 'false_condition': 'not item.pre_build_image | default(false)', 'item': {'image': 'docker.io/pycontribs/centos:7', 'name': 'centos7', 'pre_build_image': True}, 'ansible_loop_var': 'item', 'i': 0, 'ansible_index_var': 'i'})
ok: [localhost] => (item={'changed': False, 'skipped': True, 'skip_reason': 'Conditional result was False', 'false_condition': 'not item.pre_build_image | default(false)', 'item': {'image': 'docker.io/pycontribs/centos:8', 'name': 'centos8', 'pre_build_image': True}, 'ansible_loop_var': 'item', 'i': 1, 'ansible_index_var': 'i'})
ok: [localhost] => (item={'changed': False, 'skipped': True, 'skip_reason': 'Conditional result was False', 'false_condition': 'not item.pre_build_image | default(false)', 'item': {'image': 'docker.io/pycontribs/ubuntu:latest', 'name': 'ubuntu', 'pre_build_image': True}, 'ansible_loop_var': 'item', 'i': 2, 'ansible_index_var': 'i'})

TASK [Build an Ansible compatible image (new)] *********************************
skipping: [localhost] => (item=molecule_local/docker.io/pycontribs/centos:7)
skipping: [localhost] => (item=molecule_local/docker.io/pycontribs/centos:8)
skipping: [localhost] => (item=molecule_local/docker.io/pycontribs/ubuntu:latest)
skipping: [localhost]

TASK [Create docker network(s)] ************************************************
skipping: [localhost]

TASK [Determine the CMD directives] ********************************************
ok: [localhost] => (item={'image': 'docker.io/pycontribs/centos:7', 'name': 'centos7', 'pre_build_image': True})
ok: [localhost] => (item={'image': 'docker.io/pycontribs/centos:8', 'name': 'centos8', 'pre_build_image': True})
ok: [localhost] => (item={'image': 'docker.io/pycontribs/ubuntu:latest', 'name': 'ubuntu', 'pre_build_image': True})

TASK [Create molecule instance(s)] *********************************************
changed: [localhost] => (item=centos7)
changed: [localhost] => (item=centos8)
changed: [localhost] => (item=ubuntu)

TASK [Wait for instance(s) creation to complete] *******************************
failed: [localhost] (item={'failed': 0, 'started': 1, 'finished': 0, 'ansible_job_id': 'j671303280536.87953', 'results_file': '/home/ivan/.ansible_async/j671303280536.87953', 'changed': True, 'item': {'image': 'docker.io/pycontribs/centos:7', 'name': 'centos7', 'pre_build_image': True}, 'ansible_loop_var': 'item'}) => {"ansible_job_id": "j671303280536.87953", "ansible_loop_var": "item", "attempts": 1, "changed": false, "finished": 1, "item": {"ansible_job_id": "j671303280536.87953", "ansible_loop_var": "item", "changed": true, "failed": 0, "finished": 0, "item": {"image": "docker.io/pycontribs/centos:7", "name": "centos7", "pre_build_image": true}, "results_file": "/home/ivan/.ansible_async/j671303280536.87953", "started": 1}, "msg": "Unsupported parameters for (community.docker.docker_container) module: command_handling. Supported parameters include: api_version, auto_remove, blkio_weight, ca_cert, cap_drop, capabilities, cgroup_parent, cleanup, client_cert, client_key, command, comparisons, container_default_behavior, cpu_period, cpu_quota, cpu_shares, cpus, cpuset_cpus, cpuset_mems, debug, default_host_ip, detach, device_read_bps, device_read_iops, device_requests, device_write_bps, device_write_iops, devices, dns_opts, dns_search_domains, dns_servers, docker_host, domainname, entrypoint, env, env_file, etc_hosts, exposed_ports, force_kill, groups, healthcheck, hostname, ignore_image, image, init, interactive, ipc_mode, keep_volumes, kernel_memory, kill_signal, labels, links, log_driver, log_options, mac_address, memory, memory_reservation, memory_swap, memory_swappiness, mounts, name, network_mode, networks, networks_cli_compatible, oom_killer, oom_score_adj, output_logs, paused, pid_mode, pids_limit, privileged, published_ports, pull, purge_networks, read_only, recreate, removal_wait_timeout, restart, restart_policy, restart_retries, runtime, security_opts, shm_size, ssl_version, state, stop_signal, stop_timeout, sysctls, timeout, tls, tls_hostname, tmpfs, tty, ulimits, user, userns_mode, uts, validate_certs, volume_driver, volumes, volumes_from, working_dir (cacert_path, cert_path, docker_api_version, docker_url, expose, exposed, forcekill, key_path, log_opt, ports, tls_ca_cert, tls_client_cert, tls_client_key, tls_verify).", "results_file": "/home/ivan/.ansible_async/j671303280536.87953", "started": 1, "stderr": "/tmp/ansible_community.docker.docker_container_payload_mz5b6l4x/ansible_community.docker.docker_container_payload.zip/ansible_collections/community/docker/plugins/modules/docker_container.py:1193: DeprecationWarning: distutils Version classes are deprecated. Use packaging.version instead.\n", "stderr_lines": ["/tmp/ansible_community.docker.docker_container_payload_mz5b6l4x/ansible_community.docker.docker_container_payload.zip/ansible_collections/community/docker/plugins/modules/docker_container.py:1193: DeprecationWarning: distutils Version classes are deprecated. Use packaging.version instead."], "stdout": "", "stdout_lines": []}
failed: [localhost] (item={'failed': 0, 'started': 1, 'finished': 0, 'ansible_job_id': 'j774032615113.87979', 'results_file': '/home/ivan/.ansible_async/j774032615113.87979', 'changed': True, 'item': {'image': 'docker.io/pycontribs/centos:8', 'name': 'centos8', 'pre_build_image': True}, 'ansible_loop_var': 'item'}) => {"ansible_job_id": "j774032615113.87979", "ansible_loop_var": "item", "attempts": 1, "changed": false, "finished": 1, "item": {"ansible_job_id": "j774032615113.87979", "ansible_loop_var": "item", "changed": true, "failed": 0, "finished": 0, "item": {"image": "docker.io/pycontribs/centos:8", "name": "centos8", "pre_build_image": true}, "results_file": "/home/ivan/.ansible_async/j774032615113.87979", "started": 1}, "msg": "Unsupported parameters for (community.docker.docker_container) module: command_handling. Supported parameters include: api_version, auto_remove, blkio_weight, ca_cert, cap_drop, capabilities, cgroup_parent, cleanup, client_cert, client_key, command, comparisons, container_default_behavior, cpu_period, cpu_quota, cpu_shares, cpus, cpuset_cpus, cpuset_mems, debug, default_host_ip, detach, device_read_bps, device_read_iops, device_requests, device_write_bps, device_write_iops, devices, dns_opts, dns_search_domains, dns_servers, docker_host, domainname, entrypoint, env, env_file, etc_hosts, exposed_ports, force_kill, groups, healthcheck, hostname, ignore_image, image, init, interactive, ipc_mode, keep_volumes, kernel_memory, kill_signal, labels, links, log_driver, log_options, mac_address, memory, memory_reservation, memory_swap, memory_swappiness, mounts, name, network_mode, networks, networks_cli_compatible, oom_killer, oom_score_adj, output_logs, paused, pid_mode, pids_limit, privileged, published_ports, pull, purge_networks, read_only, recreate, removal_wait_timeout, restart, restart_policy, restart_retries, runtime, security_opts, shm_size, ssl_version, state, stop_signal, stop_timeout, sysctls, timeout, tls, tls_hostname, tmpfs, tty, ulimits, user, userns_mode, uts, validate_certs, volume_driver, volumes, volumes_from, working_dir (cacert_path, cert_path, docker_api_version, docker_url, expose, exposed, forcekill, key_path, log_opt, ports, tls_ca_cert, tls_client_cert, tls_client_key, tls_verify).", "results_file": "/home/ivan/.ansible_async/j774032615113.87979", "started": 1, "stderr": "/tmp/ansible_community.docker.docker_container_payload_e632gfv5/ansible_community.docker.docker_container_payload.zip/ansible_collections/community/docker/plugins/modules/docker_container.py:1193: DeprecationWarning: distutils Version classes are deprecated. Use packaging.version instead.\n", "stderr_lines": ["/tmp/ansible_community.docker.docker_container_payload_e632gfv5/ansible_community.docker.docker_container_payload.zip/ansible_collections/community/docker/plugins/modules/docker_container.py:1193: DeprecationWarning: distutils Version classes are deprecated. Use packaging.version instead."], "stdout": "", "stdout_lines": []}
failed: [localhost] (item={'failed': 0, 'started': 1, 'finished': 0, 'ansible_job_id': 'j174490992038.88004', 'results_file': '/home/ivan/.ansible_async/j174490992038.88004', 'changed': True, 'item': {'image': 'docker.io/pycontribs/ubuntu:latest', 'name': 'ubuntu', 'pre_build_image': True}, 'ansible_loop_var': 'item'}) => {"ansible_job_id": "j174490992038.88004", "ansible_loop_var": "item", "attempts": 1, "changed": false, "finished": 1, "item": {"ansible_job_id": "j174490992038.88004", "ansible_loop_var": "item", "changed": true, "failed": 0, "finished": 0, "item": {"image": "docker.io/pycontribs/ubuntu:latest", "name": "ubuntu", "pre_build_image": true}, "results_file": "/home/ivan/.ansible_async/j174490992038.88004", "started": 1}, "msg": "Unsupported parameters for (community.docker.docker_container) module: command_handling. Supported parameters include: api_version, auto_remove, blkio_weight, ca_cert, cap_drop, capabilities, cgroup_parent, cleanup, client_cert, client_key, command, comparisons, container_default_behavior, cpu_period, cpu_quota, cpu_shares, cpus, cpuset_cpus, cpuset_mems, debug, default_host_ip, detach, device_read_bps, device_read_iops, device_requests, device_write_bps, device_write_iops, devices, dns_opts, dns_search_domains, dns_servers, docker_host, domainname, entrypoint, env, env_file, etc_hosts, exposed_ports, force_kill, groups, healthcheck, hostname, ignore_image, image, init, interactive, ipc_mode, keep_volumes, kernel_memory, kill_signal, labels, links, log_driver, log_options, mac_address, memory, memory_reservation, memory_swap, memory_swappiness, mounts, name, network_mode, networks, networks_cli_compatible, oom_killer, oom_score_adj, output_logs, paused, pid_mode, pids_limit, privileged, published_ports, pull, purge_networks, read_only, recreate, removal_wait_timeout, restart, restart_policy, restart_retries, runtime, security_opts, shm_size, ssl_version, state, stop_signal, stop_timeout, sysctls, timeout, tls, tls_hostname, tmpfs, tty, ulimits, user, userns_mode, uts, validate_certs, volume_driver, volumes, volumes_from, working_dir (cacert_path, cert_path, docker_api_version, docker_url, expose, exposed, forcekill, key_path, log_opt, ports, tls_ca_cert, tls_client_cert, tls_client_key, tls_verify).", "results_file": "/home/ivan/.ansible_async/j174490992038.88004", "started": 1, "stderr": "/tmp/ansible_community.docker.docker_container_payload_95ufwtjv/ansible_community.docker.docker_container_payload.zip/ansible_collections/community/docker/plugins/modules/docker_container.py:1193: DeprecationWarning: distutils Version classes are deprecated. Use packaging.version instead.\n", "stderr_lines": ["/tmp/ansible_community.docker.docker_container_payload_95ufwtjv/ansible_community.docker.docker_container_payload.zip/ansible_collections/community/docker/plugins/modules/docker_container.py:1193: DeprecationWarning: distutils Version classes are deprecated. Use packaging.version instead."], "stdout": "", "stdout_lines": []}

PLAY RECAP *********************************************************************
localhost                  : ok=5    changed=1    unreachable=0    failed=1    skipped=5    rescued=0    ignored=0

CRITICAL Ansible return code was 2, command was: ['ansible-playbook', '--inventory', '/home/ivan/.cache/molecule/vector-role/default/inventory', '--skip-tags', 'molecule-notest,notest', '/home/ivan/.local/lib/python3.10/site-packages/molecule_docker/playbooks/create.yml']
WARNING  An error occurred during the test sequence action: 'create'. Cleaning up.
INFO     Running default > cleanup
WARNING  Skipping, cleanup playbook not configured.
INFO     Running default > destroy

PLAY [Destroy] *****************************************************************

TASK [Set async_dir for HOME env] **********************************************
ok: [localhost]

TASK [Destroy molecule instance(s)] ********************************************
changed: [localhost] => (item=centos7)
changed: [localhost] => (item=centos8)
changed: [localhost] => (item=ubuntu)

TASK [Wait for instance(s) deletion to complete] *******************************
ok: [localhost] => (item=centos7)
ok: [localhost] => (item=centos8)
ok: [localhost] => (item=ubuntu)

TASK [Delete docker networks(s)] ***********************************************
skipping: [localhost]

PLAY RECAP *********************************************************************
localhost                  : ok=3    changed=1    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0

INFO     Pruning extra files from scenario ephemeral directory



```


## Tox

Запускаю docker-контейнер:
```bash
ivan@ubuntutest$ docker run --privileged=True -v /home/ivan/vector:/opt/vector-role -w /opt/vector-role -it aragast/netology:latest /bin/bash
```
В контейнере выполняю запуск `tox`:

```bash
[root@614f73e01a31 vector-role]# tox
py37-ansible210 create: /opt/vector-role/.tox/py37-ansible210
py37-ansible210 installdeps: -rtox-requirements.txt, ansible<3.0
ERROR: invocation failed (exit code 1), logfile: /opt/vector-role/.tox/py37-ansible210/log/py37-ansible210-1.log
================================================================================================= log start ==================================================================================================
Collecting ansible<3.0
  Downloading ansible-2.10.7.tar.gz (29.9 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 29.9/29.9 MB 20.0 MB/s eta 0:00:00
  Preparing metadata (setup.py): started
  Preparing metadata (setup.py): finished with status 'done'
Collecting selinux
  Using cached selinux-0.2.1-py2.py3-none-any.whl (4.3 kB)
Collecting ansible-lint==5.1.3
  Downloading ansible_lint-5.1.3-py3-none-any.whl (113 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 113.8/113.8 KB 10.9 MB/s eta 0:00:00
Collecting yamllint==1.26.3
  Downloading yamllint-1.26.3.tar.gz (126 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 126.7/126.7 KB 13.8 MB/s eta 0:00:00
  Preparing metadata (setup.py): started
  Preparing metadata (setup.py): finished with status 'done'
Collecting lxml
  Downloading lxml-4.9.3-cp37-cp37m-manylinux_2_28_x86_64.whl (7.4 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 7.4/7.4 MB 29.3 MB/s eta 0:00:00
Collecting molecule==3.5.2
  Downloading molecule-3.5.2-py3-none-any.whl (240 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 240.2/240.2 KB 19.7 MB/s eta 0:00:00
Collecting molecule_podman
  Downloading molecule_podman-1.1.0-py3-none-any.whl (15 kB)
Collecting jmespath
  Downloading jmespath-1.0.1-py3-none-any.whl (20 kB)
Collecting wcmatch>=7.0
  Downloading wcmatch-8.4.1-py3-none-any.whl (39 kB)
Collecting enrich>=1.2.6
  Downloading enrich-1.2.7-py3-none-any.whl (8.7 kB)
Collecting typing-extensions
  Downloading typing_extensions-4.7.1-py3-none-any.whl (33 kB)
Collecting ruamel.yaml<1,>=0.15.37
  Downloading ruamel.yaml-0.17.32-py3-none-any.whl (112 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 112.2/112.2 KB 11.3 MB/s eta 0:00:00
Collecting tenacity
  Downloading tenacity-8.2.3-py3-none-any.whl (24 kB)
Collecting rich>=9.5.1
  Downloading rich-13.5.2-py3-none-any.whl (239 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 239.7/239.7 KB 18.0 MB/s eta 0:00:00
Collecting pyyaml
  Downloading PyYAML-6.0.1-cp37-cp37m-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (670 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 670.1/670.1 KB 27.5 MB/s eta 0:00:00
Collecting packaging
  Downloading packaging-23.1-py3-none-any.whl (48 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 48.9/48.9 KB 5.2 MB/s eta 0:00:00
Collecting pathspec>=0.5.3
  Downloading pathspec-0.11.2-py3-none-any.whl (29 kB)
Requirement already satisfied: setuptools in ./.tox/py37-ansible210/lib/python3.7/site-packages (from yamllint==1.26.3->-r tox-requirements.txt (line 3)) (62.1.0)
Collecting ansible-compat>=0.5.0
  Downloading ansible_compat-1.0.0-py3-none-any.whl (16 kB)
Collecting pyyaml
  Downloading PyYAML-5.4.1-cp37-cp37m-manylinux1_x86_64.whl (636 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 636.6/636.6 KB 27.9 MB/s eta 0:00:00
Collecting paramiko<3,>=2.5.0
  Downloading paramiko-2.12.0-py2.py3-none-any.whl (213 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 213.1/213.1 KB 18.0 MB/s eta 0:00:00
Collecting cookiecutter>=1.7.3
  Downloading cookiecutter-2.3.0-py3-none-any.whl (39 kB)
Collecting cerberus!=1.3.3,!=1.3.4,>=1.3.1
  Downloading Cerberus-1.3.5-py3-none-any.whl (30 kB)
Collecting pluggy<2.0,>=0.7.1
  Downloading pluggy-1.2.0-py3-none-any.whl (17 kB)
Collecting Jinja2>=2.11.3
  Downloading Jinja2-3.1.2-py3-none-any.whl (133 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 133.1/133.1 KB 14.8 MB/s eta 0:00:00
Collecting importlib-metadata
  Downloading importlib_metadata-6.7.0-py3-none-any.whl (22 kB)
Collecting click-help-colors>=0.9
  Downloading click_help_colors-0.9.2-py3-none-any.whl (5.5 kB)
Collecting click<9,>=8.0
  Downloading click-8.1.7-py3-none-any.whl (97 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 97.9/97.9 KB 10.1 MB/s eta 0:00:00
Collecting subprocess-tee>=0.3.5
  Downloading subprocess_tee-0.3.5-py3-none-any.whl (8.0 kB)
Collecting ansible-base<2.11,>=2.10.5
  Downloading ansible-base-2.10.17.tar.gz (6.1 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 6.1/6.1 MB 30.3 MB/s eta 0:00:00
  Preparing metadata (setup.py): started
  Preparing metadata (setup.py): finished with status 'done'
Collecting distro>=1.3.0
  Downloading distro-1.8.0-py3-none-any.whl (20 kB)
Collecting cryptography
  Downloading cryptography-41.0.3-cp37-abi3-manylinux_2_28_x86_64.whl (4.3 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 4.3/4.3 MB 24.5 MB/s eta 0:00:00
Collecting cached-property~=1.5
  Downloading cached_property-1.5.2-py2.py3-none-any.whl (7.6 kB)
Collecting binaryornot>=0.4.4
  Downloading binaryornot-0.4.4-py2.py3-none-any.whl (9.0 kB)
Collecting python-slugify>=4.0.0
  Downloading python_slugify-8.0.1-py2.py3-none-any.whl (9.7 kB)
Collecting arrow
  Downloading arrow-1.2.3-py3-none-any.whl (66 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 66.4/66.4 KB 7.1 MB/s eta 0:00:00
Collecting requests>=2.23.0
  Downloading requests-2.31.0-py3-none-any.whl (62 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 62.6/62.6 KB 5.2 MB/s eta 0:00:00
Collecting MarkupSafe>=2.0
  Downloading MarkupSafe-2.1.3-cp37-cp37m-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (25 kB)
Collecting six
  Using cached six-1.16.0-py2.py3-none-any.whl (11 kB)
Collecting bcrypt>=3.1.3
  Downloading bcrypt-4.0.1-cp36-abi3-manylinux_2_28_x86_64.whl (593 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 593.7/593.7 KB 28.4 MB/s eta 0:00:00
Collecting pynacl>=1.0.1
  Downloading PyNaCl-1.5.0-cp36-abi3-manylinux_2_17_x86_64.manylinux2014_x86_64.manylinux_2_24_x86_64.whl (856 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 856.7/856.7 KB 24.5 MB/s eta 0:00:00
Collecting zipp>=0.5
  Downloading zipp-3.15.0-py3-none-any.whl (6.8 kB)
Collecting pygments<3.0.0,>=2.13.0
  Downloading Pygments-2.16.1-py3-none-any.whl (1.2 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 1.2/1.2 MB 28.1 MB/s eta 0:00:00
Collecting markdown-it-py>=2.2.0
  Downloading markdown_it_py-2.2.0-py3-none-any.whl (84 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 84.5/84.5 KB 4.9 MB/s eta 0:00:00
Collecting ruamel.yaml.clib>=0.2.7
  Downloading ruamel.yaml.clib-0.2.7-cp37-cp37m-manylinux_2_17_x86_64.manylinux2014_x86_64.manylinux_2_24_x86_64.whl (500 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 500.1/500.1 KB 27.0 MB/s eta 0:00:00
Collecting bracex>=2.1.1
  Downloading bracex-2.3.post1-py3-none-any.whl (12 kB)
Collecting chardet>=3.0.2
  Downloading chardet-5.2.0-py3-none-any.whl (199 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 199.4/199.4 KB 18.3 MB/s eta 0:00:00
Collecting cffi>=1.12
  Downloading cffi-1.15.1-cp37-cp37m-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (427 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 427.9/427.9 KB 26.3 MB/s eta 0:00:00
Collecting mdurl~=0.1
  Downloading mdurl-0.1.2-py3-none-any.whl (10.0 kB)
Collecting text-unidecode>=1.3
  Downloading text_unidecode-1.3-py2.py3-none-any.whl (78 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 78.2/78.2 KB 5.6 MB/s eta 0:00:00
Collecting urllib3<3,>=1.21.1
  Downloading urllib3-2.0.4-py3-none-any.whl (123 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 123.9/123.9 KB 12.5 MB/s eta 0:00:00
Collecting certifi>=2017.4.17
  Downloading certifi-2023.7.22-py3-none-any.whl (158 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 158.3/158.3 KB 16.0 MB/s eta 0:00:00
Collecting idna<4,>=2.5
  Downloading idna-3.4-py3-none-any.whl (61 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 61.5/61.5 KB 6.1 MB/s eta 0:00:00
Collecting charset-normalizer<4,>=2
  Downloading charset_normalizer-3.2.0-cp37-cp37m-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (175 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 175.8/175.8 KB 15.0 MB/s eta 0:00:00
Collecting python-dateutil>=2.7.0
  Downloading python_dateutil-2.8.2-py2.py3-none-any.whl (247 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 247.7/247.7 KB 20.5 MB/s eta 0:00:00
Collecting pycparser
  Downloading pycparser-2.21-py2.py3-none-any.whl (118 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 118.7/118.7 KB 13.0 MB/s eta 0:00:00
Building wheels for collected packages: yamllint, ansible, ansible-base
  Building wheel for yamllint (setup.py): started
  Building wheel for yamllint (setup.py): finished with status 'done'
  Created wheel for yamllint: filename=yamllint-1.26.3-py2.py3-none-any.whl size=60820 sha256=b6bf4f6fc8094fe0dbb8c0982ee4aad74823bcc98e5fbf1e09485bd27a0f7813
  Stored in directory: /root/.cache/pip/wheels/8e/4e/d1/ca1cf422666ead85161ec19dad3c75c67febf188e3defffe21
  Building wheel for ansible (setup.py): started
  Building wheel for ansible (setup.py): finished with status 'done'
  Created wheel for ansible: filename=ansible-2.10.7-py3-none-any.whl size=48212999 sha256=69cdfdbf2d01fa3e32ad295443c918495772b262e7dcb304a26de2cfb477da93
  Stored in directory: /root/.cache/pip/wheels/8b/a7/b0/989c7da8d62f2445797244fc8c671f25b2d54139555bad354f
  Building wheel for ansible-base (setup.py): started
  Building wheel for ansible-base (setup.py): finished with status 'done'
  Created wheel for ansible-base: filename=ansible_base-2.10.17-py3-none-any.whl size=1880375 sha256=d81a5fce8dc011e3190aa3fd84e755a4e48b5c32292c2171448d1fb8b6350817
  Stored in directory: /root/.cache/pip/wheels/ff/17/b6/05cc49357ca75f3990646aae36b8da5384f31f140cce76cd56
Successfully built yamllint ansible ansible-base
Installing collected packages: text-unidecode, cached-property, zipp, urllib3, typing-extensions, tenacity, subprocess-tee, six, ruamel.yaml.clib, pyyaml, python-slugify, pygments, pycparser, pathspec, packaging, mdurl, MarkupSafe, lxml, jmespath, idna, distro, charset-normalizer, chardet, certifi, bracex, bcrypt, yamllint, wcmatch, selinux, ruamel.yaml, requests, python-dateutil, markdown-it-py, Jinja2, importlib-metadata, cffi, binaryornot, ansible-compat, rich, pynacl, pluggy, cryptography, click, cerberus, arrow, paramiko, enrich, cookiecutter, click-help-colors, ansible-base, molecule, ansible-lint, ansible, molecule_podman
ERROR: Could not install packages due to an OSError: [Errno 28] No space left on device


================================================================================================== log end ===================================================================================================
ERROR: could not install deps [-rtox-requirements.txt, ansible<3.0]; v = InvocationError("/opt/vector-role/.tox/py37-ansible210/bin/python -m pip install -rtox-requirements.txt 'ansible<3.0'", 1)
py37-ansible30 create: /opt/vector-role/.tox/py37-ansible30
py37-ansible30 installdeps: -rtox-requirements.txt, ansible<3.1
ERROR: invocation failed (exit code 1), logfile: /opt/vector-role/.tox/py37-ansible30/log/py37-ansible30-1.log
================================================================================================= log start ==================================================================================================
Collecting ansible<3.1
  Downloading ansible-3.0.0.tar.gz (30.8 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 30.8/30.8 MB 27.9 MB/s eta 0:00:00
  Preparing metadata (setup.py): started
  Preparing metadata (setup.py): finished with status 'done'
Collecting selinux
  Using cached selinux-0.2.1-py2.py3-none-any.whl (4.3 kB)
Collecting ansible-lint==5.1.3
  Using cached ansible_lint-5.1.3-py3-none-any.whl (113 kB)
Collecting yamllint==1.26.3
  Using cached yamllint-1.26.3-py2.py3-none-any.whl
Collecting lxml
  Using cached lxml-4.9.3-cp37-cp37m-manylinux_2_28_x86_64.whl (7.4 MB)
Collecting molecule==3.5.2
  Using cached molecule-3.5.2-py3-none-any.whl (240 kB)
Collecting molecule_podman
  Using cached molecule_podman-1.1.0-py3-none-any.whl (15 kB)
Collecting jmespath
  Using cached jmespath-1.0.1-py3-none-any.whl (20 kB)
Collecting wcmatch>=7.0
  Using cached wcmatch-8.4.1-py3-none-any.whl (39 kB)
Collecting enrich>=1.2.6
  Using cached enrich-1.2.7-py3-none-any.whl (8.7 kB)
Collecting typing-extensions
  Using cached typing_extensions-4.7.1-py3-none-any.whl (33 kB)
Collecting ruamel.yaml<1,>=0.15.37
  Using cached ruamel.yaml-0.17.32-py3-none-any.whl (112 kB)
Collecting tenacity
  Using cached tenacity-8.2.3-py3-none-any.whl (24 kB)
Collecting rich>=9.5.1
  Using cached rich-13.5.2-py3-none-any.whl (239 kB)
Collecting pyyaml
  Using cached PyYAML-6.0.1-cp37-cp37m-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (670 kB)
Collecting packaging
  Using cached packaging-23.1-py3-none-any.whl (48 kB)
Requirement already satisfied: setuptools in ./.tox/py37-ansible30/lib/python3.7/site-packages (from yamllint==1.26.3->-r tox-requirements.txt (line 3)) (62.1.0)
Collecting pathspec>=0.5.3
  Using cached pathspec-0.11.2-py3-none-any.whl (29 kB)
Collecting ansible-compat>=0.5.0
  Using cached ansible_compat-1.0.0-py3-none-any.whl (16 kB)
Collecting pyyaml
  Using cached PyYAML-5.4.1-cp37-cp37m-manylinux1_x86_64.whl (636 kB)
Collecting paramiko<3,>=2.5.0
  Using cached paramiko-2.12.0-py2.py3-none-any.whl (213 kB)
Collecting cookiecutter>=1.7.3
  Using cached cookiecutter-2.3.0-py3-none-any.whl (39 kB)
Collecting cerberus!=1.3.3,!=1.3.4,>=1.3.1
  Using cached Cerberus-1.3.5-py3-none-any.whl (30 kB)
Collecting pluggy<2.0,>=0.7.1
  Using cached pluggy-1.2.0-py3-none-any.whl (17 kB)
Collecting Jinja2>=2.11.3
  Using cached Jinja2-3.1.2-py3-none-any.whl (133 kB)
Collecting importlib-metadata
  Using cached importlib_metadata-6.7.0-py3-none-any.whl (22 kB)
Collecting click-help-colors>=0.9
  Using cached click_help_colors-0.9.2-py3-none-any.whl (5.5 kB)
Collecting click<9,>=8.0
  Using cached click-8.1.7-py3-none-any.whl (97 kB)
Collecting subprocess-tee>=0.3.5
  Using cached subprocess_tee-0.3.5-py3-none-any.whl (8.0 kB)
Collecting ansible-base<2.11,>=2.10.5
  Using cached ansible_base-2.10.17-py3-none-any.whl
Collecting distro>=1.3.0
  Using cached distro-1.8.0-py3-none-any.whl (20 kB)
Collecting cryptography
  Using cached cryptography-41.0.3-cp37-abi3-manylinux_2_28_x86_64.whl (4.3 MB)
Collecting cached-property~=1.5
  Using cached cached_property-1.5.2-py2.py3-none-any.whl (7.6 kB)
Collecting binaryornot>=0.4.4
  Using cached binaryornot-0.4.4-py2.py3-none-any.whl (9.0 kB)
Collecting python-slugify>=4.0.0
  Using cached python_slugify-8.0.1-py2.py3-none-any.whl (9.7 kB)
Collecting arrow
  Using cached arrow-1.2.3-py3-none-any.whl (66 kB)
Collecting requests>=2.23.0
  Using cached requests-2.31.0-py3-none-any.whl (62 kB)
Collecting MarkupSafe>=2.0
  Using cached MarkupSafe-2.1.3-cp37-cp37m-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (25 kB)
Collecting six
  Using cached six-1.16.0-py2.py3-none-any.whl (11 kB)
Collecting bcrypt>=3.1.3
  Using cached bcrypt-4.0.1-cp36-abi3-manylinux_2_28_x86_64.whl (593 kB)
Collecting pynacl>=1.0.1
  Using cached PyNaCl-1.5.0-cp36-abi3-manylinux_2_17_x86_64.manylinux2014_x86_64.manylinux_2_24_x86_64.whl (856 kB)
Collecting zipp>=0.5
  Using cached zipp-3.15.0-py3-none-any.whl (6.8 kB)
Collecting pygments<3.0.0,>=2.13.0
  Using cached Pygments-2.16.1-py3-none-any.whl (1.2 MB)
Collecting markdown-it-py>=2.2.0
  Using cached markdown_it_py-2.2.0-py3-none-any.whl (84 kB)
Collecting ruamel.yaml.clib>=0.2.7
  Using cached ruamel.yaml.clib-0.2.7-cp37-cp37m-manylinux_2_17_x86_64.manylinux2014_x86_64.manylinux_2_24_x86_64.whl (500 kB)
Collecting bracex>=2.1.1
  Using cached bracex-2.3.post1-py3-none-any.whl (12 kB)
Collecting chardet>=3.0.2
  Using cached chardet-5.2.0-py3-none-any.whl (199 kB)
Collecting cffi>=1.12
  Using cached cffi-1.15.1-cp37-cp37m-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (427 kB)
Collecting mdurl~=0.1
  Using cached mdurl-0.1.2-py3-none-any.whl (10.0 kB)
Collecting text-unidecode>=1.3
  Using cached text_unidecode-1.3-py2.py3-none-any.whl (78 kB)
Collecting urllib3<3,>=1.21.1
  Using cached urllib3-2.0.4-py3-none-any.whl (123 kB)
Collecting certifi>=2017.4.17
  Using cached certifi-2023.7.22-py3-none-any.whl (158 kB)
Collecting idna<4,>=2.5
  Using cached idna-3.4-py3-none-any.whl (61 kB)
Collecting charset-normalizer<4,>=2
  Using cached charset_normalizer-3.2.0-cp37-cp37m-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (175 kB)
Collecting python-dateutil>=2.7.0
  Using cached python_dateutil-2.8.2-py2.py3-none-any.whl (247 kB)
Collecting pycparser
  Using cached pycparser-2.21-py2.py3-none-any.whl (118 kB)
Building wheels for collected packages: ansible
  Building wheel for ansible (setup.py): started
  Building wheel for ansible (setup.py): finished with status 'error'
  error: subprocess-exited-with-error

  × python setup.py bdist_wheel did not run successfully.
  │ exit code: 1
  ╰─> [34168 lines of output]
      running bdist_wheel
      running build
      running build_py
      package init file 'ansible_collections/__init__.py' not found (or not a regular file)
      running egg_info
      writing ansible.egg-info/PKG-INFO
      writing dependency_links to ansible.egg-info/dependency_links.txt
      writing requirements to ansible.egg-info/requires.txt
      writing top-level names to ansible.egg-info/top_level.txt
      reading manifest file 'ansible.egg-info/SOURCES.txt'
      reading manifest template 'MANIFEST.in'
      warning: no files found matching 'README'
      adding license file 'COPYING'
      writing manifest file 'ansible.egg-info/SOURCES.txt'
      creating build
      creating build/lib
      creating build/lib/ansible_collections
      creating build/lib/ansible_collections/amazon
      creating build/lib/ansible_collections/amazon/aws
      copying ansible_collections/amazon/aws/.gitignore -> build/lib/ansible_collections/amazon/aws
      copying ansible_collections/amazon/aws/CHANGELOG.rst -> build/lib/ansible_collections/amazon/aws
      copying ansible_collections/amazon/aws/CONTRIBUTING.md -> build/lib/ansible_collections/amazon/aws
      copying ansible_collections/amazon/aws/COPYING -> build/lib/ansible_collections/amazon/aws
      copying ansible_collections/amazon/aws/FILES.json -> build/lib/ansible_collections/amazon/aws
      copying ansible_collections/amazon/aws/MANIFEST.json -> build/lib/ansible_collections/amazon/aws
      copying ansible_collections/amazon/aws/README.md -> build/lib/ansible_collections/amazon/aws
      copying ansible_collections/amazon/aws/requirements.txt -> build/lib/ansible_collections/amazon/aws
      copying ansible_collections/amazon/aws/shippable.yml -> build/lib/ansible_collections/amazon/aws
      creating build/lib/ansible_collections/amazon/aws/.github
      copying ansible_collections/amazon/aws/.github/BOTMETA.yml -> build/lib/ansible_collections/amazon/aws/.github
      copying ansible_collections/amazon/aws/.github/settings.yml -> build/lib/ansible_collections/amazon/aws/.github
      creating build/lib/ansible_collections/amazon/aws/changelogs
      copying ansible_collections/amazon/aws/changelogs/changelog.yaml -> build/lib/ansible_collections/amazon/aws/changelogs
      Running setup.py clean for ansible
Failed to build ansible
Installing collected packages: text-unidecode, cached-property, zipp, urllib3, typing-extensions, tenacity, subprocess-tee, six, ruamel.yaml.clib, pyyaml, python-slugify, pygments, pycparser, pathspec, packaging, mdurl, MarkupSafe, lxml, jmespath, idna, distro, charset-normalizer, chardet, certifi, bracex, bcrypt, yamllint, wcmatch, selinux, ruamel.yaml, requests, python-dateutil, markdown-it-py, Jinja2, importlib-metadata, cffi, binaryornot, ansible-compat, rich, pynacl, pluggy, cryptography, click, cerberus, arrow, paramiko, enrich, cookiecutter, click-help-colors, ansible-base, molecule, ansible-lint, ansible, molecule_podman
  Running setup.py install for ansible: started
  Running setup.py install for ansible: finished with status 'error'
Unable to print the message and arguments - possible formatting error.
Use the traceback above to help find the error.
  ERROR: Failed building wheel for ansible
--- Logging error ---
Traceback (most recent call last):
  File "/opt/vector-role/.tox/py37-ansible30/lib/python3.7/site-packages/pip/_internal/operations/build/wheel_legacy.py", line 87, in build_wheel_legacy
    spinner=spinner,
  File "/opt/vector-role/.tox/py37-ansible30/lib/python3.7/site-packages/pip/_internal/utils/subprocess.py", line 224, in call_subprocess
    raise error
pip._internal.exceptions.InstallationSubprocessError: python setup.py bdist_wheel exited with 1

During handling of the above exception, another exception occurred:

Traceback (most recent call last):
  File "/opt/vector-role/.tox/py37-ansible30/lib/python3.7/site-packages/pip/_internal/utils/logging.py", line 172, in emit
    self.console.print(renderable, overflow="ignore", crop=False, style=style)
  File "/opt/vector-role/.tox/py37-ansible30/lib/python3.7/site-packages/pip/_vendor/rich/console.py", line 1637, in print
    self._buffer.extend(new_segments)
  File "/opt/vector-role/.tox/py37-ansible30/lib/python3.7/site-packages/pip/_vendor/rich/console.py", line 837, in __exit__
    self._exit_buffer()
  File "/opt/vector-role/.tox/py37-ansible30/lib/python3.7/site-packages/pip/_vendor/rich/console.py", line 795, in _exit_buffer
    self._check_buffer()
  File "/opt/vector-role/.tox/py37-ansible30/lib/python3.7/site-packages/pip/_vendor/rich/console.py", line 1930, in _check_buffer
    self.file.flush()
OSError: [Errno 28] No space left on device
Call stack:
  File "/usr/local/lib/python3.7/runpy.py", line 193, in _run_module_as_main
    "__main__", mod_spec)
  File "/usr/local/lib/python3.7/runpy.py", line 85, in _run_code
    exec(code, run_globals)
  File "/opt/vector-role/.tox/py37-ansible30/lib/python3.7/site-packages/pip/__main__.py", line 31, in <module>
    sys.exit(_main())
  File "/opt/vector-role/.tox/py37-ansible30/lib/python3.7/site-packages/pip/_internal/cli/main.py", line 70, in main
    return command.main(cmd_args)
  File "/opt/vector-role/.tox/py37-ansible30/lib/python3.7/site-packages/pip/_internal/cli/base_command.py", line 101, in main
    return self._main(args)
  File "/opt/vector-role/.tox/py37-ansible30/lib/python3.7/site-packages/pip/_internal/cli/base_command.py", line 221, in _main
    return run(options, args)
  File "/opt/vector-role/.tox/py37-ansible30/lib/python3.7/site-packages/pip/_internal/cli/base_command.py", line 167, in exc_logging_wrapper
    status = run_func(*args)
  File "/opt/vector-role/.tox/py37-ansible30/lib/python3.7/site-packages/pip/_internal/cli/req_command.py", line 205, in wrapper
    return func(self, options, args)
  File "/opt/vector-role/.tox/py37-ansible30/lib/python3.7/site-packages/pip/_internal/commands/install.py", line 366, in run
    global_options=[],
  File "/opt/vector-role/.tox/py37-ansible30/lib/python3.7/site-packages/pip/_internal/wheel_builder.py", line 354, in build
    req.editable and req.permit_editable_wheels,
  File "/opt/vector-role/.tox/py37-ansible30/lib/python3.7/site-packages/pip/_internal/wheel_builder.py", line 223, in _build_one
    req, output_dir, build_options, global_options, editable
  File "/oUnable to print the message and arguments - possible formatting error.
Use the traceback above to help find the error.
error: legacy-install-failure

× Encountered error while trying to install package.
╰─> ansible

note: This is an issue with the package mentioned above, not pip.
hint: See above for output from the failure.

================================================================================================== log end ===================================================================================================
ERROR: could not install deps [-rtox-requirements.txt, ansible<3.1]; v = InvocationError("/opt/vector-role/.tox/py37-ansible30/bin/python -m pip install -rtox-requirements.txt 'ansible<3.1'", 1)
py39-ansible210 create: /opt/vector-role/.tox/py39-ansible210
py39-ansible210 installdeps: -rtox-requirements.txt, ansible<3.0
ERROR: invocation failed (exit code 1), logfile: /opt/vector-role/.tox/py39-ansible210/log/py39-ansible210-1.log
================================================================================================= log start ==================================================================================================
Collecting ansible<3.0
  Using cached ansible-2.10.7.tar.gz (29.9 MB)
  Preparing metadata (setup.py): started
  Preparing metadata (setup.py): finished with status 'done'
Collecting selinux
  Downloading selinux-0.3.0-py2.py3-none-any.whl (4.2 kB)
Collecting ansible-lint==5.1.3
  Using cached ansible_lint-5.1.3-py3-none-any.whl (113 kB)
Collecting yamllint==1.26.3
  Using cached yamllint-1.26.3.tar.gz (126 kB)
  Preparing metadata (setup.py): started
  Preparing metadata (setup.py): finished with status 'done'
Collecting lxml
  Downloading lxml-4.9.3-cp39-cp39-manylinux_2_28_x86_64.whl (8.0 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 8.0/8.0 MB 28.1 MB/s eta 0:00:00
Collecting molecule==3.5.2
  Using cached molecule-3.5.2-py3-none-any.whl (240 kB)
Collecting molecule_podman
  Downloading molecule_podman-2.0.3-py3-none-any.whl (15 kB)
Collecting jmespath
  Using cached jmespath-1.0.1-py3-none-any.whl (20 kB)
Collecting packaging
  Using cached packaging-23.1-py3-none-any.whl (48 kB)
Collecting tenacity
  Using cached tenacity-8.2.3-py3-none-any.whl (24 kB)
Collecting enrich>=1.2.6
  Using cached enrich-1.2.7-py3-none-any.whl (8.7 kB)
Collecting ruamel.yaml<1,>=0.15.37
  Using cached ruamel.yaml-0.17.32-py3-none-any.whl (112 kB)
Collecting pyyaml
  Downloading PyYAML-6.0.1-cp39-cp39-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (738 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 738.9/738.9 KB 44.1 MB/s eta 0:00:00
Collecting rich>=9.5.1
  Using cached rich-13.5.2-py3-none-any.whl (239 kB)
Collecting wcmatch>=7.0
  Downloading wcmatch-8.5-py3-none-any.whl (39 kB)
Collecting pathspec>=0.5.3
  Using cached pathspec-0.11.2-py3-none-any.whl (29 kB)
Requirement already satisfied: setuptools in ./.tox/py39-ansible210/lib/python3.9/site-packages (from yamllint==1.26.3->-r tox-requirements.txt (line 3)) (62.1.0)
Collecting subprocess-tee>=0.3.5
  Downloading subprocess_tee-0.4.1-py3-none-any.whl (5.1 kB)
Collecting pyyaml
  Downloading PyYAML-5.4.1-cp39-cp39-manylinux1_x86_64.whl (630 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 630.1/630.1 KB 14.5 MB/s eta 0:00:00
Collecting click-help-colors>=0.9
  Using cached click_help_colors-0.9.2-py3-none-any.whl (5.5 kB)
Collecting cerberus!=1.3.3,!=1.3.4,>=1.3.1
  Using cached Cerberus-1.3.5-py3-none-any.whl (30 kB)
Collecting ansible-compat>=0.5.0
  Downloading ansible_compat-4.1.10-py3-none-any.whl (22 kB)
Collecting click<9,>=8.0
  Using cached click-8.1.7-py3-none-any.whl (97 kB)
Collecting Jinja2>=2.11.3
  Using cached Jinja2-3.1.2-py3-none-any.whl (133 kB)
Collecting cookiecutter>=1.7.3
  Using cached cookiecutter-2.3.0-py3-none-any.whl (39 kB)
Collecting pluggy<2.0,>=0.7.1
  Downloading pluggy-1.3.0-py3-none-any.whl (18 kB)
Collecting paramiko<3,>=2.5.0
  Using cached paramiko-2.12.0-py2.py3-none-any.whl (213 kB)
Collecting ansible-base<2.11,>=2.10.5
  Using cached ansible-base-2.10.17.tar.gz (6.1 MB)
  Preparing metadata (setup.py): started
  Preparing metadata (setup.py): finished with status 'done'
Collecting distro>=1.3.0
  Using cached distro-1.8.0-py3-none-any.whl (20 kB)
Collecting molecule_podman
  Downloading molecule_podman-2.0.2-py3-none-any.whl (15 kB)
  Downloading molecule_podman-2.0.1-py3-none-any.whl (9.9 kB)
  Downloading molecule_podman-2.0.0-py3-none-any.whl (15 kB)
Collecting cryptography
  Using cached cryptography-41.0.3-cp37-abi3-manylinux_2_28_x86_64.whl (4.3 MB)
Collecting ansible-core>=2.12
  Downloading ansible_core-2.15.4-py3-none-any.whl (2.2 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 2.2/2.2 MB 21.2 MB/s eta 0:00:00
Collecting typing-extensions>=4.5.0
  Using cached typing_extensions-4.7.1-py3-none-any.whl (33 kB)
Collecting jsonschema>=4.6.0
  Downloading jsonschema-4.19.0-py3-none-any.whl (83 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 83.4/83.4 KB 8.8 MB/s eta 0:00:00
Collecting arrow
  Using cached arrow-1.2.3-py3-none-any.whl (66 kB)
Collecting python-slugify>=4.0.0
  Using cached python_slugify-8.0.1-py2.py3-none-any.whl (9.7 kB)
Collecting binaryornot>=0.4.4
  Using cached binaryornot-0.4.4-py2.py3-none-any.whl (9.0 kB)
Collecting requests>=2.23.0
  Using cached requests-2.31.0-py3-none-any.whl (62 kB)
Collecting MarkupSafe>=2.0
  Downloading MarkupSafe-2.1.3-cp39-cp39-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (25 kB)
Collecting six
  Using cached six-1.16.0-py2.py3-none-any.whl (11 kB)
Collecting bcrypt>=3.1.3
  Using cached bcrypt-4.0.1-cp36-abi3-manylinux_2_28_x86_64.whl (593 kB)
Collecting pynacl>=1.0.1
  Using cached PyNaCl-1.5.0-cp36-abi3-manylinux_2_17_x86_64.manylinux2014_x86_64.manylinux_2_24_x86_64.whl (856 kB)
Collecting markdown-it-py>=2.2.0
  Downloading markdown_it_py-3.0.0-py3-none-any.whl (87 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 87.5/87.5 KB 9.7 MB/s eta 0:00:00
Collecting pygments<3.0.0,>=2.13.0
  Using cached Pygments-2.16.1-py3-none-any.whl (1.2 MB)
Collecting ruamel.yaml.clib>=0.2.7
  Downloading ruamel.yaml.clib-0.2.7-cp39-cp39-manylinux_2_17_x86_64.manylinux2014_x86_64.manylinux_2_24_x86_64.whl (519 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 519.4/519.4 KB 22.9 MB/s eta 0:00:00
Collecting bracex>=2.1.1
  Downloading bracex-2.4-py3-none-any.whl (11 kB)
Collecting importlib-resources<5.1,>=5.0
  Downloading importlib_resources-5.0.7-py3-none-any.whl (24 kB)
Collecting resolvelib<1.1.0,>=0.5.3
  Downloading resolvelib-1.0.1-py2.py3-none-any.whl (17 kB)
Collecting chardet>=3.0.2
  Using cached chardet-5.2.0-py3-none-any.whl (199 kB)
Collecting cffi>=1.12
  Downloading cffi-1.15.1-cp39-cp39-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (441 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 441.2/441.2 KB 24.6 MB/s eta 0:00:00
Collecting jsonschema-specifications>=2023.03.6
  Downloading jsonschema_specifications-2023.7.1-py3-none-any.whl (17 kB)
Collecting referencing>=0.28.4
  Downloading referencing-0.30.2-py3-none-any.whl (25 kB)
Collecting attrs>=22.2.0
  Downloading attrs-23.1.0-py3-none-any.whl (61 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 61.2/61.2 KB 6.8 MB/s eta 0:00:00
Collecting rpds-py>=0.7.1
  Downloading rpds_py-0.10.3-cp39-cp39-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (1.2 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 1.2/1.2 MB 26.0 MB/s eta 0:00:00
Collecting mdurl~=0.1
  Using cached mdurl-0.1.2-py3-none-any.whl (10.0 kB)
Collecting text-unidecode>=1.3
  Using cached text_unidecode-1.3-py2.py3-none-any.whl (78 kB)
Collecting urllib3<3,>=1.21.1
  Using cached urllib3-2.0.4-py3-none-any.whl (123 kB)
Collecting idna<4,>=2.5
  Using cached idna-3.4-py3-none-any.whl (61 kB)
Collecting charset-normalizer<4,>=2
  Downloading charset_normalizer-3.2.0-cp39-cp39-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (202 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 202.1/202.1 KB 14.9 MB/s eta 0:00:00
Collecting certifi>=2017.4.17
  Using cached certifi-2023.7.22-py3-none-any.whl (158 kB)
Collecting python-dateutil>=2.7.0
  Using cached python_dateutil-2.8.2-py2.py3-none-any.whl (247 kB)
Collecting pycparser
  Using cached pycparser-2.21-py2.py3-none-any.whl (118 kB)
Building wheels for collected packages: yamllint, ansible, ansible-base
  Building wheel for yamllint (setup.py): started
  Building wheel for yamllint (setup.py): finished with status 'done'
  Created wheel for yamllint: filename=yamllint-1.26.3-py2.py3-none-any.whl size=60820 sha256=0e4e1e6d8ba8c3854678722308d79f72101decea1ab8eed9a7a291a98c5ecb10
  Stored in directory: /root/.cache/pip/wheels/ad/e7/53/f6ab69bd61ed0a887ee815302635448de42a0bc04035d5c1e9
  Building wheel for ansible (setup.py): started
  Building wheel for ansible (setup.py): finished with status 'error'
  error: subprocess-exited-with-error

  × python setup.py bdist_wheel did not run successfully.
  │ exit code: 1
  ╰─> [14964 lines of output]
      running bdist_wheel
      running build
      running build_py
      package init file 'ansible_collections/__init__.py' not found (or not a regular file)
      running egg_info
      writing ansible.egg-info/PKG-INFO
      writing dependency_links to ansible.egg-info/dependency_links.txt
      writing requirements to ansible.egg-info/requires.txt
      writing top-level names to ansible.egg-info/top_level.txt
      reading manifest file 'ansible.egg-info/SOURCES.txt'
      reading manifest template 'MANIFEST.in'
      warning: no files found matching 'README'
      adding license file 'COPYING'
      writing manifest file 'ansible.egg-info/SOURCES.txt'
      creating build
      creating build/lib
      creating build/lib/ansible_collections
      creating build/lib/ansible_collections/amazon
      creating build/lib/ansible_collections/amazon/aws
      copying ansible_collections/amazon/aws/.gitignore -> build/lib/ansible_collections/amazon/aws
      copying ansible_collections/amazon/aws/CHANGELOG.rst -> build/lib/ansible_collections/amazon/aws
      copying ansible_collections/amazon/aws/CONTRIBUTING.md -> build/lib/ansible_collections/amazon/aws
      copying ansible_collections/amazon/aws/COPYING -> build/lib/ansible_collections/amazon/aws
      copying ansible_collections/amazon/aws/FILES.json -> build/lib/ansible_collections/amazon/aws
      copying ansible_collections/amazon/aws/MANIFEST.json -> build/lib/ansible_collections/amazon/aws
      copying ansible_collections/amazon/aws/README.md -> build/lib/ansible_collections/amazon/aws
      copying ansible_collections/amazon/aws/requirements.txt -> build/lib/ansible_collections/amazon/aws
      copying ansible_collections/amazon/aws/shippable.yml -> build/lib/ansible_collections/amazon/aws
      creating build/lib/ansible_collections/amazon/aws/.github
      copying ansible_collections/amazon/aws/.github/BOTMETA.yml -> build/lib/ansible_collections/amazon/aws/.github
      copying ansible_collections/amazon/aws/.github/settings.yml -> build/lib/ansible_collections/amazon/aws/.github
      creating build/lib/ansible_collections/amazon/aws/changelogs
      copying ansible_collections/amazon/aws/changelogs/changelog.yaml -> build/lib/ansible_collections/amazon/aws/changelogs
      copying ansible_collections/amazon/aws/changelogs/config.yaml -> build/lib/ansible_collections/amazon/aws/changelogs
      creating build/lib/ansible_collections/amazon/aws/changelogs/fragments
      copying ansible_collections/amazon/aws/changelogs/fragments/.keep -> build/lib/ansible_collections/amazon/aws/changelogs/fragments
      creating build/lib/ansible_collections/amazon/aws/docs
      copying ansible_collections/amazon/aws/docs/amazon.aws.aws_account_attribute_lookup.rst -> build/lib/ansible_collections/amazon/aws/docs
      copying ansible_collections/amazon/aws/docs/amazon.aws.aws_az_info_module.rst -> build/lib/ansible_collections/amazon/aws/docs
      copying ansible_collections/amazon/aws/docs/amazon.aws.aws_caller_info_module.rst -> build/lib/ansible_collections/amazon/aws/docs
      copying ansible_collections/amazon/aws/docs/amazon.aws.aws_ec2_inventory.rst -> build/lib/ansible_collections/amazon/aws/docs
      copying ansible_collections/amazon/aws/docs/amazon.aws.aws_rds_inventory.rst -> build/lib/ansible_collections/amazon/aws/docs
      copying ansible_collections/amazon/aws/docs/amazon.aws.aws_s3_module.rst -> build/lib/a  Running setup.py clean for ansible
  Building wheel for ansible-base (setup.py): started
  Building wheel for ansible-base (setup.py): finished with status 'done'
  Created wheel for ansible-base: filename=ansible_base-2.10.17-py3-none-any.whl size=1880375 sha256=d3749ea54ef94411d94fa77d2f35e077680ad672b3b179386c66c3a2eddf4fa5
  Stored in directory: /root/.cache/pip/wheels/77/01/15/0d4b716065c1270fd0b9c28e5bd44d5fd907c43a85791747d7
Successfully built yamllint ansible-base
Failed to build ansible
Installing collected packages: text-unidecode, resolvelib, cerberus, urllib3, typing-extensions, tenacity, subprocess-tee, six, ruamel.yaml.clib, rpds-py, pyyaml, python-slugify, pygments, pycparser, pluggy, pathspec, packaging, mdurl, MarkupSafe, lxml, jmespath, importlib-resources, idna, distro, click, charset-normalizer, chardet, certifi, bracex, bcrypt, attrs, yamllint, wcmatch, selinux, ruamel.yaml, requests, referencing, python-dateutil, markdown-it-py, Jinja2, click-help-colors, cffi, binaryornot, rich, pynacl, jsonschema-specifications, cryptography, arrow, paramiko, jsonschema, enrich, cookiecutter, ansible-core, ansible-base, ansible-lint, ansible-compat, ansible, molecule, molecule_podman
  Running setup.py install for ansible: started
  Running setup.py install for ansible: finished with status 'error'
--- Logging error ---
  ERROR: Failed building wheel for ansible
--- Logging error ---
--- Logging error ---
  error: subprocess-exited-with-error

  × Running setup.py install for ansible did not run successfully.
  │ exit code: 1
  ╰─> [1748 lines of output]
      running install
      /opt/vector-role/.tox/py39-ansible210/lib/python3.9/site-packages/setuptools/command/install.py:34: SetuptoolsDeprecationWarning: setup.py install is deprecated. Use build and pip and other standards-based tools.
        warnings.warn(
      running build
      running build_py
      package init file 'ansible_collections/__init__.py' not found (or not a regular file)
      running egg_info
      writing ansible.egg-info/PKG-INFO
      writing dependency_links to ansible.egg-info/dependency_links.txt
      writing requirements to ansible.egg-info/requires.txt
      writing top-level names to ansible.egg-info/top_level.txt
      reading manifest file 'ansible.egg-info/SOURCES.txt'
      reading manifest template 'MANIFEST.in'
      warning: no files found matching 'README'
      adding license file 'COPYING'
      writing manifest file 'ansible.egg-info/SOURCES.txt'
      creating build
      creating build/lib
      creating build/lib/ansible_collections
      creating build/lib/ansible_collections/amazon
      creating build/lib/ansible_collections/amazon/aws
      copying ansible_collections/amazon/aws/.gitignore -> build/lib/ansible_collections/amazon/aws
      copying ansible_collections/amazon/aws/CHANGELOG.rst -> build/lib/ansible_collections/amazon/aws
      copying ansible_collections/amazon/aws/CONTRIBUTING.md -> build/lib/ansible_collections/amazon/aws
      copying ansible_collections/amazon/aws/COPYING -> build/lib/ansible_collections/amazon/aws
      copying ansible_collections/amazon/aws/FILES.json -> build/lib/ansible_collections/amazon/aws
      copying ansible_collections/amazon/aws/MANIFEST.json -> build/lib/ansible_collections/amazon/aws
      copying ansible_collections/amazon/aws/README.md -> build/lib/ansible_collections/amazon/aws
      copying ansible_collections/amazon/aws/requirements.txt -> build/lib/ansible_collections/amazon/aws
      copying ansible_collections/amazon/aws/shippable.yml -> build/lib/ansible_collections/amazon/aws
      creating build/lib/ansible_collections/amazon/aws/.github
      copying ansible_collections/amazon/aws/.github/BOTMETA.yml -> build/lib/ansible_collections/amazon/aws/.github
      copying ansible_collections/amazon/aws/.github/settings.yml -> build/lib/ansible_collections/amazon/aws/.github
      creating build/lib/ansible_collections/amazon/aws/changelogs
      copying ansible_collections/amazon/aws/changelogs/changelog.yaml -> build/lib/ansible_collections/amazon--- Logging error ---
error: legacy-install-failure

× Encountered error while trying to install package.
╰─> ansible

note: This is an issue with the package mentioned above, not pip.
hint: See above for output from the failure.

================================================================================================== log end ===================================================================================================
ERROR: could not install deps [-rtox-requirements.txt, ansible<3.0]; v = InvocationError("/opt/vector-role/.tox/py39-ansible210/bin/python -m pip install -rtox-requirements.txt 'ansible<3.0'", 1)
py39-ansible30 create: /opt/vector-role/.tox/py39-ansible30
py39-ansible30 installdeps: -rtox-requirements.txt, ansible<3.1
ERROR: invocation failed (exit code 1), logfile: /opt/vector-role/.tox/py39-ansible30/log/py39-ansible30-1.log
================================================================================================= log start ==================================================================================================
Collecting ansible<3.1
  Using cached ansible-3.0.0.tar.gz (30.8 MB)
  Preparing metadata (setup.py): started
  Preparing metadata (setup.py): finished with status 'done'
Collecting selinux
  Using cached selinux-0.3.0-py2.py3-none-any.whl (4.2 kB)
Collecting ansible-lint==5.1.3
  Using cached ansible_lint-5.1.3-py3-none-any.whl (113 kB)
Collecting yamllint==1.26.3
  Using cached yamllint-1.26.3-py2.py3-none-any.whl
Collecting lxml
  Using cached lxml-4.9.3-cp39-cp39-manylinux_2_28_x86_64.whl (8.0 MB)
Collecting molecule==3.5.2
  Using cached molecule-3.5.2-py3-none-any.whl (240 kB)
Collecting molecule_podman
  Using cached molecule_podman-2.0.3-py3-none-any.whl (15 kB)
Collecting jmespath
  Using cached jmespath-1.0.1-py3-none-any.whl (20 kB)
Collecting packaging
  Using cached packaging-23.1-py3-none-any.whl (48 kB)
Collecting tenacity
  Using cached tenacity-8.2.3-py3-none-any.whl (24 kB)
Collecting enrich>=1.2.6
  Using cached enrich-1.2.7-py3-none-any.whl (8.7 kB)
Collecting ruamel.yaml<1,>=0.15.37
  Using cached ruamel.yaml-0.17.32-py3-none-any.whl (112 kB)
Collecting pyyaml
  Using cached PyYAML-6.0.1-cp39-cp39-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (738 kB)
Collecting rich>=9.5.1
  Using cached rich-13.5.2-py3-none-any.whl (239 kB)
Collecting wcmatch>=7.0
  Using cached wcmatch-8.5-py3-none-any.whl (39 kB)
Collecting pathspec>=0.5.3
  Using cached pathspec-0.11.2-py3-none-any.whl (29 kB)
Requirement already satisfied: setuptools in ./.tox/py39-ansible30/lib/python3.9/site-packages (from yamllint==1.26.3->-r tox-requirements.txt (line 3)) (62.1.0)
Collecting subprocess-tee>=0.3.5
  Using cached subprocess_tee-0.4.1-py3-none-any.whl (5.1 kB)
Collecting pyyaml
  Using cached PyYAML-5.4.1-cp39-cp39-manylinux1_x86_64.whl (630 kB)
Collecting click-help-colors>=0.9
  Using cached click_help_colors-0.9.2-py3-none-any.whl (5.5 kB)
Collecting cerberus!=1.3.3,!=1.3.4,>=1.3.1
  Using cached Cerberus-1.3.5-py3-none-any.whl (30 kB)
Collecting ansible-compat>=0.5.0
  Using cached ansible_compat-4.1.10-py3-none-any.whl (22 kB)
Collecting click<9,>=8.0
  Using cached click-8.1.7-py3-none-any.whl (97 kB)
Collecting Jinja2>=2.11.3
  Using cached Jinja2-3.1.2-py3-none-any.whl (133 kB)
Collecting cookiecutter>=1.7.3
  Using cached cookiecutter-2.3.0-py3-none-any.whl (39 kB)
Collecting pluggy<2.0,>=0.7.1
  Using cached pluggy-1.3.0-py3-none-any.whl (18 kB)
Collecting paramiko<3,>=2.5.0
  Using cached paramiko-2.12.0-py2.py3-none-any.whl (213 kB)
Collecting ansible-base<2.11,>=2.10.5
  Using cached ansible_base-2.10.17-py3-none-any.whl
Collecting distro>=1.3.0
  Using cached distro-1.8.0-py3-none-any.whl (20 kB)
Collecting molecule_podman
  Using cached molecule_podman-2.0.2-py3-none-any.whl (15 kB)
  Using cached molecule_podman-2.0.1-py3-none-any.whl (9.9 kB)
  Using cached molecule_podman-2.0.0-py3-none-any.whl (15 kB)
Collecting cryptography
  Using cached cryptography-41.0.3-cp37-abi3-manylinux_2_28_x86_64.whl (4.3 MB)
Collecting ansible-core>=2.12
  Using cached ansible_core-2.15.4-py3-none-any.whl (2.2 MB)
Collecting typing-extensions>=4.5.0
  Using cached typing_extensions-4.7.1-py3-none-any.whl (33 kB)
Collecting jsonschema>=4.6.0
  Using cached jsonschema-4.19.0-py3-none-any.whl (83 kB)
Collecting arrow
  Using cached arrow-1.2.3-py3-none-any.whl (66 kB)
Collecting python-slugify>=4.0.0
  Using cached python_slugify-8.0.1-py2.py3-none-any.whl (9.7 kB)
Collecting binaryornot>=0.4.4
  Using cached binaryornot-0.4.4-py2.py3-none-any.whl (9.0 kB)
Collecting requests>=2.23.0
  Using cached requests-2.31.0-py3-none-any.whl (62 kB)
Collecting MarkupSafe>=2.0
  Using cached MarkupSafe-2.1.3-cp39-cp39-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (25 kB)
Collecting six
  Using cached six-1.16.0-py2.py3-none-any.whl (11 kB)
Collecting bcrypt>=3.1.3
  Using cached bcrypt-4.0.1-cp36-abi3-manylinux_2_28_x86_64.whl (593 kB)
Collecting pynacl>=1.0.1
  Using cached PyNaCl-1.5.0-cp36-abi3-manylinux_2_17_x86_64.manylinux2014_x86_64.manylinux_2_24_x86_64.whl (856 kB)
Collecting markdown-it-py>=2.2.0
  Using cached markdown_it_py-3.0.0-py3-none-any.whl (87 kB)
Collecting pygments<3.0.0,>=2.13.0
  Using cached Pygments-2.16.1-py3-none-any.whl (1.2 MB)
Collecting ruamel.yaml.clib>=0.2.7
  Using cached ruamel.yaml.clib-0.2.7-cp39-cp39-manylinux_2_17_x86_64.manylinux2014_x86_64.manylinux_2_24_x86_64.whl (519 kB)
Collecting bracex>=2.1.1
  Using cached bracex-2.4-py3-none-any.whl (11 kB)
Collecting importlib-resources<5.1,>=5.0
  Using cached importlib_resources-5.0.7-py3-none-any.whl (24 kB)
Collecting resolvelib<1.1.0,>=0.5.3
  Using cached resolvelib-1.0.1-py2.py3-none-any.whl (17 kB)
Collecting chardet>=3.0.2
  Using cached chardet-5.2.0-py3-none-any.whl (199 kB)
Collecting cffi>=1.12
  Using cached cffi-1.15.1-cp39-cp39-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (441 kB)
Collecting jsonschema-specifications>=2023.03.6
  Using cached jsonschema_specifications-2023.7.1-py3-none-any.whl (17 kB)
Collecting referencing>=0.28.4
  Using cached referencing-0.30.2-py3-none-any.whl (25 kB)
Collecting attrs>=22.2.0
  Using cached attrs-23.1.0-py3-none-any.whl (61 kB)
Collecting rpds-py>=0.7.1
  Using cached rpds_py-0.10.3-cp39-cp39-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (1.2 MB)
Collecting mdurl~=0.1
  Using cached mdurl-0.1.2-py3-none-any.whl (10.0 kB)
Collecting text-unidecode>=1.3
  Using cached text_unidecode-1.3-py2.py3-none-any.whl (78 kB)
Collecting urllib3<3,>=1.21.1
  Using cached urllib3-2.0.4-py3-none-any.whl (123 kB)
Collecting idna<4,>=2.5
  Using cached idna-3.4-py3-none-any.whl (61 kB)
Collecting charset-normalizer<4,>=2
  Using cached charset_normalizer-3.2.0-cp39-cp39-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (202 kB)
Collecting certifi>=2017.4.17
  Using cached certifi-2023.7.22-py3-none-any.whl (158 kB)
Collecting python-dateutil>=2.7.0
  Using cached python_dateutil-2.8.2-py2.py3-none-any.whl (247 kB)
Collecting pycparser
  Using cached pycparser-2.21-py2.py3-none-any.whl (118 kB)
Building wheels for collected packages: ansible
  Building wheel for ansible (setup.py): started
  Building wheel for ansible (setup.py): finished with status 'error'
  error: subprocess-exited-with-error

  × python setup.py bdist_wheel did not run successfully.
  │ exit code: 1
  ╰─> [5907 lines of output]
      running bdist_wheel
      running build
      running build_py
      package init file 'ansible_collections/__init__.py' not found (or not a regular file)
      running egg_info
      writing ansible.egg-info/PKG-INFO
      writing dependency_links to ansible.egg-info/dependency_links.txt
      writing requirements to ansible.egg-info/requires.txt
      writing top-level names to ansible.egg-info/top_level.txt
      reading manifest file 'ansible.egg-info/SOURCES.txt'
      reading manifest template 'MANIFEST.in'
      warning: no files found matching 'README'
      adding license file 'COPYING'
      writing manifest file 'ansible.egg-info/SOURCES.txt'
      creating build
      creating build/lib
      creating build/lib/ansible_collections
      creating build/lib/ansible_collections/amazon
      creating build/lib/ansible_collections/amazon/aws
      copying ansible_collections/amazon/aws/.gitignore -> build/lib/ansible_collections/amazon/aws
      copying ansible_collections/amazon/aws/CHANGELOG.rst -> build/lib/ansible_collections/amazon/aws
      copying ansible_collections/amazon/aws/CONTRIBUTING.md -> build/lib/ansible_collections/amazon/aws
      copying ansible_collections/amazon/aws/COPYING -> build/lib/ansible_collections/amazon/aws
      copying ansible_collections/amazon/aws/FILES.json -> build/lib/ansible_collections/amazon/aws
      copying ansible_collections/amazon/aws/MANIFEST.json -> build/lib/ansible_collections/amazon/aws
      copying ansible_collections/amazon/aws/README.md -> build/lib/ansible_collections/amazon/aws
      copying ansible_collections/amazon/aws/requirements.txt -> build/lib/ansible_collections/amazon/aws
      copying ansible_coll  Running setup.py clean for ansible
Failed to build ansible
Installing collected packages: text-unidecode, resolvelib, cerberus, urllib3, typing-extensions, tenacity, subprocess-tee, six, ruamel.yaml.clib, rpds-py, pyyaml, python-slugify, pygments, pycparser, pluggy, pathspec, packaging, mdurl, MarkupSafe, lxml, jmespath, importlib-resources, idna, distro, click, charset-normalizer, chardet, certifi, bracex, bcrypt, attrs, yamllint, wcmatch, selinux, ruamel.yaml, requests, referencing, python-dateutil, markdown-it-py, Jinja2, click-help-colors, cffi, binaryornot, rich, pynacl, jsonschema-specifications, cryptography, arrow, paramiko, jsonschema, enrich, cookiecutter, ansible-core, ansible-base, ansible-lint, ansible-compat, ansible, molecule, molecule_podman
--- Logging error ---
  ERROR: Failed building wheel for ansible
--- Logging error ---
--- Logging error ---
ERROR: Could not install packages due to an OSError: [Errno 28] No space left on device


================================================================================================== log end ===================================================================================================
ERROR: could not install deps [-rtox-requirements.txt, ansible<3.1]; v = InvocationError("/opt/vector-role/.tox/py39-ansible30/bin/python -m pip install -rtox-requirements.txt 'ansible<3.1'", 1)
__________________________________________________________________________________________________ summary ___________________________________________________________________________________________________
ERROR:   py37-ansible210: could not install deps [-rtox-requirements.txt, ansible<3.0]; v = InvocationError("/opt/vector-role/.tox/py37-ansible210/bin/python -m pip install -rtox-requirements.txt 'ansible<3.0'", 1)
ERROR:   py37-ansible30: could not install deps [-rtox-requirements.txt, ansible<3.1]; v = InvocationError("/opt/vector-role/.tox/py37-ansible30/bin/python -m pip install -rtox-requirements.txt 'ansible<3.1'", 1)
ERROR:   py39-ansible210: could not install deps [-rtox-requirements.txt, ansible<3.0]; v = InvocationError("/opt/vector-role/.tox/py39-ansible210/bin/python -m pip install -rtox-requirements.txt 'ansible<3.0'", 1)
ERROR:   py39-ansible30: could not install deps [-rtox-requirements.txt, ansible<3.1]; v = InvocationError("/opt/vector-role/.tox/py39-ansible30/bin/python -m pip install -rtox-requirements.txt 'ansible<3.1'", 1)
```




Playbook, который использовался в моей домашней работе доступен [по ссылке](https://github.com/Seleznev-Ivan/devops-netology-ansible/tree/main/08-ansible-05-testing/playbook)


