
// данный документ раздельно описывает параметры конфигурации монеты Dash 0.12.4.3

// с примерами реализации и с указателем расположения конфигурационных

// файлов. параметры пронумерованы с тем, чтобы *в основном development-*

// *документе* они были указаны под номерами со ссылками на **данный** документ.




0.Конфигурация genesis-блока
-----------------

  **chainparams.cpp** *(файл расположен по адресу: src/chainparams.cpp)*
  
  
### описание файла


  пример_кода:

```const char* pszTimestamp = "Bloomberg 26.03.2018 China's Oil Contract to Rival WTI and Brent Starts Trading";
        CMutableTransaction txNew;
        txNew.vin.resize(1);
        txNew.vout.resize(1);
        txNew.vin[0].scriptSig = CScript() << 486604799 << CScriptNum(4) << vector<unsigned char>((const unsigned char*)pszTimestamp, (const unsigned char*)pszTimestamp + strlen(pszTimestamp));
        txNew.vout[0].nValue = 50 * COIN;
        txNew.vout[0].scriptPubKey = CScript() << ParseHex("0455e668c4bf7e51c779189441519f4ad5e08febefaee4395e74ac034b07d156624bee56ff9976cd5a8203f901e1cac277d1ea05f2056b6de3f82eba43c7120a3c") << OP_CHECKSIG;
        genesis.vtx.push_back(txNew);
        genesis.hashPrevBlock = 0;
        genesis.hashMerkleRoot = genesis.BuildMerkleTree();
        genesis.nVersion = 1;
        genesis.nTime    = 1522055189;
        genesis.nBits    = 0x1e0ffff0;
        genesis.nNonce   = 288066;

        hashGenesisBlock = genesis.GetHash();
        assert(hashGenesisBlock == uint256("0x000001b2ec509acef0ae8276d11b6229e577e0201cc66a1ba51c115b77d8ccb0"));
        assert(genesis.hashMerkleRoot == uint256("0xe186397c206c4ccdb7ecca057d2cb613feaa521f7895323c24383586c15585d2"));```
  
  `+ что должно стать в новом файле`
  
  
  
  
1.Балансировка сложности
----------------------

  **config.h** *(файл расположен по адресу: адрес/расположения/файла)*
  
  
### описание файла


  пример_кода:
  
  `- что было в оригинальном файле`
  
  `+ что должно стать в новом файле`



2.Конфигурация премайна
---------------------

  **config.h** *(файл расположен по адресу: адрес/расположения/файла)*
  
  
### описание файла


  пример_кода:
  
  `- что было в оригинальном файле`
  
  `+ что должно стать в новом файле`
  
  
  
3.Список мастернод *(имплементация)*
------------------------------------

  **config.h** *(файл расположен по адресу: адрес/расположения/файла)*
  
  
### описание файла


  пример_кода:
  
  `- что было в оригинальном файле`
  
  `+ что должно стать в новом файле`
  
  
  
4.Список чекаутов блокчейна *(имплементация снапшотов блокчейна)*
----------------------

  **config.h** *(файл расположен по адресу: адрес/расположения/файла)*
  
  
### описание файла


  пример_кода:
  
  `- что было в оригинальном файле`
  
  `+ что должно стать в новом файле`
  
  
4.Размер эмиссии
----------------------------

  **config.h** *(файл расположен по адресу: адрес/расположения/файла)*
  
  
### описание файла


  пример_кода:
  
  `- что было в оригинальном файле`
  
  `+ что должно стать в новом файле`
  
