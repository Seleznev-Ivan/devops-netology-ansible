# Домашнее задание к занятию 1 «Введение в Ansible»

## Подготовка к выполнению

1. Установите Ansible версии 2.10 или выше.
2. Создайте свой публичный репозиторий на GitHub с произвольным именем.
3. Скачайте [Playbook](./playbook/) из репозитория с домашним заданием и перенесите его в свой репозиторий.

## Основная часть

1. Попробуйте запустить playbook на окружении из `test.yml`, зафиксируйте значение, которое имеет факт `some_fact` для указанного хоста при выполнении playbook.

   ### Решение:

Значение `some_fact: 12`
```bash
root@ubuntu-netology:/home/ivan/netology-ansible/devops-netology-ansible/playbook# ansible-playbook site.yml -i inventory/test.yml

PLAY [Print os facts] *********************************************************************************

TASK [Gathering Facts] ********************************************************************************
ok: [localhost]

TASK [Print OS] ***************************************************************************************
ok: [localhost] => {
    "msg": "Ubuntu"
}

TASK [Print fact] *************************************************************************************
ok: [localhost] => {
    "msg": 12
}

PLAY RECAP ********************************************************************************************
localhost                  : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

2. Найдите файл с переменными (group_vars), в котором задаётся найденное в первом пункте значение, и поменяйте его на `all default fact`.

   ### Решение:
Файл находится в `/playbook/group_vars/all/examp.yml`
```bash
nano ./group_vars/all/examp.yml

---
  some_fact: "all default fact"
```
   
3. Воспользуйтесь подготовленным (используется `docker`) или создайте собственное окружение для проведения дальнейших испытаний.

   ### Решение:
```bash
ivan@ubuntu-netology:~/netology-ansible/devops-netology-ansible/playbook$ docker ps
CONTAINER ID   IMAGE               COMMAND       CREATED          STATUS          PORTS     NAMES
6af190054f48   pycontribs/ubuntu   "/bin/bash"   13 seconds ago   Up 12 seconds             ubuntu
9ff88673301d   centos:7            "/bin/bash"   21 minutes ago   Up 21 minutes             centos7
```

4. Проведите запуск playbook на окружении из `prod.yml`. Зафиксируйте полученные значения `some_fact` для каждого из `managed host`.

   ### Решение:
```bash
ivan@ubuntu-netology:~/netology-ansible/devops-netology-ansible/playbook$ ansible-playbook -i inventory/prod.yml site.yml

PLAY [Print os facts] *********************************************************************************

TASK [Gathering Facts] ********************************************************************************
ok: [ubuntu]
ok: [centos7]

TASK [Print OS] ***************************************************************************************
ok: [centos7] => {
    "msg": "CentOS"
}
ok: [ubuntu] => {
    "msg": "Ubuntu"
}

TASK [Print fact] *************************************************************************************
ok: [centos7] => {
    "msg": "el"
}
ok: [ubuntu] => {
    "msg": "deb"
}

PLAY RECAP ********************************************************************************************
centos7                    : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
ubuntu                     : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```
   
5. Добавьте факты в `group_vars` каждой из групп хостов так, чтобы для `some_fact` получились значения: для `deb` — `deb default fact`, для `el` — `el default fact`.

   ### Решение:
```bash
nano ./group_vars/deb/examp.yml
---
  some_fact: "deb default fact"

nano ./group_vars/el/examp.yml
---
  some_fact: "el default fact"
```
  
6.  Повторите запуск playbook на окружении `prod.yml`. Убедитесь, что выдаются корректные значения для всех хостов.

   ### Решение:
```bash
ivan@ubuntu-netology:~/netology-ansible/devops-netology-ansible/playbook$ ansible-playbook -i inventory/prod.yml site.yml

PLAY [Print os facts] ***************************************************************************************************************

TASK [Gathering Facts] **************************************************************************************************************
ok: [ubuntu]
ok: [centos7]

TASK [Print OS] *********************************************************************************************************************
ok: [centos7] => {
    "msg": "CentOS"
}
ok: [ubuntu] => {
    "msg": "Ubuntu"
}

TASK [Print fact] *******************************************************************************************************************
ok: [centos7] => {
    "msg": "el default fact"
}
ok: [ubuntu] => {
    "msg": "deb default fact"
}

