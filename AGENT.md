# Aturan & Panduan Agen AI: Integrasi n8n (Portal Warga)

File ini mendefinisikan aturan dan standar kerja bagi AI Assistant dalam merancang, membangun, menguji, dan mengelola workflow otomasi **n8n** untuk proyek **Portal Warga** (`portalwarga-dev`).

---

## 1. Aturan Pengorganisasian Workflow (Wajib)

1. **Folder "Portal Warga"**:
   - **Setiap workflow n8n yang berkaitan dengan proyek ini WAJIB ditempatkan di dalam folder bernama `"Portal Warga"`** pada instance n8n (`https://n8n-proxmox.dyudhiantoro.my.id`).
   - Saat membuat workflow baru menggunakan MCP (`n8n_create_workflow`), selalu set `parentFolderId` ke ID folder **Portal Warga**.
   - Jika folder **Portal Warga** belum ada, buat atau pastikan folder tersebut tersedia terlebih dahulu menggunakan tool folder management (`n8n_manage_folders`) atau beri tag `Portal Warga`.

2. **Standar Penamaan Workflow**:
   - Gunakan prefix terstruktur agar mudah diidentifikasi:
     - `[PW] <Modul/Fitur> - <Fungsi Workflow>`
   - Contoh:
     - `[PW] Auth - WhatsApp OTP Verification`
     - `[PW] Iuran - Notifikasi Pembayaran Bulanan`
     - `[PW] Pengaduan - Sync Tiket Warga ke Admin`

---

## 2. Standar Desain & Implementasi Workflow n8n

1. **Template-First Approach**:
   - Sebelum membuat alur kerja dari nol, cari template yang relevan menggunakan `search_templates` atau periksa node yang tersedia dengan `search_nodes`.

2. **Konfigurasi Node Eksplisit (Never Trust Defaults)**:
   - Semua parameter wajib pada setiap node harus didefinisikan secara eksplisit. Jangan mengandalkan nilai default n8n karena dapat menyebabkan error saat runtime.
   - Gunakan ekspresi n8n standar: `$json`, `$('Nama Node').item.json`, dsb.

3. **Multi-Level Validation**:
   - Lakukan validasi konfigurasi node dengan `validate_node` (`mode: 'minimal'` atau `mode: 'full'`).
   - Lakukan validasi struktur alur kerja dengan `validate_workflow` sebelum mendeploy ke server n8n.

4. **Integrasi dengan Database Supabase**:
   - Integrasi database wajib terhubung ke project Supabase **Staging Portal Palm Village** (`vfhaiusgtyufvnzuoduq`).
   - Gunakan REST API atau Node Supabase / Postgres dengan kredensial yang telah dikonfigurasi.

5. **Penanganan Error (Error Handling)**:
   - Setiap workflow produksi harus memiliki node penanganan error (Error Trigger / Stop and Error / Fallback route).
   - Pastikan kegagalan eksekusi tercatat di execution log atau terkirim notifikasi peringatan.

6. **Deploy & Pembaruan Aman**:
   - Gunakan `n8n_update_partial_workflow` untuk update parsial guna menghemat token dan menjaga integritas workflow.
   - Uji alur kerja setelah deployment dengan `n8n_test_workflow` atau `n8n_validate_workflow`.
