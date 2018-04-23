
// данный документ раздельно описывает параметры конфигурации монеты Dash 0.12.4.3

// с примерами реализации и с указателем расположения конфигурационных

// файлов. параметры пронумерованы с тем, чтобы *в основном development-*

// *документе* они были указаны под номерами со ссылками на **данный** документ.




0.Конфигурация genesis-блока
-----------------

  **chainparams.cpp** *(файл расположен по адресу: src/chainparams.cpp)*

### описание файла

  пример_кода:

    const char* pszTimestamp = "Bloomberg 26.03.2018 China's Oil Contract to Rival WTI and Brent Starts Trading";
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
    assert(genesis.hashMerkleRoot == uint256("0xe186397c206c4ccdb7ecca057d2cb613feaa521f7895323c24383586c15585d2"));  
  
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
  
3.Список DNS сидов *(имплементация)*
------------------------------------

  **chainparamsseeds.h** *(файл расположен по адресу: src/chainparamsseeds.h)*
  
### описание файла

  пример_кода:

    static SeedSpec6 pnSeed6_main[] = {
    {{0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0xff,0xff,0x7f,0x00,0x00,0x01}, 9888}
    };

    static SeedSpec6 pnSeed6_test[] = {
    //    {{0xfd,0x87,0xd8,0x7e,0xeb,0x43,0x99,0xcb,0x26,0x31,0xba,0x48,0x51,0x31,0x39,0x0d}, 18333},
    //    {{0xfd,0x87,0xd8,0x7e,0xeb,0x43,0x44,0xf4,0xf4,0xf0,0xbf,0xf7,0x7e,0x6d,0xc4,0xe8}, 18333}
    };
  
4.Список чекпоинтов блокчейна *(имплементация снапшотов блокчейна)*
----------------------

  **chainparams.cpp** *(файл расположен по адресу: src/chainparams.cpp)*
  
### описание файла

  пример_кода:
  
    /**
     * What makes a good checkpoint block?
     * + Is surrounded by blocks with reasonable timestamps
     *   (no blocks before with a timestamp after, none after with
     *    timestamp before)
     * + Contains no strange transactions
     */

    static Checkpoints::MapCheckpoints mapCheckpoints =
            boost::assign::map_list_of
            (  0, uint256("0x000001b2ec509acef0ae8276d11b6229e577e0201cc66a1ba51c115b77d8ccb0"))
            ;
    static const Checkpoints::CCheckpointData data = {
            &mapCheckpoints,
            1522055189, // * UNIX timestamp of last checkpoint block
            0,     // * total number of transactions between genesis and last checkpoint
                        //   (the tx=... number in the SetBestChain debug.log lines)
            2800        // * estimated number of transactions per day after checkpoint
        };

    static Checkpoints::MapCheckpoints mapCheckpointsTestnet =
            boost::assign::map_list_of
            ( 0, uint256("0000064385d07b1da55f314270fb61f6d36692f8af126f8bc8679d1491c1b842"))
            ;
    static const Checkpoints::CCheckpointData dataTestnet = {
            &mapCheckpointsTestnet,
            1522055190,
            0,
            500
        };

    static Checkpoints::MapCheckpoints mapCheckpointsRegtest =
            boost::assign::map_list_of
            ( 0, uint256("0x58e35f033e2fd956f5ac7d1d023c56c0e39a1c7115936620cd92f2e6c66d47dc"))
            ;
    static const Checkpoints::CCheckpointData dataRegtest = {
            &mapCheckpointsRegtest,
            0,
            0,
            0
        };
  
4.Размер эмиссии
----------------------------

  **config.h** *(файл расположен по адресу: адрес/расположения/файла)*
  
  
### описание файла


  пример_кода:
  
  `- что было в оригинальном файле`
  
  `+ что должно стать в новом файле`
  