PLAY RECAP **************************************************************************************************************************
centos7                    : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
ubuntu                     : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

7. При помощи `ansible-vault` зашифруйте факты в `group_vars/deb` и `group_vars/el` с паролем `netology`.
    
   ### Решение:
```bash
ivan@ubuntu-netology:~/netology-ansible/devops-netology-ansible/playbook$ ansible-vault encrypt group_vars/deb/examp.yml
New Vault password:
Confirm New Vault password:
Encryption successful

ivan@ubuntu-netology:~/netology-ansible/devops-netology-ansible/playbook$ ansible-vault encrypt group_vars/el/examp.yml
New Vault password:
Confirm New Vault password:
Encryption successful
```

8. Запустите playbook на окружении `prod.yml`. При запуске `ansible` должен запросить у вас пароль. Убедитесь в работоспособности.
    
   ### Решение:
```bash
ivan@ubuntu-netology:~/netology-ansible/devops-netology-ansible/playbook$ ansible-playbook -i inventory/prod.yml site.yml --ask-vault-pass
Vault password:

PLAY [Print os facts] ***************************************************************************************************************

TASK [Gathering Facts] **************************************************************************************************************
ok: [ubuntu]
ok: [centos7]

TASK [Print OS] *********************************************************************************************************************
ok: [centos7] => {
    "msg": "CentOS"
}
ok: [ubuntu] => {
    "msg": "Ubuntu"
}

TASK [Print fact] *******************************************************************************************************************
ok: [centos7] => {
    "msg": "el default fact"
}
ok: [ubuntu] => {
    "msg": "deb default fact"
}

PLAY RECAP **************************************************************************************************************************
centos7                    : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
ubuntu                     : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

9. Посмотрите при помощи `ansible-doc` список плагинов для подключения. Выберите подходящий для работы на `control node`.
    
   ### Решение:
```bash
ivan@ubuntu-netology:~/netology-ansible/devops-netology-ansible$ ansible-doc --type connection --list
ansible.builtin.local          execute on controller
ansible.builtin.paramiko_ssh   Run tasks via python ssh (paramiko)
ansible.builtin.psrp           Run tasks over Microsoft PowerShell Remoting Protocol
ansible.builtin.ssh            connect via SSH client binary
ansible.builtin.winrm          Run tasks over Microsoft's WinRM
ansible.netcommon.grpc         Provides a persistent connection using the gRPC protocol
ansible.netcommon.httpapi      Use httpapi to run command on network appliances
ansible.netcommon.libssh       Run tasks using libssh for ssh connection
ansible.netcommon.netconf      Provides a persistent connection using the netconf protocol
ansible.netcommon.network_cli  Use network_cli to run command on network appliances
ansible.netcommon.persistent   Use a persistent unix socket for connection
community.aws.aws_ssm          connect to EC2 instances via AWS Systems Manager
community.docker.docker        Run tasks in docker containers
community.docker.docker_api    Run tasks in docker containers
community.docker.nsenter       execute on host running controller container
community.general.chroot       Interact with local chroot
community.general.funcd        Use funcd to connect to target
community.general.iocage       Run tasks in iocage jails
community.general.jail         Run tasks in jails
community.general.lxc          Run tasks in lxc containers via lxc python library
community.general.lxd          Run tasks in lxc containers via lxc CLI
community.general.qubes        Interact with an existing QubesOS AppVM
community.general.saltstack    Allow ansible to piggyback on salt minions
community.general.zone         Run tasks in a zone instance
community.kubernetes.kubectl   Execute tasks in pods running on Kubernetes
community.libvirt.libvirt_lxc  Run tasks in lxc containers via libvirt
community.libvirt.libvirt_qemu Run tasks on libvirt/qemu virtual machines
community.okd.oc               Execute tasks in pods running on OpenShift
community.vmware.vmware_tools  Execute tasks inside a VM via VMware Tools
containers.podman.buildah      Interact with an existing buildah container
containers.podman.podman       Interact with an existing podman container
kubernetes.core.kubectl        Execute tasks in pods running on Kubernetes
```

Выполняю команду `ansible-doc`, которая отобразит список плагинов по типу подключения и выбираю подходящий:
`ansible.builtin.local          execute on controller`

10. В `prod.yml` добавьте новую группу хостов с именем  `local`, в ней разместите localhost с необходимым типом подключения.
    
   ### Решение:
```bash
nano ./inventory/prod.yml
```
Добавляю новую группу хостов:
```bash
---
  el:
    hosts:
      centos7:
        ansible_connection: docker
  deb:
    hosts:
      ubuntu:
        ansible_connection: docker
  local:
    hosts:
      localhost:
        ansible_connection: local
