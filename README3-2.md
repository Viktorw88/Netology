Домашнее задание к занятию "3.2. Работа в терминале, лекция 2"

1. Какого типа команда cd? Попробуйте объяснить, почему она именно такого типа; опишите ход своих мыслей, если считаете что она могла бы быть другого типа.
```
Ответ:
cd - встроенная команда.
Встроенная, потому что, при работе внутри сессии терминала логичнее менять указатель на текущую директорию внутренней функцией

Если использовать внешний вызов, то он будет работать со своим окружением, и менять текущий каталог внутри своего окружения, 
на вызвавший shell влиять не будет  

Теоретически можно сделать cd внешней программой, но после смены директории потребуется вызвать bash из нового каталога, но тогда 
мы получим новый shell.
И выходя из сессии придется выходить из всех сессий, которые создали при каждом вызове внешней cd.
```

2. Какая альтернатива без pipe команде grep <some_string> <some_file> | wc -l? man grep поможет в ответе на этот вопрос. 
Ознакомьтесь с документом о других подобных некорректных вариантах использования pipe.
```
Ответ:
grep <some_string> <some_file> -c

Пример:
vagrant@vagrant:~$ cat textfile.txt
sometext
lorem ipsum
123
1234
vagrant@vagrant:~$ grep 123 textfile.txt -c
2
vagrant@vagrant:~$ grep 123 textfile.txt |wc -l
2
```

3. Какой процесс с PID 1 является родителем для всех процессов в вашей виртуальной машине Ubuntu 20.04?
Ответ: ![img](https://user-images.githubusercontent.com/94568542/148688518-0a20965a-c444-4240-9ae9-4a450603fb17.jpg) systemd(1)-VBoxService(863)

4. Как будет выглядеть команда, которая перенаправит вывод stderr ls на другую сессию терминала?

```
Ответ:

Вызов из dev/tty1:
vagrant@vagrant:~$ ls -l \root 2>/dev/tty4
vagrant@vagrant:~$ sudo chvt 4

Вывод в другой сессии dev/tty4:    

vagrant@vagrant:~$ who
vagrant  tty1        2022-01-10 15:38 
vagrant  tty4        2022-01-10 16:11 
vagrant@vagrant:~$ ls: cannot access 'root': No such file or directory
```
![4](https://user-images.githubusercontent.com/94568542/148800260-bacc183d-bd79-4d64-99e3-843f2b65de28.jpg)


5. Получится ли одновременно передать команде файл на stdin и вывести ее stdout в другой файл? Приведите работающий пример.

Ответ: Да. пример:

![5](https://user-images.githubusercontent.com/94568542/148794000-b6a7aba8-2eca-4dea-9862-4afcc7f0a437.jpg)

6. Получится ли вывести находясь в графическом режиме данные из PTY в какой-либо из эмуляторов TTY? Сможете ли вы наблюдать выводимые данные?



7. Выполните команду bash 5>&1. К чему она приведет? Что будет, если вы выполните echo netology > /proc/$$/fd/5? Почему так происходит?

Ответ:
```
bash 5>&1 - Создаст дескриптор с 5 и перенаправит его в stdout
echo netology > /proc/$$/fd/5 - выведет в дескриптор "5", который был перенаправлен в stdout

если запустить echo netology > /proc/$$/fd/5 в новой сессии, получим ошибку, так как такого дескриптора нет на данный момент 
в текущей(новой) сессии
    
vagrant@vagrant:~$ echo netology > /proc/$$/fd/5
-bash: /proc/1507/fd/5: No such file or directory
vagrant@vagrant:~$ bash 5>&1
vagrant@vagrant:~$ echo netology > /proc.$$/fd/5
bash: /proc.1579/fd/5: No such file or directory
vagrant@vagrant:~$ echo netology > /proc/$$/fd/5
netology
vagrant@vagrant:~$ 
```
![7](https://user-images.githubusercontent.com/94568542/148802429-087023b3-7d5a-444f-87aa-777211470fcb.jpg)

