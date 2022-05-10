# Домашнее задание к занятию "6.3. MySQL"

## Введение

Перед выполнением задания вы можете ознакомиться с 
[дополнительными материалами](https://github.com/netology-code/virt-homeworks/tree/master/additional/README.md).

## Задача 1

Используя docker поднимите инстанс MySQL (версию 8). Данные БД сохраните в volume.

Изучите [бэкап БД](https://github.com/netology-code/virt-homeworks/tree/master/06-db-03-mysql/test_data) и 
восстановитесь из него.

Перейдите в управляющую консоль `mysql` внутри контейнера.

Используя команду `\h` получите список управляющих команд.

Найдите команду для выдачи статуса БД и **приведите в ответе** из ее вывода версию сервера БД.

Подключитесь к восстановленной БД и получите список таблиц из этой БД.

**Приведите в ответе** количество записей с `price` > 300.

В следующих заданиях мы будем продолжать работу с данным контейнером.

Ответ:
![11](https://user-images.githubusercontent.com/94568542/167471057-a4bee151-102c-48ca-8371-93f29a73109f.jpg)



## Задача 2

Создайте пользователя test в БД c паролем test-pass, используя:
- плагин авторизации mysql_native_password
- срок истечения пароля - 180 дней 
- количество попыток авторизации - 3 
- максимальное количество запросов в час - 100
- аттрибуты пользователя:
    - Фамилия "Pretty"
    - Имя "James"

Предоставьте привелегии пользователю `test` на операции SELECT базы `test_db`.
    
Используя таблицу INFORMATION_SCHEMA.USER_ATTRIBUTES получите данные по пользователю `test` и 
**приведите в ответе к задаче**.

Ответ:

Создание пользователя
```
CREATE USER 'test'@'localhost' IDENTIFIED BY 'test-pass';

ALTER USER 'test'@'localhost' ATTRIBUTE '{"fname":"James", "lname":"Pretty"}';

ALTER USER 'test'@'localhost' 
    -> IDENTIFIED BY 'test-pass' 
    -> WITH
    -> MAX_QUERIES_PER_HOUR 100
    -> PASSWORD EXPIRE INTERVAL 180 DAY
    -> FAILED_LOGIN_ATTEMPTS 3 PASSWORD_LOCK_TIME 2;
```
Предоставление прав доступа
```
GRANT SELECT ON test_db.orders TO 'test'@'localhost';
```
Данные по пользователю test
![22](https://user-images.githubusercontent.com/94568542/167682248-5ad0f680-ba44-4822-bfad-b8748f24408f.jpg)



## Задача 3

Установите профилирование `SET profiling = 1`.
Изучите вывод профилирования команд `SHOW PROFILES;`.

Исследуйте, какой `engine` используется в таблице БД `test_db` и **приведите в ответе**.

Измените `engine` и **приведите время выполнения и запрос на изменения из профайлера в ответе**:
- на `MyISAM`
- на `InnoDB`

Ответ:
```
 В таблице БД `test_db` используется engine InnoDB:
```
![31](https://user-images.githubusercontent.com/94568542/167685240-b79fe72e-141e-449f-82c8-598a9ca511f4.jpg)

```
Запросы на изменения из профайлера:
```
![33](https://user-images.githubusercontent.com/94568542/167685384-cda0e390-6bf5-43a9-a14c-6f82b40bbb5d.jpg)
```
Продолжительность переключения на MyISAM: 0.032
Продолжительность переключения на InnoDB: 0.051
```

## Задача 4 

Изучите файл `my.cnf` в директории /etc/mysql.

Измените его согласно ТЗ (движок InnoDB):
- 1 Скорость IO важнее сохранности данных
- 2 Нужна компрессия таблиц для экономии места на диске
- 3 Размер буффера с незакомиченными транзакциями 1 Мб
- 4 Буффер кеширования 30% от ОЗУ
- 5 Размер файла логов операций 100 Мб

Приведите в ответе измененный файл `my.cnf`.

Ответ:
```
[mysqld]
pid-file        = /var/run/mysqld/mysqld.pid
socket          = /var/run/mysqld/mysqld.sock
datadir         = /var/lib/mysql
secure-file-priv= NULL

# 1
innodb_flush_log_at_trx_commit = 0 

# 2
innodb_file_format = Barracuda

# 3
innodb_log_buffer_size = 1M

# 4
key_buffer_size = 1280М

# 5
max_binlog_size	= 100M
```



---

### Как оформить ДЗ?

Выполненное домашнее задание пришлите ссылкой на .md-файл в вашем репозитории.

---
