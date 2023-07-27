

# Домашнее задание к занятию "Домашнее задание к занятию «Базы данных в облаке»" - Соловьёв Андрей SYS-18

---


## Задание 1.Создание кластера



Перейдите на главную страницу сервиса Managed Service for PostgreSQL.
Создайте кластер PostgreSQL со следующими параметрами:
класс хоста: s2.micro, диск network-ssd любого размера;
хосты: нужно создать два хоста в двух разных зонах доступности и указать необходимость публичного доступа, то есть публичного IP адреса, для них;
установите учётную запись для пользователя и базы.
Остальные параметры оставьте по умолчанию либо измените по своему усмотрению.

Нажмите кнопку «Создать кластер» и дождитесь окончания процесса создания, статус кластера = RUNNING. Кластер создаётся от 5 до 10 минут.

![Cluster_creating.PNG](https://github.com/Andrewsolo1969/12-9-hw/blob/main/img/Cluster_creating.PNG)


![CCluster_created.PNG](https://github.com/Andrewsolo1969/12-9-hw/blob/main/img/Cluster_created.PNG)



Подключение к мастеру и реплике


Используйте инструкцию по подключению к кластеру, доступную на вкладке «Обзор»: cкачайте SSL-сертификат и подключитесь к кластеру с помощью утилиты psql, указав hostname всех узлов и атрибут target_session_attrs=read-write.

![instruction.PNG](https://github.com/Andrewsolo1969/12-9-hw/blob/main/img/instruction.PNG)


![Cluster_connection.PNG](https://github.com/Andrewsolo1969/12-9-hw/blob/main/img/Cluster_connection.PNG)

Проверьте, что подключение прошло к master-узлу.

select case when pg_is_in_recovery() then 'REPLICA' else 'MASTER' end;

![master_connect.PNG](https://github.com/Andrewsolo1969/12-9-hw/blob/main/img/master_connect.PNG)


Посмотрите количество подключенных реплик:
select count(*) from pg_stat_replication;

![replica_count.PNG](https://github.com/Andrewsolo1969/12-9-hw/blob/main/img/replica_count.PNG)


Проверьте работоспособность репликации в кластере

Создайте таблицу и вставьте одну-две строки.

CREATE TABLE test_table(text varchar);
insert into test_table values('Строка 1');

Выйдите из psql командой \q.


![create_table.PNG](https://github.com/Andrewsolo1969/12-9-hw/blob/main/img/create_table.PNG)

Теперь подключитесь к узлу-реплике. Для этого из команды подключения удалите атрибут target_session_attrs и в параметре атрибут host передайте только имя хоста-реплики. Роли хостов можно посмотреть на соответствующей вкладке UI консоли.

Проверьте, что подключение прошло к узлу-реплике.

select case when pg_is_in_recovery() then 'REPLICA' else 'MASTER' end;

![replica.PNG](https://github.com/Andrewsolo1969/12-9-hw/blob/main/img/replica.PNG)


Проверьте состояние репликации
select status from pg_stat_wal_receiver;

Для проверки, что механизм репликации данных работает между зонами доступности облака, выполните запрос к таблице, созданной на предыдущем шаге:
select * from test_table;


![replica_select.PNG](https://github.com/Andrewsolo1969/12-9-hw/blob/main/img/replica_select.PNG)



















 
