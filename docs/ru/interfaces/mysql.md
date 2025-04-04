---
slug: /ru/interfaces/mysql
sidebar_position: 20
sidebar_label: "MySQL-интерфейс"
---

# MySQL-интерфейс {#mysql-interface}

ClickHouse поддерживает взаимодействие по протоколу MySQL. Данная функция включается настройкой [mysql_port](../operations/server-configuration-parameters/settings.md#server_configuration_parameters-mysql_port) в конфигурационном файле:

``` xml
<mysql_port>9004</mysql_port>
```

Пример подключения с помощью стандартного клиента mysql:

``` bash
$ mysql --protocol tcp -u default -P 9004
```

Вывод в случае успешного подключения:

``` text
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 4
Server version: 20.2.1.1-ClickHouse

Copyright (c) 2000, 2019, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql>
```

Для совместимости со всеми клиентами рекомендуется задавать пароль пользователя в конфигурационном файле с помощью двойного хэша [SHA1](/operations/settings/settings-users#user-namepassword).
В случае указания пароля с помощью [SHA256](/sql-reference/functions/hash-functions#sha1-sha224-sha256-sha512-sha512_256) некоторые клиенты не смогут пройти аутентификацию (mysqljs и старые версии стандартного клиента mysql).

Ограничения:

-   не поддерживаются подготовленные запросы

-   некоторые типы данных отправляются как строки

Чтобы прервать долго выполняемый запрос, используйте запрос `KILL QUERY connection_id` (во время выполнения он будет заменен на `KILL QUERY WHERE query_id = connection_id`). Например:

``` bash
$ mysql --protocol tcp -h mysql_server -P 9004 default -u default --password=123 -e "KILL QUERY 123456;"
```