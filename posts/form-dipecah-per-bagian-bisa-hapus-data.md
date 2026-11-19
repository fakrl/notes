---
title: Memecah form jadi beberapa bagian bisa menghapus data diam-diam
description: Satu form profil dipecah per bagian, dan menyimpan satu bagian malah menimpa bagian lain.
date: 2026-11-19
tags: [laravel, form, debugging]
draft: true
---

## Memecah form jadi beberapa bagian bisa menghapus data diam-diam

Gua mau nulis soal satu pelajaran penting tentang desain controller yang gua dapat waktu menangani update profil. Masalah ini kelihatan sepele di awal, tapi efeknya fatal karena data bisa terhapus diam-diam tanpa disadari.

Awalnya, controller update profil yang lama dibuat dengan asumsi bahwa semua field akan selalu dikirim bersamaan dalam satu request. Masalah baru benar-benar muncul setelah form profil tersebut dipecah menjadi beberapa bagian terpisah.

Karena controller update profil yang lama menganggap semua field selalu dikirim, menyimpan satu bagian saja langsung merusak data bagian lainnya. Waktu gua atau pengguna menyimpan satu bagian form, field dari bagian lain yang memang tidak ikut dikirim malah tetap diproses. Hasilnya, aksi menyimpan satu bagian itu menimpa tautan tambahan dengan array kosong. Nggak cuma itu, nomor WA juga ikut tertimpa dengan null. Masalah lainnya, username juga jadi wajib di semua bagian, padahal bagian yang sedang disimpan belum tentu berkaitan dengan username.

Data yang sebelumnya sudah ada pun akhirnya hilang begitu saja karena controller memproses data yang absen seolah-olah nilainya sengaja dikosongkan.

## Langkah Perbaikan

Untuk menyelesaikan masalah ini, controller lama tidak bisa dibiarkan begitu saja. Perbaikan yang dilakukan berfokus pada pemisahan logika dan pengujian:

1. Tiap bagian form dibuatkan method simpan sendiri. Jadi, pemrosesan data tidak lagi disatukan dalam satu controller yang serakah.
2. Dibuat test isolasi untuk memastikan bahwa menyimpan satu bagian benar-benar tidak mengubah data di bagian lain.

Pelajaran dari kasus ini cukup jelas. Ketika form dipecah per bagian, controller di belakangnya juga harus disesuaikan. Membiarkan controller tetap menganggap semua field selalu dikirim saat form sudah dipecah cuma akan berujung pada data yang tertimpa secara diam-diam.
