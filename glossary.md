Глоссарий
=========


*UTXO (Unspent Transaction Output)*
-----------------------------------

### Определение

Неизрасходованный выход транзакции - может быть использван как вход в новую транзакцию

### Ссылки

- [UTXO](https://bitcoin.org/en/developer-guide#term-utxo) — Bitcoin.org Developer Guide
- [How big is the UTXO database?](http://bitcoin.stackexchange.com/q/35980) — Bitcoin StackExchange


*Input, Transaction Input, TxIn*
--------------------------------

### Определение

Вход в транзакцию, которая содержит три поля: outpoint, signature script(скрипт подписи) и sequence number (порядковый номер). Outpoint ссылается на предыдущий вывод, и скрипт подписи позволяет потратить его.

### Ссылки

- [Input](https://bitcoin.org/en/developer-guide#term-input) — Bitcoin.org Developer Guide
- [TxIn](https://bitcoin.org/en/developer-reference#txin) — Bitcoin.org Developer Reference
- [Inputs](https://en.bitcoin.it/wiki/Transaction#Input) — Bitcoin Wiki


*Output, Transaction Output, TxOut*
-----------------------------------

### Определение

Вывод в транзакции, который содержит два поля: поле значения для передачи нулевого или большего количества символов satoshis и сценарий pubkey для указания того, какие условия должны быть выполнены для тех, кто будет продолжать тратить.

### Не путать c

- Outpoint (ссылка на конкретный output)

### Ссылки

- [Output](https://bitcoin.org/en/developer-guide#term-output) — Bitcoin.org Developer Guide
- [TxOut](https://bitcoin.org/en/developer-reference#txout) — Bitcoin.org Developer Reference
- [Output](https://en.bitcoin.it/wiki/Transaction#Output) — Bitcoin Wiki


*Pubkey Script, ScriptPubKey*
-----------------------------

### Определение

Сценарий, включенный в outputs, который устанавливает условия, которые должны выполняться для тех, кто будет потрачен. Данные для выполнения условий могут быть предоставлены в скрипте подписи (signature script). Сценарии Pubkey называется scriptPubKey в коде.

### Не путать с

- Pubkey (открытый ключ, который может использоваться как часть сценария pubkey, но не обеспечивает программируемый механизм аутентификации)

- Signature script (скрипт, который предоставляет данные сценарию pubkey)

### Ссылки

- [Pubkey script](https://bitcoin.org/en/developer-guide#term-pubkey-script) — Bitcoin.org Developer Guide


*Signature Script, ScriptSig*
-----------------------------

### Определение

Данные, генерируемые "траттером" =), который почти всегда используется в качестве переменных для удовлетворения сценария pubkey. Скрипты подписи называются scriptSig в коде.

### Не путать с

ECDSA signature (подпись, которая может использоваться как часть сценария pubkey в дополнение к другим данным)

### Ссылки

- [Signature script](https://bitcoin.org/en/developer-guide#term-signature-script) — Bitcoin.org Developer Guide


*P2PKH Address, Pay To PubKey Hash*
-----------------------------------

### Определение 

Bitcoin адрес, содержащий хеш открытого ключа, позволяющий сгенерировать стандартный сценарий pubkey, который платит PubKey Hash (P2PKH).

### Ссылки

- [P2PKH](https://bitcoin.org/en/developer-guide#term-p2pkh) — Bitcoin.org Developer Guide

- [P2PKH versus P2PK](http://bitcoin.stackexchange.com/a/32642) — Bitcoin StackExchange


*P2SH Address, Pay To Script Hash*
----------------------------------

### Bitcoin адрес, содержащий хеш скрипта, позволяющий сгенерировать стандартный сценарий pubkey, который платит за скрипт-хэш (P2SH). Скрипт может быть почти любым допустимым скриптом pubkey

### Ссылки

- [P2SH](https://bitcoin.org/en/developer-guide#term-p2sh) — Bitcoin.org Developer Guide

- [BIP16: pay to script hash](https://github.com/bitcoin/bips/blob/master/bip-0016.mediawiki) — Bitcoin Improvement Proposals

- [P2SH](https://en.bitcoin.it/wiki/P2SH) — Bitcoin Wiki