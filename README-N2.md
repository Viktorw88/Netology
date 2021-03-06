Домашнее задание к занятию "Компьютерные сети, лекция 2"

1. Проверьте список доступных сетевых интерфейсов на вашем компьютере. Какие команды есть для этого в Linux и в Windows?

В Windows для этого есть команда ipconfig /all
В Linux команда ip. 
![2](https://user-images.githubusercontent.com/94568542/149670893-9ba66198-c813-43a8-a38d-d60ec0a10b8e.jpg)

2. Какой протокол используется для распознавания соседа по сетевому интерфейсу? Какой пакет и команды есть в Linux для этого?

Для этого используется команда lldpctl из пакета lldpd:

![21](https://user-images.githubusercontent.com/94568542/149671623-6376bca1-7b52-4e47-8cce-47e275fe7479.jpg)


3. Какая технология используется для разделения L2 коммутатора на несколько виртуальных сетей? Какой пакет и команды есть в Linux для этого? Приведите пример конфига.

![31](https://user-images.githubusercontent.com/94568542/149671998-decf092f-e289-4f56-9637-c6c2b5df6e19.jpg)

Добавил VLAN 5 на интерфейс eth0 и назначил ему новый IP адрес 10.0.0.3/24

![32](https://user-images.githubusercontent.com/94568542/149672006-6a2bc323-aa74-42fa-ba32-6bed8c367af7.jpg)

4. Какие типы агрегации интерфейсов есть в Linux? Какие опции есть для балансировки нагрузки? Приведите пример конфига.
```
Типы агрегации (объединения) интерфейсов в Linux
mode=0 (balance-rr)
Этот режим используется по-умолчанию, если в настройках не указано другое. balance-rr обеспечивает балансировку нагрузки и отказоустойчивость. В данном режиме пакеты отправляются "по кругу" от первого интерфейса к последнему и сначала. Если выходит из строя один из интерфейсов, пакеты отправляются на остальные оставшиеся.При подключении портов к разным коммутаторам, требует их настройки.

mode=1 (active-backup)
При active-backup один интерфейс работает в активном режиме, остальные в ожидающем. Если активный падает, управление передается одному из ожидающих. Не требует поддержки данной функциональности от коммутатора.

mode=2 (balance-xor)
Передача пакетов распределяется между объединенными интерфейсами по формуле ((MAC-адрес источника) XOR (MAC-адрес получателя)) % число интерфейсов. Один и тот же интерфейс работает с определённым получателем. Режим даёт балансировку нагрузки и отказоустойчивость.

mode=3 (broadcast)
Происходит передача во все объединенные интерфейсы, обеспечивая отказоустойчивость.

mode=4 (802.3ad)
Это динамическое объединение портов. В данном режиме можно получить значительное увеличение пропускной способности как входящего так и исходящего трафика, используя все объединенные интерфейсы. Требует поддержки режима от коммутатора, а так же (иногда) дополнительную настройку коммутатора.

mode=5 (balance-tlb)
Адаптивная балансировка нагрузки. При balance-tlb входящий трафик получается только активным интерфейсом, исходящий - распределяется в зависимости от текущей загрузки каждого интерфейса. Обеспечивается отказоустойчивость и распределение нагрузки исходящего трафика. Не требует специальной поддержки коммутатора.

mode=6 (balance-alb)
Адаптивная балансировка нагрузки (более совершенная). Обеспечивает балансировку нагрузки как исходящего (TLB, transmit load balancing), так и входящего трафика (для IPv4 через ARP). Не требует специальной поддержки коммутатором, но требует возможности изменять MAC-адрес устройства.
```

Файлы конфигурации сети для Ubuntu находятся по пути /etc/netplan/имяфайла.yaml
Если нужен bonding, приводим файл к виду:
```
bonds:
    bond0:
      dhcp4: no
      interfaces: [eth0, eth0.5]
      parameters: 
        mode: 802.3ad
        mii-monitor-interval: 1
```
bonds: — блок, поясняющий что мы будем настраивать bonding

bond0: — произвольное имя интерфейса

interfaces: — набор интерфейсов собираемых в bonding

parameters: — описываем блок настройки параметров

mode: — указываем мод по которому будет работать bonding.

mii-monitor-interval: — задаем интервал мониторинга 1 сек.
        

5. Сколько IP адресов в сети с маской /29 ? Сколько /29 подсетей можно получить из сети с маской /24. Приведите несколько примеров /29 подсетей внутри сети 10.10.10.0/24.

ipcalc для сети 10.10.10.0/29:

![51](https://user-images.githubusercontent.com/94568542/149673150-a97fdb61-c5fc-484a-a55f-983f6b9f5c18.jpg)


В этой подсети 8 адресов, включая адрес подсети и броадкаст. На хосты остается 6 адресов.

ipcalc для сети 10.10.10.0/24:

![52](https://user-images.githubusercontent.com/94568542/149673220-f2370b2e-b9ff-4c48-96e6-2ac2c429940c.jpg)

В /24 подсети 256 адресов, значит, подсеть 10.10.10.0/24 можно разделить на 32 подсети 10.10.10.0/29
Подсети будут такими: 
Network: 10.10.10.0/29 Network: 10.10.10.8/29
Network: 10.10.10.16/29 и т.д. до Network: 10.10.10.248/29

6. Задача: вас попросили организовать стык между 2-мя организациями. Диапазоны 10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16 уже заняты. Из какой подсети допустимо взять частные IP адреса? Маску выберите из расчета максимум 40-50 хостов внутри подсети.

Есть ещё диапазон 100.64.0.0 — 100.127.255.255 с маской 255.192.0.0 (/10)
Если нам необходимы подсети размером 50 хостов, можно посчитать так:

![6](https://user-images.githubusercontent.com/94568542/149673483-e869cc75-b971-447a-9e97-125be234c1b6.jpg)

Оптимальным решением будут подсети /26 по 64 хоста.

7. Как проверить ARP таблицу в Linux, Windows? Как очистить ARP кеш полностью? Как из ARP таблицы удалить только один нужный IP?

В Windows это команда arp -a:
![71](https://user-images.githubusercontent.com/94568542/149673536-cd626797-a8f3-4b81-b1d1-17feca9b9808.jpg)

В Linux есть утилита arp входящая в пакет net-tools. 
![72](https://user-images.githubusercontent.com/94568542/149673622-3da389ac-78c2-4814-9de1-4825049f6bd8.jpg)


В Windows чтобы очистить ARP-кэш, необходимо выполнить команду: netsh interface ip delete arpcache

Удаление одого адреса из таблицы:

![73](https://user-images.githubusercontent.com/94568542/149673842-27189ec0-7513-437f-8850-dd260bd56a05.jpg)

Удаление всех записей:

![755](https://user-images.githubusercontent.com/94568542/149673921-33294da4-db82-4f96-8065-b554f0cbcac7.jpg)


