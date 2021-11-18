Домашнее задание к занятию «1.1. Введение в DevOps»
Задание №1 - Подготовка рабочей среды

Скриншоты с именем в созданных файлах:

markdown
http://joxi.ru/1A5vYK6Hz6n49r

terraform
http://joxi.ru/DmBqY8kSgkwP6r

bash
http://joxi.ru/Vm6lYZWiRWDzXA

yaml
http://joxi.ru/p27vYadHndovxm

jsonnet
http://joxi.ru/52aEpgdcgQ4B92

Задание №2 - Описание жизненного цикла задачи (разработки нового функционала)

1) Менеджер проекта формирует и описывает техническое задание (ТЗ) с учетом:
требования заказчика;
требований и спецификаций используемого ПО;
требований используемой инфраструктуры.
Согласовывает и передает ТЗ на разработку, впоследствии организует процесс по этапам (контроль сроков и т.п.)

2) Команда разработчиков выполняет разработку и готовит проект к релизу на тестовый стенд

3) По завершении разработки DevOps выполняет накат на релизный(тестовый) стенд, если есть проблемы в деплое решает вопросы, возможно вносятся изменения в проект (при необходимости)

4) На релизном(тестовом) стенде Тестировщики тестируют проект, оформляют задания на доработку(при наличии необходимости и выявлении багов)
Так же могут выполнять дополнительные нагрузочные тесты после исправления всех недоработок

6) В идеале выполняется релиз на Pre Prod стенд, который должен быть идентичен продовому, так же осуществляется Devops-ом (если нет ошибок и прочего, иначе доработка и полный круг с пункта 3)

7) В согласованную дату Релиза в прод осуществляется накат на продстенд DevOps-ом. В случае необходимости внесения внерелизных изменений Devops также участвует в процессе исправления ошибок в связке с командой разработки

8) Система передается в Промышленную Эксплуатацию и на сопровождение команде сопровождения, DevOps может входить в команду сопровождения, и также сопровождать систему в итоге 