```
        
11. Запустите playbook на окружении `prod.yml`. При запуске `ansible` должен запросить у вас пароль. Убедитесь, что факты `some_fact` для каждого из хостов определены из верных `group_vars`.

   ### Решение:
```bash
ivan@ubuntu-netology:~/netology-ansible/devops-netology-ansible/playbook$ ansible-playbook -i inventory/prod.yml site.yml --ask-vault-pass
Vault password:

PLAY [Print os facts] ***************************************************************************************************************

TASK [Gathering Facts] **************************************************************************************************************
ok: [localhost]
ok: [ubuntu]
ok: [centos7]

TASK [Print OS] *********************************************************************************************************************
ok: [centos7] => {
    "msg": "CentOS"
}
ok: [ubuntu] => {
    "msg": "Ubuntu"
}
ok: [localhost] => {
    "msg": "Ubuntu"
}

TASK [Print fact] *******************************************************************************************************************
ok: [centos7] => {
    "msg": "el default fact"
}
ok: [ubuntu] => {
    "msg": "deb default fact"
}
ok: [localhost] => {
    "msg": "all default fact"
}

PLAY RECAP **************************************************************************************************************************
centos7                    : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
localhost                  : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
ubuntu                     : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

12. Заполните `README.md` ответами на вопросы. Сделайте `git push` в ветку `master`. В ответе отправьте ссылку на ваш открытый репозиторий с изменённым `playbook` и заполненным `README.md`.

   ### Решение:
