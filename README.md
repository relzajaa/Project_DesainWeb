# Modul 10 – Git

> Kumpulan karya praktikum beserta ringkasan materi dan panduan penggunaan Git (Modul 10).

| | |
|---|---|
| **Nama** | Farrel Muflif Muzzaki |
| **NIM** | K3525024 |
| **Kelas** | C |
| **Mata Kuliah** | Desain Web |

---

## Daftar Isi

1. [Tujuan Praktikum](#tujuan-praktikum)
2. [Dasar Teori](#dasar-teori)
3. [Perintah Dasar Git](#perintah-dasar-git)
4. [GitFlow](#gitflow)
5. [SSH](#ssh)
6. [Panduan Praktikum](#panduan-praktikum)
7. [Membuat RSA Key](#membuat-rsa-key-untuk-ssh)
8. [Tugas Praktikum](#tugas-praktikum)
9. [Referensi](#referensi)

---

## Tujuan Praktikum

- Mengenal teknik versioning dengan Git.
- Mampu berkolaborasi dalam tim dengan Git.
- Mampu mengembangkan aplikasi perangkat lunak dengan Git.

**Alokasi waktu:** 1 x 50 menit

---

## Dasar Teori

**Git** adalah *version control system* untuk mengembangkan software bersama-sama. Git mencatat baris kode yang ditambah, diganti, atau dihapus, sehingga:

- Perubahan mudah dilacak tanpa membuat file baru (`index2.html`, `index3.html`, dst.).
- Kode tidak mudah bentrok, karena tiap developer bisa bekerja di *branch* sendiri.
- Setiap perubahan diberi pesan commit, jadi jelas apa, kapan, dan siapa yang mengubah.
- Revisi mudah dilakukan jika terjadi kesalahan.

**Platform hosting Git:** GitHub, Bitbucket, GitLab.

### Tiga "Trees" pada Repositori Lokal

| Bagian | Fungsi |
|---|---|
| **Working Directory** | Berkas aktual yang sedang dikerjakan |
| **Staging Area (Index)** | Menyimpan apa yang akan masuk ke commit berikutnya |
| **HEAD** | Menunjuk ke commit terakhir (snapshot file) |

```
Working Directory --add--> Staging Area --commit--> Local Repo (HEAD) --push--> Remote Repo
Remote Repo --fetch--> Local Repo --merge--> Working Directory   (pull = fetch + merge)
```

### Alur Kolaborasi

1. `git clone` – ambil salinan repository dari remote.
2. Ubah file, lalu `git add` dan `git commit`.
3. `git pull` / `git fetch` – ambil perubahan terbaru dari developer lain.
4. `git push` – kirim perubahan ke remote.

---

## Perintah Dasar Git

| Perintah | Fungsi |
|---|---|
| `git init` | Membuat repository di folder lokal (muncul folder `.git`) |
| `git status` | Melihat status repository |
| `git add` | Menambahkan file ke staging area |
| `git commit` | Menyimpan perubahan ke repository lokal |
| `git push` | Mengirim commit ke remote |
| `git branch` | Melihat daftar branch |
| `git checkout` | Berpindah branch |
| `git merge` | Menggabungkan branch ke branch aktif |
| `git clone` | Menyalin repository |

---

## GitFlow

Strategi branching yang diciptakan Vincent Driessen, dianggap praktik terbaik untuk pengembangan berkelanjutan dan DevOps.

| Branch | Peran | Dibuat dari | Di-merge ke |
|---|---|---|---|
| `main` / `master` | Kode produksi yang stabil (diberi tag rilis) | – | – |
| `develop` | Cabang utama pengembangan | `main` | – |
| `feature` | Pengembangan fitur baru | `develop` | `develop` |
| `release` | Persiapan rilis (hanya perbaikan bug) | `develop` | `develop` dan `main` |
| `hotfix` | Perbaikan bug serius di produksi | `main` | `main` dan `develop` |

---

## SSH

SSH (Secure Shell) adalah pengganti telnet, rsh, dan rlogin yang jauh lebih aman, digunakan untuk mengakses mesin secara remote. SSH Key terdiri dari sepasang kunci:

- **Public Key** – diletakkan di sistem tujuan (mis. GitHub/server).
- **Private Key** – disimpan di komputer Anda. **Jangan pernah dibagikan.**

Dengan SSH, `git push` ke GitHub tidak perlu memasukkan kredensial berulang seperti pada HTTPS.

---

## Panduan Praktikum

### 1. Membuat / Menyalin Repositori

```bash
git init                                   # buat repositori baru
git clone /jalur/ke/repositori             # salin repositori lokal
git clone namapengguna@host:/jalur/repo    # salin dari server jarak jauh
```

### 2. Tambah, Komit, dan Kirim

```bash
git add <namaberkas>        # atau: git add *
git commit -m "Pesan komit"
git remote add origin <alamat git>   # jika belum terhubung ke remote
git push origin main                 # ganti main dengan master/cabang lain
```

### 3. Percabangan

```bash
git checkout -b fitur_x      # buat cabang baru dan pindah ke sana
git checkout main            # kembali ke cabang utama
git branch -d fitur_x        # hapus cabang
git push origin <cabang>     # kirim cabang ke remote
```

### 4. Pembaruan dan Penggabungan

```bash
git pull                          # ambil dan gabungkan perubahan dari remote
git fetch && git merge <cabang>   # gabungkan cabang lain ke cabang aktif
git diff <cabang_asal> <cabang_tujuan>   # pratinjau sebelum merge
git reflog                        # riwayat posisi HEAD (untuk membatalkan merge)
```

Jika terjadi **konflik**: buka berkas yang ditunjuk Git, perbaiki manual, lalu tandai dengan `git add <namaberkas>`.

### 5. Tagging

Dianjurkan untuk perangkat lunak yang dirilis:

```bash
git tag 1.0.0 1b2e1d63ff    # 10 karakter pertama ID commit
```

### 6. Log

```bash
git log                                      # riwayat repositori
git log --author=bob                         # commit dari penulis tertentu
git log --pretty=oneline                     # satu baris per commit
git log --graph --oneline --decorate --all   # pohon ASCII semua cabang
git log --name-status                        # berkas yang berubah
```

### 7. Mengembalikan Perubahan Lokal

```bash
git checkout -- <namaberkas>       # kembalikan berkas ke versi HEAD
git fetch origin
git reset --hard origin/master     # samakan dengan server
```

> ⚠️ `git reset --hard` menghapus perubahan lokal secara permanen. Gunakan dengan hati-hati.

### 8. Tips Lainnya

```bash
gitk                              # GUI bawaan Git
git config color.ui true          # output berwarna
git config format.pretty oneline  # log satu baris
git add -i                        # penambahan interaktif
```

---

## Membuat RSA Key untuk SSH

1. Buat sepasang kunci:

   ```bash
   ssh-keygen -t rsa
   ```

2. Tekan **Enter** untuk lokasi default, lalu isi *passphrase* (atau Enter jika tidak perlu).
   - Private Key: `~/.ssh/id_rsa`
   - Public Key: `~/.ssh/id_rsa.pub`
3. Salin Public Key ke server:

   ```bash
   ssh-copy-id user@ip_server_anda
   ```

   Alternatif: tempel isi `id_rsa.pub` ke `~/.ssh/authorized_keys` di server.

**Untuk GitHub:** salin isi `id_rsa.pub` ke *Settings → SSH and GPG keys → New SSH key*, lalu gunakan alamat repo SSH (`git@github.com:username/repo.git`).

---

## Tugas Praktikum

- [x] Buat akun di web Git (GitHub / GitLab / Bitbucket).
- [x] Buat repositori baru berisi kumpulan karya selama praktikum.
- [x] Tuliskan URL repository pada assignment Google Classroom.

---

## Referensi

- The Net Ninja – Git Tutorial: <https://www.youtube.com/watch?v=3RjQznt-8kE&list=PL4cUxeGkcC9goXbgTDQ0n_4TBzOO0ocPR>
- IDStack Tutorial: <https://www.youtube.com/watch?v=i7fnAxHAp0M&list=PL1aMeb5UP_PHXTV_Xpt-19x_rVPXrymOM>
- GitFlow in Practice: <https://medium.com/easyread/gitflow-in-practice-1d8a2bbdc3a1>

---

<p align="center">© 2026 Farrel Muflif Muzzaki (K3525024). All rights reserved.</p>
