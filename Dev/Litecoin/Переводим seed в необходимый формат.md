Переводим seed в необходимый формат
===================================

Для того чтобы захаркодить seed node его ip необходимо привести к следующему виду:

	{{0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0xff,0xff,0xb9,0xb9,0x46,0x04}, 5888}

В этом нам поможет скрипт `share/seeds/generate-seed.py`.

В директории `share/seeds` открываем файл `nodes_main.txt`

Удалем все ip адреса. И записываем туда ip своих seed node в следующем формате:

	<ip>
    <ip>:<port>
    [<ipv6>]
    [<ipv6>]:<port>
    <onion>.onion
    0xDDBBCCAA (IPv4 little-endian old pnSeeds format)

*Разделяем ip переносом строки*

Затем в директории `share/seeds` открываем файл `nodes_test.txt`

Удалем все ip адреса. И записываем туда ip своих seed node  *для тестовой сети* в следующем формате:

	<ip>
    <ip>:<port>
    [<ipv6>]
    [<ipv6>]:<port>
    <onion>.onion
    0xDDBBCCAA (IPv4 little-endian old pnSeeds format)

*Разделяем ip переносом строки*

Запускаем bash

Подтягиваем зависимости:

	sudo apt-get install python3-dnspython