[Ответы на вопросы](https://github.com/Seleznev-Ivan/devops-netology-ansible/blob/main/README.md)


## Необязательная часть

1. При помощи `ansible-vault` расшифруйте все зашифрованные файлы с переменными.
   
   ### Решение:
```bash
ivan@ubuntu-netology:~/netology-ansible/devops-netology-ansible/playbook$ ansible-vault decrypt group_vars/el/examp.yml
Vault password:
Decryption successful
ivan@ubuntu-netology:~/netology-ansible/devops-netology-ansible/playbook$ ansible-vault decrypt group_vars/deb/examp.yml
Vault password:
Decryption successful
```
   
2. Зашифруйте отдельное значение `PaSSw0rd` для переменной `some_fact` паролем `netology`. Добавьте полученное значение в `group_vars/all/exmp.yml`.
   
   ### Решение:
```bash
ivan@ubuntu-netology:~/netology-ansible/devops-netology-ansible/playbook$ ansible-vault encrypt_string
New Vault password:
Confirm New Vault password:
Reading plaintext input from stdin. (ctrl-d to end input, twice if your content does not already have a newline)
PaSSw0rd
Encryption successful
!vault |
          $ANSIBLE_VAULT;1.1;AES256
          30616439663237663136386534383536303939396463333264393263353732303165663164393038
          3364356566643439643562306538653035356236663232650a386632313933363962383761623635
          37333265353765653837366233663937373261336666356132303133646135393964626365346537
          3466396136643064310a386366386263333761306639656662303533636364633636326233323936
          6430
```

Добавляю полученное значение в `group_vars/all/exmp.yml`:
```bash
ivan@ubuntu-netology:~/netology-ansible/devops-netology-ansible/playbook$ nano group_vars/all/examp.yml

---
  some_fact: !vault |
          $ANSIBLE_VAULT;1.1;AES256
          31633330386632663635663463613330613032653632636436646563343266343966356266366239
          6164656533393666356337396234323533366431626432610a626434343837383333323064386365
          61396136316637343864323633363537363633653461376338646466643462646162653634363139
          6133313465613431300a303232656231353837323961373733373563366230353166356464656566
          3330
 ```
  
3. Запустите `playbook`, убедитесь, что для нужных хостов применился новый `fact`.

   ### Решение:
```bash
ivan@ubuntu-netology:~/netology-ansible/devops-netology-ansible/playbook$ ansible-playbook -i inventory/prod.yml site.yml --ask-vault-pass
Vault password:

PLAY [Print os facts] ***************************************************************************************************************

TASK [Gathering Facts] **************************************************************************************************************
ok: [localhost]
ok: [ubuntu]
ok: [centos7]

TASK [Print OS] *********************************************************************************************************************
ok: [centos7] => {
    "msg": "CentOS"
}
ok: [ubuntu] => {
    "msg": "Ubuntu"
}
ok: [localhost] => {
    "msg": "Ubuntu"
}

TASK [Print fact] *******************************************************************************************************************
ok: [centos7] => {
    "msg": "el default fact"
}
ok: [ubuntu] => {
    "msg": "deb default fact"
}
ok: [localhost] => {
    "msg": "PaSSw0rd"
}

PLAY RECAP **************************************************************************************************************************
centos7                    : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
localhost                  : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
ubuntu                     : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```
   
4. Добавьте новую группу хостов `fedora`, самостоятельно придумайте для неё переменную. В качестве образа можно использовать [этот вариант](https://hub.docker.com/r/pycontribs/fedora).

   ### Решение:
Запускаю новый docker-контейнер c fedora:
```bash
docker run -d -t --name fedora pycontribs/fedora
```
Список всех запущенных контейнеров:
```bash
ivan@ubuntu-netology:~/netology-ansible/devops-netology-ansible/playbook$ docker ps
CONTAINER ID   IMAGE               COMMAND       CREATED         STATUS         PORTS     NAMES
08b42f3fc490   pycontribs/fedora   "/bin/bash"   8 seconds ago   Up 7 seconds             fedora
6af190054f48   pycontribs/ubuntu   "/bin/bash"   3 hours ago     Up 3 hours               ubuntu
9ff88673301d   centos:7            "/bin/bash"   4 hours ago     Up 4 hours               centos7
```
Редактирую inventory-файл:
```bash
ivan@ubuntu-netology:~/netology-ansible/devops-netology-ansible/playbook$ nano ./inventory/prod.yml
```
Добавляю новую группу:
```bash
  fedora:
    hosts:
      fedora:
        ansible_connection: docker
```

Добавляю переменную для группы 
```bash
ivan@ubuntu-netology:~/netology-ansible/devops-netology-ansible/playbook$ mkdir group_vars/fedora
ivan@ubuntu-netology:~/netology-ansible/devops-netology-ansible/playbook$ nano group_vars/fedora/examp.yml

---
  some_fact: "Fedora test fact"
```
Запускаю playbook:
```bash
ivan@ubuntu-netology:~/netology-ansible/devops-netology-ansible/playbook$ ansible-playbook -i inventory/prod.yml site.yml --ask-vault-pass
Vault password:
[WARNING]: Found both group and host with same name: fedora

PLAY [Print os facts] ***************************************************************************************************************

TASK [Gathering Facts] **************************************************************************************************************
ok: [localhost]
ok: [ubuntu]
ok: [fedora]
ok: [centos7]

TASK [Print OS] *********************************************************************************************************************
ok: [centos7] => {
    "msg": "CentOS"
}
ok: [ubuntu] => {
    "msg": "Ubuntu"
}
ok: [localhost] => {
    "msg": "Ubuntu"
}
ok: [fedora] => {
    "msg": "Fedora"
}

TASK [Print fact] *******************************************************************************************************************
ok: [centos7] => {
    "msg": "el default fact"
}
ok: [ubuntu] => {
    "msg": "deb default fact"
}
ok: [fedora] => {
    "msg": "Fedora test fact"
}
ok: [localhost] => {
    "msg": "PaSSw0rd"
}

PLAY RECAP **************************************************************************************************************************
centos7                    : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
fedora                     : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
localhost                  : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
ubuntu                     : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```
   
5. Напишите скрипт на bash: автоматизируйте поднятие необходимых контейнеров, запуск ansible-playbook и остановку контейнеров.
6. Все изменения должны быть зафиксированы и отправлены в ваш личный репозиторий.

---

### Как оформить решение задания

Выполненное домашнее задание пришлите в виде ссылки на .md-файл в вашем репозитории.

---
