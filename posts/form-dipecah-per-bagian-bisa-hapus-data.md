---
title: Memecah form jadi beberapa bagian bisa menghapus data diam-diam
description: Satu form profil dipecah per bagian, dan menyimpan satu bagian malah menimpa bagian lain.
date: 2026-10-22
tags: [laravel, form, debugging]
draft: true
---

## Pelajaran desain controller saat memecah form

Gua dapet pelajaran penting soal desain controller pas memecah form. Masalah ini berakar dari controller update profil lama yang perilakunya menganggap semua field selalu dikirim dalam satu kali proses.

Waktu form akhirnya dipecah per bagian, cara kerja controller update profil lama ini langsung menimbulkan masalah serius. Menyimpan satu bagian ternyata menimpa tautan tambahan dengan array kosong dan nomor WA dengan null. Nggak cuma itu, username juga jadi wajib di semua bagian form. Kondisi ini bikin data terhapus diam-diam hanya karena gua atau pengguna cuma berniat memperbarui satu bagian tertentu saja [ISI: detail alur pengisian form oleh pengguna].

Penyebab utamanya balik lagi ke asumsi awal: controller update profil lama menganggap semua field selalu dikirim. Jadi, ketika form dipecah per bagian dan field lain tidak dikirimkan, controller tetap memprosesnya dan akhirnya menimpa data yang ada dengan nilai kosong tersebut. Validasi username yang jadi wajib di semua bagian juga makin memperjelas kalau controller lama ini tidak cocok untuk form yang sudah terpisah-pisah.

Dari kejadian ini, ada langkah perbaikan yang gua lakukan:
1. Memastikan tiap bagian punya method simpan sendiri di controller, sehingga tiap bagian hanya fokus menangani datanya masing-masing.
2. Menulis test isolasi yang memastikan menyimpan satu bagian tidak mengubah bagian lain.

Pelajaran buat gua, saat bentuk pengiriman data berubah—seperti memecah form per bagian—asumsi di controller juga harus dirombak. Controller tidak boleh lagi menganggap semua field selalu dikirim. Solusi dengan membuat method simpan sendiri untuk tiap bagian, ditambah test isolasi yang memastikan perubahan di satu bagian tidak mengganggu bagian lain, terbukti ampuh mencegah data nomor WA tertimpa null atau tautan tambahan tertimpa array kosong.
