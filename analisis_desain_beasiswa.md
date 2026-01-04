# ANALISIS DAN DESAIN SISTEM FUZZY - PEMBERIAN BEASISWA
## Menggunakan Inferensi Mamdani

---

## 1. DATA MAHASISWA

| NIM | Nama | TOEFL | IPK | Penghasilan Ortu |
|-----|------|-------|-----|------------------|
| 01 | Toyes | 450 | 4 | 750.000 |
| 02 | Bowo | 480 | 3 | 1.500.000 |
| 03 | Erna | 360 | 3 | 1.255.000 |
| 04 | Astuti | 270 | 2 | 1.040.000 |
| 05 | Yuni | 420 | 4 | 950.000 |
| 06 | Heribertus | 390 | 4 | 1.600.000 |
| 07 | Edy | 370 | 3 | 1.250.000 |
| 08 | Usman | 255 | 3 | 550.000 |
| 09 | Pujiono | 325 | 2 | 735.000 |
| 10 | Slamet | 250 | 1 | 860.000 |

**Range Data:**
- TOEFL: 250 - 480
- IPK: 1 - 4
- Penghasilan: 550.000 - 1.600.000

---

## 2. ATURAN INFERENSI (5 Rules)

**[R1]** IF IPK BAGUS AND (Penghasilan KECIL OR SEDANG) 
       THEN Nilai Kelayakan TINGGI

**[R2]** IF IPK BAGUS AND (Penghasilan BESAR OR SANGAT BESAR) 
       THEN Nilai Kelayakan RENDAH

**[R3]** IF IPK CUKUP AND (Penghasilan KECIL OR SEDANG) 
       THEN Nilai Kelayakan RENDAH

**[R4]** IF TOEFL TINGGI AND (Penghasilan KECIL OR SEDANG) 
       THEN Nilai Kelayakan TINGGI

**[R5]** IF TOEFL MENENGAH AND (Penghasilan KECIL OR SEDANG) 
       THEN Nilai Kelayakan TINGGI

**Logika:**
- IPK Bagus + Penghasilan Rendah = Layak (prioritas tinggi)
- IPK Bagus + Penghasilan Tinggi = Tidak prioritas (sudah mampu)
- IPK Cukup + Penghasilan Rendah = Layak tapi tidak prioritas
- TOEFL Tinggi + Penghasilan Rendah = Layak
- TOEFL Menengah + Penghasilan Rendah = Layak

---

## 3. DESAIN FUNGSI KEANGGOTAAN INPUT

### 3.1. IPK (Range: 1.0 - 4.0)

**Kategori: CUKUP, BAGUS**

**IPK CUKUP** (Trapezoidal):
- Parameter: [1.0, 1.0, 2.5, 3.0]
- Interpretasi: IPK 1.0-2.5 = fully CUKUP, mulai turun di 2.5-3.0

**IPK BAGUS** (Trapezoidal):
- Parameter: [2.5, 3.0, 4.0, 4.0]
- Interpretasi: IPK 2.5-3.0 mulai naik, 3.0-4.0 = fully BAGUS

Formula:
```
μ_IPK_CUKUP(x) = trapmf(x, 1.0, 1.0, 2.5, 3.0)
μ_IPK_BAGUS(x) = trapmf(x, 2.5, 3.0, 4.0, 4.0)
```

---

### 3.2. TOEFL (Range: 250 - 480)

**Kategori: RENDAH, MENENGAH, TINGGI**

**TOEFL RENDAH** (Trapezoidal):
- Parameter: [250, 250, 300, 350]
- Interpretasi: 250-300 = fully RENDAH, turun di 300-350

**TOEFL MENENGAH** (Triangular):
- Parameter: [300, 375, 450]
- Interpretasi: Puncak di 375

**TOEFL TINGGI** (Trapezoidal):
- Parameter: [400, 450, 480, 480]
- Interpretasi: 450-480 = fully TINGGI, naik di 400-450

