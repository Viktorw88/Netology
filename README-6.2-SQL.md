# Домашнее задание к занятию "6.2. SQL"

## Введение

Перед выполнением задания вы можете ознакомиться с 
[дополнительными материалами](https://github.com/netology-code/virt-homeworks/tree/master/additional/README.md).

## Задача 1

Используя docker поднимите инстанс PostgreSQL (версию 12) c 2 volume, 
в который будут складываться данные БД и бэкапы.

Приведите получившуюся команду или docker-compose манифест.

Ответ:
```
docker-compose.yml: 

version: "3.9"

volumes:
  pgresdata:  
  backup:

services:
  
  postgressql:
    image: postgres:12-bullseye 
    container_name: postgre-docker
    user: postgres
    environment:
      - PGDATA=/var/lib/postgresql/data/
      - POSTGRES_PASSWORD=pwdpg
    volumes:
      - pgresdata:/var/lib/postgresql/data
      - backup:/backup
    network_mode: "host"
 
    Далее:

docker-compose up -d    
docker exec -it postgre-docker bash
psql -U postgres

postgres=# \l
```
![1new](https://user-images.githubusercontent.com/94568542/167464772-a048e5ba-3982-45d9-a4b6-f8c8082a8e55.jpg)





## Задача 2

В БД из задачи 1: 
- создайте пользователя test-admin-user и БД test_db
- в БД test_db создайте таблицу orders и clients (спeцификация таблиц ниже)
- предоставьте привилегии на все операции пользователю test-admin-user на таблицы БД test_db
- создайте пользователя test-simple-user  
- предоставьте пользователю test-simple-user права на SELECT/INSERT/UPDATE/DELETE данных таблиц БД test_db

Таблица orders:
- id (serial primary key)
- наименование (string)
- цена (integer)

Таблица clients:
- id (serial primary key)
- фамилия (string)
- страна проживания (string, index)
- заказ (foreign key orders)

Приведите:
- итоговый список БД после выполнения пунктов выше,
- описание таблиц (describe)
- SQL-запрос для выдачи списка пользователей с правами над таблицами test_db
- список пользователей с правами над таблицами test_db

Ответ:
```
CREATE DATABASE test_db;
\c test_db
CREATE ROLE "test-admin-user" SUPERUSER NOCREATEDB NOCREATEROLE NOINHERIT LOGIN;

CREATE TABLE orders 
(id integer,name text,price integer,PRIMARY KEY (id));

CREATE TABLE clients 
(id integer PRIMARY KEY, lastname text, country text, booking integer, FOREIGN KEY (booking) REFERENCES orders (Id));

CREATE ROLE "test-simple-user" NOSUPERUSER NOCREATEDB NOCREATEROLE NOINHERIT LOGIN;
GRANT SELECT ON TABLE public.orders TO "test-simple-user";
GRANT INSERT ON TABLE public.orders TO "test-simple-user";
GRANT UPDATE ON TABLE public.orders TO "test-simple-user";
GRANT DELETE ON TABLE public.orders TO "test-simple-user";
GRANT SELECT ON TABLE public.clients TO "test-simple-user";
GRANT INSERT ON TABLE public.clients TO "test-simple-user";
GRANT UPDATE ON TABLE public.clients TO "test-simple-user";
GRANT DELETE ON TABLE public.clients TO "test-simple-user";


- SQL-запрос для выдачи списка пользователей с правами над таблицами test_db
select * from information_schema.table_privileges where grantee in ('test-admin-user', 'test-simple-user');
```

итоговый список БД после выполнения пунктов выше:
![1-2](https://user-images.githubusercontent.com/94568542/167300592-2d1758ac-cb83-44e4-b00c-26201236ed0b.jpg)

список пользователей с правами над таблицами test_db:
![1-3](https://user-images.githubusercontent.com/94568542/167300826-dfc4595d-baad-484d-8743-8aea1d3d9827.jpg)



## Задача 3

Используя SQL синтаксис - наполните таблицы следующими тестовыми данными:

Таблица orders

|Наименование|цена|
|------------|----|
|Шоколад| 10 |
|Принтер| 3000 |
|Книга| 500 |
|Монитор| 7000|
|Гитара| 4000|

Таблица clients

|ФИО|Страна проживания|
|------------|----|
|Иванов Иван Иванович| USA |
|Петров Петр Петрович| Canada |
|Иоганн Себастьян Бах| Japan |
|Ронни Джеймс Дио| Russia|
|Ritchie Blackmore| Russia|

Используя SQL синтаксис:
- вычислите количество записей для каждой таблицы 
- приведите в ответе:
    - запросы 
    - результаты их выполнения.

Ответ:
```
insert into orders VALUES (1, 'Шоколад', 10), (2, 'Принтер', 3000), (3, 'Книга', 500), (4, 'Монитор', 7000), (5, 'Гитара', 4000);

insert into clients VALUES (1, 'Иванов Иван Иванович', 'USA'), (2, 'Петров Петр Петрович', 'Canada'), (3, 'Иоганн Себастьян Бах', 'Japan'), (4, 'Ронни Джеймс Дио', 'Russia'), (5, 'Ritchie Blackmore', 'Russia');

select count (*) from orders;
select count (*) from clients;
```
![3](https://user-images.githubusercontent.com/94568542/167300879-064fe7e8-2cce-4f13-92d3-3cff2d77d51f.jpg)

## Задача 4

Часть пользователей из таблицы clients решили оформить заказы из таблицы orders.

Используя foreign keys свяжите записи из таблиц, согласно таблице:

|ФИО|Заказ|
|------------|----|
|Иванов Иван Иванович| Книга |
|Петров Петр Петрович| Монитор |
|Иоганн Себастьян Бах| Гитара |

Приведите SQL-запросы для выполнения данных операций.

Приведите SQL-запрос для выдачи всех пользователей, которые совершили заказ, а также вывод данного запроса.
 
Подсказк - используйте директиву `UPDATE`.

Ответ:
```
update  clients set booking = 3 where id = 1;
update  clients set booking = 4 where id = 2;
update  clients set booking = 5 where id = 3;
```
SQL-запрос для выдачи всех пользователей, которые совершили заказ (первый вариант) -  учитывает наличие в таблице заказов данных:
```
select * from clients as c where  exists (select id from orders as o where c.booking = o.id);
```

SQL-запрос для выдачи всех пользователей, которые совершили заказ (второй вариант) - проверяет на непустые значения у клиентов по заказам:
```
select * from clients where booking is not null;
```
![4](https://user-images.githubusercontent.com/94568542/167301036-92de1851-e63e-47e4-93f4-3042873de776.jpg)


## Задача 5

Получите полную информацию по выполнению запроса выдачи всех пользователей из задачи 4 
(используя директиву EXPLAIN).

Приведите получившийся результат и объясните что значат полученные значения.
Ответ:
```
explain select * from clients as c where  exists (select id from orders as o where c.booking = o.id);
Вариант 1
Показывает cost-стоимость(время) на выполнение/нагрузку на исполнение запроса (неоптимальный в сравнении со вторым вариантом)
Показывает шаги связи, и сбор сканирование таблиц после связи


explain select * from clients where booking is not null;
Вариант 2
Так же показывает cost-стоимость(время) на выполнение/нагрузку на исполнение запроса, и фильтрацию по полю Booking для выборки.

Как видно "Вариант 2" оптимальнее
```
![5](https://user-images.githubusercontent.com/94568542/167302067-5b2cfd13-6528-443d-a25c-1568e86c73a0.jpg)


## Задача 6

Создайте бэкап БД test_db и поместите его в volume, предназначенный для бэкапов (см. Задачу 1).

Остановите контейнер с PostgreSQL (но не удаляйте volumes).

Поднимите новый пустой контейнер с PostgreSQL.

Восстановите БД test_db в новом контейнере.

Приведите список операций, который вы применяли для бэкапа данных и восстановления. 

Ответ:
```
Для бэкапа данных:
docker exec -t postgre-docker pg_dumpall -U postgres > /mnt/c/Users/Wizz/backup/dump2.sql 

Для восстановления:
cat /mnt/c/Users/Wizz/backup/dump2.sql | docker exec -i pgdocker psql -U postgres
```

### Как cдавать задание

Выполненное домашнее задание пришлите ссылкой на .md-файл в вашем репозитории.

---
