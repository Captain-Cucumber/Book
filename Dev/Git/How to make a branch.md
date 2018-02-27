на машине имею репо и файл index.md в нем
копирую машину


на копии 

создание ветки dev
git branch dev
переключение на нее
git checkout dev
Публикуем ее на github:
git push origin dev


на основной(ветка мастер)

произвожу изменение файла index.md
коммитчу и пушу на github


на копии 
создаю ветку slave
так же произвожу изменение файла в ней
коммитчу
После коммита надо влить в нашу ветку изменения из ветки dev и master:

git pull origin dev 
git pull origin master


возникла проблема слития веток так как файл index.md изменен в 2-х ветках
CONFLICT (content): Merge conflict in index.md
Automatic merge failed; fix conflict and then commit the result

Чтобы решить такой конфликт, надо просто выбрать — какая версия файла будет использоваться: ваша или из вливаемой ветки. Чтобы использовать свой вариант файла, вводим команду:

git checkout --ours index.md
git add index.md


Если мы выбираем версию из вливаемой ветки:

git checkout --theirs index.md
git add index.md

далее коммитим и пушим на нашу ветку
git push origin slave



Создаю файлы с именем ветки на машинах работающих с данными ветками
touch ./slave.txt 
touch ./dev.txt 

пушу их в соответствующие ветки
на машине master
git pull origin dev
git pull origin slave
git status 
если вдруг мердж не прошел коммитим и пушим на гитхаб





