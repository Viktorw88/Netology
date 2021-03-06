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
vagrant@vagrant:~$ chmod 0755 hltest 
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

![52](https://user-images.githubusercontent.com/94568542/149619371-b9cf2b5f-37e4-4900-a1ed-8d40b1e93968.jpg)


6. Соберите mdadm RAID1 на паре разделов 2 Гб.
![6](https://user-images.githubusercontent.com/94568542/149619380-7f7e5214-a6b7-45d9-923d-e896accf054e.jpg)


7. Соберите mdadm RAID0 на второй паре маленьких разделов.
![7](https://user-images.githubusercontent.com/94568542/149619384-ded96fba-009c-4cac-8370-cdbcd39e8af0.jpg)

8. Создайте 2 независимых PV на получившихся md-устройствах.

![8](https://user-images.githubusercontent.com/94568542/149619386-84f849f4-f4e4-4011-9f8a-f02a860222e7.jpg)


9. Создайте общую volume-group на этих двух PV.

```
root@vagrant:~# vgcreate vgr1 /dev/md1 /dev/md0
  Volume group "vgr1" successfully created

root@vagrant:~# pvdisplay
```

![91](https://user-images.githubusercontent.com/94568542/149619402-72bcfb1f-fdcc-44a0-997d-df831b99e805.jpg)



10. Создайте LV размером 100 Мб, указав его расположение на PV с RAID0.

![a10](https://user-images.githubusercontent.com/94568542/149619407-7066a04c-0d21-4977-ace0-975afa117ffd.jpg)

11. Создайте mkfs.ext4 ФС на получившемся LV.

![a11](https://user-images.githubusercontent.com/94568542/149619409-4ac9e43c-2df5-4532-8146-bda2fa871572.jpg)

![ab11](https://user-images.githubusercontent.com/94568542/149619434-7020f5b1-0258-4028-8067-0ee02e4add61.jpg)


12. Смонтируйте этот раздел в любую директорию, например, /tmp/new.

![a12](https://user-images.githubusercontent.com/94568542/149619411-be9f12a6-b670-465e-86e2-bfa50fa62cad.jpg)

13. Поместите туда тестовый файл, например wget https://mirror.yandex.ru/ubuntu/ls-lR.gz -O /tmp/new/test.gz.

![a13](https://user-images.githubusercontent.com/94568542/149619413-6d773551-f742-4fcb-b60b-7f5597fcb517.jpg)

14. Прикрепите вывод lsblk.

![a14](https://user-images.githubusercontent.com/94568542/149619416-96f2de6f-079b-4d35-a3f4-d9f5c1c3ee57.jpg)

15. Протестируйте целостность файла:
```
root@vagrant:~# gzip -t /tmp/new/test.gz
root@vagrant:~# echo $?
0
```

![a15](https://user-images.githubusercontent.com/94568542/149619419-56f2a3c7-7aa2-4290-88ed-4c76b9f3bd61.jpg)



16. Используя pvmove, переместите содержимое PV с RAID0 на RAID1.
![a16](https://user-images.githubusercontent.com/94568542/149619422-c0970075-8e97-461a-815e-666c1012a944.jpg)

17. Сделайте --fail на устройство в вашем RAID1 md.
![a17](https://user-images.githubusercontent.com/94568542/149619423-b2b546e2-98fe-444f-9ef0-8ae432d972ba.jpg)

18. Подтвердите выводом dmesg, что RAID1 работает в деградированном состоянии.
![a18](https://user-images.githubusercontent.com/94568542/149619425-6a5723e2-6451-4316-b769-31c80b45e0f2.jpg)

19. Протестируйте целостность файла, несмотря на "сбойный" диск он должен продолжать быть доступен:
```
root@vagrant:~# gzip -t /tmp/new/test.gz
root@vagrant:~# echo $?
0
```

![a19](https://user-images.githubusercontent.com/94568542/149619430-34231b7b-f3bd-4dc4-98ae-a6a080ec4afe.jpg)


20. Погасите тестовый хост, vagrant destroy.

![a20](https://user-images.githubusercontent.com/94568542/149619438-5fa942e3-9397-46f0-8227-154958b35256.jpg)

