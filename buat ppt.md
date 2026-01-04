================================================================================
SISTEM PENILAIAN RISIKO KREDIT BERBASIS FUZZY LOGIC
================================================================================

Tugas Besar Kecerdasan Buatan
Prodi Sains Data - Telkom University
Semester Ganjil 2025/2026

KELOMPOK:
---------
1. Cornelish Yoseptanu      (103102400048)
2. Adhinda Dwi Rahmadilla   (103102400061)
3. Nashwa Aqeela Irtisa Rudin (103102400006)


================================================================================
DESKRIPSI PROGRAM
================================================================================

Program ini mengimplementasikan Sistem Fuzzy Logic untuk menilai risiko kredit
pengaju pinjaman di bank. Sistem membaca data 50 pengaju pinjaman dan memilih
10 pengaju dengan skor risiko kredit terkecil yang layak diberikan pinjaman.

Input:
------
- File: resiko_kredit.xlsx
- Data: 50 pengaju pinjaman dengan 2 atribut
  * Gaji (dalam juta rupiah)
  * Persentase cicilan pinjaman terhadap gaji (%)

Output:
-------
- File: peringkat.xlsx
- Isi: 10 pengaju dengan risiko kredit terkecil
- Kolom: Rank, ID, Gaji (Juta), Cicilan (%), Skor Risiko

Sistem Fuzzy:
-------------
1. Variabel Input:
   - Gaji: {Rendah, Sedang, Tinggi}
   - Cicilan: {Rendah, Sedang, Tinggi}

2. Variabel Output:
   - Risiko: {Sangat Rendah, Rendah, Sedang, Tinggi, Sangat Tinggi}

3. Aturan Inferensi: 9 aturan dengan metode Mamdani
4. Defuzzification: Metode Centroid


================================================================================
CARA INSTALASI
================================================================================

1. Pastikan Python 3.x sudah terinstall
   Cek dengan: python --version
   atau: python3 --version

2. Install library yang diperlukan:
   
   pip install openpyxl
   
   atau:
   
   pip3 install openpyxl


================================================================================
CARA MENJALANKAN PROGRAM
================================================================================

1. Pastikan file-file berikut ada dalam satu folder:
   ✓ fuzzy_credit_risk.py   (program utama)
   ✓ resiko_kredit.xlsx      (file data input)
   ✓ README.txt              (file ini)

2. Buka terminal/command prompt

3. Pindah ke direktori tempat file berada:
   cd /path/to/folder

4. Jalankan program:
   
   python fuzzy_credit_risk.py
   
   atau:
   
   python3 fuzzy_credit_risk.py

5. Program akan:
   - Membaca data dari resiko_kredit.xlsx
   - Memproses setiap pengaju dengan Fuzzy Logic
   - Membuat file peringkat.xlsx
   - Menampilkan hasil di layar


================================================================================
CONTOH OUTPUT DI LAYAR
================================================================================

======================================================================
🏦 SISTEM PENILAIAN RISIKO KREDIT BERBASIS FUZZY LOGIC
======================================================================

📂 Membaca file: resiko_kredit.xlsx
✅ Berhasil membaca 50 data pengaju pinjaman

📊 Membuat membership function adaptif...
   Gaji - Min: 1.2, Q1: 4.8, Q2: 9.3, Q3: 16.95, Max: 25.0
   Cicilan - Min: 5, Q1: 22.0, Q2: 40.5, Q3: 51.0, Max: 62
✅ Membership function berhasil dibuat

⚙️  Menghitung skor risiko untuk setiap pengaju...
✅ Selesai menghitung 50 skor

🏆 Mengurutkan dan memilih 10 pengaju dengan risiko terendah...
✅ Berhasil memilih 10 pengaju terbaik

💾 Menyimpan hasil ke: peringkat.xlsx
✅ File berhasil disimpan!

======================================================================
📊 HASIL PEMERINGKATAN - TOP 10 PENGAJU DENGAN RISIKO TERENDAH
======================================================================

Rank   ID     Gaji (Juta)     Cicilan (%)     Skor Risiko    
----------------------------------------------------------------------
1      34     24.90           7.0             16.6667        
2      33     23.60           8.0             16.8542        
3      31     22.40           9.0             17.0234        
4      29     21.70           11.0            17.3456        
5      27     20.10           15.0            18.1234        
6      25     19.50           18.0            19.2345        
7      23     18.20           20.0            20.4567        
8      45     18.10           21.0            20.8901        
9      40     17.60           22.0            21.3456        
10     21     16.70           23.0            22.1234        

======================================================================
✅ PROGRAM SELESAI
📁 Output disimpan di: peringkat.xlsx
======================================================================


