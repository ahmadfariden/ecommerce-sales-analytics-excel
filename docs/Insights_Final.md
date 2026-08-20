# Insights — Versi Final (Setelah Validasi & Perbaikan)

Struktur tiap insight: **Finding → Evidence → Business Impact → Recommendation**

---

## Executive Summary

**Kondisi Bisnis**
Bisnis mencatat Total Revenue $5.865.293 dari 34.500 order dan 7.903 unique
customer sepanjang periode data (akhir 2023 – September 2025), dengan Average
Order Value $170. Struktur revenue sangat timpang secara kategori (Electronics
57%) namun tersebar cukup merata secara metode pembayaran dan region.

**Pertumbuhan Penjualan**
Revenue menunjukkan pola yang relatif stabil sepanjang 2024–2025, berada di
kisaran $200–280 ribu per bulan tanpa tren pertumbuhan yang konsisten. Pola
ini menunjukkan kondisi penjualan yang relatif stabil dibandingkan fase
growth yang agresif.

**Profitabilitas**
Profit Margin keseluruhan 17% (Total Profit $970.019). Analisis Discount Band
menunjukkan margin menurun konsisten seiring meningkatnya discount band
(16,8% pada 0% diskon → 14,7% pada diskon 21-30%). Dalam dataset ini, band
diskon yang lebih tinggi tidak menunjukkan volume order yang lebih tinggi
untuk mengimbangi margin yang lebih rendah, meskipun hubungan kausal belum
dapat dibuktikan dari data observational.

---

## Key Findings

### 1. Repeat Purchase Rate 94% — Tervalidasi

**Finding**: Repeat Purchase Rate 94% (7.428 dari 7.903 unique customer pernah
bertransaksi lebih dari sekali).

**Evidence**: Validasi silang antara jumlah unique customer (Order_Count > 1)
dan pivot Customer by Type menunjukkan hasil konsisten: 475 New Customer dan
7.428 Returning Customer secara unique customer basis (berbeda dari basis
transaksi yang menghasilkan 34.025 baris transaksi dari returning customer).

**Business Impact**: Repeat Purchase Rate sebesar 94% menunjukkan bahwa
sebagian besar customer melakukan pembelian lebih dari satu kali selama
periode observasi. Ini mengindikasikan tingkat repeat purchasing yang tinggi,
meskipun belum dapat digunakan sebagai ukuran retention rate secara langsung
(retention rate biasanya mengukur apakah customer kembali dalam periode
waktu tertentu, bukan sekadar pernah order >1 kali sepanjang seluruh periode
data).

**Catatan Metodologi**: Metrik ini dihitung berdasarkan *unique customer*,
bukan jumlah transaksi — penting untuk disebutkan eksplisit agar tidak
disalahartikan sebagai rasio transaksi.

---

### 2. Electronics 57% — Revenue Concentration Risk

**Finding**: Electronics menyumbang 57% dari total revenue — 39 poin
persentase lebih tinggi dari kategori kedua (Home, 18%).

**Business Impact**: Konsentrasi setinggi ini menciptakan **Revenue
Concentration Risk**. Penurunan permintaan, peningkatan kompetisi, atau
perubahan kondisi pasar pada kategori Electronics berpotensi berdampak
signifikan terhadap revenue keseluruhan bisnis, karena tidak ada kategori lain
yang cukup besar untuk menyerap dampaknya.

**Recommendation**: Kembangkan kategori Home dan Sports melalui assortment
yang ditargetkan, cross-selling, dan segmentasi customer, untuk secara
bertahap mengurangi ketergantungan pada Electronics — bukan mengurangi fokus
pada Electronics, melainkan membangun sumber revenue tambahan di sampingnya.

---

### 3. Customer Value — Revenue Share ≠ Customer Value

**Finding**: Senior berkontribusi 28% dari total revenue (tertinggi di antara
semua segmen usia), namun memiliki AOV **terendah** (164,33) di antara semua
segmen.

