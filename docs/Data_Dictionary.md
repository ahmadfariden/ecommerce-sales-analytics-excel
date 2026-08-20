# Data Dictionary — 03_Fact_Sales_Clean

Dokumen ini mendefinisikan seluruh kolom di tabel final `Fact_Sales_Clean`,
yang menjadi sumber data untuk semua Pivot Table dan Dashboard di project ini.

Total: 31 kolom (17 asli dari dataset + 14 kolom turunan/derived).

---

## Kolom Asli (dari `ecommerce_sales_34500.csv`)

| Kolom | Tipe Data | Deskripsi |
|---|---|---|
| order_id | Text | ID unik untuk setiap order/transaksi |
| customer_id | Text | ID unik untuk setiap customer |
| product_id | Text | ID unik untuk setiap produk |
| category | Text | Kategori produk (Electronics, Fashion, Home, Beauty, Sports, Toys, Grocery) |
| price | Decimal | Harga satuan produk sebelum diskon |
| discount | Decimal | Besaran diskon dalam format desimal (0,3 = 30%) |
| quantity | Whole Number | Jumlah item yang dibeli dalam satu order |
| payment_method | Text | Metode pembayaran (Credit Card, Debit Card, UPI, PayPal, COD, Wallet) |
| order_date | Date | Tanggal transaksi terjadi |
| delivery_time_days | Whole Number | Jumlah hari pengiriman dari order sampai diterima |
| region | Text | Wilayah geografis customer (Central, East, North, South, West) |
| returned | Text | Status retur order ("Yes" / "No") |
| total_amount | Decimal | Nilai akhir tagihan setelah diskon — dipakai sebagai basis Revenue |
| shipping_cost | Decimal | Biaya pengiriman untuk order tersebut |
| profit_margin | Decimal | **Nominal profit** per order (bukan rasio/persentase, meski namanya "margin" — lihat catatan di 02_Data_Cleaning) |
| customer_age | Whole Number | Usia customer (18–70) |
| customer_gender | Text | Gender customer (Male / Female / Other) |

---

## Kolom Turunan (Derived — dibuat via Power Query & Excel)

| Kolom | Tipe Data | Formula / Logic | Deskripsi |
|---|---|---|---|
| Revenue | Decimal | `= total_amount` | Revenue resmi per order (source of truth, bukan hasil hitung manual price×qty×discount) |
| Profit | Decimal | `= profit_margin` | Profit nominal per order, diambil langsung dari kolom profit_margin |
| Margin_Percent | Decimal | `= Profit / Revenue` | Rasio profit terhadap revenue per order |
| Return_Flag | Whole Number (0/1) | `IF returned="Yes" THEN 1 ELSE 0` | Representasi numerik dari status retur, dipakai untuk AVERAGE (Return Rate) |
| Delivery_Group | Text | Fast (≤3 hari), Normal (4–7 hari), Slow (>7 hari) | Kategorisasi kecepatan pengiriman |
| Customer_Segment | Text | Young Adult (<25), Adult (25–34), Mid-Age (35–44), Mature (45–54), Senior (55+) | Kategorisasi customer berbasis usia |
| Discount_Band | Text | 0%, 1–10%, 11–20%, 21–30%, >30% | Kategorisasi besaran diskon (dari nilai desimal dikonversi ke band persen) |
| Customer_Type | Text | New (Order_Count = 1), Returning (Order_Count > 1) | Status customer berdasarkan frekuensi transaksi sepanjang periode data |
| Year | Whole Number | Diturunkan dari order_date | Tahun transaksi |
| Month | Whole Number | Diturunkan dari order_date | Bulan transaksi (angka 1–12) |
| Month Name | Text | Diturunkan dari order_date | Nama bulan (January, February, dst) |
| Day Name | Text | Diturunkan dari order_date | Nama hari (Monday, Tuesday, dst) |
| Quarter | Whole Number | Diturunkan dari order_date | Kuartal transaksi (1–4) |
| Month_Year | Text | `Text.Start(Month Name,3) & "-" & Year` | Gabungan bulan-tahun untuk sumbu chart trend (contoh: "Dec-2023") |

---

## Kolom Tambahan di 04_Pivot_Store (Excel Formula, bukan Power Query)

| Kolom | Formula | Deskripsi |
|---|---|---|
| Revenue_Band | `IF(Revenue<=PERCENTILE(...,0.33),"Low Revenue", IF(...,"Medium Revenue","High Revenue"))` | Kategorisasi order berdasarkan percentile Revenue |
| ABC Class (Product Ranking) | Berdasarkan Cumulative % Revenue per Product ID | A Product (0–80% cumulative), B Product (80–95%), C Product (95–100%) |

---

## Catatan Penting

- **profit_margin** bukan rasio/persentase seperti namanya menyiratkan — nilai maksimumnya mencapai 1.536 dan rata-rata 28,1, yang tidak masuk akal sebagai persentase margin. Data ini adalah **nominal profit** langsung per order (lihat 02_Data_Cleaning.md untuk detail investigasi).
- **discount** disimpan dalam format desimal (0,3 = 30%), bukan format persen langsung — dikonfirmasi lewat pengecekan MAX value.
- **Customer Name** dan **Product Name** tidak tersedia di dataset asli — seluruh identifikasi customer dan produk menggunakan ID.
