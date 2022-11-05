# Multiple Long/Short Orders

## Bahan bahan

1. Node Sei Network yang sudah sinkron block dan sudah import wallet yang kalian gunakan untuk testing Vortex Protocol
2. Kesabaran dan ketelitian
3. Secangkir kopi

Jika kalian masih belum menjalankan Node Sei, maka kalian perlu melakukan instalasi terlebih dahulu. Berikut ada 2 referensi untuk instalasi Node Sei

>- [Tutorial dari Mamang KJ](https://github.com/kj89/testnet_manuals/blob/main/sei/README.md)

>- [Tutorial dari Mamang Brocha](https://brocha.in/testnet/sei/installation/)

NB. Kalau saya pribadi menggunakan tutorial instalasi dari Brocha karena versi node nya sudah yang paling terbaru dan untuk tutorial setelah penginstalan saya ambil beberapa command dari KJ seperti command untuk import wallet

## Tutorial

Saya anggap kalian sudah sukses menginstall Node Sei dan wallet sudah ready. Kalian hanya perlu mengikuti tutorial ini, usahakan dan biasakan untuk teliti agar mengurangi risiko error dan lain sebagainya

### 1. Copy & paste command di bawah ini langsung ke VPS  / CLI kalian

```bash
echo '{
  "body": {
    "messages": [
      {
        "@type": "/seiprotocol.seichain.dex.MsgPlaceOrders",
        "creator": "YOUR_ADDRESS",
        "orders": [
          {
            "id": "0",
            "status": "PLACED",
            "account": "YOUR_ADDRESS",
            "contractAddr": "sei1466nf3zuxpya8q9emxukd7vftaf6h4psr0a07srl5zw74zh84yjqpeheyc",
            "price": "25.000000000000000000",
            "quantity": "10.000000000000000000",
            "priceDenom": "UST2",
            "assetDenom": "ATOM",
            "orderType": "LIMIT",
            "positionDirection": "LONG",
            "data": "{\"position_effect\":\"Open\",\"leverage\":\"1\"}",
            "statusDescription": ""
          }
        ],
        "contractAddr": "sei1466nf3zuxpya8q9emxukd7vftaf6h4psr0a07srl5zw74zh84yjqpeheyc",
        "funds": [
          {
            "denom": "ust2",
            "amount": "10"
          }
        ]
      },
      {
        "@type": "/seiprotocol.seichain.dex.MsgPlaceOrders",
        "creator": "YOUR_ADDRESS",
        "orders": [
          {
            "id": "0",
            "status": "PLACED",
            "account": "YOUR_ADDRESS",
            "contractAddr": "sei1466nf3zuxpya8q9emxukd7vftaf6h4psr0a07srl5zw74zh84yjqpeheyc",
            "price": "25.000000000000000000",
            "quantity": "10.000000000000000000",
            "priceDenom": "UST2",
            "assetDenom": "ATOM",
            "orderType": "MARKET",
            "positionDirection": "LONG",
            "data": "{\"position_effect\":\"Open\",\"leverage\":\"1\"}",
            "statusDescription": ""
          }
        ],
        "contractAddr": "sei1466nf3zuxpya8q9emxukd7vftaf6h4psr0a07srl5zw74zh84yjqpeheyc",
        "funds": [
          {
            "denom": "ust2",
            "amount": "10"
          }
        ]
      }
    ],
    "memo": "",
    "timeout_height": "0",
    "extension_options": [],
    "non_critical_extension_options": []
  },
  "auth_info": {
    "signer_infos": [],
    "fee": {
      "amount": [
        {
          "denom": "usei",
          "amount": "0"
        }
      ],
      "gas_limit": "0",
      "payer": "",
      "granter": ""
    }
  },
  "signatures": []
}' > $HOME/gen_tx.json
```

### 2. Penyesuaian data program

Edit file gen_tx.json

 ```bash
 nano $HOME/gen_tx.json
 ```
 
 Ganti YOUR_ADDRESS dengan address Sei kalian, kalian juga bisa mengedit price, quantity, orderan ( Long atau Short ) dan jenis orderan ( Market atau Limit ) berdasarkan kebutuhan dan keinginan kalian
 
 Setelah mengedit file gen_tx.json, lakukan Save dengan cara tekan `CTRL` + `X` lalu tekan `Y` kemudian `Enter`
 
 ### 3. Membuat variable
 
 ```bash
ACC=$(seid q account YOUR_ADDRESS -o json | jq -r .account_number)
```

Ganti YOUR_ADDRESS dengan address Sei kalian

```bash
seq=$(seid q account YOUR_ADDRESS -o json | jq -r .sequence)
```

Ganti YOUR_ADDRESS dengan address Sei kalian

### 4. Eksekusi program

```bash
seid tx sign $HOME/gen_tx.json -s $seq -a $ACC --offline \
--from wallet --chain-id atlantic-1 \
--output-document $HOME/txs.json
```

Lakukan pengecekan apakah eksekusi nya berhasil atau gagal dengan command ini

```bash
seid tx broadcast $HOME/txs.json
```

<img width="500" alt="Contoh" src="https://user-images.githubusercontent.com/73088644/200134310-f7433750-be06-4e70-8c97-89d026c55860.png">

Jika `code: 0` seperti gambar di atas, maka program berjalan lancar dan tx hash yang muncul adalah Valid dan sudah terbaca di dalam blockchain, kalian bisa copy paste tx hash tersebut ke [Sei Explorer](https://sei.explorers.guru) agar lebih yakin

Tapi, jika muncul error atau code nya bukan angka 0 maka dapat dipastikan program tersebut gagal dieksekusi karena suatu kondisi.


## NB. Setiap transaksi baru wajib diulang dari langkah ke 2 hingga langkah ke 4 agar tidak mengalami error yang tidak diinginkan.
