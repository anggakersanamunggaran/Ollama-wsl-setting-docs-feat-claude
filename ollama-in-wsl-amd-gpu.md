Berikut dokumentasi yang bisa kamu pakai untuk README / catatan setup.

---

# Dokumentasi Menjalankan Ollama + Qwen2.5-Coder di WSL Ubuntu 24.04 dengan GPU AMD (RX 6600XT / ROCm)

Dokumentasi ini menjelaskan cara menjalankan **model coding Qwen2.5-Coder menggunakan Ollama** di **WSL Ubuntu 24.04** dengan **GPU AMD Radeon RX 6600XT** menggunakan **ROCm di WSL**.

Referensi instalasi ROCm mengikuti dokumentasi resmi AMD:
[https://rocm.docs.amd.com/projects/radeon-ryzen/en/latest/docs/install/installrad/wsl/install-radeon.html](https://rocm.docs.amd.com/projects/radeon-ryzen/en/latest/docs/install/installrad/wsl/install-radeon.html)

---

# 1. Requirements

Hardware:

* GPU: AMD Radeon RX 6600XT
* CPU yang mendukung virtualisasi
* RAM minimal 16GB (direkomendasikan untuk model coding)

Software:

* Windows 11
* WSL2
* Ubuntu 24.04
* ROCm for WSL
* Ollama

---

# 2. Install WSL

Install WSL dari PowerShell (Administrator):

```powershell
wsl --install -d Ubuntu-24.04
```

Cek status WSL:

```powershell
wsl --status
```

Pastikan versi **WSL2**.

---

# 3. Install ROCm di WSL

Ikuti dokumentasi resmi AMD untuk WSL:

[https://rocm.docs.amd.com/projects/radeon-ryzen/en/latest/docs/install/installrad/wsl/install-radeon.html](https://rocm.docs.amd.com/projects/radeon-ryzen/en/latest/docs/install/installrad/wsl/install-radeon.html)

Di Ubuntu WSL jalankan:

```bash
sudo apt update
sudo apt upgrade -y
```

Install package ROCm:

```bash
sudo apt install rocm
```

Tambahkan user ke group video:

```bash
sudo usermod -aG video $USER
```

Restart WSL:

```powershell
wsl --shutdown
```

Masuk kembali ke Ubuntu.

---

# 4. Verifikasi GPU ROCm

Cek apakah GPU terbaca:

```bash
rocminfo
```

atau

```bash
rocm-smi
```

Jika berhasil, GPU **RX 6600XT** akan muncul pada output.

---

# 5. Install Ollama

Install Ollama:

```bash
curl -fsSL https://ollama.com/install.sh | sh
```

Cek versi:

```bash
ollama --version
```

Jalankan service:

```bash
ollama serve
```

---

# 6. Pull Model Qwen2.5-Coder

Download model coding dari Ollama:

```bash
ollama pull qwen2.5-coder
```

Model ini merupakan **LLM khusus coding** yang cukup kuat untuk:

* code generation
* debugging
* refactoring
* scripting

---

# 7. Menjalankan Claude CLI dengan Model Qwen

Untuk menjalankan **Claude CLI interface** tetapi menggunakan **model Qwen2.5-Coder dari Ollama**, gunakan perintah:

```bash
ollama launch claude --model qwen2.5-coder
```

Perintah ini akan:

* menjalankan CLI interface Claude
* menggunakan backend model **qwen2.5-coder**
* inference dijalankan melalui **Ollama**

---

# 8. Contoh Penggunaan

Contoh prompt:

```
create a simple express js api with sqlite
```

atau

```
fix this python code
```

Model akan menghasilkan kode langsung di terminal.

---

# 9. Monitoring GPU Usage

Cek penggunaan GPU:

```bash
rocm-smi
```

Jika GPU digunakan oleh model, akan terlihat:

* GPU usage
* VRAM usage
* temperature

---

# 10. Troubleshooting

## GPU tidak terdeteksi

Cek:

```bash
rocminfo
```

Jika kosong, kemungkinan:

* ROCm belum terinstall benar
* GPU belum didukung
* WSL belum restart

---

## Ollama tidak menggunakan GPU

Cek log Ollama:

```bash
ollama serve
```

Jika GPU aktif biasanya muncul log seperti:

```
using rocm device
```

---

## Model terlalu lambat

Solusi:

* gunakan quantization lebih kecil
* pastikan GPU terdeteksi
* cek VRAM usage

---

# 11. Informasi Model

Model yang digunakan:

**Qwen2.5-Coder**

Keunggulan:

* optimasi untuk programming
* support banyak bahasa
* performa bagus untuk debugging

Bahasa yang didukung:

* Python
* JavaScript / TypeScript
* Go
* Rust
* C++
* Java
* Bash

---

# 12. Arsitektur Setup

```
Windows 11
   │
WSL2
   │
Ubuntu 24.04
   │
ROCm (AMD GPU RX6600XT)
   │
Ollama
   │
Qwen2.5-Coder
   │
Claude CLI Interface
```

---

Jika kamu mau, aku juga bisa buatkan:

* **versi dokumentasi yang lebih rapih untuk GitHub README**
* **setup yang auto install (script bash 1 file)**
* **cara optimasi VRAM RX 6600XT untuk Ollama**
* **cara bikin Claude Code full local tanpa internet**. 🚀
