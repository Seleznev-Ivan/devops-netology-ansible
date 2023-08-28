# Домашнее задание к занятию 2 «Работа с Playbook»

## Подготовительный этап

* Установил сервер centos01-ansible
* Установил сервер centos02
* На сервер centos01-ansible установил ansible 2.9.27, ansible-lint 3.5.1
```bash
yum install epel-release
yum install ansible ansible-lint
```
* Настроил доступ к серверу centos02 по ssh-ключу (rsa)
```bash
ssh-keygen -t rsa
ssh-copy-id root@centos02
```

## Clickhouse
Установка Clickhouse производится на все хосты, указанные в инвенторе-файле  `inventory/prod.yml`
Задачи:
* Get clickhouse distrib - скачивается дистрибутив версии 22.3.3.44, который указан в переменных  `group_vars/clickhouse/vars.yml`
* Install clickhouse packages - устанавливается скачанный дистрибутив Clickhouse.
* Start clickhouse service - запускается сервис Clickhouse.
* Create database - создается база логов.

## Vector
Установка Vector производится на все хосты, указанные в инвенторе-файле  `inventory/prod.yml`
Задачи:
* Get vector distrib - скачивается дистрибутив версии 0.32.1, который указан в переменных  `group_vars/vector/vars.yml`
* Install vector packages - устанаю шаблон для запуска сервиса `vector.service`
Переменные, используемые в шаблоне указываю в `group_vars/vector/vars.yml`
Использовал следующие переменные:
- vector_version
- vector_user
- vector_group
- vector_bin_path
- vector_exec_name
- vector_log_output


## Проверка установленных компонентов на втором сервере (centos02):

Теперь проверяю установленные с помощью Ansible сервисы с помощью следующих команд:
```bash
vector --version
systemctl status clickhouse-server
clickhouse-client --version
```


