В локальном репозитории имеется файл index.md

**Копия репозитория**

### Cоздание ветки dev

    git branch dev

Переключение на нее

    git checkout dev

Публикуем ее на github:

    git push origin dev


**Основной репозиторий (мастер ветка)**

произвожу изменение файла index.md
коммитчу и пушу на github

**Копия репозитория**

Создаю ветку slave, так же произвожу изменение файла в ней коммитчу

После коммита надо влить в нашу ветку изменения из ветки dev и master:

    git pull origin dev 
    git pull origin master

Если возникла проблема слияния веток так как файл index.md изменен в 2-х ветках:

    CONFLICT (content): Merge conflict in index.md
    Automatic merge failed; fix conflict and then commit the result

Чтобы решить такой конфликт, надо просто выбрать — какая версия файла будет использоваться: ваша или из вливаемой ветки. Чтобы использовать свой вариант файла, вводим команду:

    git checkout --ours index.md
    git add index.md

Если мы выбираем версию из вливаемой ветки:

    git checkout --theirs index.md
    git add index.md

далее коммитим и пушим на нашу ветку:

    git push origin slave

**Копия репозитория**

Создаем файлы с именем ветки на машинах работающих с данными ветками:

    touch ./slave.txt 
    touch ./dev.txt 

Делаем коммит и пушим на github

**Основной репозиторий (мастер ветка)**
Делаем pull в соответствующие ветки

    git pull origin dev
    git pull origin slave
    git status
