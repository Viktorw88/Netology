Домашнее задание к занятию "3.3. Операционные системы, лекция 1"

1.На лекции мы познакомились с node_exporter. В демонстрации его исполняемый файл запускался в background. Этого достаточно для демо, но не для настоящей production-системы, где процессы должны находиться под внешним управлением. Используя знания из лекции по systemd, создайте самостоятельно простой unit-файл для node_exporter:
поместите его в автозагрузку,
предусмотрите возможность добавления опций к запускаемому процессу через внешний файл (посмотрите, например, на systemctl cat cron),
удостоверьтесь, что с помощью systemctl процесс корректно стартует, завершается, а после перезагрузки автоматически поднимается.
```
Ответ:
Установил node_exporter, 

"предусмотрите возможность добавления опций к запускаемому процессу через внешний файл (посмотрите, например, на systemctl cat cron)", 
посмотрел пример с systemctl cat cron и доп. материалы, 
```

![999](https://user-images.githubusercontent.com/94568542/149830196-1304f469-7cc4-488a-b4c0-1069add8e739.jpg)

```
$OPTIONS - позволяет добавлять опции к запускаемому процессу через внешний файл

Прописал unit файл:

[Unit]
Description=Node Exporter

[Service]
EnvironmentFile=-/etc/default/node_exporter
ExecStart=/usr/local/bin/node_exporter $OPTIONS

[Install]
WantedBy=multi-user.target

Перезапускаю сервис:
$ sudo systemctl daemon-reload
$ sudo systemctl start node_exporter

Добавляю его в автозагрузку:
$ sudo systemctl enable node_exporter

Проверяю статус процесса:
$ sudo systemctl status node_exporter
```
![122](https://user-images.githubusercontent.com/94568542/149415287-ff726ab8-5e23-48fa-85ba-8b7c4c65daf3.jpg)
```
Порт  9100 пробросил на хостовую машину:
```
![19100](https://user-images.githubusercontent.com/94568542/149414411-b5caae85-796d-4e15-9d68-8ec4be07850e.jpg)
```
Сервис стартует и перезапускается корректно
1. Проверка после перезапуска работы процесса
2. Остановка
3. Проверка работы процесса
4. Запуск процесса 
5. Проверка работы процесса
```
![111155](https://user-images.githubusercontent.com/94568542/149417836-08d8c86f-b4e4-403f-87f2-8d5e095a5ceb.jpg)

2. Ознакомьтесь с опциями node_exporter и выводом /metrics по-умолчанию. 
Приведите несколько опций, которые вы бы выбрали для базового мониторинга хоста по CPU, памяти, диску и сети.
```
Ответ:

$ curl -s http://localhost:9100/metrics
Самые полезные на мой взгляд будут такие:
CPU:
    node_cpu_seconds_total{cpu="1",mode="idle"} 6506.5
    node_cpu_seconds_total{cpu="1",mode="system"} 31.04
    node_cpu_seconds_total{cpu="1",mode="user"} 3.68
    process_cpu_seconds_total 
    
Memory:
    node_memory_MemAvailable_bytes 
    node_memory_MemFree_bytes
    
Disk(если несколько дисков, то для каждого):
    node_disk_io_time_seconds_total{device="sda"} 
    node_disk_read_bytes_total{device="sda"} 
    node_disk_read_time_seconds_total{device="sda"} 
    node_disk_write_time_seconds_total{device="sda"}
    
Network(так же для каждого активного адаптера):
    node_network_receive_errs_total{device="eth0"} 
    node_network_receive_bytes_total{device="eth0"} 
    node_network_transmit_bytes_total{device="eth0"}
    node_network_transmit_errs_total{device="eth0"}
```
![2221](https://user-images.githubusercontent.com/94568542/149416851-002aa8d2-66c9-448b-ad95-010224a0aa85.jpg)


3. Установите в свою виртуальную машину Netdata. Воспользуйтесь готовыми пакетами для установки (sudo apt install -y netdata). После успешной установки:
в конфигурационном файле /etc/netdata/netdata.conf в секции [web] замените значение с localhost на bind to = 0.0.0.0,
добавьте в Vagrantfile проброс порта Netdata на свой локальный компьютер и сделайте vagrant reload:
```
config.vm.network "forwarded_port", guest: 19999, host: 19999
```
После успешной перезагрузки в браузере на своем ПК (не в виртуальной машине) вы должны суметь зайти на localhost:19999. 
Ознакомьтесь с метриками, которые по умолчанию собираются Netdata и с комментариями, которые даны к этим метрикам.

Ответ:
![444](https://user-images.githubusercontent.com/94568542/149564033-deaf21df-b8e8-4fae-aec1-3f8932c99465.jpg)


4. Можно ли по выводу dmesg понять, осознает ли ОС, что загружена не на настоящем оборудовании, а на системе виртуализации?
```
Ответ:
Да, по выводу dmesg можно понять, что ОС "осознает" что загружена на системе виртуализации
```
![4](https://user-images.githubusercontent.com/94568542/149564573-3e1eae9b-55e1-4c3d-9cf0-2d67a58b09c1.jpg)


5. Как настроен sysctl fs.nr_open на системе по-умолчанию? Узнайте, что означает этот параметр. 
Какой другой существующий лимит не позволит достичь такого числа (ulimit --help)?
```
Ответ:

vagrant@vagrant:~$ /sbin/sysctl -n fs.nr_open
1048576

1048576 - это максимальное число открытых дескрипторов для ядра (системы), для пользователя задать 
больше этого числа нельзя (если не менять). 
Число задается кратное 1024, в данном случае = 1024*1024

Но макс.предел ОС можно посмотреть так:
vagrant@vagrant:~$ cat /proc/sys/fs/file-max
9223372036854775807

vagrant@vagrant:~$ ulimit -Sn
1024
Мягкий лимит (также ulimit -n) на пользователя (может быть увеличен процессом во время работы)

vagrant@vagrant:~$ ulimit -Hn
1048576
Жесткий лимит на пользователя (не может быть увеличен, только уменьшен)
Оба ulimit -n НЕ могут превысить системный fs.nr_open
```
![5](https://user-images.githubusercontent.com/94568542/149567455-64b12a9b-ced7-452e-ba0c-39f36933b985.jpg)


6. Запустите любой долгоживущий процесс (не ls, который отработает мгновенно, а, например, sleep 1h) в отдельном неймспейсе процессов; 
покажите, что ваш процесс работает под PID 1 через nsenter. Для простоты работайте в данном задании под root (sudo -i). 
Под обычным пользователем требуются дополнительные опции (--map-root-user) и т.д.

Ответ:
![6](https://user-images.githubusercontent.com/94568542/149568504-54b97927-e9e3-4d90-86a0-c4fb68431777.jpg)


7. Найдите информацию о том, что такое :(){ :|:& };:. Запустите эту команду в своей виртуальной машине Vagrant с Ubuntu 20.04 (это важно, поведение в других ОС не проверялось).
Некоторое время все будет "плохо", после чего (минуты) – ОС должна стабилизироваться. Вызов dmesg расскажет, какой механизм помог автоматической стабилизации. 
Как настроен этот механизм по-умолчанию, и как изменить число процессов, которое можно создать в сессии?
```
Ответ:
Нашел информацию что это так называемая fork bomb - функция fork рекурсивно вызывающая сама себя до тех пор
пока не забьёт все ресурсы системы.
dmesg сообщает:
[ 3095.676761] cgroup: fork rejected by pids controller in /user.slice/user-1000.slice/session-3.scope

Судя по всему, система на основании этих файлов в пользовательской зоне ресурсов имеет определенное ограничение 
на создаваемые ресурсы и соответственно при превышении начинает блокировать создание 

Если установить например ulimit -u 40 - число процессов будет ограничено 40 для пользователя.
```
