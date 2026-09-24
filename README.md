# n8n-lead-routing-system

# PROJECT 02: Automated Lead Routing & Qualification System

**Project Category:** Data Routing, CRM Integration, & Notification System  
**Difficulty:** Intermediate  
**Type:** Concept Project / Portfolio  

## 1. Business Problem
Tim Sales sering kehilangan momentum berharga karena prospek klien (*leads*) bernilai tinggi yang masuk melalui formulir *website* harus menunggu disortir secara manual setiap penghujung hari oleh admin. Sementara itu, *lead* dengan *budget* rendah tetap memakan waktu pengerjaan administratif untuk dimasukkan ke dalam *database*.

## 2. Objective
Membangun sistem *gatekeeper* otomatis yang mampu menangkap data pendaftaran secara *real-time*, mengkualifikasi *lead* berdasarkan parameter *budget*, dan merutekan data tersebut ke saluran yang tepat secara instan.

## 3. Solution
Sistem *event-driven* menggunakan n8n yang mendengarkan *payload* pendaftaran via Webhook. Logika percabangan (IF Node) diterapkan untuk menyeleksi parameter JSON `budget`. Prospek dengan *budget* > Rp 10.000.000 memicu Telegram Action untuk notifikasi tim Sales instan, sedangkan prospek di bawah batas tersebut otomatis ditambahkan sebagai baris baru ke dalam Google Sheets CRM.

## 4. Workflow Architecture
`Webhook Trigger (POST)` ➡️ `IF Node (Logic Branching)` 
  ┣━ `True (Budget > 10M)` ➡️ `Telegram Action (Alert)`
  ┗━ `False (Budget <= 10M)` ➡️ `Google Sheets Action (Append Row)`

## 5. Tools & Technologies
*   **n8n:** Workflow Automation Platform
*   **Hoppscotch/Postman:** API Testing & Webhook Payload Simulation
*   **Google Sheets:** Lightweight CRM / Database
*   **Telegram API:** Instant Alert System
*   **JSON:** Data Transport Format

## 6. n8n Nodes Used
1.  **Webhook Trigger:** Menangkap *HTTP POST request* secara *real-time*.
2.  **IF:** Mengevaluasi kondisi parameter bertipe *Number*.
3.  **Telegram Action:** Mendistribusikan data *True* dengan *template* pesan dinamis.
4.  **Google Sheets:** Menjalankan operasi `Append Row` untuk menyimpan data *False*.

## 7. Workflow Explanation
*   **Input:** Data *lead* (Nama, Perusahaan, Email, Budget) dikirimkan dalam format JSON ke URL *Webhook endpoint*.
*   **Processing:** *Node* IF mengekstrak variabel `{{ $json.budget }}` dan mengujinya terhadap ambang batas statis (10.000.000).
*   **Output:** Notifikasi mendesak via Telegram untuk *Sales*, atau penambahan data ke baris terakhir *spreadsheet*.

## 8. Error Handling & Security
*   **Error Handling:** (Basic) Eksekusi tertahan pada *node* jika JSON *payload* tidak sesuai skema (akan disempurnakan dengan *Data Validation* pada level lanjutan). Isu `matching column` di Google Sheets diselesaikan dengan mengubah operasi menjadi *Append Row* murni.
*   **Security:** *Webhook test URL* diisolasi. Kredensial OAuth2 Google dan Telegram Bot Token diamankan menggunakan fitur *Credentials Manager* n8n.

## 9. Result & Business Value
*   **Before:** Waktu respons *Sales* (*Lead Response Time*) tertunda hingga 8-24 jam akibat *bottleneck* administratif.
*   **After:** Waktu respons *Sales* untuk *High-Value Lead* turun menjadi < 1 menit (Instan).
*   **Estimated Time Saved:** ~5 jam/minggu pekerjaan klerikal entri data *spreadsheet*.

## 10. Lessons Learned & Future Improvements
*   **Lessons Learned:** Pentingnya mereset koneksi *node* (*caching issue*) saat terjadi pembaruan skema kolom di database pihak ketiga (Google Sheets).
*   **Future Improvements:** Menambahkan parameter *UTM Source* pada *payload* untuk melacak asal kampanye iklan, dan mengintegrasikan notifikasi *email* penolakan/sambutan otomatis.

---
*Screenshots: [Sisipkan Gambar Kanvas Keseluruhan, Pengaturan Node IF, dan Bukti Data di Google Sheets/Telegram]*
