# Install-Node-Pipe-Network-Testnet

# 🚀 Tutorial Instalasi Node Pipe Network (Testnet)

> 📅 Terakhir diperbarui: 11 Mei 2025  
> 💡 Sumber referensi: [docs.pipe.network](https://docs.pipe.network/nodes/testnet)  

---

## 🖥️ Spesifikasi Minimum Sistem

| Komponen | Rekomendasi |
|----------|-------------|
| CPU      | 4 Cores     |
| RAM      | 16 GB       |
| Storage  | 100 GB      |
| Internet | 1 Gbps      |

**Catatan**: Hanya bisa digunakan bagi yang sudah mendapat email undangan untuk menjalankan node.

---

## 🧪 Step 1: Install Dependency (Ubuntu 24.04)

```bash
apt update && apt install libssl-dev ca-certificates -y
```

---

## ⚙️ Step 2: Optimalkan Pengaturan Jaringan

```bash
sudo bash -c 'cat > /etc/sysctl.d/99-popcache.conf << EOL
net.ipv4.ip_local_port_range = 1024 65535
net.core.somaxconn = 65535
net.ipv4.tcp_low_latency = 1
net.ipv4.tcp_fastopen = 3
net.ipv4.tcp_slow_start_after_idle = 0
net.ipv4.tcp_window_scaling = 1
net.ipv4.tcp_wmem = 4096 65536 16777216
net.ipv4.tcp_rmem = 4096 87380 16777216
net.core.wmem_max = 16777216
net.core.rmem_max = 16777216
EOL'
sudo sysctl -p /etc/sysctl.d/99-popcache.conf
```

---

## 🔧 Step 3: Tingkatkan Batas File (File Descriptor)

```bash
sudo bash -c 'cat > /etc/security/limits.d/popcache.conf << EOL
*    hard nofile 65535
*    soft nofile 65535
EOL'
```

> 🚪 Logout dan login kembali ke VPS sebelum lanjut ke step berikutnya.

---

## 📁 Step 4: Buat Direktori Instalasi

```bash
sudo mkdir -p /opt/popcache
sudo mkdir -p /opt/popcache/logs
cd /opt/popcache
```

---

## ⬇️ Step 5: Unduh & Ekstrak Binary

```bash
wget https://download.pipe.network/static/pop-v0.3.0-linux-x64.tar.gz
sudo tar -xzf pop-v0.3.0-linux-*.tar.gz
chmod 755 /opt/popcache/pop
```

---

## 📝 Step 6: Buat File `config.json`

```bash
nano config.json
```

Edit Data Diri Anda :

```json
{
  "pop_name": "nama-pop-anda",
  "pop_location": "kota, negara",
  "invite_code": "code-invite-anda",
  "server": {
    "host": "0.0.0.0",
    "port": 443,
    "http_port": 80,
    "workers": 40
  },
  "cache_config": {
    "memory_cache_size_mb": 4096,
    "disk_cache_path": "./cache",
    "disk_cache_size_gb": 100,
    "default_ttl_seconds": 86400,
    "respect_origin_headers": true,
    "max_cacheable_size_mb": 1024
  },
  "api_endpoints": {
    "base_url": "https://dataplane.pipenetwork.com"
  },
  "identity_config": {
    "node_name": "Nama_Node_Anda",
    "name": "Nama_Anda",
    "email": "email.anda@example.com",
    "website": "https://website-anda.com",
    "discord": "discord_username_anda",
    "telegram": "telegram_anda",
    "solana_pubkey": "SOLANA_WALLET_ADDRESS_ANDA_UNTUK_REWARDS"
  }
}
```

Simpan: tekan `CTRL+X`, lalu `Y`, lalu `Enter`.

---

## 🛠️ Step 7: Buat Service Systemd

```bash
sudo bash -c 'cat > /etc/systemd/system/popcache.service << EOL
[Unit]
Description=POP Cache Node
After=network.target

[Service]
Type=simple
User=root
Group=root
WorkingDirectory=/opt/popcache
ExecStart=/opt/popcache/pop
Restart=always
RestartSec=5
LimitNOFILE=65535
StandardOutput=append:/opt/popcache/logs/stdout.log
StandardError=append:/opt/popcache/logs/stderr.log
Environment=POP_CONFIG_PATH=/opt/popcache/config.json

[Install]
WantedBy=multi-user.target
EOL'
```

---

## 🚀 Step 8: Jalankan Node

```bash
sudo systemctl enable popcache
sudo systemctl daemon-reload
sudo systemctl start popcache
```

---

## 🧪 Step 9: Cek Status

### Cek Service:
```bash
sudo systemctl status popcache
```

### Cek Endpoint Kesehatan:
```bash
curl http://localhost/health
```

### Cek dari Browser:
```
https://<IP-VPS-ANDA>/state
```

---

## ✅ DONE!

> 💡 Pastikan node kamu tetap aktif dan koneksi stabil agar memenuhi syarat reward.

---

## 📚 Referensi

- [Dokumentasi Resmi Pipe Network](https://docs.pipe.network/nodes/testnet)
- [Join My Channel Airdrop](https://t.me/CryptoProID)