Formula:
```
μ_TOEFL_RENDAH(x) = trapmf(x, 250, 250, 300, 350)
μ_TOEFL_MENENGAH(x) = trimf(x, 300, 375, 450)
μ_TOEFL_TINGGI(x) = trapmf(x, 400, 450, 480, 480)
```

---

### 3.3. PENGHASILAN (Range: 550.000 - 1.600.000)

**Kategori: KECIL, SEDANG, BESAR, SANGAT BESAR**

**Penghasilan KECIL** (Trapezoidal):
- Parameter: [550000, 550000, 750000, 900000]
- Interpretasi: ≤750rb = fully KECIL, turun di 750rb-900rb

**Penghasilan SEDANG** (Triangular):
- Parameter: [750000, 1000000, 1250000]
- Interpretasi: Puncak di 1jt

**Penghasilan BESAR** (Triangular):
- Parameter: [1100000, 1300000, 1500000]
- Interpretasi: Puncak di 1.3jt

**Penghasilan SANGAT BESAR** (Trapezoidal):
- Parameter: [1400000, 1500000, 1600000, 1600000]
- Interpretasi: ≥1.5jt = fully SANGAT BESAR

Formula:
```
μ_PENG_KECIL(x) = trapmf(x, 550000, 550000, 750000, 900000)
μ_PENG_SEDANG(x) = trimf(x, 750000, 1000000, 1250000)
μ_PENG_BESAR(x) = trimf(x, 1100000, 1300000, 1500000)
μ_PENG_SANGAT_BESAR(x) = trapmf(x, 1400000, 1500000, 1600000, 1600000)
```

---

## 4. DESAIN FUNGSI KEANGGOTAAN OUTPUT

### 4.1. NILAI KELAYAKAN (Range: 0 - 100)

**Kategori: RENDAH, TINGGI**

**Kelayakan RENDAH** (Trapezoidal):
- Parameter: [0, 0, 30, 50]
- Interpretasi: Skor 0-30 = fully RENDAH (tidak layak)

**Kelayakan TINGGI** (Trapezoidal):
- Parameter: [50, 70, 100, 100]
- Interpretasi: Skor 70-100 = fully TINGGI (sangat layak)

Formula:
```
μ_KELAYAKAN_RENDAH(z) = trapmf(z, 0, 0, 30, 50)
μ_KELAYAKAN_TINGGI(z) = trapmf(z, 50, 70, 100, 100)
```

**Interpretasi Skor:**
- 0-30: Tidak Layak
- 30-50: Kurang Layak
- 50-70: Cukup Layak
- 70-100: Sangat Layak

---

## 5. IMPLEMENTASI ATURAN INFERENSI MAMDANI

### Proses:
1. **Fuzzification**: Hitung derajat keanggotaan input
2. **Rule Evaluation**: Evaluasi 5 aturan dengan operator MIN/AND
3. **Aggregation**: Gabungkan output dengan operator MAX
4. **Defuzzification**: Gunakan metode Centroid

### Rule Detail:

**R1: IPK BAGUS AND (Penghasilan KECIL OR SEDANG) → Kelayakan TINGGI**
```
α1 = μ_IPK_BAGUS(ipk) AND MAX(μ_PENG_KECIL(p), μ_PENG_SEDANG(p))
α1 = MIN(μ_IPK_BAGUS, MAX(μ_PENG_KECIL, μ_PENG_SEDANG))
Output: TINGGI dengan strength α1
```

**R2: IPK BAGUS AND (Penghasilan BESAR OR SANGAT BESAR) → Kelayakan RENDAH**
```
α2 = MIN(μ_IPK_BAGUS, MAX(μ_PENG_BESAR, μ_PENG_SANGAT_BESAR))
Output: RENDAH dengan strength α2
```

**R3: IPK CUKUP AND (Penghasilan KECIL OR SEDANG) → Kelayakan RENDAH**
```
α3 = MIN(μ_IPK_CUKUP, MAX(μ_PENG_KECIL, μ_PENG_SEDANG))
Output: RENDAH dengan strength α3
```

