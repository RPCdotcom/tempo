<img width="1500" height="500" alt="image" src="https://github.com/user-attachments/assets/9b02101b-06be-4cbc-a040-e9afa8f4ed02" />


| X        | Minimum              | Önerilen |
|------------------|----------------------------|----------------------------|
| **CPU**          | 8+ | 16++ |
| **RAM**          | 16+  | 32++ GB                   |
| **Disk**      | 100GB NVME SSD| 1TB+ NVME GB SDD                   |
| **Ağ Hızı**      | 100 Mbps | 1 GBPS |
| **UBUNTU**      | UBUNTU 22.04 | + |


- Web : https://tempo.xyz/
- X : https://x.com/tempo

- Web : https://rpcdot.com
- X : https://x.com/rpcdot

- ⚠️ Önemli:
Tempo validator’ları şu an permissioned. Yani node’u çalıştırabilirsin ama aktif validator olabilmen için Tempo ekibinin senin public key + IP’yi whitelist etmesi gerekir. Aksi halde node senkron olur ama blok üretmez.

## 1. Server Güncelleme : 

```bash
sudo apt update -y && sudo apt upgrade -y
```
## 2. Topluca Paketleri İndirme :

```bash
sudo apt install htop ca-certificates zlib1g-dev libncurses5-dev libgdbm-dev libnss3-dev tmux iptables curl nvme-cli git wget make jq libleveldb-dev build-essential pkg-config ncdu tar clang bsdmainutils lsb-release libssl-dev libreadline-dev libffi-dev jq gcc screen file nano btop unzip lz4 -y
```

## 3. Rust ; 

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

<img width="640" height="795" alt="image" src="https://github.com/user-attachments/assets/f2532bba-abf0-4b24-83b1-e92501bb2ef6" />


- 1 Enter.

- Source ; 
```bash
source $HOME/.cargo/env
```

<img width="480" height="221" alt="image" src="https://github.com/user-attachments/assets/690af1ac-df78-4913-9dd6-4a9b3ce0f0a1" />


- Version Kontrol ; 
```bash
rustc --version
cargo --version
```

<img width="336" height="86" alt="image" src="https://github.com/user-attachments/assets/f2b616ff-ec21-4e93-acd7-a52225c5d44c" />


## 4. Tempo'yu İndirelim ; 

```bash
cargo install --git https://github.com/tempoxyz/tempo.git tempo --root /usr/local
```

<img width="744" height="83" alt="image" src="https://github.com/user-attachments/assets/d14aaff8-5d62-4ce2-ac74-5dceb16c94cb" />


- Doğru İnmişmi Doğrulayalım ; 

```bash
which tempo
```

- Örnek ; 

<img width="653" height="75" alt="image" src="https://github.com/user-attachments/assets/58eae681-c6de-4789-bfae-5b80db190b07" />

- Kod değildir. Örnek ; 

```bash
root@Ubuntu-2404-noble-amd64-base ~ # which tempo
/usr/local/bin/tempo
```


## 5. ED25519 Signing Key Oluşturalım ; 

```bash
mkdir -p ~/tempo/keys
tempo consensus generate-private-key --output ~/tempo/keys/validator.key
```
- Örnek Sonuç ; 

<img width="483" height="87" alt="image" src="https://github.com/user-attachments/assets/3e88e015-7ad0-41b1-a3b3-6c10cc400912" />

- Key'i Doğrulayalım - Yukarı Resimdeki Üstü Çizili Olan Public Key İle Aşağıdaki Aynı Olmalı ; 

```bash
tempo consensus calculate-public-key --private-key ~/tempo/keys/validator.key
```

<img width="716" height="56" alt="image" src="https://github.com/user-attachments/assets/67e404ae-1653-44dc-a09b-39a5d6b0c078" />

## 6. Snapshot İndirelim ; 

- Uzun Süreceği İçin Screen Açalım ; 

```bash
screen -S tempo
```

```bash
mkdir -p ~/tempo/data
tempo download --datadir ~/tempo/data
```

<img width="1217" height="141" alt="image" src="https://github.com/user-attachments/assets/c4225d78-c235-4cab-a29f-2555823b4ca2" />

