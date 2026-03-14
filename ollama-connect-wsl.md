Siap, Bos! Ini versi **"Anti-Ribet & Paling Bersih"**. Kita tidak akan install aplikasi Ollama dua kali. Kita cuma akan "menghubungkan jembatan" antara Windows dan WSL.

Berikut adalah tutorial lengkapnya:

---

## 🚀 Tutorial: Gunakan Ollama Windows di WSL Ubuntu

### Langkah 1: Install & Set Up di Windows (Server)

Pastikan Ollama sudah terinstall di Windows kamu. Sekarang kita buka "pintu" agar WSL bisa masuk.

1. Buka **Start Menu** > ketik **"Environment Variables"** > Pilih **Edit the system environment variables**.
2. Klik tombol **Environment Variables** di pojok kanan bawah.
3. Di bagian **User variables**, klik **New**.
* **Variable name:** `OLLAMA_HOST`
* **Variable value:** `0.0.0.0`


4. Klik **OK** di semua jendela.
5. **Penting:** Klik kanan icon Ollama di *system tray* (pojok kanan bawah deket jam), pilih **Quit**, lalu buka lagi aplikasi Ollama-nya.

---

### Langkah 2: Buat "Jalan Pintas" di WSL (Client)

Kita tidak akan install Ollama di Ubuntu. Kita cuma akan buat **Alias** (nama panggilan) agar saat kamu ngetik `ollama` di Ubuntu, yang jalan sebenarnya adalah `ollama.exe` milik Windows.

1. Buka terminal **WSL Ubuntu** kamu.
2. Masukkan perintah sakti ini (langsung copas saja):
```bash
echo "alias ollama='ollama.exe'" >> ~/.bashrc

```


3. Refresh settingan terminalnya:
```bash
source ~/.bashrc

```



---

### Langkah 3: Tes Koneksi

Sekarang saatnya pembuktian. Ketik perintah ini di terminal WSL:

```bash
ollama list

```

**Apa yang terjadi?**

* WSL akan memanggil `ollama.exe` di Windows.
* Karena kita sudah set `OLLAMA_HOST=0.0.0.0`, si Windows akan menjawab dengan daftar model yang sudah kamu download (termasuk **Qwen**).

---

### Langkah 4: Jalankan Model Qwen

Sekarang kamu bisa langsung ngobrol sama Qwen lewat terminal Linux kamu:

```bash
ollama run qwen

```

---

### Kenapa Cara Ini Paling Oke?

* **Zero Storage:** Gak makan memori tambahan di Ubuntu.
* **Satu Gudang Model:** Model yang kamu download di Windows otomatis bisa dipakai di WSL. Gak perlu download dua kali (sayang kuota!).
* **Gak Perlu Install `zstd` dkk:** Karena kita gak jalanin installer Linux sama sekali.

**Sudah muncul daftar model Qwen-nya di WSL? Kalau sudah, selamat! Kamu sudah resmi jadi "Bridge Master" Windows-Linux.** Ada lagi yang mau ditanyakan soal settingan GPU-nya?
```bash
running model qwen

ollama launch claude --model qwen2.5-coder
````