mysqlfs - файловая система пользовательского окружения (FUSE)
=============================================================
Позволяет хранить файлы в БД MySQL/MariaDB, что вкупе с возможностью 
кластеризации БД дает возможность построения отказоустойчивых файловых
кластеров для приложений, не поддерживающих такую возможность.

Здесь хранятся собранные пакеты для Debian
Исходный код и документацию можно найти по адресу
[https://github.com/skeyby/mysqlfs](https://github.com/skeyby/mysqlfs)

УСТАНОВКА
---------
    #> dpkg -i <имя пакета>

УДАЛЕНИЕ
--------
    #> dpkg -r mysqlfs

ИСПОЛЬЗОВАНИЕ
-------------
### А. Инициализация БД

    #> mysql
    mysql> CREATE DATABASE mysqlfs;
    mysql> GRANT ALL PRIVILEGES ON mysqlfs.* TO mysqlfs@"%" IDENTIFIED BY 'pass';
    mysql> FLUSH PRIVILEGES;
    mysql> \q
    #> mysqlfs_setup

* далее ответить на вопросы программы
* имена базы, пользователя, хоста и пароль можно менять произвольно, главное помнить,
что при ответах на вопросы mysql_setup и в параметрах подключения позже будут использоваться
те значения, которые вы указали при создании БД

### Б. Подключение ФС к БД

#### 1. Из командной строки
    #> mysqlfs -ohost=<host> -ouser=<user> -opassword=<pass> -odatabase=<mysqlfs> /mnt/fs

#### 2. Автозагрузка (добавить в /etc/fstab строку)
    mysqlfs /mnt/fs fuse host=<host>,user=<user>,password=<pass>,database=<mysqlfs>,allow_other,big_writes,x-systemd.automount 0 2
