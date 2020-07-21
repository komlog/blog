# Установка песочницы FOLIO

Чтобы посмотреть на работающую версию FOLIO необязательно её устанавливать. Можно зайти в одну из [доступных онлайн-песочниц](https://folio-snapshot-stable.aws.indexdata.com/) и поэкспериментировать в ней.
  

## Требования к серверу

Установка стабильной версии платформы со стандартным набором приложений потребует минимум 10 Гб оперативной памяти. 

> Почему так много памяти? Ядро платформы состоит примерно из 25 бэкенд модулей (а всего около 50 модулей), для каждого из них создаётся образ размером около 160 Мб (в среднем). Некоторые из модулей требуют загрузки данных и создания базы данных.

## Параметры

В данном эксперименте используются следующие параметры:

Сервер под управлением Debian 9.12 x64

В качестве виртуальной машины мы выбрали Vagrant на базе VirtualBox.

## Пререквизиты

Для установки необходим пользователь с правами суперпользователя.

Не забудьте обновить пакеты, установленные в системе:

```
sudo apt update
sudo apt upgrade
```

## Установка VirtualBox

Для начала импортируем ключи GPG репозитория Oracle VirtualBox в систему с помощью следующих команд wget:

```
wget -q https://www.virtualbox.org/download/oracle_vbox_2016.asc -O- | sudo apt-key add -
wget -q https://www.virtualbox.org/download/oracle_vbox.asc -O- | sudo apt-key add -
```

Обе команды должны выдать `OK`.

Добавляем репозиторий VirtualBox в список доступных источников:

```
sudo add-apt-repository "deb [arch=amd64] http://download.virtualbox.org/virtualbox/debian $(lsb_release -cs) contrib"
```

Переменная $(lsb_release -cs) напечатает кодовое имя Debian. В нашем случае, это был stretch.

В случае сообщения об ошибке `add-apt-repository command not found` установите пакет `software-properties-common`.

Теперь можно установить пакет VirtualBox

```
sudo apt update && sudo apt install virtualbox-6.0
```

## Установка Vagrant

Загружаем пакет Vagrant с помощью команды

```
curl -O https://releases.hashicorp.com/vagrant/2.2.6/vagrant_2.2.6_x86_64.deb
```

Устанавливаем пакет

```
sudo apt install ./vagrant_2.2.6_x86_64.deb
```

Проверить установку можно с помощью команды

```
vagrant --version
```


## Запуск виртуальной машины с образом FOLIO

На сайте [Vagrant Cloud](https://app.vagrantup.com/folio) можно выбрать один из образов платформы FOLIO.

Например, можно установить `folio/snapshot`

```
vagrant init folio/snapshot
vagrant up
```

> Если у вас ограниченные мощности сервера, можно установить версию `folio/snapshot-core` с ограниченным набором приложений. В нашем случае её удалось поднять на сервере с 8Гб оперативной памяти.

Сервер будет доступен по адресу `http://localhost:3000/about`

Логин: diku_admin

Пароль: admin



Несколько команд для работы с виртуальным сервером:

* Запуск виртуальной машины

```
vagrant up
```

* Соединение с виртуальной машиной по ssh
```
vagrant ssh
```

* Принудительный останов виртуальной машины
```
vagrant halt
```

* Уничтожение виртуальной машины
```
vagrant destroy
```

## Ссылки

1. [How to Install VirtualBox on Debian Linux 9](https://linuxize.com/post/how-to-install-virtualbox-on-debian-9/)

1. [How to Install Vagrant on Debian 9](https://linuxize.com/post/how-to-install-vagrant-on-debian-9/)

1. [Vagrant Cloud - FOLIO](https://app.vagrantup.com/folio)

1. [FOLIO deployment: single server](https://github.com/folio-org/folio-install/tree/master/runbooks/single-server#folio-deployment-single-server)

1. [FOLIO snapshot demo](https://folio-snapshot-stable.aws.indexdata.com/)