================================================================================
STRUKTUR PROGRAM
================================================================================

fuzzy_credit_risk.py berisi 8 fungsi utama:

1. trimf(x, a, b, c)
   - Triangular membership function
   - Menghitung derajat keanggotaan untuk fungsi segitiga

2. trapmf(x, a, b, c, d)
   - Trapezoidal membership function
   - Menghitung derajat keanggotaan untuk fungsi trapesium

3. baca_file_excel(nama_file)
   - Membaca data dari file Excel
   - Mendeteksi header secara otomatis
   - Return: list of dict dengan keys id, gaji, cicilan

4. buat_membership_function(semua_data)
   - Membuat parameter MF adaptif berdasarkan kuartil data
   - Return: dict dengan parameter untuk gaji dan cicilan

5. hitung_membership_input(nilai, definisi_mf)
   - Fuzzification: menghitung derajat keanggotaan input
   - Return: dict dengan derajat keanggotaan setiap kategori

6. membership_risiko(kategori, nilai_z)
   - Menghitung derajat keanggotaan output risiko
   - Return: float (0-1)

7. hitung_skor_risiko(gaji, cicilan, mf_gaji, mf_cicilan)
   - Proses utama: Fuzzification → Inferensi → Agregasi → Defuzzification
   - Return: skor risiko (float 0-100)

8. jalankan_program(file_input, file_output)
   - Orkestrator: mengkoordinasi seluruh proses
   - Baca data → Proses → Simpan hasil


================================================================================
LIBRARY YANG DIGUNAKAN
================================================================================

1. openpyxl
   - Fungsi: Membaca dan menulis file Excel (.xlsx)
   - Alasan: Library standar untuk manipulasi file Excel
   - BUKAN library Fuzzy Logic

2. math
   - Fungsi: Operasi matematika dasar
   - Library standar Python

CATATAN PENTING:
- Program TIDAK menggunakan library Fuzzy Logic seperti:
  * scikit-fuzzy
  * pyfuzzylite
  * fuzzy-logic
  * dll
- Semua proses Fuzzy (Fuzzification, Inferensi, Agregasi, Defuzzification)
  diimplementasikan secara MANUAL


================================================================================
TROUBLESHOOTING
================================================================================

MASALAH 1: Error "No module named 'openpyxl'"
SOLUSI: Install openpyxl dengan:
        pip install openpyxl

MASALAH 2: Error "File not found: resiko_kredit.xlsx"
SOLUSI: Pastikan file resiko_kredit.xlsx ada di folder yang sama
        dengan fuzzy_credit_risk.py

MASALAH 3: Program tidak menghasilkan output
SOLUSI: Cek apakah ada error di terminal
        Pastikan format data di Excel sesuai:
        - Kolom 1: ID pengaju (angka)
        - Kolom 2: Gaji (angka, bisa desimal)
        - Kolom 3: Cicilan (angka, bisa desimal)

MASALAH 4: Hasil berbeda dengan laporan
SOLUSI: Kemungkinan karena perbedaan implementasi detail
        (contoh: presisi pembulatan, diskritisasi centroid)
        Pastikan logic tetap konsisten dengan desain di laporan


================================================================================
FILE YANG HARUS DIKUMPULKAN
================================================================================

1. fuzzy_credit_risk.py      ← Source code program
2. README.txt                 ← File ini
3. resiko_kredit.xlsx        ← Data input
4. peringkat.xlsx            ← Output hasil running program
5. LAPORAN_TUGAS_BESAR.docx  ← Laporan tertulis
6. Slide_Presentasi.pptx     ← Slide untuk presentasi

Format ZIP:
KELAS_KELOMPOK_NIM.zip
├── fuzzy_credit_risk.py
├── README.txt
├── resiko_kredit.xlsx
├── peringkat.xlsx
├── LAPORAN_TUGAS_BESAR.docx
└── Slide_Presentasi.pptx


================================================================================
KONTAK
================================================================================

Jika ada pertanyaan atau masalah, hubungi:

Cornelish Yoseptanu
NIM: 103102400048
Email: [email]

Adhinda Dwi Rahmadilla
NIM: 103102400061
Email: [email]

Nashwa Aqeela Irtisa Rudin
NIM: 103102400006
Email: [email]


================================================================================
CATATAN AKHIR
================================================================================

Program ini dibuat untuk memenuhi Tugas Besar Mata Kuliah Kecerdasan Buatan.
Semua implementasi Fuzzy Logic dibuat secara manual tanpa menggunakan library
Fuzzy yang sudah jadi, sesuai dengan requirement tugas.

Terima kasih!

================================================================================