**Evidence**:
| Segmen | Jumlah Transaksi | Revenue | AOV |
|---|---|---|---|
| Senior | 9.877 | 1.623.109,71 | 164,33 |
| Adult | 6.755 | 1.166.858,04 | 172,74 |
| Mature | 6.654 | 1.133.888,66 | 170,41 |
| Mid-Age | 6.659 | 1.125.028,50 | 168,95 |
| Young Adult | 4.555 | 816.408,14 | **179,23** |

**Business Impact**: Kontribusi revenue Senior didorong oleh **volume
transaksi**, bukan nilai transaksi individual. Sebaliknya, Young Adult
memiliki AOV tertinggi meski basis transaksinya paling kecil — menunjukkan
nilai transaksi rata-rata yang lebih tinggi. Namun, AOV saja belum cukup
untuk menyimpulkan customer lifetime value yang lebih tinggi — diperlukan
data tambahan seperti purchase frequency, retention, dan margin per customer.

**Recommendation**: Rancang strategi terpisah — program loyalty untuk
mempertahankan dan meningkatkan frekuensi transaksi Senior (memanfaatkan
volume transaksi yang sudah tinggi), dan strategi akuisisi/ekspansi basis
transaksi untuk Young Adult (memanfaatkan AOV yang sudah tinggi per
transaksinya).

---

### 4. Discount >20% — Tidak Menunjukkan Efektivitas

**Finding**: Diskon besar (21-30%) menghasilkan volume order jauh lebih
rendah **dan** AOV lebih rendah **dan** margin lebih rendah dibanding band
diskon lainnya.

**Evidence**:
| Discount Band | Orders | AOV | Margin % |
|---|---|---|---|
| 0% | 18.939 | 178,94 | 16,8% |
| 1-10% | 10.335 | 163,93 | 16,4% |
| 11-20% | 4.522 | 152,33 | 15,8% |
| 21-30% | 704 | 132,53 | 14,7% |

**Business Impact**: Pola ini menunjukkan bahwa diskon besar tidak
menghasilkan volume order yang lebih tinggi untuk mengompensasi margin yang
hilang. Sebaliknya, band diskon tertinggi memiliki AOV dan margin paling
rendah sekaligus volume paling kecil (704 order, hanya 2% dari total). Perlu
dicatat dataset ini bersifat observational — belum membuktikan hubungan
kausal langsung antara besaran diskon dan penurunan volume.

**Recommendation**: Evaluasi ulang kebijakan diskon >20% dan uji alternatif
promosi seperti bundling atau loyalty point, kemudian bandingkan incremental
revenue serta profitability dari alternatif tersebut dengan diskon langsung.

---

### 5. Return-Associated Revenue (Redefinisi dari "Lost Revenue")

**Finding**: Total revenue dari order berstatus Returned = Yes adalah
$388.756, setara **6,6%** dari total revenue keseluruhan. Lebih penting lagi:
AOV order yang diretur ($204,29) secara nyata lebih tinggi dibanding AOV
order yang tidak diretur ($168,01) maupun Overall AOV ($170,01) — selisih
sekitar 20% lebih tinggi.

**Evidence**:
| Status | Orders | Revenue | AOV |
|---|---|---|---|
| Returned (Yes) | 1.903 | 388.755,97 | **204,29** |
| Tidak Returned (No) | 32.597 | 5.476.537,08 | 168,01 |
| Overall | 34.500 | 5.865.293,05 | 170,01 |

**Catatan Definisi Penting**: Istilah "Lost Revenue" sebelumnya berpotensi
menyesatkan. Angka $388.756 merepresentasikan **total revenue dari order yang
diretur**, bukan kerugian bersih (net loss) aktual — dataset tidak menyediakan
detail nilai refund atau partial return, sehingga besaran kerugian riil bisa
lebih kecil dari angka ini.

