1. Найдите полный хеш и комментарий коммита, хеш которого начинается на `aefea`.

```
Ответ:
git show aefea 
aefead2207ef7e2aa5dc81a34aedf0cad4c32545
Комментарий коммита: Update CHANGELOG.md
```

![11](https://user-images.githubusercontent.com/94568542/150346807-1a94e74c-5001-4a84-8f32-cabb41bdd8bc.jpg)



2. Какому тегу соответствует коммит `85024d3`?


```
Ответ:
git show 85024d3
commit 85024d3100126de36331c6982bfaac02cdab9e76 (tag: v0.12.23)
tag: v0.12.23
```

![22](https://user-images.githubusercontent.com/94568542/150350580-9711edf8-93f1-419d-a1cb-d6d73ce39380.jpg)

3. Сколько родителей у коммита `b8d720`? Напишите их хеши.

```
Ответ:
git show --pretty=format:' %P' b8d720

2 родителя:
56cd7859e05c36c06b56d013b55a252d0bb7e158 
9ea88f22fc6269854151c571162c5bcf958bee2b
```

![33](https://user-images.githubusercontent.com/94568542/150353815-edc76aaa-5170-4001-967e-4bc6734316dc.jpg)



4. Перечислите хеши и комментарии всех коммитов которые были сделаны между тегами  v0.12.23 и v0.12.24.

```
Ответ:
git log  v0.12.23..v0.12.24  --oneline

33ff1c03b (tag: v0.12.24) v0.12.24
b14b74c49 [Website] vmc provider links
3f235065b Update CHANGELOG.md
6ae64e247 registry: Fix panic when server is unreachable
5c619ca1b website: Remove links to the getting started guide's old location
06275647e Update CHANGELOG.md
d5f9411f5 command: Fix bug when using terraform login on Windows
4b6d06cc5 Update CHANGELOG.md
dd01a3507 Update CHANGELOG.md
225466bc3 Cleanup after v0.12.23 release
```

![44](https://user-images.githubusercontent.com/94568542/150355083-fc28cfb2-ce98-42b8-970b-355cc5796752.jpg)



5. Найдите коммит в котором была создана функция `func providerSource`, ее определение в коде выглядит 
так `func providerSource(...)` (вместо троеточего перечислены аргументы).

```
Ответ:
git log -S 'func providerSource' --oneline

5af1e6234 main: Honor explicit provider_installation CLI config when present
8c928e835 main: Consult local directories as potential mirrors of providers

Можно посмотреть коммиты:
```

![55](https://user-images.githubusercontent.com/94568542/150358490-3d7528c8-7b90-481e-82ef-ec6bf4b9d277.jpg)



6. Найдите все коммиты в которых была изменена функция `globalPluginDirs`.


7. Кто автор функции `synchronizedWriters`? 