**R4: TOEFL TINGGI AND (Penghasilan KECIL OR SEDANG) → Kelayakan TINGGI**
```
α4 = MIN(μ_TOEFL_TINGGI, MAX(μ_PENG_KECIL, μ_PENG_SEDANG))
Output: TINGGI dengan strength α4
```

**R5: TOEFL MENENGAH AND (Penghasilan KECIL OR SEDANG) → Kelayakan TINGGI**
```
α5 = MIN(μ_TOEFL_MENENGAH, MAX(μ_PENG_KECIL, μ_PENG_SEDANG))
Output: TINGGI dengan strength α5
```

### Agregasi:
```
μ_RENDAH_aggregated = MAX(α2, α3)
μ_TINGGI_aggregated = MAX(α1, α4, α5)
```

### Defuzzification (Centroid):
```
Skor = Σ(z × μ(z)) / Σμ(z)
dimana z ∈ [0, 100]
```

---

## 6. PREDIKSI HASIL (Estimasi Manual)

Berdasarkan aturan, prediksi ranking (belum dihitung):

**Kandidat Kuat (IPK Tinggi + Penghasilan Rendah):**
1. **Toyes** (TOEFL=450, IPK=4, Gaji=750k) 
   → R1: IPK BAGUS + KECIL → TINGGI
   → R4: TOEFL TINGGI + KECIL → TINGGI
   → **Skor Tertinggi!**

2. **Yuni** (TOEFL=420, IPK=4, Gaji=950k)
   → R1: IPK BAGUS + SEDANG → TINGGI
   → R4: TOEFL TINGGI + SEDANG → TINGGI
   → **Skor Sangat Tinggi**

3. **Usman** (TOEFL=255, IPK=3, Gaji=550k)
   → R1: IPK BAGUS (parsial) + KECIL → TINGGI
   → Skor Tinggi (tapi TOEFL rendah)

**Kandidat Menengah:**
4. **Pujiono** (TOEFL=325, IPK=2, Gaji=735k)
   → R3: IPK CUKUP + KECIL → RENDAH
   → R5: TOEFL MENENGAH + KECIL → TINGGI
   → Konfliks rules, skor sedang

5. **Erna** (TOEFL=360, IPK=3, Gaji=1.255k)
   → R1: IPK BAGUS (parsial) + SEDANG → TINGGI
   → Skor Menengah

**Kandidat Lemah (Penghasilan Tinggi atau IPK Rendah):**
6. **Heribertus** (TOEFL=390, IPK=4, Gaji=1.6jt)
   → R2: IPK BAGUS + SANGAT BESAR → RENDAH
   → **Skor Rendah** (meski IPK tinggi, tapi mampu)

7. **Bowo** (TOEFL=480, IPK=3, Gaji=1.5jt)
   → R2: IPK BAGUS (parsial) + BESAR → RENDAH
   → Skor Rendah

8. **Slamet** (TOEFL=250, IPK=1, Gaji=860k)
   → IPK sangat rendah, TOEFL rendah
   → **Skor Terendah**

---

## 7. KESIMPULAN DESAIN

**Kelebihan Desain:**
1. Mengakomodasi 3 kriteria: IPK, TOEFL, Penghasilan
2. Prioritas untuk mahasiswa berprestasi tapi kurang mampu
3. Fungsi keanggotaan overlap mencerminkan kondisi real
4. 5 aturan cukup untuk cover semua kondisi penting

**Asumsi:**
1. IPK ≥ 3.0 = BAGUS
2. TOEFL ≥ 400 = TINGGI
3. Penghasilan ≤ 900rb = KECIL
4. Semakin tinggi skor kelayakan, semakin prioritas dapat beasiswa

---

## 8. IMPLEMENTASI

File yang akan dibuat:
1. **fuzzy_beasiswa.py** - Source code lengkap
2. **hasil_kelayakan.xlsx** - Output hasil perhitungan
3. **README.txt** - Dokumentasi

Program akan:
- Baca data 10 mahasiswa
- Hitung nilai kelayakan untuk setiap mahasiswa
- Ranking berdasarkan skor kelayakan (descending)
- Export ke Excel
- Tampilkan hasil di console

