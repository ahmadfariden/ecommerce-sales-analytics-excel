# Power Query Documentation

## Tujuan

Seluruh proses transformasi data (cleaning & helper table) dibangun di
**Power Query**, bukan formula Excel manual. Tujuannya: refresh data cukup 1
klik, mengurangi formula Excel yang berat, dan memisahkan Data Layer (Power
Query) dari Reporting Layer (Pivot + Dashboard).

---

## Rule Pembagian Layer

| Layer | Tools | Fungsi |
|---|---|---|
| Data Layer | Power Query | ETL & Data Preparation (cleaning, validasi, derived field) |
| Aggregation Layer | Pivot Table | Aggregation dari Fact_Sales_Clean |
| Presentation Layer | Dashboard | Visualization & KPI Monitoring |

---

## Struktur Query

```text
Raw_Sales (Connection Only)
├── Data Type Conversion (locale Indonesian untuk kolom desimal)
├── Trimmed Text, Cleaned Text (category, payment_method, region,
│   returned, customer_gender)
└── digunakan sebagai basis untuk:

Customer_Type_Lookup (Loaded to sheet)
├── Reference dari Raw_Sales
├── Remove Other Columns (hanya sisakan customer_id)
├── Group By customer_id → Count Rows (Order_Count)
└── Custom Column: Customer_Type = IF Order_Count > 1 THEN "Returning"
    ELSE "New"

Fact_Sales_Clean (Loaded to sheet "03_Fact_Sales_Clean")
├── Merge Queries: Raw_Sales + Customer_Type_Lookup (JOIN by customer_id,
│   Left Outer — 34.500 dari 34.500 baris berhasil match)
├── Expand kolom Customer_Type dari hasil merge
├── Added Custom Columns:
│   ├── Revenue = [total_amount]
│   ├── Profit = [profit_margin]
│   ├── Margin_Percent = [Profit] / [Revenue]
│   ├── Return_Flag = IF [returned]="Yes" THEN 1 ELSE 0
│   ├── Delivery_Group = IF [delivery_time_days]<=3 THEN "Fast"
│   │   ELSE IF <=7 THEN "Normal" ELSE "Slow"
│   ├── Customer_Segment = kategorisasi [customer_age] (5 kelas)
│   └── Discount_Band = kategorisasi [discount] (5 band)
├── Date Intelligence (dari order_date):
│   ├── Inserted Year
│   ├── Inserted Month (angka)
│   ├── Inserted Month Name
│   ├── Inserted Day Name (of Week)
│   ├── Inserted Quarter
│   └── Custom Column Month_Year = Text.Start([Month Name],3) & "-" &
│       Text.From([Year])
└── Output final: 31 kolom, ±34.500 baris
```

---

## Data Flow Diagram

```text
ecommerce_sales_34500.csv
            │
            ▼
      Raw_Sales
            │
            ▼
    Data Cleaning
            │
            ├── Changed Type (locale Indonesian)
            ├── Trimmed Text / Cleaned Text
            └── Text Standardization (5 kolom teks)
            │
            ▼
     Merge dengan Customer_Type_Lookup
            │
            ▼
   Derived Columns + Date Intelligence
            │
            ▼
    Fact_Sales_Clean (sheet "03_Fact_Sales_Clean")
            │
            ▼
      Pivot_Store (15+ Pivot Table)
            │
            ▼
       Dashboards (05–09)
            │
            ▼
         Insights (10)
```

---

## Load Settings

| Query | Load To |
|---|---|
| Raw_Sales | Connection Only |
| Customer_Type_Lookup | Table → New Worksheet (dipakai untuk pivot Purchase Frequency Distribution) |
| Fact_Sales_Clean | Table → New Worksheet ("03_Fact_Sales_Clean") |

---

## Refresh Mechanism

```text
CSV Replace → Refresh All → Power Query Refresh → Pivot Refresh
→ Dashboard Update
```

Untuk refresh data baru: ganti file CSV sumber (nama file harus sama), lalu
klik **Data → Refresh All** di Excel. Seluruh query, pivot table, dan chart
akan ter-update otomatis mengikuti data baru.

---

## Temuan Teknis Selama Proses Build

1. **Locale Indonesia** — data desimal di CSV memakai koma (`24,73`), bukan
   titik. Kolom numerik harus di-Change Type dengan eksplisit memilih
   **Locale: Indonesian**, kalau tidak, formula matematika (seperti
   Custom Column perkalian) akan error karena kolom terbaca sebagai Text.
2. **Merge Queries di Excel 2019** secara default membuat query baru
   ("Merge1"), bukan menempel langsung ke query asal — perlu di-rename
   manual setelah proses merge selesai.
3. **Group By dengan grouping column tunggal** (misal untuk hitung rata-rata
   satu kolom) di Basic mode Power Query tidak bisa langsung menghasilkan 1
   angka ringkasan — solusinya tambahkan kolom bantu bernilai konstan
   (misal `GroupKey = 1`) sebagai grouping key, baru hitung Average/Max/Min
   pada kolom target.
4. **Scatter Chart tidak bisa dibuat langsung dari data di dalam
   PivotTable** — Excel mewajibkan data di-copy dan Paste Special (Values
   Only) ke range biasa terlebih dahulu sebelum bisa memilih tipe chart
   X Y (Scatter).
