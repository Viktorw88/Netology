# Домашнее задание к занятию "4.2. Использование Python для решения типовых DevOps задач"

## Обязательная задача 1

Есть скрипт:
```python
#!/usr/bin/env python3
a = 1
b = '2'
c = a + b
```

### Вопросы:
| Вопрос  | Ответ |
| ------------- | ------------- |
| Какое значение будет присвоено переменной `c`?  | Будет ошибка, т.к. типы переменных не соответствуют для операции, попытка сложить разные типы переменных int и str  |
| Как получить для переменной `c` значение 12?  | Привести "a" к строке: c=str(a)+b  |
| Как получить для переменной `c` значение 3?  | Привести "b" к целому числу: c=a+int(b)  |

## Обязательная задача 2
Мы устроились на работу в компанию, где раньше уже был DevOps Engineer. Он написал скрипт, позволяющий узнать, какие файлы модифицированы в репозитории, относительно локальных изменений. Этим скриптом недовольно начальство, потому что в его выводе есть не все изменённые файлы, а также непонятен полный путь к директории, где они находятся. Как можно доработать скрипт ниже, чтобы он исполнял требования вашего руководителя?

```python
#!/usr/bin/env python3

import os

bash_command = ["cd ~/netology/sysadm-homeworks", "git status"]
result_os = os.popen(' && '.join(bash_command)).read()
is_change = False
for result in result_os.split('\n'):
    if result.find('modified') != -1:
        prepare_result = result.replace('\tmodified:   ', '')
        print(prepare_result)
        break
```

### Ваш скрипт:
```python
#!/usr/bin/env python3

import os

bash_command = ["cd ~/Netology", "git status"]
result_os = os.popen(' && '.join(bash_command)).read()
#is_change = False
for result in result_os.split('\n'):
    if result.find('modified') != -1:
        prepare_result = result.replace('\tmodified:   ', '')
        print(os.path.abspath(prepare_result))
#break
```

Присутствует лишняя логическая переменная "is_change"
Также лишняя команда break, так как break прерывает обработку при первом же найденом вхождении


### Вывод скрипта при запуске при тестировании:

![22](https://user-images.githubusercontent.com/94568542/150192306-302c41f0-ed88-4991-9d2a-16789c670e1b.jpg)


## Обязательная задача 3
1. Доработать скрипт выше так, чтобы он мог проверять не только локальный репозиторий в текущей директории, а также умел воспринимать путь к репозиторию, который мы передаём как входной параметр. Мы точно знаем, что начальство коварное и будет проверять работу этого скрипта в директориях, которые не являются локальными репозиториями.

### Ваш скрипт:
```python
#!/usr/bin/env python3

import os
import sys

cmd = os.getcwd()

if len(sys.argv)>=2:
    cmd = sys.argv[1]
bash_command = ["cd "+cmd, "git status 2>&1"]

print('\033[36m')
result_os = os.popen(' && '.join(bash_command)).read()
#is_change = False
for result in result_os.split('\n'):
    if result.find('fatal') != -1:
        print('\033[36m Каталог \033[1m '+cmd+'\033[0m\033[36m не является GIT репозиторием\033[0m')    
    if result.find('modified') != -1:
        prep_result = result.replace('\tmodified: ', '')

        prep_result = prep_result.replace(' ', '') 
        # replace-замена всех оставшихся пробелов в строке в выводе
        print(cmd+prep_result)
#break
print('\033[0m')
```

Добавлена обработка наличия входных параметров:
при наличии параметра берется введенный каталог; 
при отсутствии параметра берется текущий рабочий каталог; 
при ошибке наличия GIT каталог отмечается ошибкой другого цвета

### Вывод скрипта при запуске при тестировании:

![35555](https://user-images.githubusercontent.com/94568542/150215145-3af5f4e8-1a1f-4409-8064-af8f48e7c84c.jpg)



## Обязательная задача 4
1. Наша команда разрабатывает несколько веб-сервисов, доступных по http. Мы точно знаем, что на их стенде нет никакой балансировки, кластеризации, за DNS прячется конкретный IP сервера, где установлен сервис. Проблема в том, что отдел, занимающийся нашей инфраструктурой очень часто меняет нам сервера, поэтому IP меняются примерно раз в неделю, при этом сервисы сохраняют за собой DNS имена. Это бы совсем никого не беспокоило, если бы несколько раз сервера не уезжали в такой сегмент сети нашей компании, который недоступен для разработчиков. Мы хотим написать скрипт, который опрашивает веб-сервисы, получает их IP, выводит информацию в стандартный вывод в виде: <URL сервиса> - <его IP>. Также, должна быть реализована возможность проверки текущего IP сервиса c его IP из предыдущей проверки. Если проверка будет провалена - оповестить об этом в стандартный вывод сообщением: [ERROR] <URL сервиса> IP mismatch: <старый IP> <Новый IP>. Будем считать, что наша разработка реализовала сервисы: `drive.google.com`, `mail.google.com`, `google.com`.

### Ваш скрипт:
```python
#!/usr/bin/env python3

import socket as s
import time as t
import datetime as dt

# ниже обозначаем переменные 
i = 1
wait = 3 # интервал проверок в секундах
services = {'drive.google.com':'0.0.0.0', 'mail.google.com':'0.0.0.0', 'google.com':'0.0.0.0'}
init=0

print('script starts')
print(services)
print(' ')

while 1==1 :  #число проверок для отладки
  for host in services:
    ip = s.gethostbyname(host)
    if ip != services[host]:
      if i==1 and init !=1:
        print(str(dt.datetime.now().strftime("%Y-%m-%d %H:%M:%S")) +' [ERROR] ' + str(host) +' IP mistmatch: '+services[host]+' '+ip)
      services[host]=ip
# закомментировать для бесконечного цикла следующие 3 строки
  i+=1 
  if i >= 40 : 
    break
  t.sleep(wait)
```

### Вывод скрипта при запуске при тестировании:


![444](https://user-images.githubusercontent.com/94568542/150223113-fef29daf-228b-4a24-b883-eca9cc18122a.jpg)


