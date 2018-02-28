
на машине
---------

Cрздаём пустой репозиторий и файл `index.md` в нем

копирую машину


на копии 
--------


### создание ветки dev:

    `git branch dev`
    
переключение на нее
    
    `git checkout dev`
    
Публикуем ее на github:

    `git push origin dev`


### ветка master

произвожу изменение файла index.md

создаём коммит и push на github


на копии 


### ветка slave 

так же производим изменение файла в ней | создаём коммит

После коммита надо влить в нашу ветку изменения из ветки `dev` и `master`:

    `git pull origin dev`
    `git pull origin master`


возникла проблема слития веток так как файл index.md изменен в 2-х ветках
CONFLICT (content): Merge conflict in index.md
Automatic merge failed; fix conflict and then commit the result

Чтобы решить такой конфликт, нужно выбрать — какая версия файла будет использоваться: ваша или из вливаемой ветки. 
Чтобы использовать *свой вариант файла* пишем:

    `git checkout --ours index.md`
    `git add index.md`



Выбор версии из вливаемой ветки:
--------------------------------

    `git checkout --theirs index.md`
    `git add index.md`

далее коммитим и пушим на нашу ветку:

    `git push origin slave`



Создаём файлы с именем ветки на машинах, работающих с данными ветками:

    `touch ./slave.txt` 
    `touch ./dev.txt`

Делаем `push` в соответствующие ветки:

на машине **master**:

    `git pull origin dev`
    
на машине **slave**:

    `git pull origin slave`

контрольная проверка состояния гита:

`git status`
 
 
 
Если мердж не прошел коммитим и пушим на гитхаб


