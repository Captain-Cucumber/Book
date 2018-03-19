Растем до 0.10
==============

- <yourcoin> = Название вашей монеты
- <YOUR> = Шильдик вашей монеты *например TST*

### Заменяем все что связано с litecoin на своё:

	find . -type f -print0 | xargs -0 sed -i 's/litecoin/<yourcoin>/g'
	find . -type f -print0 | xargs -0 sed -i 's/Litecoin/<Yourcoin>/g'
	find . -type f -print0 | xargs -0 sed -i 's/LiteCoin/<Yourcoin>/g'
	find . -type f -print0 | xargs -0 sed -i 's/LITECOIN/<YOURCOIN>/g'
	find . -type f -print0 | xargs -0 sed -i 's/LTC/<YOUR>/g'

### Замена портов

	find . -type f -print0 | xargs -0 sed -i 's/9333/<порт>/g'
	find . -type f -print0 | xargs -0 sed -i 's/9332/<порт на 1 меньше>/g'

*Например <порт> = 5988 & <порт на 1 меньше> = 5987*

### Изменяем префиксы в адресах монеты: 

Начиная с версии 0.10 большинство основных параметров переехало в файл `src/chainparams.cpp`

Префиксы для публичных адресов меняются в строках `166` для основной сети, `228` для тестовой

	base58Prefixes[PUBKEY_ADDRESS] = std::vector<unsigned char>(1,<ваше_значение>);

