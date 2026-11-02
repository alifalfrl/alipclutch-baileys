# Clutch Baileys

<p align="center">
  <img src="https://cdn-alip.clutch.web.id/api/u/mq1sahji.jpg" width="450" style="border-radius:12px; box-shadow: 0 4px 8px rgba(0,0,0,0.2);">
</p>

<p align="center">
  <strong>Enterprise-Grade WhatsApp Business API</strong><br>
  <sub>High-performance modified Baileys library for scalable WhatsApp integrations</sub>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Node.js-v20+-green?style=for-the-badge&logo=node.js" alt="Node.js">
  <img src="https://img.shields.io/badge/Modified-Baileys-blue?style=for-the-badge" alt="Modified Baileys">
  <img src="https://img.shields.io/badge/License-MIT-yellow?style=for-the-badge" alt="License">
</p>

---

## Deskripsi
**Clutch Baileys** (`alipclutch-baileys`) adalah versi modifikasi dari *Baileys library* yang dioptimalkan untuk kebutuhan bisnis, otomasi, dan integrasi skala besar. Pustaka ini mendukung berbagai fitur pesan interaktif terbaru dari WhatsApp Business API yang tidak tersedia di library standar.

---

## Fitur Unggulan
* **Auto-Reject Calls & Auto-Reply**: Blokir panggilan telepon suara/video masuk secara otomatis dan kirim pesan respon kustom.
* **Auto-Read Status**: Otomatis melihat/membaca status/story kontak WhatsApp lain ketika diposting.
* **Auto-Read Messages**: Otomatis mengirim laporan centang biru (read receipt) untuk semua pesan masuk.
* **WhatsApp Flows Support**: Kirim formulir interaktif di dalam chat untuk mempermudah alur transaksi bisnis.
* **Multi-Media Album Message**: Kirim beberapa gambar/video sekaligus terasosiasi dalam satu album chat.
* **Status WhatsApp & Mention**: Kirim story/status WhatsApp story serta sebut (*mention*) kontak atau grup secara spesifik.
* **Interactive & Product Message**: Dukungan penuh untuk list, native flow buttons, dan katalog produk.
* **Custom Message ID Prefix**: Setiap pesan otomatis menggunakan generator ID kustom berawalan `'CLUTCH-'`.

---

## Persyaratan Sistem
* **Node.js**: Versi `20` atau yang lebih baru.

---

## Instalasi
```bash
npm install alipclutch-baileys
# atau menggunakan yarn
yarn add alipclutch-baileys
```

---

## Contoh Penggunaan & Konfigurasi

### 1. Inisialisasi & Konfigurasi Auto-Reject Calls
```javascript
const makeWASocket = require('alipclutch-baileys').default;
const { useMultiFileAuthState } = require('alipclutch-baileys');

async function startSock() {
    const { state, saveCreds } = await useMultiFileAuthState('auth_info_folder');
    
    const sock = makeWASocket({
        auth: state,
        printQRInTerminal: true,
        
        // PENGATURAN KUSTOM AUTO-REJECT CALLS & AUTO-READ
        rejectCalls: true, // Setel ke true untuk menolak semua panggilan masuk secara otomatis
        callRejectMessage: { text: "Maaf, akun ini tidak menerima panggilan suara/video." }, // Pesan balasan otomatis
        autoReadStatus: true, // Setel ke true untuk membaca otomatis semua status/story kontak
        autoReadMessages: false // Setel ke true jika ingin otomatis mengirim centang biru untuk semua chat masuk
    });

    sock.ev.on('creds.update', saveCreds);

    sock.ev.on('connection.update', (update) => {
        const { connection, lastDisconnect } = update;
        if (connection === 'close') {
            console.log('Koneksi terputus, mencoba menghubungkan kembali...');
            startSock();
        } else if (connection === 'open') {
            console.log('Koneksi berhasil terbuka!');
        }
    });

    return sock;
}

startSock();
```

---

## Dokumentasi API Pengiriman Pesan Kustom

Seluruh fitur di bawah diakses menggunakan method `sendMessage` standar atau instance utilitas `sock.rahmi`.

### 1. Mengirim WhatsApp Flows (Form Interaktif)
Kirim antarmuka formulir interaktif menggunakan tipe `flowMessage`.
```javascript
await sock.sendMessage(jid, {
    flowMessage: {
        title: "Pilih Layanan",
        body: "Silakan isi formulir pemesanan dengan menekan tombol di bawah ini.",
        footer: "Clutch Flows",
        flowId: "1234567890", // ID Flow dari Meta Developer
        buttonText: "Buka Formulir",
        screen: "RESERVATION_SCREEN", // ID Screen awal Flow Anda
        params: {
            user_name: "Alip Clutch",
            phone: "628xxx"
        }
    }
});
```

