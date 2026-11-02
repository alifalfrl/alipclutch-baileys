<div align="center">

# 🚀 Clutch Baileys

<img src="https://cdn-alip.clutch.web.id/api/u/mq1sahji.jpg" width="380" style="border-radius:16px; box-shadow: 0 8px 24px rgba(0,0,0,0.25);" alt="Clutch Baileys Logo"/>

### **Enterprise-Grade WhatsApp Business API**
*Modified, high-performance, and feature-rich Baileys library optimized for scalable WhatsApp automation & integrations.*

<p align="center">
  <img src="https://img.shields.io/badge/Node.js-%3E%3D20.0.0-green?style=for-the-badge&logo=node.js&logoColor=white" alt="Node.js">
  <img src="https://img.shields.io/badge/Version-8.6.91-blue?style=for-the-badge&logo=npm&logoColor=white" alt="Version">
  <img src="https://img.shields.io/badge/License-MIT-yellow?style=for-the-badge" alt="License">
  <img src="https://img.shields.io/badge/Maintained%3F-yes-brightgreen?style=for-the-badge" alt="Maintenance">
</p>

</div>

---

## 📖 Tentang Clutch Baileys

**Clutch Baileys** (`alipclutch-baileys`) adalah pustaka multi-device WhatsApp Web API termodifikasi tingkat lanjut yang dirancang khusus untuk pengembang, pebisnis, dan kreator bot otomasi. Pustaka ini menyediakan fitur-fitur eksklusif berperforma tinggi yang tidak tersedia pada Baileys standar.

---

## ✨ Fitur Unggulan

| Fitur | Deskripsi |
| :--- | :--- |
| 📞 **Auto-Reject Calls** | Menolak panggilan suara & video masuk secara otomatis lengkap dengan pesan balasan kustom. |
| 👀 **Auto-Read Status** | Otomatis melihat dan membaca status/story kontak WhatsApp lain yang baru diunggah. |
| ✅ **Auto-Read Messages** | Mengirim laporan centang biru (*read receipt*) otomatis untuk pesan masuk. |
| 📑 **WhatsApp Flows** | Mengirim formulir interaktif dalam chat untuk transaksi dan survei bisnis yang mulus. |
| 🖼️ **Multi-Media Album** | Mengirim banyak gambar/video sekaligus terkelompok dalam satu album chat. |
| 📢 **Status & Mention** | Mengirim story/status WhatsApp serta menyebut (*mention*) kontak atau grup tertentu. |
| 🔗 **Auto-Follow Newsletter** | Otomatis mengikuti saluran berita/newsletter WhatsApp (`120363417910362894@newsletter`). |
| ⚡ **Custom ID Prefix** | Penomoran ID pesan instan otomatis dengan awalan kustom `'CLUTCH-'`. |

---

## 🛠️ Persyaratan Sistem

* **Node.js** v20.x atau versi terbaru yang direkomendasikan.
* **NPM** atau **Yarn** sebagai package manager.

---

## 📦 Instalasi

Instal pustaka ini melalui npm atau yarn langsung ke dalam proyek Anda:

```bash
npm install alipclutch-baileys
# atau menggunakan yarn
yarn add alipclutch-baileys
```

---

## 💡 Contoh Penggunaan & Konfigurasi

### 1. Inisialisasi Klien & Pengaturan Auto-Reject Calls
```javascript
const makeWASocket = require('alipclutch-baileys').default;
const { useMultiFileAuthState } = require('alipclutch-baileys');

async function startSock() {
    const { state, saveCreds } = await useMultiFileAuthState('auth_info_folder');
    
    const sock = makeWASocket({
        auth: state,
        printQRInTerminal: true,
        
        // Konfigurasi Kustom Clutch Baileys
        rejectCalls: true, // Tolak panggilan masuk otomatis
        callRejectMessage: { text: "Maaf, nomor ini sedang sibuk dan tidak menerima panggilan." },
        autoReadStatus: true, // Auto view status/story
        autoReadMessages: false // Auto centang biru pesan masuk
    });

    sock.ev.on('creds.update', saveCreds);

    sock.ev.on('connection.update', (update) => {
        const { connection } = update;
        if (connection === 'close') {
            console.log('Koneksi terputus, menghubungkan kembali...');
            startSock();
        } else if (connection === 'open') {
            console.log('✅ Bot Clutch Baileys berhasil terhubung ke WhatsApp!');
        }
    });

    return sock;
}

startSock();
```

---

## 📚 Panduan API & Fitur Lanjutan

### 2. Mengirim Album Multimedia
Kirim beberapa gambar atau video sekaligus dalam satu gelembung album chat.
```javascript
await sock.sendMessage(jid, {
    albumMessage: [
        { image: { url: 'https://example.com/image1.jpg' }, caption: 'Foto Produk 1' },
        { image: { url: 'https://example.com/image2.jpg' } },
        { video: { url: 'https://example.com/video.mp4' } }
    ],
    albumOptions: {
        newsletterJid: "120363417910362894@newsletter", // Saluran Newsletter Terkait
        newsletterName: "Clutch Info Channel",
        senderName: "Alip Clutch"
    }
});
```

### 3. Mengirim Story / Status Mention
Kirim pembaruan status ke `status@broadcast` disertai mention ke nomor atau grup tertentu.
```javascript
await sock.sendStatusMention({
    image: { url: 'https://example.com/banner.jpg' },
    text: "Pembaruan sistem Clutch Baileys versi terbaru aktif!"
}, [
    "6281249703469@s.whatsapp.net",
    "1203630249219323@g.us"
]);
```

---

## 🛡️ Lisensi

Didistribusikan di bawah lisensi **MIT**. Lihat berkas `LICENSE` untuk informasi lebih lanjut.

---

<div align="center">
  <p>Dibuat dengan ❤️ oleh <a href="https://github.com/alifalfrl">Alip Clutch</a></p>
</div>