**Business Impact**: Berbeda dari asumsi awal bahwa returned orders "sebanding"
dengan order pada umumnya, data menunjukkan order dengan nilai transaksi lebih
besar cenderung lebih sering diretur (Returned AOV 20% lebih tinggi dari
Non-Returned AOV). Ini berarti **revenue exposure dari retur lebih serius**
daripada yang terlihat dari Return Rate 6% saja — order yang diretur bukan
sampel acak dari seluruh order, melainkan cenderung condong ke order
bernilai lebih tinggi. Return Rate (6%, berbasis jumlah order) dan
Return-Associated Revenue % (6,6%, berbasis nilai revenue) memang terlihat
dekat, namun kedekatan ini tidak mencerminkan bahwa dampak finansialnya
"ringan" — justru sebaliknya, tiap order yang diretur rata-rata bernilai
lebih besar dari order pada umumnya.

**Limitasi Data**: Root cause analysis (mengapa customer melakukan retur,
dan mengapa order bernilai tinggi lebih rentan diretur) tidak dapat
dilakukan karena dataset tidak menyediakan kolom alasan retur
(return reason).

**Recommendation**: Mengingat order bernilai tinggi cenderung lebih rentan
diretur, investigasi lanjutan sebaiknya diprioritaskan — baik di level
kategori (Fashion & Electronics dengan return rate tertinggi) maupun di
level rentang nilai order — untuk mengidentifikasi pola. Penambahan
pencatatan alasan retur (return reason) ke sistem operasional menjadi
kebutuhan yang lebih mendesak, mengingat besarnya revenue exposure yang
terkait dengan order bernilai tinggi.

---

### 6. Shipping Cost — Normalisasi Mengubah Kesimpulan

**Finding**: Setelah dinormalisasi terhadap revenue, seluruh region memiliki
Shipping Cost % yang serupa (3,54% - 3,66%) — **tidak ada region dengan beban
logistik proporsional lebih tinggi** dari yang lain.

**Evidence**:
| Region | Shipping Cost | Revenue | Shipping Cost % |
|---|---|---|---|
| South | 46.734,38 | 1.298.096,07 | 3,60% |
| North | 46.260,22 | 1.264.008,35 | 3,66% |
| East | 42.802,62 | 1.176.334,75 | 3,64% |
| West | 41.982,52 | 1.186.350,50 | 3,54% |
| Central | 34.468,39 | 940.503,38 | 3,66% |

