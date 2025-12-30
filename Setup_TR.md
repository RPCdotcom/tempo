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
