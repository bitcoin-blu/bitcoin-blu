Bitcoin-Blu
===========

Bitcoin-Blu is a copy of Bitcoin.

How is Bitcoin-Blu different from Bitcoin?
- changed default ports for all networks (mainet, testnet3, testnet4, signet and regtest
- create and restart a new blockchain
- changed prefixes of addresses, keys, messages
- changed icon
- changed currency name


Constants Explicitly Not Changed

| Constant Name           | Value                         | Description                  |
|------------------------|-------------------------------|------------------------------|
| Coinbase Maturity      | `100`                         | Blocks before coinbase is spendable |
| Block Reward           | `50 * COIN`                   | Initial block subsidy        |
| Halving Interval       | `210000`                      | Blocks per subsidy halving   |
| Max Money Check Value  | `MAX_MONEY`                   | Max supply enforcement       |
| PoW Target Timespan    | `14 * 24 * 60 * 60` (1209600) | Two weeks                    |
| PoW Target Spacing     | `10 * 60` (600)               | Ten minutes per block        |
| MAX_MONEY              | `21000000`                    | 21 million BTC cap           |
| Max Halvings           | `64`                          | Reward stops after 64 halvings |

Compare chain name
------------------

network | Bitcoin | Bitcoin-Blu
| :--- | ---: | :---:
| Mainnet | `main` | `main-blu`
| Testnet | `test` | not started
| Testnet4 | `testnet4` | not started
| Regtest | `regtest` | not started
| Signet | `signet` | not started


Compare the ports used
----------------------

network | Bitcoin | Bitcoin-Blu
| :--- | ---: | :---:
__Mainnet__    ||
P2P Port   | `8333` | `8343`
RPC Port   | `8332` | `8342`
incom. TOR | `8334` | `8344`
TOR map to | `8332` | `8342`
SSL?       | `28333`| `28343`
SSL?       | `28332`| `28342`
__Testnet3__   ||
P2P Port   | `18333` | `18343`
RPC Port   | `18332` | `18342`
incom. TOR | `18334` | `18343`
TOR map to | `18332` | `18342`
__Testnet4__   ||
P2P Port   | `48333` | `48343`
RPC Port   | `48332` | `48342`
incom. TOR | `48334` | `48344`
TOR map to | `48332` | `48342`
__Regtest__   ||
P2P Port   | `18444` | `18454`
RPC Port   | `18443` | `18453`
incom. TOR | `18445` | `18455`
TOR map to | `18443` | `18453`
__Signet__     ||
P2P Port   | `38333` | `38343`
RPC Port   | `38332` | `38342`
incom. TOR | `38334` | `38344`
TOR map to | `38332` | `38342`



Compare magic bytes
-------------------

network | Bitcoin | Char | Bitcoin-Blu
| :--- | ---: | :--: | :--:
Mainnet  | `f9beb4d9` | `ù ¾ ´ Ù`       | `f9beb5da`
Testnet3 | `0b110907` | `ACK VT HT BEL` | `0b110a08`
Testnet4 | `1c163f28` | `FS SYN > (`    | `1c164029`
Regtest	 | `fabfb5da` | `ú ¿ µ Ú`       | `fabfb6db`
Signet   | `0A03CF40` | `LF ETX Ï @`    | ---> generated hash



Compare genesis block message
-----------------------------

network | value
| :--- | :---
Bitcoin | Mainnet, Testnet3, Signet, $regtest |
message | `The Times 03/Jan/2009 Chancellor on brink of second bailout for banks`        |
Bitcoin | Testnet4 |
message | `03/May/2024 000000000000000000001ebd58c244970b3aa9d783bb001011fbe8ea8e98e00e` |
Bitcoin-Blu | Mainnet, Testnet3, Signet, $regtest |
message | `24/July/2025 Professional wrestling icon Hulk Hogan dies at 71` |
Bitcoin-Blu | Testnet4|
message | `24/July/2025 Professional wrestling icon Hulk Hogan dies at 71` |

__Timestamp__
network | Bitcoin | Date | Bitcoin-Blu | Date
| :--- | ---: | :--: | :--: | :--:
Mainnet  | `1231006505` | `GMT: Saturday, 3 January 2009 18:15:05`   | `1753427181` | `2025-07-25 07:06:21 UTC`
Testnet3 | `1296688602` | `GMT: Wednesday, 2 February 2011 23:16:42` | not started | -
Testnet4 | `1714777860` | `GMT: Friday, 3 May 2024 23:11:00`         | not started | -
Regtest  | `1598918400` | `GMT: Tuesday, 1 September 2020 00:00:00`  | not started | -
Signet   | `1296688602` | `GMT: Wednesday, 2 February 2011 23:16:42` | not started | -

__Timestamp__
network | difficulty | v. | blockrevard | Bitcoin-Blu
| :--- | ---: | :--: | :--: | :--: |
Mainnet  | `0x1d00ffff` | `1` |`50 * COIN`| _same_
Testnet3 | `0x1d00ffff` | `1` |`50 * COIN`| _same_
Testnet4 | `0x1d00ffff` | `1` |`50 * COIN`| _same_
Regtest  | `0x1d00ffff` | `1` |`50 * COIN`| _same_
Signet   | `0x1e0377ae` | `1` |`50 * COIN`| _same_



Wallet adress prefixes
----------------------

network | legacy (P2PKH) | Bitcoin-Blu legacy (P2PKH) |
| :--- | :--- | :--- |
mainet          | `1`        | `B`
testnet 3 and 4 | `m` OR `n` |
regtest         | `m` OR `n` |
signet          | `s`        |

network | SegWit (P2SH-P2WPKH) | Bitcoin-Blu (P2SH-P2WPKH)
| :--- | :--- | :--- |
mainet          | `3`  | `b`
testnet 3 and 4 | `2`  |
regtest         | `2`  |
signet          | `3`  |

network | native Segwit (P2WPKH) | Bitcoin-Blu native Segwit
| :--- | :--- | :--- |
mainet          | `bc`               | `bb`
testnet 3 and 4 | `tb`               | `tt`
regtest         | `bcrt`             | `bbrt`
signet          | `sb` untknown `tb` | `tt`

WIF Prefixes | bitcoin | bitcoin-blu
| :--- | :--- | :---
mainnet prefix uncomressed | `5`        | `7`
mainnet prefix compressed  | `K` or `L` | `U`


### BIP84 / BIP44

Coin type | Path component (coin_type')	| Symbol | Coin
| :--- | :--- | :--- | :---
0 | 0x80000000     | BTC      | Bitcoin
1 | 0x80000001     |          | Testnet (all coins)
? | --> 0x80426c63 | --> BBLU | Bitcoin-Blu


### base58 prefixes

Parameter | Bitcoin | Bitcoin-Blu
| :--- | :--- | :---
`mainnet`    ||
P2PKH Prefix                 | `0x00` (1)    | `0x19` (B)
P2SH Prefix                  | `0x05` (3)    | `0x56` (b)
wif                          | `0x80` (128)  | ---> `0xbc` (188)
Bech32 Prefix                | `bc1`         | `bb`
Extended public keys (xpub)  | `0x0488b21e`  | `0x0488b31f`
Extended private keys (xprv) | `0x0488ade4`  | `0x0488afe5`
`Signet, Regtest, Testnet Value` ||
Testnet P2PKH Prefix         | `0x6f` (m/n)  | `0x6f` (m/n) |
Testnet P2SH Prefix          | `0xc4` (2)    | `0xc4` (2)
wif                          | `0xef` (239)  | `0xef` (239)
Bech32 Testnet Prefix        | `bcrt1`       | `bbrt1`
Extended public keys (xpub)  | `0x043587cf`  | `0x043587cf`
Extended private keys (xprv) | `0x04358394`  | `0x04358394`
`all networks` ||
messagePrefix | Bitcoin Signed Message: | Bitcoin-Blu Signed Msg:


### Block Versions

parameter | version | raw binary | (little endian) | comment
| -- | -- | -- | -- | -- |
Bitcoin mainnet Genesis block version | 1         | `00000001` | `01000000` |
Bitcoin mainner Block 600'000 version | 536870912 | `20000000` | `00000020` | BIP9
||||
Bitcoin-Blu mainnet Genesis block version | 1         | `00000001` | `01000000` |
Bitcoin-Blu mainner Block 1 version       | 536870912 | `20000000` | `00000020` | BIP9



### Transaction Versions

parameter | version | raw binary | (little endian) | comment
| -- | -- | -- | -- | -- |
Bitcoin mainnet Genesis block coinbase transaction version | 1         | `00000001` | `01000000` |
Bitcoin mainner Block 600'000 coinbase transaction version | 1         | `00000001` | `01000000` |
Bitcoin mainner Block 600'000 transaction 01 version       | 2         | `00000002` | `02000000` | BIP 68/112/113
|||
Bitcoin-Blu mainnet Genesis block coinbase transaction version | 1         | `00000001` | `01000000` |
Bitcoin-Blu mainner Block 1 coinbase transaction version       | 1         | `00000001` | `01000000` |
Bitcoin-Blu mainner Block 1 transaction 01 version             | 2         | `00000002` | `02000000` | BIP 68/112/113