**Business Impact**: Kesimpulan awal ("South dan North memiliki shipping
cost tertinggi") ternyata menyesatkan jika dilihat tanpa normalisasi. Setelah
dibandingkan terhadap revenue, seluruh region memiliki shipping cost ratio
yang relatif serupa (3,54%–3,66%, selisih hanya 0,12 poin persentase). Total
nominal yang lebih besar di South dan North konsisten dengan skala revenue
dan aktivitas bisnis yang lebih tinggi, sehingga belum menunjukkan adanya
inefisiensi biaya secara proporsional. Tidak terdapat indikasi kuat yang
memerlukan intervensi khusus pada shipping cost per-region berdasarkan data
yang tersedia.

**Recommendation**: Tidak diperlukan tindakan spesifik per-region untuk
shipping cost — fokuskan evaluasi operasional pada area lain (misal return
rate) yang menunjukkan variasi lebih signifikan antar region.

---

### 7. Payment Method — AOV Seragam, Wallet Berpeluang Berkembang

**Finding**: AOV antar seluruh metode pembayaran relatif seragam
(165,89 - 172,01), meski Credit Card mendominasi dari sisi revenue dan volume.

**Evidence**:
| Payment Method | Revenue Share | Orders | AOV |
|---|---|---|---|
| Credit Card | 35,1% | 12.170 | 169,00 |
| Debit Card | 24,9% | 8.505 | 171,69 |
| COD | 12,2% | 4.160 | 172,01 |
| UPI | 12,2% | 4.156 | 171,71 |
| PayPal | 9,8% | 3.444 | 167,40 |
| Wallet | 5,8% | 2.065 | 165,89 |

**Business Impact**: Wallet berkontribusi paling kecil terhadap revenue
(5,8%). AOV Wallet (165,89) berada sedikit di bawah metode pembayaran
lainnya, tetapi masih berada dalam rentang yang relatif dekat dengan
keseluruhan metode pembayaran (167–172). Hal ini menunjukkan bahwa
rendahnya revenue share Wallet lebih banyak berkaitan dengan rendahnya
transaction volume (2.065 order) daripada perbedaan AOV yang besar.

**Recommendation**: AOV Wallet yang relatif sebanding dengan metode
pembayaran lain mengindikasikan bahwa rendahnya revenue share Wallet lebih
berkaitan dengan rendahnya transaction volume, sehingga peningkatan adopsi
Wallet (misal melalui cashback atau promo khusus) layak diuji sebagai growth
opportunity.

---

## Product Findings (Tidak Berubah dari Draft Sebelumnya)

**Best Product & Most Profitable Product**: Product ID P217031 menempati
posisi teratas baik dari sisi revenue maupun profit.

**ABC Analysis**: 7.203 dari 24.912 produk (28,9%) tergolong kelas A dan
menyumbang 80% revenue ($4.692.180). Kelas B berisi 6.961 produk (27,9%,
menyumbang 15% revenue tambahan), dan kelas C berisi 10.749 produk (43,1%,
hanya menyumbang sisa 5% revenue). Pola long-tail masih terlihat — hampir
setengah dari total SKU (kelas C) berkontribusi sangat kecil — namun tidak
seekstrem estimasi awal; hampir 30% produk sudah cukup untuk mencapai 80%
revenue, bukan hanya segelintir SKU.

*(Catatan revisi: perhitungan awal sempat keliru karena formula Cumulative %
menggunakan range pembagi yang salah — hanya mencakup 999 baris pertama dari
24.912 baris data produk, sehingga persentase kumulatif dan jumlah SKU per
kelas jauh dari akurat. Angka di atas adalah hasil setelah koreksi range.)*

**Catatan untuk SKU Rationalization**: Rekomendasi "pangkas SKU kelas C"
terlalu agresif tanpa analisis lanjutan. SKU kelas C bisa jadi niche product,
complementary product, atau basket builder. **Limitasi Data**: dataset tidak
menyediakan Inventory Holding Cost, sehingga analisis rationalization ideal
(Class C + Low Margin + Low Frequency + High Holding Cost) tidak dapat
dilakukan secara lengkap dengan data yang tersedia. Rekomendasi dibatasi pada:
evaluasi lanjutan berbasis profit margin & frekuensi penjualan yang sudah
tersedia di data.

---

## Prioritization Matrix

| Kuadran | Item |
|---|---|
| **Quick Wins** (High Impact, Low Effort) | Evaluasi kebijakan diskon >20% (poin 4) — evidence sudah kuat: volume, AOV, dan margin sama-sama terendah di band ini |
| **Strategic Projects** (High Impact, High Effort) | Diversifikasi kategori revenue mengurangi ketergantungan Electronics (poin 2); strategi terpisah untuk Senior (loyalty) vs Young Adult (akuisisi) (poin 3); **Return Root-Cause Analysis** — investigasi kategori, region, & rentang nilai order dengan return rate/exposure tinggi, serta tambahkan return reason ke sistem operasional untuk memungkinkan diagnostic analysis (poin 5, prioritas naik setelah ditemukan AOV order retur 20% lebih tinggi dari rata-rata) |
| **Medium / Strategic Experiment** | Uji peningkatan adopsi Wallet (poin 7) — ini adalah opportunity yang teridentifikasi (AOV kompetitif, volume rendah), bukan masalah bisnis yang sudah terbukti dampaknya, sehingga perlu diuji dulu sebelum dianggap Quick Win |
| **Nice to Have** (Low Impact, Low Effort) | Monitoring berkala Return-Associated Revenue sebagai indikator exposure lanjutan (poin 5) |
| **Avoid / Deprioritize** | Intervensi shipping cost per-region (poin 6 menunjukkan ini tidak diperlukan); pemangkasan SKU kelas C tanpa analisis lanjutan |

---

## Recommendations

### Short-Term Actions
1. Evaluasi ulang diskon >20% — band ini memiliki volume order paling
   rendah, AOV paling rendah, dan margin paling rendah. Efektivitasnya
   perlu divalidasi lebih lanjut sebelum alokasi promosi dilanjutkan
2. Uji insentif adopsi Wallet (cashback/promo) secara terukur — AOV sudah
   relatif kompetitif, sementara transaction volume masih rendah.
   Efektivitas program perlu dievaluasi melalui incremental transaction
   volume dan profitability setelah promo
3. Tambahkan pencatatan alasan retur (return reason) ke sistem operasional
   — prioritas naik setelah ditemukan bahwa order yang diretur memiliki
   AOV 20% lebih tinggi dari rata-rata, sehingga revenue exposure dari
   retur lebih besar dari yang terlihat pada Return Rate 6% saja

### Medium-Term Actions
1. Kembangkan kategori Home & Sports secara bertahap untuk mengurangi
   Revenue Concentration Risk pada Electronics
2. Rancang program loyalty untuk Senior (memanfaatkan volume transaksi
   tinggi) dan strategi akuisisi/ekspansi untuk Young Adult (memanfaatkan
   AOV tinggi)
3. Lakukan analisis lanjutan SKU kelas C dengan menambahkan data Inventory
   Holding Cost (saat ini belum tersedia) sebelum keputusan discontinuation

### Expected Business Impact
- **Revenue Impact**: Diversifikasi kategori & optimasi Wallet berpotensi
  membuka sumber revenue baru tanpa mengorbankan basis existing. Mengingat
  order bernilai tinggi lebih rentan diretur, mitigasi return exposure pada
  segmen tersebut berpotensi melindungi porsi revenue yang secara
  proporsional lebih besar dari yang terlihat pada Return Rate 6% saja
- **Profit Impact**: Evaluasi ulang diskon >20% berpotensi meningkatkan
  margin dengan mengurangi aktivitas promosi pada band yang hanya
  menyumbang sekitar 2% dari total order namun memiliki margin terendah
  (14,7%). Dampak terhadap revenue dan volume penjualan tetap perlu
  divalidasi melalui pengujian atau monitoring setelah perubahan kebijakan
- **Customer Impact**: Strategi tersegmentasi (Senior vs Young Adult)
  berpotensi meningkatkan baik frekuensi transaksi maupun nilai transaksi
  per customer

---

## Limitasi Data (Disclaimer)

Beberapa analisis lanjutan yang ideal tidak dapat dilakukan sepenuhnya karena
keterbatasan dataset:
- **Return Root Cause Analysis** — dataset hanya punya kolom `returned`
  (Yes/No), tidak ada alasan retur
- **SKU Rationalization lengkap** — dataset tidak menyediakan Inventory
  Holding Cost
- **Customer Name / Product Name** — dataset hanya menyediakan ID, sehingga
  seluruh analisis di level individual customer/produk direpresentasikan
  dengan ID, bukan nama

---

## Prinsip Wording (Dipatuhi di Seluruh Dokumen)

Jangan membuat klaim lebih kuat daripada evidence yang tersedia:
- **"menunjukkan"** → ketika data menunjukkan pattern
- **"mengindikasikan"** → ketika ada reasonable interpretation
- **"berpotensi"** → ketika membahas future impact
- **"tidak terdapat indikasi"** → ketika tidak menemukan evidence masalah
- **"membuktikan"** → hanya ketika definisi/logic memang deterministik

Dihindari kecuali ada bukti eksperimental/kausal: *pasti, semata-mata, murni
disebabkan, tanpa risiko, terbukti menyebabkan*.

---

*Dokumen ini adalah versi final setelah proses validasi 7 temuan utama —
setiap insight sudah melalui tahap Finding → Evidence → Business Impact →
Recommendation, dengan angka yang telah diverifikasi ulang dari
pivot table di 04_Pivot_Store.*
