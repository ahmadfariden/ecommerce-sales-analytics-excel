# KPI Documentation

Rangkuman seluruh formula KPI yang dipakai di 5 dashboard (05–09). Semua
referensi kolom mengacu ke sheet `03_Fact_Sales_Clean` kecuali disebutkan lain.

---

## 05_Executive_Dashboard

| KPI | Formula |
|---|---|
| Total Revenue | `=SUM(Revenue)` |
| Total Profit | `=SUM(Profit)` |
| Profit Margin % | `=Total Profit / Total Revenue` |
| Total Orders | `=COUNTA(order_id) - 1` (kurangi 1 untuk header) |
| Total Customers | `=SUMPRODUCT(1/COUNTIF(customer_id_range, customer_id_range))` (hitung unique customer) |
| Average Order Value (AOV) | `=Total Revenue / Total Orders` |

---

## 06_Sales_Dashboard

| KPI | Formula |
|---|---|
| Revenue | `=SUM(Revenue)` |
| Profit | `=SUM(Profit)` |
| Quantity Sold | `=SUM(quantity)` |
| Average Selling Price | `=SUM(Revenue) / SUM(quantity)` |
| Revenue Growth % | `=(Bulan_Terakhir - Bulan_Sebelumnya) / Bulan_Sebelumnya` (referensi manual ke 2 sel terakhir pada pivot Revenue by Month) |
| Average Discount % | `=AVERAGE(discount) * 100` |

---

## 07_Customer_Dashboard

| KPI | Formula |
|---|---|
| Total Customers | `=SUMPRODUCT(1/COUNTIF(customer_id_range, customer_id_range))` |
| New Customers | `=SUMPRODUCT((Customer_Type_range="New") / COUNTIF(customer_id_range, customer_id_range))` |
| Returning Customers | Sama seperti New Customers, ganti kondisi jadi `="Returning"` |
| Repeat Purchase Rate | `=Returning Customers / Total Customers` |
| Average Revenue per Customer | `=SUM(Revenue) / Total Customers` |

---

## 08_Product_Dashboard

| KPI | Formula |
|---|---|
| Total Products Sold | `=SUMPRODUCT(1/COUNTIF(product_id_range, product_id_range))` |
| Total Categories | `=SUMPRODUCT(1/COUNTIF(category_range, category_range))` |
| Best Selling Product | `=INDEX(product_id_range, MATCH(MAX(revenue_by_product_range), revenue_by_product_range, 0))` — dari pivot Revenue vs Profit by Product |
| Most Profitable Product | `=INDEX(product_id_range, MATCH(MAX(profit_by_product_range), profit_by_product_range, 0))` — dari pivot yang sama, kolom Profit |

---

## 09_Operations_Dashboard

| KPI | Formula |
|---|---|
| Return Rate | `=AVERAGE(Return_Flag)` (format Percentage) |
| Returned Orders | `=COUNTIF(returned, "Yes")` |
| Average Delivery Days | `=AVERAGE(delivery_time_days)` |
| Return-Associated Revenue *(sebelumnya "Lost Revenue")* | `=SUMIF(returned, "Yes", Revenue)` |
| % of Total Revenue (Return-Associated) | `=Return_Associated_Revenue / Total_Revenue` |
| Total Shipping Cost | `=SUM(shipping_cost)` |
| Average Shipping Cost | `=Total Shipping Cost / Total Orders` |
| Shipping Cost % | `=Total Shipping Cost / Total Revenue` |

---

## KPI Pendukung Analisis (dihitung di 04_Pivot_Store, untuk Insights)

| KPI | Formula | Kegunaan |
|---|---|---|
| AOV per Segment | `=Sum_Revenue_Segment / Count_Transaksi_Segment` | Poin 3 Insights — Customer Value |
| Margin % per Discount Band | `=Sum_Profit_Band / Sum_Revenue_Band` | Poin 4 Insights — Discount Effectiveness |
| Shipping Cost % per Region | `=Sum_ShippingCost_Region / Sum_Revenue_Region` | Poin 6 Insights — Shipping Normalization |
| AOV per Payment Method | `=Sum_Revenue_Method / Count_Orders_Method` | Poin 7 Insights — Payment Method |
| AOV Returned vs Non-Returned | `=Sum_Revenue_Status / Count_Orders_Status` (basis kolom `returned`) | Poin 5 Insights — Return Revenue Exposure |
| A/B/C Product Count | `=COUNTIF(ABC_Class_range, "A Product")` (ulangi untuk B, C) | Product Dashboard — ABC Analysis chart |

---

## Definisi Metrik Penting

- **Repeat Purchase Rate** dihitung berdasarkan *unique customer* (Order_Count
  > 1 sepanjang seluruh periode data), **bukan** retention rate dalam artian
  time-windowed (customer kembali dalam periode tertentu).
- **Return-Associated Revenue** (rename dari "Lost Revenue from Returns")
  merepresentasikan total revenue dari order berstatus Returned = Yes, bukan
  kerugian bersih (net loss) aktual — dataset tidak menyediakan detail nilai
  refund atau partial return.
- **Return Rate** (basis jumlah order) dan **Return-Associated Revenue %**
  (basis nilai revenue) memiliki denominator berbeda dan tidak boleh
  disamakan langsung meski angkanya berdekatan (6% vs 6,6%).
