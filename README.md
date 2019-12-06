# mysql-bash-partitioning
Скрипт для автоматического создания партиций по дням в MySQL 5.6+ для Zabbix v3.2 и выше.

# Описание
После каждой установки Zabbix, лучше сразу провести партиционирование таблиц с данными истории для более гибкого управления.

Скрипт создаёт:
- партиции по дням как для таблиц с уже существующими данными, так и для пустых;
- таблицы в MySQL для управления партициями;
- заполняет таблицы информацией для хранения партиций согласно значениям из переменных;
- создаёт процедуры MySQL по созданию и удалению партиций;
- создаёт планировщик для автоматического запуска.

# Таблицы, подлежащие партиционированию:
history
history_uint
history_str
history_text
history_log
trends
trends_uint

# Требования для успешного выполнения скрипта
0) Доступ в MySQL без ввода пароля для пользователя, от которого будет запускаться скрипт (или же поправить в самом скрипте доступ)
1) Возможность записи в директорию /tmp для хранения промежуточных данных с SQL-запросами
2) Заполненные переменные history_..._cnt и trends_cnt для нужного времени хранения партиций каждой партиционируемой таблицы. По умолчанию значения уже заданы, подлежат изменению
3) Включенный планировщик в my.cnf или через SET GLOBAL event_scheduler = ON (надо прописать в конфиг);
4) Заполненная переменная evnt_start с датой и временем запуска планировщика в MySQL. По умолчанию также уже заполнена.

# После выполнения скрипта
Отключить хаускипер в веб-интерфейсе забикса для истории и динамики изменений.

# Примечания
Перед выполнением не забывайте сделать бэкап всего и вся!

Лучше использовать MySQL 5.6+, для MySQL 5.5 будет ошибка Invalid default value for ‘partition_action_date’, подробнее тут: https://dba.stackexchange.com/questions/132951/cant-default-date-to-current-timestamp-in-mysql-5-5
