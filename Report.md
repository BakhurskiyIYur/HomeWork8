# Домашнее задание №8 (Тема "Настройка PostgreSQL")

  > <img src="pic/1.JPG" align="center" />

Описание/Пошаговая инструкция выполнения домашнего задания:

* 1 Развернуть виртуальную машину любым удобным способом
* 2 Поставить на неё PostgreSQL 15 любым способом
* 3 Настроить кластер PostgreSQL 15 на максимальную производительность не обращая внимание на возможные проблемы с надежностью в случае аварийной перезагрузки виртуальной машины

ALTER SYSTEM set checkpoint_timeout to '30s'; --сделать создание точек восстановления раз в 20 минут, чтобы на время тестирования процесс создания точек восстановления не пересекался с создаваемой нагрузкой.
Show wal_buffers; (4MB)
 alter system set wal_buffers to '16MB';
Show max_wal_size; (1GB)
alter system set max_wal_size to '5GB';
Show min_wal_size; (80MB)
alter system set min_wal_size to '1GB';
Show checkpoint_timeout; (5min)
alter system set checkpoint_timeout to '59min';
Show work_mem; (4MB)
alter system set work_mem to '6553kb'; --'16MB';
Show shared_buffers; (128MB)
alter system set shared_buffers to '1GB';
Show maintenance_work_mem; (64MB)
alter system set maintenance_work_mem to '2GB';
Show Max_connections; (100)
alter system set Max_connections to '5';
Show effective_cache_size; (4GB)
alter system set effective_cache_size to '15GB';
Show synchronous_commit;
alter system set synchronous_commit to 'off';
SELECT pg_reload_conf();
alter system set autovacuum to 'off';

select sourceline, name, setting, applied from pg_file_settings order by applied;
* 4 Нагрузить кластер через утилиту через утилиту pgbench (https://postgrespro.ru/docs/postgrespro/14/pgbench)
sudo apt-get install postgresql-contrib
sudo su postgres
pgbench -i postgres -- Требуется для вызова режима инициализации.

pgbench -c8 -j 2 -P 60 -T 600 -U postgres postgres
-c8 Количество моделируемых клиентов, т. е. количество одновременных сеансов базы данных
-j 2 Количество рабочих потоков в pgbench. Использование нескольких потоков может быть полезно на многопроцессорных машинах. Клиенты распределяются максимально равномерно по доступным потокам. Значение по умолчанию — 1.
-P 60  Показывайте отчет о ходе выполнения каждые секунды
-T 600  Выполняйте тест в течение этого количества секунд, а не фиксированного количества транзакций на клиента.
-U postgres postgres  Имя пользователя, к которому необходимо подключиться

* 5 Написать какого значения tps удалось достичь, показать какие параметры в какие значения устанавливали и почему


