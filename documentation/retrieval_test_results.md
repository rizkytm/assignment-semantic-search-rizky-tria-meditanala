# Hasil Uji Relevansi Retrieval dan Response AI

Pengujian dengan 5 query sesuai ketentuan brief: 2 query yang jawabannya ada di knowledge base, 2 query berbahasa sendiri (parafrase), dan 1 query yang sengaja tidak ada jawabannya di knowledge base. Angka distance adalah tingkat kemiripan dari FAISS — makin kecil berarti makin mirip.

| # | Query | Tipe | FAQ teratas yang ditemukan | Distance |
| :- | :---- | :--- | :------------------------ | -------: |
| 1 | Apa saja metode pembayaran yang tersedia? | Langsung | Metode pembayaran apa saja yang tersedia? | 0.31 |
| 2 | Berapa lama estimasi waktu pengiriman? | Langsung | Berapa lama estimasi waktu pengiriman barang? | 0.21 |
| 3 | gimana cara tahu pesananku sudah dikirim ke mana? | Parafrase | Bagaimana cara melacak paket yang sedang dikirim? | 0.38 |
| 4 | kalau barang yang datang cacat atau beda gambar gimana? | Parafrase | Saya menerima barang yang salah, apa yang harus saya lakukan? | 0.34 |
| 5 | Apakah bisa tukar tambah HP lama saya dengan produk baru? | Di luar knowledge base | Apakah bisa menukar barang dengan ukuran/warna lain daripada refund? | 0.59 |

## Analisis per Query

**1. Apa saja metode pembayaran yang tersedia?** (ada jawabannya di FAQ)
FAQ yang ketemu langsung yang dimaksud (distance 0.31), dan jawabannya persis isi FAQ: transfer bank, kartu kredit/debit, e-wallet, dan minimarket. Aman.

**2. Berapa lama estimasi waktu pengiriman?** (ada jawabannya di FAQ)
Sama, langsung kena (0.21, paling mirip dari semua percobaan). Jawabannya benar: 2-5 hari dalam kota, 5-10 hari luar pulau.

**3. gimana cara tahu pesananku sudah dikirim ke mana?** (pakai bahasa sendiri, beda dari FAQ)
Di pertanyaan ini tidak ada kata lacak atau resi sama sekali, tapi FAQ yang ketemu tetap soal melacak paket (0.38). Jadi sistemnya mencari maknanya, bukan kata yang kebetulan sama. Jawabannya juga benar soal lacak paket lewat resi di halaman Pesanan Saya.

**4. kalau barang yang datang cacat atau beda gambar gimana?** (pakai bahasa sendiri, soalnya dua hal sekaligus)
Tiga FAQ yang muncul kebetulan pas semua: barang salah, prosedur pengembalian, dan paket rusak. Tapi jawaban LLM-nya agak keteteran, dua kasus itu dijadikan satu dan yang disebut cuma aturan maksimal 3 hari. Padahal untuk barang rusak di FAQ ada aturan sendiri, maksimal 2x24 jam. Lumayan membantu, tapi tidak lengkap.

**5. Apakah bisa tukar tambah HP lama saya dengan produk baru?** (sengaja yang jawabannya tidak ada di FAQ)
Sistem tetap menampilkan 3 FAQ terdekat soal tukar ukuran/warna, ubah nomor telepon, dan bandingkan produk. Sebenarnya tidak ada yang nyambung, tapi LLM-nya jujur bilang belum punya informasi dan menyuruh hubungi customer service, tidak mengarang. Ini karena di prompt ada aturan hanya boleh jawab dari FAQ.

## Kesimpulan Singkat

- Retrieval berhasil menangkap makna pada query parafrase, bukan cuma mencocokkan kata.
- Jawaban LLM setia pada isi FAQ dan berani mengaku tidak tahu saat informasinya tidak ada.
- Query yang soalnya dua hal sekaligus hasilnya belum konsisten lengkap.
- Query yang benar-benar tidak ada jawabannya punya distance di atas 0.5, jauh dari query yang valid (0.21–0.38) — ini bisa dipakai sebagai acuan ambang batas di pengembangan berikutnya.

Detail lengkap peringkat 1–3 tiap query dan jawaban LLM utuh ada di `notebook/starter_notebook_semantic_search.ipynb` bagian pengujian.