<img width="1233" height="164" alt="image" src="https://github.com/user-attachments/assets/01e4bf07-7563-4e17-87ed-eae9053cd743" />

- İndirilmiş Hali.

## Başlatalım ; 

- Normal Başlatma ( Screen gerek ve sürekli çalışmayabilir.)
```bash
tempo node --datadir /root/tempo/data \
    --port 30303 \
    --discovery.addr 0.0.0.0 \
    --discovery.port 30303 \
    --consensus.signing-key /root/tempo/keys/validator.key \
    --consensus.fee-recipient <yandaki tırnakları kaldır buraya 0x ile baslayan fee'leri alacak cüzdan adresini koy> \
```

- Servisli Hali ; 
```bash
nano /etc/systemd/system/tempo.service
```
```bash
[Unit]
Description=Tempo Validator Node
After=network-online.target
Wants=network-online.target

[Service]
User=root
Type=simple
ExecStart=/usr/local/bin/tempo node \
  --datadir /root/tempo/data \
  --port 30303 \
  --discovery.addr 0.0.0.0 \
  --discovery.port 30303 \
  --consensus.signing-key /root/tempo/keys/validator.key \
  --consensus.fee-recipient <yandaki tırnakları kaldır buraya 0x ile baslayan fee'leri alacak cüzdan adresini koy> \
  --metrics 0.0.0.0:8002
Restart=always
RestartSec=10
LimitNOFILE=1048576

[Install]
WantedBy=multi-user.target
```

- CTRL X sonrasında CTRL Y ve ENTER. Kayıt Edilecek.
```bash
systemctl daemon-reexec
systemctl daemon-reload
systemctl enable tempo
systemctl start tempo
```

- Sonrasında Loglar İçin ; 
```bash
journalctl -u tempo -f
```

<img width="956" height="248" alt="image" src="https://github.com/user-attachments/assets/c40b902e-57e9-4510-86fd-081f84ee5062" />

 - LOG ANALİZİ (SATIR SATIR)

 ```bash
✅ OLUMLU KISIMLAR (ÇOK ÖNEMLİ)

Bunlar validator’ın doğru şekilde ayağa kalktığını gösteriyor:

INFO Starting consensus engine
INFO using public ed25519 verifying key derived from provided private ed25519 signing key


➡️ Signing key okunmuş ✅
➡️ Consensus engine başlamış ✅

INFO network started
INFO Status connected_peers=1 latest_block=7685327


➡️ P2P ağına bağlandı ✅
➡️ Zincirin güncel bloğunu görüyor ✅

Yani:

❗ Node çalışıyor, senkron, ağda

⚠️ GÖRDÜĞÜN ERROR NE?
ERROR initialize_epoch_state: failed decoding the genesis.extra_data field as an initial DKG outcome

🔑 BU NE ANLAMA GELİYOR?

Bu hata:

❌ Signing key hatası değil

❌ Snapshot bozuk değil

❌ Yanlış binary değil

Bu hata şu anlama gelir:

Sen genesis’te tanımlı aktif validator setinde değilsin

Tempo’da:

İlk DKG (Distributed Key Generation)

Sadece whitelisted / active validator’lar için geçerli

Senin node:

Validator modunda çalışıyor ✅

Ama henüz active set’e alınmamış ⏳

Bu yüzden:

Genesis’teki extra_data → senin için decode edilemiyor

Ama node çalışmaya devam ediyor (loglarda görüyorsun)

👉 Bu beklenen ve NORMAL bir durum.

🟢 ÇOK KRİTİK NOKTA

Bu hata şunlara engel DEĞİL:

İşlem	Durum
Sync	✅
P2P	✅
DKG gözlemleme	✅
Active set’e girince çalışmak	✅
⏳ ŞİMDİ NE OLACAK?

Tempo ekibi:

Validator identity’ni on-chain ekleyecek

IP adresini whitelist edecek

Bir sonraki DKG ceremony (≈48 saat) çalışacak

Node:

Signing share alacak

Proposal üretmeye başlayacak

⏱️ Genelde 48–72 saat
```

- Sevgiler.
- RPCdot Ekibi.
