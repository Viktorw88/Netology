Домашнее задание к занятию "3.5. Файловые системы"

1. Узнайте о sparse (разряженных) файлах.
```
Ответ:
Прочитал об этом виде файлов. Полезно, спасибо
```

2. Могут ли файлы, являющиеся жесткой ссылкой на один объкт, иметь разные права доступа и владельца? Почему?

```
Ответ:
Так как hardlink это ссылка на тот же самый файл и имеет тот же inode, то права будут одни и теже.
Проверил на практике:
vagrant@vagrant:~$ touch hltest
vagrant@vagrant:~$ ln hltest test-link
vagrant@vagrant:~$ ls -ilh
```

![11](https://user-images.githubusercontent.com/94568542/149618626-b8b8b91c-5515-4847-86bf-6ad9da68e890.jpg)

```
vagrant@vagrant:~$ chmod 0755 test_hl 
vagrant@vagrant:~$ ls -ilh
```

![12](https://user-images.githubusercontent.com/94568542/149618677-77886f5d-de53-43d4-a5c6-ea5f47b8ba42.jpg)

3. Сделайте vagrant destroy на имеющийся инстанс Ubuntu. Замените содержимое Vagrantfile следующим:
```
Vagrant.configure("2") do |config|
  config.vm.box = "bento/ubuntu-20.04"
  config.vm.provider :virtualbox do |vb|
    lvm_experiments_disk0_path = "/tmp/lvm_experiments_disk0.vmdk"
    lvm_experiments_disk1_path = "/tmp/lvm_experiments_disk1.vmdk"
    vb.customize ['createmedium', '--filename', lvm_experiments_disk0_path, '--size', 2560]
    vb.customize ['createmedium', '--filename', lvm_experiments_disk1_path, '--size', 2560]
    vb.customize ['storageattach', :id, '--storagectl', 'SATA Controller', '--port', 1, '--device', 0, '--type', 'hdd', '--medium', lvm_experiments_disk0_path]
    vb.customize ['storageattach', :id, '--storagectl', 'SATA Controller', '--port', 2, '--device', 0, '--type', 'hdd', '--medium', lvm_experiments_disk1_path]
  end
end
```
Данная конфигурация создаст новую виртуальную машину с двумя дополнительными неразмеченными дисками по 2.5 Гб.

Ответ:
![3](https://user-images.githubusercontent.com/94568542/149618762-41177ecd-a2f5-483c-a889-c941714c068e.jpg)

4. Используя fdisk, разбейте первый диск на 2 раздела: 2 Гб, оставшееся пространство.
![4](https://user-images.githubusercontent.com/94568542/149619309-ae10df1f-8c08-40b5-97fa-1f65430d66b5.jpg)

5. Используя sfdisk, перенесите данную таблицу разделов на второй диск.
![51](https://user-images.githubusercontent.com/94568542/149619325-39efe766-b9ef-4143-a1bd-28e4869071f4.jpg)



6. Соберите mdadm RAID1 на паре разделов 2 Гб.

7. Соберите mdadm RAID0 на второй паре маленьких разделов.

8. Создайте 2 независимых PV на получившихся md-устройствах.

9. Создайте общую volume-group на этих двух PV.

10. Создайте LV размером 100 Мб, указав его расположение на PV с RAID0.

11. Создайте mkfs.ext4 ФС на получившемся LV.

12. Смонтируйте этот раздел в любую директорию, например, /tmp/new.

13. Поместите туда тестовый файл, например wget https://mirror.yandex.ru/ubuntu/ls-lR.gz -O /tmp/new/test.gz.

14. Прикрепите вывод lsblk.

15. Протестируйте целостность файла:

root@vagrant:~# gzip -t /tmp/new/test.gz
root@vagrant:~# echo $?
0

16. Используя pvmove, переместите содержимое PV с RAID0 на RAID1.

17. Сделайте --fail на устройство в вашем RAID1 md.

18. Подтвердите выводом dmesg, что RAID1 работает в деградированном состоянии.

19. Протестируйте целостность файла, несмотря на "сбойный" диск он должен продолжать быть доступен:

root@vagrant:~# gzip -t /tmp/new/test.gz
root@vagrant:~# echo $?
0

20. Погасите тестовый хост, vagrant destroy.


