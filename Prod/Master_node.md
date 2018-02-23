Создание мастер ноды
====================

Основные параметры узла
-----------------------

Для мастер ноды необходим выделенный сервер со следующими рекомендуемыми параметрами:

- ОЗУ: не менее 2 Гб
- Жёсткий диск: не менее 32 Гб
- Процессор: не менее 1 ядра
- ОС: Ubuntu 16.04.2 x64

Настройка системы и окружения
-----------------------------

!Сменить дефолтный пароль для root *(пароль root должен содержать от 8 символов с цифрами, буквами разного регистра и символами)*

Обновляем систему *(от лица root-пользователя)*:

	sudo apt-get update
	sudo apt-get upgrade

Устанавливаем софт облегчающий работу:

	sudo apt-get install htop mc pydf

### Подключение к серверу через SSH-клиент [Bitvise](https://www.youtube.com/redirect?v=_ti-d5t1WX8&event=video_description&redir_token=Fb0zYWgHWbS7dkGtIwXEbcoKo1R8MTUxODkzMDkyMUAxNTE4ODQ0NTIx&q=https%3A%2F%2Fwww.bitvise.com%2F)

После установки вводим в Bitvise следующие настройки:

- Host: <IP-адрес вашего сервера>
- Port: 22 *(это SSH-порт по умолчанию)*
- Username: root
- Initial method: password
- Password: <пароль от root>

Производим логин на сервер через Bitvise

### Генерация ключей SSH для удаленной работы с сервером

В Bitvise открываем раздел `Client key manager` и нажимаем `Generate New`. В открывшемся окне выбираем:

- Location: Profile
- Algorithm: RSA
- Size: 3072
- Passphrase: (пароль) *для дополнительной защиты доступа к профилю и ключам в самом Bitvise, опционально*


### Экспортируем публичный ключ

В разделе `Client key manager` выбираем созданую пару ключей и нажимаем `export`.
В открывшемся окне выбираем `openSSH format` и нажимаем `export`


### Настройка SSH на сервере

После логина на сервере открываем окно `Bitvise sftp`. В той части окна, где находятся удалённые файлы, переходим в папку `/root/.ssh`, тут должен быть файл `authorized_keys`. Открываем его при помощи блокнота.

Далее открываем (так же в блокноте) ранее экспортированный публичный ключ SSH и копируем его содержимое в открытый файл `authorized_keys`.

После этого переходим в директорию `/etc/ssh` находим файл `sshd_config`

Раскомментируем следущие строки:

	RSAAuthentication yes
	PubkeyAuthentication yes
	AuthorizedKeysFile	%h/.ssh/authorized_keys

Для PasswordAuthentication ставим флаг `no` и раскомменчиваем его, если он закомментирован:

	# Change to no to disable tunnelled clear text passwords
	PasswordAuthentication no

Указываем *свой* порт SSH:

	# What ports, IPs and protocols we listen for
	Port 88 *например*

Перезапускаем ssh-сервер, чтобы активировать внесенные изменения:

	sudo service sshd restart

*Кроме того, внесенные изменения необходимо тщательно протестировать, чтобы убедиться, что все работает должным образом.*

*Рекомендуется открыть несколько сессий при внесении изменений. Это позволит вернуть конфигурации в случае необходимости.*

### Подключение к серверу через SSH в Bitvise

В Bitvise меняем следущие настройки:

- Host: <ip вашего сервера>
- Port: *тот порт, который вы указали в `sshd_config`*
- Username: root
- Initial method: publickey
- Client key: <выбираем созданые нами ключи>
- Passphrase: *<если **не** вводили парольную фразу при создании ключей - оставляем это поле пустым>*

После этого логинимся


### Создание нового юзера

	adduser <username>
	
Задаём пароль пользователя:

	Set password prompts:
	Enter new UNIX password:
	Retype new UNIX password:
	passwd: password updated successfully

Следуем инструкциям системы, чтобы указать дополнительную информацию о пользователе. Можно оставить значения по умолчанию (Enter):

	User information prompts:
	Changing the user information for username
	Enter the new value, or press ENTER for the default
	Full Name []:
	Room Number []:
	Work Phone []:
	Home Phone []:
	Other []:
	Is the information correct? [Y/n]

Используем команду `usermod`, чтобы добавить пользователя в группу sudo:

	usermod -aG sudo <username>

*в системе Ubuntu все пользователи, состоящие в группе sudo, по умолчанию имеют права sudo*

Переходим в сессию нового системного пользователя:

	su - <username>

Пробуем запустить любую команду, добавив перед ней sudo.
К примеру, чтобы просмотреть содержимое каталога `/root`, нужно выполнить:

	sudo ls -la /root

Дополнительно см. [здесь](https://www.8host.com/blog/sozdanie-polzovatelya-sudo-v-ubuntu/)

### Настройка SSH для user'a на сервере

В домашней дирректории пользователя создаем скрытую папку `.ssh`:

	cd /home/<username>
	mkdir .ssh

Внутри папки `.ssh` создаем файл `authorized_keys`:

	cd .ssh
	touch authorized_keys

На основной машине открываем Bitvise и создаем новый профиль:

- Host: <ip вашего сервера>
- Port: 88 *(тот порт который мы указали в `sshd_config`)*
- Username: <username>
- Initial method: publickey
- Client key: <созадаем новую пару ключей SSH как описано выше>
- Passphrase: <если не вводили пасфразу при создании ключей оставляем это поле пустым>

Возвращаемся в окно `bitvise sftp`, в части окна, где находятся удалённые файлы, переходим в папку `/home/<username>/.ssh`, открываем файл `authorized_keys` при помощи блокнота.

Открываем ранее экспортированный публичный ключ SSH для user'a, (так же через блокнот) и копируем его содержимое в открытый файл `authorized_keys`.

И после этого пытаемся залогинится

Компиляция демона монеты
------------------------

### Устанавливаем зависимости

	sudo apt-get install git
	sudo apt-get install build-essential libtool autotools-dev automake pkg-config libssl-dev libevent-dev bsdmainutils
	sudo apt-get install libboost-all-dev
	sudo apt-get install software-properties-common
	sudo add-apt-repository ppa:bitcoin/bitcoin
	sudo apt-get update
	sudo apt-get install libdb4.8-dev libdb4.8++-dev
	sudo apt-get install libminiupnpc-dev
	sudo apt-get install libzmq3-dev
	sudo apt-get install libqt5gui5 libqt5core5a libqt5dbus5 qttools5-dev qttools5-dev-tools libprotobuf-dev protobuf-compiler
	sudo apt-get install libqt4-dev libprotobuf-dev protobuf-compiler

### Клонируем исходники

В домашнюю дирректорию user'a клонируем исходники монеты:

	git clone https://github.com/<yourrepo>/<yourcoin>.git

### Компиляция

	cd <yourcoin>/src
	make -f makefile.unix

Запуск и настройка демона
-------------------------

В папке `<yourcoin>/src` запускаем файл `<yourcoin>d`:

	cd /home/<username>/<yourcoin>/src
	./<yourcoin>d

Далее переходим в папку `/home/<username>/.<yourcoin>` создаем файл `<yourcoin>.conf` и открываем его:

	cd /home/<username>/.<yourcoin>
	touch <yourcoin>.conf
	nano <yourcoin>.conf

В файле `<yourcoin>.conf` прописываем:

	server=1
	rpcuser=<username>
	rpcpassword=<password>

После этого перезапускаем демона:

	cd /home/<username>/<yourcoin>/src
	./<yourcoin>d -stop
	./<yourcoin>d
