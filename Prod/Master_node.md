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

Первоначально необходимо установить пароль для пользователя root *сложный пароль, а не 123qwerty*

### Подключение к серверу через bitvise

*[Bitvise тут](https://www.youtube.com/redirect?v=_ti-d5t1WX8&event=video_description&redir_token=Fb0zYWgHWbS7dkGtIwXEbcoKo1R8MTUxODkzMDkyMUAxNTE4ODQ0NTIx&q=https%3A%2F%2Fwww.bitvise.com%2F)*

Сразу после установки bitvise вводим следущие настройки:

- Host: <ip вашего сервера>
- Port: 22 *По умолчанию*
- Username: root
- Initial method: password
- Password: <Ваш пароль от root>

После этого пытаемся залогиниться

### Генерация ключей SSH для удаленной работы с сервером

В bitvise открываем `Client key manager` и нажимаем `Generate New`. В открывшемся окне выбираем:

- Location: Profile
- Algorithm: RSA
- Size: 3072

*Можно указать passphrase для доболнительной защиты ключей*

После этого необходимо экспортировать публичный ключ

Для этого в `Client key manager` выбираем созданую пару ключей и нажимаем `export`.
В открывшемся окне выбираем фомат openSSH format и нажимаем `export`



### Настройка SSH на сервере

После того как вы залогинились на сервере через bitvise открываем окно bitvise sftp
в части окна, где находятся удаленные файлы, переходим в папку `/root/.ssh`,
тут должен быть файл `authorized_keys` его открываем при помощи блокнота.

Далее открываем ранее экспортированный публичный ключ SSH тоже через блокнот и копируем его содержимое в открытый файл `authorized_keys`.

После этого переходим в директорию `/etc/ssh` находим файл `sshd_config`

Раскоментируем следущие строки:

	RSAAuthentication yes
	PubkeyAuthentication yes
	AuthorizedKeysFile	%h/.ssh/authorized_keys

Для PasswordAuthentication ставим парамет `no` и раскоменчиваем его, если он закометирован

	# Change to no to disable tunnelled clear text passwords
	PasswordAuthentication no

Так же необходимо указать свой порт SSH:

	# What ports, IPs and protocols we listen for
	Port 88 *например*

### Подключение к серверу через SSH в bitvise

В bitvise меняем следущие настройки:

- Host: <ip вашего сервера>
- Port: 88 *тот порт который мы указали в `sshd_config`*
- Username: root
- Initial method: publickey
- Client key: <выбираем созданые нами ключи>
- Passphrase: <если не вводили пасфразу при создании ключей оставляем это поле пустым>

После этого пытаемся залогиниться

### Создание нового юзера

	adduser <username>

Вводим пароль пользователя:

	Set password prompts:
	Enter new UNIX password:
	Retype new UNIX password:
	passwd: password updated successfully

Следуем инструкциям системы, чтобы указать дополнительную информацию о пользователе. Можно оставить значения по умолчанию:

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

	usermod -aG sudo username

*В системе Ubuntu все пользователи, состоящие в группе sudo, по умолчанию имеют права sudo.*

Переходим в сессию нового системного пользователя:

	su - username

Пробуем запустить любую команду, добавив перед ней sudo.
К примеру, чтобы просмотреть содержимое каталога /root, нужно выполнить:

	sudo ls -la /root

Дополнительно см. [тут](https://www.8host.com/blog/sozdanie-polzovatelya-sudo-v-ubuntu/)

### Настройка SSH на сервере для user'a


