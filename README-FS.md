Домашнее задание к занятию "3.5. Файловые системы"

1. Узнайте о sparse (разряженных) файлах.
```
Ответ:
Прочитал об этом виде файлов. Полезно, спасибо
```

2. Могут ли файлы, являющиеся жесткой ссылкой на один объкт, иметь разные права доступа и владельца? Почему?
3. ```
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