### 2. Mengirim Album (Multi-Media Terkait)
Kirim beberapa gambar atau video sekaligus terkelompok dalam satu album pesan di WhatsApp.
```javascript
await sock.sendMessage(jid, {
    albumMessage: [
        { image: { url: 'https://source.unsplash.com/random/800x600?nature' }, caption: 'Foto Alam 1' },
        { image: { url: 'https://source.unsplash.com/random/800x600?water' } },
        { video: { url: 'http://techslides.com/demos/sample-videos/small.mp4' } }
    ],
    albumOptions: {
        newsletterJid: "120363417910362894@newsletter", // Opsional
        newsletterName: "Clutch Info Channel", // Opsional
        senderName: "Alip Clutch" // Opsional
    }
});
```

### 3. Mengirim Status / Story Mention (Sebut Kontak/Grup)
Kirim status WhatsApp baru ke `status@broadcast` serta mention nomor kontak atau grup tertentu.
```javascript
await sock.sendStatusMention({
    image: { url: 'https://source.unsplash.com/random/1080x1920?business' },
    text: "Pembaruan sistem Clutch Baileys siap digunakan!"
}, [
    "6281249703469@s.whatsapp.net",
    "1203630249219323@g.us"
]);
```

### 4. Mengirim Pesan Produk & Katalog
```javascript
await sock.sendMessage(jid, {
    productMessage: {
        title: "Clutch Premium Bot License",
        description: "Akses bot premium 1 bulan penuh tanpa batas.",
        body: "Dapatkan penawaran terbaik hari ini!",
        footer: "Klik tombol di bawah untuk detail produk",
        productId: "998877",
        retailerId: "clutch-001",
        priceAmount1000: 50000000, // Rp 50.000 (1000 * harga asli)
        currencyCode: "IDR",
        thumbnail: { url: "https://cdn-alip.clutch.web.id/api/u/mq1sahji.jpg" },
        buttons: [
            {
                name: "send_location",
                buttonParamsJson: "{}"
            }
        ]
    }
});
```

### 5. Mengirim Pesan Pembayaran (`PAYMENT`)
```javascript
await sock.sendMessage(jid, {
    requestPaymentMessage: {
        amount: 25000000, // Rp 25.000
        currency: "IDR",
        expiry: Math.floor(Date.now() / 1000) + 86400, // Berlaku 24 Jam
        from: "0@s.whatsapp.net",
        note: "Pembayaran lisensi bot bulanan",
        // Atau lampirkan stiker
        // sticker: { stickerMessage: ... }
    }
});
```

### 6. Mengirim Pesan Acara (`EVENT`)
```javascript
await sock.sendMessage(jid, {
    eventMessage: {
        name: "Clutch Gathering 2026",
        description: "Temu komunitas developer & pengguna bot WhatsApp Indonesia.",
        startTime: Date.now() + 86400000,
        endTime: Date.now() + 86400000 + 7200000,
        location: {
            degreesLatitude: -6.200000,
            degreesLongitude: 106.816666,
            name: "Jakarta Convention Center"
        },
        joinLink: "https://chat.whatsapp.com/..."
    }
});
```

### 7. Mengirim Hasil Poling (`POLL_RESULT`)
```javascript
await sock.sendMessage(jid, {
    pollResultMessage: {
        name: "Hasil polling makan siang hari ini:",
        pollVotes: [
            { optionName: "Nasi Goreng", optionVoteCount: 15 },
            { optionName: "Mie Ayam", optionVoteCount: 8 }
        ]
    }
});
```

### 8. Mengirim Pesan Interaktif (Native Flow Buttons / List / Tombol)
Kirim pesan interaktif dengan tombol native seperti pilihan menu (Single-Select List) atau tombol link (CTA URL).
```javascript
await sock.sendMessage(jid, {
    interactiveMessage: {
        header: "Layanan Clutch",
        title: "Silakan pilih salah satu menu di bawah ini untuk memulai transaksi.",
        footer: "Clutch Bot System",
        // Opsional: Sertakan gambar/media di bagian atas
        // image: { url: "https://example.com/banner.jpg" },
        buttons: [
            {
                name: "single_select",
                buttonParamsJson: JSON.stringify({
                    title: "Buka Menu",
                    sections: [
                        {
                            title: "Daftar Produk",
                            rows: [
                                { id: "license_premium", title: "Lisensi Premium", description: "Beli lisensi bot 30 hari" },
                                { id: "topup_balance", title: "Top Up Saldo", description: "Isi ulang saldo instan" }
                            ]
                        }
                    ]
                })
            },
            {
                name: "cta_url",
                buttonParamsJson: JSON.stringify({
                    display_text: "Gabung Grup",
                    url: "https://chat.whatsapp.com/...",
                    merchant_url: "https://chat.whatsapp.com/..."
                })
            }
        ]
    }
});
```

---

## Kontak & Informasi Pengembang
Jika Anda memerlukan bantuan teknis, integrasi berbayar, atau ingin berkontribusi:
* **Developer**: Alip Clutch
* **Instagram**: [@alifalfrl__](https://instagram.com/alifalfrl__)
* **Telegram**: [@bigedikeman](https://t.me/bigedikeman)
* **WhatsApp**: [+62 812-4970-3469](https://wa.me/6281249703469)
