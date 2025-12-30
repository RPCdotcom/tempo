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

