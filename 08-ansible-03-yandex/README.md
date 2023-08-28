# Домашнее задание к занятию 3 «Использование Ansible»»

## Подготовительный этап

* Установил сервер centos01-ansible
* Установил 3 сервера centos02-c, centos03-v и centos04-l на Яндекс.Cloud
* На сервер centos01-ansible установил ansible 2.9.27, ansible-lint 3.5.1
```bash
yum install epel-release
yum install ansible ansible-lint
```
* Настроил доступ к серверу centos02-c, centos03-v и centos04-l по ssh-ключу (rsa)
```bash
ssh-keygen -t rsa ed25519
```

## Clickhouse
Установка Clickhouse производится на хост centos02-c, указанный в инвенторе-файле  `inventory/prod.yml`

Задачи:
* Get clickhouse distrib - скачивается дистрибутив версии 22.3.4.20, который указан в переменных  `group_vars/clickhouse/vars.yml`
* Install clickhouse packages - устанавливается скачанный дистрибутив Clickhouse.
* Start clickhouse service - запускается сервис Clickhouse.
* Create database - создается база логов.

## Vector
Установка Vector производится на хост centos03-v, указанный в инвенторе-файле  `inventory/prod.yml`

Задачи:
* Get vector distrib - скачивается дистрибутив версии 0.32.1, который указан в переменных  `group_vars/vector/vars.yml`
* Install vector packages - устанавливаю дистрибутив Vector
* Create vector config - устанавливаю шаблон конфигурации Vector `vector.toml`
* Configure service Vector - устанавливаю шаблон для запуска сервиса `vector.service`
  
Переменные, используемые в шаблонах, указываю в `group_vars/vector/vars.yml`

## LightHouse 

Для работы LightHouse  необходимо установить веб-сервер Nginx.
* Устанавливаю репозиторий epel-release
* Скачиваю, устанавливаю и запускаю веб-сервер Nginx.
  
Так как репозиторий LightHouse  располагается на github, то для его работы нужно установить на сервер git
* Копирую дистрибутив из гита
* Создаю конфигурационный файл Clickhouse
  
Установка LightHouse  и Nginx производится на хост centos04-l, указанный в инвенторе-файле  `inventory/prod.yml`


## Проверка установленных компонентов:

Теперь проверяю установленные с помощью Ansible сервисы с помощью следующих команд:
```bash
vector --version
systemctl status clickhouse-server
clickhouse-client --version
```
Проверку работы Nginx можно с помощью команды: 
```bash
systemctl status nginx
```
или обратиться через браузер по ссылке [http://51.250.23.214/](http://51.250.23.214/) Должна открыться страница Nginx.
Проверку LightHouse можно сделать, обратившись через браузер по ссылке [http://51.250.23.214:9000/](http://51.250.23.214:9000/)

## Дополнение
Playbook, который использовался в моей домашней работе доступен [по ссылке](https://github.com/Seleznev-Ivan/devops-netology-ansible/tree/main/08-ansible-03-yandex/playbook)


