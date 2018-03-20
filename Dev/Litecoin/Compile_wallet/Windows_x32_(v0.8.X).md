Компиляция кошелька под Windows (x32)
-------------------------------------

### Установка VM

[Скачиваем образ Debian 8.5.0 х64](https://www.youtube.com/redirect?v=_ti-d5t1WX8&event=video_description&redir_token=Fb0zYWgHWbS7dkGtIwXEbcoKo1R8MTUxODkzMDkyMUAxNTE4ODQ0NTIx&q=https%3A%2F%2Fcdimage.debian.org%2Fmirror%2Fcdimage%2Farchive%2F8.5.0-live%2Famd64%2Fiso-hybrid%2Fdebian-live-8.5.0-amd64-standard.iso)

Минимальные системные требования *физической машины*:

- ОЗУ: не менее 4 Гб
- Жёсткий диск: не менее 100 Гб

Минимальные системные требования *виртуальной машины*:

- ОЗУ: не менее 2 Гб
- Жёсткий диск: не менее 40 Гб
- Процессор: не менее 1 ядра (количество ядер процессора влияет на скорость компиляции)

Необходимо выполнить настройки сети:

В настройках сети открываем вкладку `Дополнительно (Addition)` => `Проброс портов (Port forwarding)`

Создаём новое правило:

- Имя: `SSH`

- Протокол: `TCP`

- Порт хоста: `22222`

- Порт гостя: `22`

При создании виртуальной машины указываем:
- пароль root: `password`
- имя пользователя: `debian`
- пароль: `debianpassword`

После установки логинимcя под root

ВНИМАНИЕ! Не нужно обновлять систему!
*можно установить htop, mc*

Даем право логинится как root через SSH:

	sed -i 's/^PermitRootLogin.*/PermitRootLogin yes/' /etc/ssh/sshd_config
	/etc/init.d/ssh restart

В основной системе устанавливаем [Bitvise](https://www.youtube.com/redirect?v=_ti-d5t1WX8&event=video_description&redir_token=Fb0zYWgHWbS7dkGtIwXEbcoKo1R8MTUxODkzMDkyMUAxNTE4ODQ0NTIx&q=https%3A%2F%2Fwww.bitvise.com%2F)

В Bitvise задаём следующие параметры:

- Host: `localhost`
- Port: `22222`
- Username: `root`
- Initial method: `password`
- Password: `password`

Далее сохраняем профиль и логинимся

В открывшемся терминале вводим следующие команды:

	apt-get install git ruby sudo apt-cacher-ng qemu-utils debootstrap lxc python-cheetah parted kpartx bridge-utils make ubuntu-archive-keyring curl

	sudo apt-get install zip unzip

	adduser debian sudo

	echo "%sudo ALL=NOPASSWD: /usr/bin/lxc-start" > /etc/sudoers.d/gitian-lxc
	echo "%sudo ALL=NOPASSWD: /usr/bin/lxc-execute" >> /etc/sudoers.d/gitian-lxc
	echo '#!/bin/sh -e' > /etc/rc.local
	echo 'brctl addbr br0' >> /etc/rc.local
	echo 'ifconfig br0 10.0.3.2/24 up' >> /etc/rc.local
	echo 'iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE' >> /etc/rc.local
	echo 'echo 1 > /proc/sys/net/ipv4/ip_forward' >> /etc/rc.local
	echo 'exit 0' >> /etc/rc.local
	echo 'export USE_LXC=1' >> /home/debian/.profile
	echo 'export GITIAN_HOST_IP=10.0.3.2' >> /home/debian/.profile
	echo 'export LXC_GUEST_IP=10.0.3.5' >> /home/debian/.profile
	reboot

Далее в Bitvise изменяем имя пользователя на `debian` и вводим пароль `debianpassword` и заново логинимся

В открывшемся терминале вводим следующие команды:

	wget http://archive.ubuntu.com/ubuntu/pool/universe/v/vm-builder/vm-builder_0.12.4+bzr494.orig.tar.gz
	echo "76cbf8c52c391160b2641e7120dbade5afded713afaa6032f733a261f13e6a8e  vm-builder_0.12.4+bzr494.orig.tar.gz" | sha256sum -c
	# (verification -- must return OK)
	tar -zxvf vm-builder_0.12.4+bzr494.orig.tar.gz
	cd vm-builder-0.12.4+bzr494
	sudo python setup.py install
	cd ..

Клонируем `gitian-builder` в домашнюю директорию:

	git clone https://github.com/devrandom/gitian-builder.git

Так же поступаем с <yourcoin>

	git clone https://github.com/<yourrepo>/<yourcoin>.git
	cd testcoin
	git checkout 0.8
	cd ..

*Что, сложна, блеать? Тогда читаем [как создать свой репозиторий на github](Команды_git.md)*

### Собираем базовую VM

	cd gitian-builder
	bin/make-base-vm --lxc --arch amd64 --suite precise

### Устанавливаем зависимости

	mkdir -p inputs; cd inputs/
	wget 'http://miniupnp.free.fr/files/download.php?file=miniupnpc-1.9.20140401.tar.gz' -O 'miniupnpc-1.9.20140401.tar.gz'
	wget 'https://www.openssl.org/source/openssl-1.0.1k.tar.gz'
	wget 'http://download.oracle.com/berkeley-db/db-4.8.30.NC.tar.gz'
	wget 'https://www.zlib.net/fossils/zlib-1.2.8.tar.gz'
	wget 'ftp://ftp.simplesystems.org/pub/libpng/png/src/history/libpng16/libpng-1.6.8.tar.gz'
	wget 'http://fukuchi.org/works/qrencode/qrencode-3.4.3.tar.bz2'
	wget 'http://downloads.sourceforge.net/project/boost/boost/1.55.0/boost_1_55_0.tar.bz2'
	wget 'https://download.qt.io/archive/qt/4.8/4.8.5/qt-everywhere-opensource-src-4.8.5.tar.gz'
	wget 'https://svn.boost.org/trac/boost/raw-attachment/ticket/7262/boost-mingw.patch' -O boost-mingw-gas-cross-compile-2013-03-03.patch

### Собираем зависимости

	./bin/gbuild ../<yourcoin>/contrib/gitian-descriptors/boost-win32.yml
	mv build/out/boost-*.zip inputs/
 
	./bin/gbuild ../<yourcoin>/contrib/gitian-descriptors/deps-win32.yml
	mv build/out/bitcoin*.zip inputs/
 
	./bin/gbuild ../<yourcoin>/contrib/gitian-descriptors/qt-win32.yml
	mv build/out/qt*.zip inputs/

### Компилируем монету

	./bin/gbuild --commit <yourcoin>=v<tag_version> ../<yourcoin>/contrib/gitian-descriptors/gitian-win32.yml

После успешной компиляции:

	pushd build/out
	zip -r <yourcoin>-${VERSION}-linux.zip *
	mv <yourcoin>-${VERSION}-linux.zip ../../
	popd

и копируем архив `<yourcoin>-${VERSION}-linux.zip` в основную систему


Пробуем запустить файл `<yourcoin>-qt.exe`
