Домашнее задание по лекции "Компьютерные сети (лекция 1)"

1. Работа c HTTP через телнет.
Подключитесь утилитой телнет к сайту stackoverflow.com telnet stackoverflow.com 80
отправьте HTTP запрос
```
GET /questions HTTP/1.0
HOST: stackoverflow.com
[press enter]
[press enter]
```
В ответе укажите полученный HTTP код, что он означает?

![1](https://user-images.githubusercontent.com/94568542/149666458-df0d0da1-d770-4922-9bc4-42294c135e4c.jpg)

HTTP код 301 сообщает что запрошенный ресурс окончательно переехал по адресу указанному в строке: location: https://stackoverflow.com/questions

2. Повторите задание 1 в браузере, используя консоль разработчика F12.
откройте вкладку Network
отправьте запрос http://stackoverflow.com
найдите первый ответ HTTP сервера, откройте вкладку Headers
укажите в ответе полученный HTTP код.
проверьте время загрузки страницы, какой запрос обрабатывался дольше всего?
приложите скриншот консоли браузера в ответ.

```
Первый ответ http сервера:

Request URL: https://stackoverflow.com/
Request Method: GET
Status Code: 200 
Remote Address: 151.101.129.69:443
Referrer Policy: strict-origin-when-cross-origin

HTTP код 200. Запрос https://stackoverflow.com/ обрабатывался дольше всего - время загрузки 336ms
```
![2](https://user-images.githubusercontent.com/94568542/149666756-e4648b2c-a290-4f1a-9935-1238bb55f208.jpg)

3. Какой IP адрес у вас в интернете?
![3](https://user-images.githubusercontent.com/94568542/149666931-49131157-86b2-48bd-9de9-87aa6086a737.jpg)

4. Какому провайдеру принадлежит ваш IP адрес? Какой автономной системе AS? Воспользуйтесь утилитой whois

netname:        NCN-BBCUST
origin:         AS42610

![4](https://user-images.githubusercontent.com/94568542/149667487-55fe020e-b0b1-4b88-9299-c33c7b2fdbd5.jpg)


5. Через какие сети проходит пакет, отправленный с вашего компьютера на адрес 8.8.8.8? Через какие AS? Воспользуйтесь утилитой traceroute
![51](https://user-images.githubusercontent.com/94568542/149668545-6686ce05-9eed-4cb0-bbf1-52dda899f971.jpg)

6. Повторите задание 5 в утилите mtr. На каком участке наибольшая задержка - delay?
![6](https://user-images.githubusercontent.com/94568542/149669021-e415d899-fa7c-4886-b6b0-4ff10e80ae2b.jpg)
Худший результат - строка 4.

7. Какие DNS сервера отвечают за доменное имя dns.google? Какие A записи? воспользуйтесь утилитой dig
![7](https://user-images.githubusercontent.com/94568542/149669552-03004deb-028c-4da3-845d-d9fc7910723e.jpg)

8. Проверьте PTR записи для IP адресов из задания 7. Какое доменное имя привязано к IP? воспользуйтесь утилитой dig
