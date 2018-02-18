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

!Сменить дефолтный пароль для root *пароль root должен содержать от 8 символов с цифрами, буквами разного регистра и символами*

### Подключение к серверу через Bitvise

*[Bitvise тут](https://www.youtube.com/redirect?v=_ti-d5t1WX8&event=video_description&redir_token=Fb0zYWgHWbS7dkGtIwXEbcoKo1R8MTUxODkzMDkyMUAxNTE4ODQ0NTIx&q=https%3A%2F%2Fwww.bitvise.com%2F)*

После установки вводим в Bitvise следующие настройки:

- Host: <ip вашего сервера>
- Port: 22 *это SSH-порт по умолчанию*
- Username: root
- Initial method: password
- Password: <Ваш пароль от root>

Производим логин на сервер через Bitvise

### Генерация ключей SSH для удаленной работы с сервером

В Bitvise открываем раздел `Client key manager` и нажимаем `Generate New`. В открывшемся окне выбираем:

- Location: Profile
- Algorithm: RSA
- Size: 3072
*- Passphrase: для дополнительной защиты профиля и ключей, опционально*


### Экспортируем публичный ключ

В разделе `Client key manager` выбираем созданую пару ключей и нажимаем `export`.
В открывшемся окне выбираем `openSSH format` и нажимаем `export`


### Настройка SSH на сервере

После логина на сервере открываем окно `Bitvise sftp`.
В той части окна, где находятся удалённые файлы, переходим в папку `/root/.ssh`,
тут должен быть файл `authorized_keys`. Открываем его при помощи блокнота.

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
	

### Подключение к серверу через SSH в Bitvise

В Bitvise меняем следущие настройки:

- Host: <ip вашего сервера>
- Port: *тот порт, который вы указали в `sshd_config`*
- Username: root
- Initial method: publickey
- Client key: <выбираем созданые нами ключи>
- Passphrase: <если не вводили пассфразу при создании ключей - оставляем это поле пустым>

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

	usermod -aG sudo username

*в системе Ubuntu все пользователи, состоящие в группе sudo, по умолчанию имеют права sudo*

Переходим в сессию нового системного пользователя:

	su - username

Пробуем запустить любую команду, добавив перед ней sudo.
К примеру, чтобы просмотреть содержимое каталога /root, нужно выполнить:

	sudo ls -la /root

Дополнительно см. [тут](https://www.8host.com/blog/sozdanie-polzovatelya-sudo-v-ubuntu/)


### Настройка SSH на сервере для user'a


