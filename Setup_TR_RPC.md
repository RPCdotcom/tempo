<img width="1500" height="500" alt="image" src="https://github.com/user-attachments/assets/9b02101b-06be-4cbc-a040-e9afa8f4ed02" />

# TEMPO RPC Node Kurulum

| X        | Minimum              | Önerilen |
|------------------|----------------------------|----------------------------|
| **CPU**          | 8+ | 16++ |
| **RAM**          | 16+  | 32++ GB                   |
| **Disk**      | 250GB NVME SSD| 500+ NVME GB SDD                   |
| **Ağ Hızı**      | 500 Mbps | 1 GBPS |
| **UBUNTU**      | UBUNTU 24.04 | + |


- Web : https://tempo.xyz/
- X : https://x.com/tempo

- Web : https://rpcdot.com
- X : https://x.com/rpcdot

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

## 5. Snapshot İndirelim ; 

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
tempo node \
  --follow \
  --http --http.port 8545 \
  --http.api eth,net,web3,txpool,trace
```

- Servisli Hali ; 

```bash
sudo tee /etc/systemd/system/tempo.service > /dev/null <<EOF
[Unit]
Description=Tempo RPC Node
After=network.target
Wants=network.target
 
[Service]
Type=simple
User=$USER
Group=$USER
Environment=RUST_LOG=info
WorkingDirectory=$HOME/tempo
ExecStart=/usr/local/bin/tempo node \\
  --datadir /root/tempo/data \\
  --follow \\
  --port 30303 \\
  --discovery.addr 0.0.0.0 \\
  --discovery.port 30303 \\
  --http \\
  --http.addr 0.0.0.0 \\
  --http.port 8545 \\
  --http.api eth,net,web3,txpool,trace \\
  --metrics 9000 \\
 
Restart=always
RestartSec=10
StandardOutput=journal
StandardError=journal
SyslogIdentifier=tempo
LimitNOFILE=infinity
 
[Install]
WantedBy=multi-user.target
EOF
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
sudo journalctl -u tempo -f
```

<img width="1598" height="597" alt="image" src="https://github.com/user-attachments/assets/5f1c5dea-8afc-4c4d-ac1c-d6536f09b308" />


## Diğer Komutlar ;

- Servis Durum Kontrol ; 

```bash
sudo systemctl status tempo
 ```

- Aktif Peer ( Bağlantı Sayısı )
 
```bash
cast rpc net_peerCount --rpc-url http://localhost:8545
 ```

 - Güncel Blok Numarası ; 

```bash
cast block-number --rpc-url http://localhost:8545
cast block --rpc-url http://localhost:8545
 ```

 - Bunla bakmak için Foundry Tool'u kurmak lazım ama olmadan bakmak isterseniz ; 
```bash
 curl -s -X POST http://127.0.0.1:8545 \
  -H "Content-Type: application/json" \
  --data '{"jsonrpc":"2.0","method":"eth_blockNumber","params":[],"id":1}'
```

- Loglarda Error ( Hata Arama )
```bash
sudo journalctl -u tempo -n 1000 | grep -i "error"
```

- Sevgiler.
- RPCdot Ekibi.