*!Для того чтобы правильно подобрать значение соответсвующее символу см. [тут](https://en.bitcoin.it/wiki/List_of_address_prefixes)*

### Заменяем Alert ключи

В версии 0.8 (сдесь подразумевается уже ваша монета) AlertKeys находились в файле `src/alert.cpp`. В версии 0.10 ключи переехали в файл `src/chainparams.cpp`

#### Для основной сети

Копируем свой ключ из строки `22` файла `alert.cpp`.

	static const char* pszMainKey = "04f0801cbbf15f959de9e345888efb74222fa13844d5a428e33c52c0dbb8cb9c1a654bde53bd2415c4d7f54ba9b92beabc7c3074d5c1bd91b971072fec7cdfcd94";

*Ваш ключ должен выглядеть примерно так: `04f0801cbbf15f959de9e345888efb74222fa13844d5a428e33c52c0dbb8cb9c1a654bde53bd2415c4d7f54ba9b92beabc7c3074d5c1bd91b971072fec7cdfcd94`*

***P.S. Не надо икать это в 0.10 ветке! Ищи в 0.8 на github в своем репозитории***

И вставляем в строку `118` файла `chainparams.cpp` уже 0.10 ветки вашего проекта.

	vAlertPubKey = ParseHex("04f0801cbbf15f959de9e345888efb74222fa13844d5a428e33c52c0dbb8cb9c1a654bde53bd2415c4d7f54ba9b92beabc7c3074d5c1bd91b971072fec7cdfcd94");

#### Для тестовой сети

Копируем свой ключ из строки `23` файла `alert.cpp`.

	static const char* pszTestKey = "04d429c63946b6f404b387c0ef6a49790d3c904e80004afc223b306b148ed52e2dbd412fc88d121c16ad9658a6a165300c14ac3cf8d111929c8123c93c5022cb87";

*Ваш ключ должен выглядеть примерно так: `04d429c63946b6f404b387c0ef6a49790d3c904e80004afc223b306b148ed52e2dbd412fc88d121c16ad9658a6a165300c14ac3cf8d111929c8123c93c5022cb87`*

И вставляем в строку `206` файла `chainparams.cpp` уже 0.10 ветки вашего проекта.

	 vAlertPubKey = ParseHex("04d429c63946b6f404b387c0ef6a49790d3c904e80004afc223b306b148ed52e2dbd412fc88d121c16ad9658a6a165300c14ac3cf8d111929c8123c93c5022cb87");

*Вспомнить для чего необходимы эти ключи можно [здесь](https://bitcoin.stackexchange.com/questions/583/what-is-the-alert-system-in-the-bitcoin-protocol-how-does-it-work)*

#### Замена scriptPubKey

scriptPubKey перехал в `chainparams.cpp` из `main.cpp` строка `2788`

	txNew.vout[0].scriptPubKey = CScript() << ParseHex("04d429c63946b6f404b387c0ef6a49790d3c904e80004afc223b306b148ed52e2dbd412fc88d121c16ad9658a6a165300c14ac3cf8d111929c8123c93c5022cb87") << OP_CHECKSIG;

Копируем ключ

*Ваш ключ должен выглядеть примерно так: `04d429c63946b6f404b387c0ef6a49790d3c904e80004afc223b306b148ed52e2dbd412fc88d121c16ad9658a6a165300c14ac3cf8d111929c8123c93c5022cb87`*

И вставляем в строку `146` файла `chainparams.cpp`

	txNew.vout[0].scriptPubKey = CScript() << ParseHex("040184710fa689ad5023690c80f3a49c8f13f8d45b8c857fbcbc8bc4a8e4d3eb4b10f4d4604fa08dce601aaf0f470216fe1b51850b4acf21b179c45070ac7b03a9") << OP_CHECKSIG;

### Магические биты 

В версии 0.8 (сдесь подразумевается уже ваша монета) Магические биты находились в файле `src/main.cpp`. В версии 0.10 ключи переехали в файл `src/chainparams.cpp`

#### Для основной сети

Открываем файл `main.cpp` и переходим к строке `3082`

	unsigned char pchMessageStart[4] = { 0xf2, 0xca, 0xbf, 0xd2 }; // Yourcoin: absolute random.

Копируем шестнадцатиричные значения в файл `src/chainparams.cpp` в строки `114-117`

	pchMessageStart[0] = 0xf2;
    pchMessageStart[1] = 0xca;
    pchMessageStart[2] = 0xbf;
	pchMessageStart[3] = 0xd2;

#### Для основной сети

Открываем файл `main.cpp` и переходим к строке `2745`

	pchMessageStart[0] = 0xf8;
	pchMessageStart[1] = 0xc4;
	pchMessageStart[2] = 0xbf;
	pchMessageStart[3] = 0xd8;

Копируем шестнадцатиричные значения в файл `src/chainparams.cpp` в строки `202-205`

	pchMessageStart[0] = 0xf2;
    pchMessageStart[1] = 0xca;
    pchMessageStart[2] = 0xbf;
	pchMessageStart[3] = 0xd2;

### Удаляем DNS Seed

В файле `chainparams.cpp` удаляем все что похоже на:

	vSeeds.push_back(CDNSSeedData("litecointools.com", "dnsseed.litecointools.com"));

### Удаление захардкоженых Node

В файле `src/chainparamsseeds.h` приводим все к следующему виду:

	#ifndef BITCOIN_CHAINPARAMSSEEDS_H
	#define BITCOIN_CHAINPARAMSSEEDS_H
	/**
	 * List of fixed seed nodes for the bitcoin network
	 * AUTOGENERATED by share/seeds/generate-seeds.py
	 *
	 * Each line contains a 16-byte IPv6 address and a port.
	 * IPv4 as well as onion addresses are wrapped inside a IPv6 address accordingly.
	 */
	static SeedSpec6 pnSeed6_main[] = {
	    {{0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0xff,0xff,0xb9,0xb9,0x46,0x04}, 5888}
	};

	static SeedSpec6 pnSeed6_test[] = {
	    {{0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0xff,0xff,0x00,0x00,0x00,0x00}, 15890}
	};
	#endif // BITCOIN_CHAINPARAMSSEEDS_H

*Где `0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0xff,0xff,0xb9,0xb9,0x46,0x04` ip вашей мастер ноды*

### Настройка основных параметров монеты

В файле `chainparams.cpp` изменяем следующие параметры:

- nSubsidyHalvingInterval - Интервал уменьшения вознаграждения за добычу блока.

- nTargetTimespan - Интервал пересчета сложности

- nTargetSpacing - Интервал добычи блоков

В файле `main.cpp`:

- CAmount nSubsidy - вознаграждение за добытый блок

В файле `main.h`:

- COINBASE_MATURITY - колличество подтверждений (добытых блоков после нахождения блока/совершения транзакции) необходимых для созревания монет, полученных в результате майнинга.

В файле `src\amount.h`

MAX_MONEY - максимальное колличество монет
