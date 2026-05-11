# 🛍️ Phân Tích Hành Vi Mua Sắm Khách Hàng

> **Dự án Portfolio Data Analytics End-to-End** | Python · PostgreSQL · Power BI

## 📌 Tổng Quan Dự Án

Dự án này mô phỏng một **quy trình phân tích dữ liệu hoàn chỉnh theo tiêu chuẩn doanh nghiệp**, thể hiện khả năng chuyển hóa dữ liệu thô thành thông tin kinh doanh có giá trị. Toàn bộ pipeline bao gồm: làm sạch dữ liệu, mô hình hóa, phân tích SQL, trực quan hóa bằng Power BI, và trình bày kết quả cho stakeholders.

Bộ dữ liệu gồm các giao dịch mua sắm của khách hàng với các thông tin: nhân khẩu học, lịch sử mua hàng, danh mục sản phẩm, phương thức thanh toán, tùy chọn vận chuyển và trạng thái đăng ký hội viên.

---

## 🎯 Vấn Đề Kinh Doanh

Một công ty bán lẻ muốn hiểu rõ **điều gì thúc đẩy quyết định mua sắm của khách hàng** nhằm:

- Xác định các phân khúc khách hàng có giá trị cao
- Tối ưu chiến lược giảm giá và khuyến mãi
- Cải thiện danh mục sản phẩm theo từng nhóm
- Tăng tỷ lệ đăng ký hội viên và giữ chân khách hàng
- Cá nhân hóa marketing theo độ tuổi và giới tính

> 📄 Chi tiết vấn đề kinh doanh: [`Business Problem Document.pdf`](Business%20Problem%20%20Document.pdf)

---

## 🔄 Quy Trình Thực Hiện

```
Dữ liệu thô (CSV)
       │
       ▼
[1] Python — Làm sạch dữ liệu & Phân tích khám phá (EDA)
       │
       ▼
[2] PostgreSQL — Phân tích SQL theo câu hỏi kinh doanh
       │
       ▼
[3] Power BI — Dashboard tương tác cho stakeholders
       │
       ▼
[4] Báo cáo & Thuyết trình kết quả
```

---

## 🛠️ Công Nghệ Sử Dụng

| Công cụ | Mục đích |
|---|---|
| **Python** (pandas, matplotlib, seaborn) | Làm sạch dữ liệu, EDA, feature engineering |
| **PostgreSQL** | Truy vấn SQL, phân tích kinh doanh |
| **Power BI** | Dashboard tương tác & trực quan hóa |
| **Jupyter Notebook** | Môi trường phân tích tái sử dụng được |

---

## 📁 Cấu Trúc Repository

```
customer_behavior_analysis/
│
├── 📓 customer_shopping_behavior.ipynb          # Python EDA & Chuẩn bị dữ liệu
├── 🗄️ customer_behavior_PostgreSQL_queries.sql  # 10 SQL queries phân tích kinh doanh
├── 📊 customer_behavior_dashboard.pbix          # Dashboard Power BI
├── 📂 customer_shopping_behavior.csv            # Bộ dữ liệu gốc
│
├── 📄 Business Problem Document.pdf             # Tài liệu vấn đề kinh doanh
├── 📄 CustomerShoppingBehaviorAnalysis.pdf      # Báo cáo phân tích đầy đủ
└── 📊 Pre-Customer-Shopping-Behavior-Analysis.pptx  # Slide thuyết trình
```

---

## 🐍 Giai Đoạn 1 — Python: Chuẩn Bị Dữ Liệu & EDA

**File:** `customer_shopping_behavior.ipynb`

### Các bước thực hiện:
- **Làm sạch dữ liệu:** Xử lý giá trị thiếu, chỉnh sửa kiểu dữ liệu, loại bỏ bản ghi trùng lặp
- **Feature Engineering:**
  - Tạo cột `age_group` phân nhóm độ tuổi (18–25, 26–35, 36–50, 51+)
  - Tạo cột `customer_segment` (New / Returning / Loyal) dựa trên số lần mua trước đó
- **Phân tích khám phá (EDA):**
  - Phân phối nhân khẩu học: độ tuổi, giới tính, địa lý
  - Phân tích phân phối giá trị đơn hàng và phát hiện outliers
  - Hiệu suất theo danh mục sản phẩm và từng sản phẩm cụ thể
  - Phân tích tương quan giữa các biến chính
- **Trực quan hóa:** Bar charts, histograms, heatmaps, box plots bằng `matplotlib` và `seaborn`

---

## 🗄️ Giai Đoạn 2 — SQL: Phân Tích Kinh Doanh

**File:** `customer_behavior_PostgreSQL_queries.sql`

10 câu hỏi kinh doanh được trả lời bằng PostgreSQL:

| # | Câu hỏi kinh doanh | Kỹ thuật SQL |
|---|---|---|
| Q1 | Tổng doanh thu theo giới tính | `GROUP BY`, `SUM` |
| Q2 | Khách dùng giảm giá nhưng chi tiêu trên mức trung bình | Subquery, `WHERE` |
| Q3 | Top 5 sản phẩm có rating cao nhất | `AVG`, `ORDER BY`, `LIMIT` |
| Q4 | Chi tiêu trung bình: vận chuyển Standard vs Express | `ROUND`, `AVG`, `WHERE` |
| Q5 | So sánh chi tiêu: hội viên vs không hội viên | `COUNT`, `AVG`, `SUM` |
| Q6 | Top 5 sản phẩm có tỷ lệ áp dụng giảm giá cao nhất | `CASE WHEN`, tính phần trăm |
| Q7 | Phân khúc khách hàng: New / Returning / Loyal | `CTE`, `CASE WHEN` |
| Q8 | Top 3 sản phẩm bán chạy nhất trong mỗi danh mục | `CTE`, `ROW_NUMBER() OVER (PARTITION BY)` |
| Q9 | Khách mua lặp lại có xu hướng đăng ký hội viên không? | `WHERE`, `GROUP BY` |
| Q10 | Đóng góp doanh thu theo từng nhóm tuổi | `SUM`, `ORDER BY` |

### Ví dụ Query — Phân Khúc Khách Hàng với CTE & Window Function:
```sql
-- Customer Segmentation (Q7)
WITH customer_type AS (
    SELECT customer_id, previous_purchases,
        CASE
            WHEN previous_purchases = 1 THEN 'New'
            WHEN previous_purchases BETWEEN 2 AND 10 THEN 'Returning'
            ELSE 'Loyal'
        END AS customer_segment
    FROM customer
)
SELECT customer_segment, COUNT(*) AS "Số lượng khách hàng"
FROM customer_type
GROUP BY customer_segment;

-- Top 3 sản phẩm mỗi danh mục (Q8) — Window Function
WITH item_counts AS (
    SELECT category, item_purchased,
           COUNT(customer_id) AS total_orders,
           ROW_NUMBER() OVER (PARTITION BY category ORDER BY COUNT(customer_id) DESC) AS item_rank
    FROM customer
    GROUP BY category, item_purchased
)
SELECT item_rank, category, item_purchased, total_orders
FROM item_counts
WHERE item_rank <= 3;
```

---

## 📊 Giai Đoạn 3 — Power BI: Dashboard Tương Tác

**File:** `customer_behavior_dashboard.pbix`

Dashboard chuyển hóa toàn bộ kết quả phân tích thành báo cáo trực quan, tương tác dành cho ban lãnh đạo và stakeholders.

### Các thành phần chính:

**KPI Cards (Chỉ số tổng quan):**
- Tổng doanh thu · Giá trị đơn hàng trung bình · Tổng số khách hàng · Tỷ lệ đăng ký hội viên

**Biểu đồ phân tích:**
- 📊 Doanh thu theo giới tính — so sánh mức chi tiêu Nam vs Nữ
- 📊 Doanh thu theo nhóm tuổi — xác định phân khúc sinh lời nhất
- 📊 Top sản phẩm theo doanh số & rating — đánh giá hiệu suất danh mục
- 📊 Phân bố khách hàng New / Returning / Loyal
- 📊 Tác động của discount — doanh thu theo trạng thái giảm giá
- 📊 So sánh vận chuyển Standard vs Express
- 📊 Hiệu quả chương trình hội viên — subscriber vs non-subscriber

> 💡 *Để xem dashboard, mở file `customer_behavior_dashboard.pbix` bằng [Power BI Desktop](https://powerbi.microsoft.com/desktop/) (tải miễn phí).*

---

## 💡 Giai Đoạn 4 — Key Findings & Khuyến Nghị Kinh Doanh

### 🔍 Phát Hiện Chính (Key Findings)

**1. Khách hàng hội viên chi tiêu cao hơn đáng kể và đóng góp tỷ trọng doanh thu lớn hơn số lượng của họ**

Phân tích Q5 cho thấy nhóm subscriber không chỉ có giá trị đơn hàng trung bình cao hơn mà còn đóng góp doanh thu tổng lớn hơn không tương xứng so với số lượng. Đây là tín hiệu mạnh: chương trình hội viên đang hoạt động hiệu quả và cần được đầu tư mở rộng.

**2. Khách hàng Loyal tạo ra phần lớn doanh thu dù chỉ chiếm thiểu số**

Phân khúc Loyal (trên 10 lần mua) đóng góp doanh thu cao nhất nhưng chiếm tỷ lệ nhỏ trong tổng khách hàng. Điều này xác nhận nguyên tắc Pareto trong bán lẻ: chi phí giữ chân khách cũ luôn hiệu quả hơn chi phí thu hút khách mới. Chính sách retention cần được ưu tiên hàng đầu.

**3. Discount có chọn lọc mang lại hiệu quả — tránh giảm giá đại trà**

Q6 cho thấy một số sản phẩm nhất định có tỷ lệ discount cao nhưng vẫn duy trì rating tốt và doanh thu ổn định. Điều này chứng minh giảm giá đúng đối tượng và đúng sản phẩm sẽ kích cầu hiệu quả, trong khi discount đại trà sẽ làm xói mòn biên lợi nhuận mà không tăng được lòng trung thành.

**4. Khách chọn vận chuyển Express thể hiện sức chi tiêu cao hơn — nhóm tiềm năng cho sản phẩm premium**

Q4 cho thấy có chênh lệch rõ ràng về giá trị đơn hàng giữa nhóm Express và Standard. Nhóm này sẵn sàng trả thêm cho sự tiện lợi — đây là tệp khách hàng lý tưởng để giới thiệu các sản phẩm/dịch vụ cao cấp hoặc gói hội viên premium.

**5. Khách mua từ 5+ lần là "điểm nóng" chuyển đổi sang hội viên**

Q9 cho thấy khách có từ 5 lần mua trở lên có xác suất đăng ký subscription cao hơn đáng kể. Đây là ngưỡng tự nhiên trong hành trình khách hàng — doanh nghiệp nên kích hoạt thông điệp mời đăng ký hội viên đúng tại thời điểm khách đạt ngưỡng này, không phải sớm hơn hay muộn hơn.

**6. Nhóm tuổi 26–50 là động lực doanh thu chính của doanh nghiệp**

Phân tích Q10 cho thấy nhóm khách 26–50 tuổi không phải đông nhất nhưng chi tiêu mạnh nhất. Đây là phân khúc phù hợp để tập trung ngân sách marketing và triển khai các chiến dịch sản phẩm cao cấp, vì họ có thu nhập ổn định và nhu cầu đa dạng.

---

### 💼 Khuyến Nghị Chiến Lược

| Vấn đề phát hiện | Hành động đề xuất |
|---|---|
| Tỷ lệ subscription còn thấp | Kích hoạt thông điệp đăng ký hội viên khi khách đạt 4–5 lần mua |
| Khách Returning chưa chuyển sang Loyal | Thiết kế chương trình tích điểm hoặc milestone reward |
| Discount chưa được nhắm đúng mục tiêu | Áp dụng discount chọn lọc theo sản phẩm và phân khúc cụ thể |
| Nhóm Express chưa được khai thác | Upsell gói Premium cho tệp khách hàng chi tiêu cao này |
| Marketing chưa cá nhân hóa theo độ tuổi | Tập trung ngân sách vào nhóm 26–50 với sản phẩm phù hợp |

> 📄 Báo cáo đầy đủ: [`CustomerShoppingBehaviorAnalysis.pdf`](CustomerShoppingBehaviorAnalysis.pdf)  
> 📊 Slide thuyết trình: [`Pre-Customer-Shopping-Behavior-Analysis.pptx`](Pre-Customer-Shopping-Behavior-Analysis.pptx)

---

## 🚀 Hướng Dẫn Chạy Dự Án

### Yêu cầu hệ thống:
- Python 3.10+
- Jupyter Notebook hoặc JupyterLab
- PostgreSQL 15+
- Power BI Desktop (miễn phí)

### Bước 1 — Chạy EDA với Python
```bash
# Clone repository
git clone https://github.com/HoLeTrungBao/customer_behavior_analysis.git
cd customer_behavior_analysis

# Cài đặt thư viện
pip install pandas numpy matplotlib seaborn jupyter

# Mở notebook
jupyter notebook customer_shopping_behavior.ipynb
```

### Bước 2 — Chạy SQL Queries
```sql
-- Tạo database trong PostgreSQL
CREATE DATABASE customer_behavior;
\c customer_behavior

-- Import dữ liệu (dùng pgAdmin)
COPY customer FROM '/đường/dẫn/tới/customer_shopping_behavior.csv'
DELIMITER ',' CSV HEADER;

-- Chạy toàn bộ queries
\i customer_behavior_PostgreSQL_queries.sql
```

### Bước 3 — Xem Dashboard Power BI
```
1. Tải Power BI Desktop tại powerbi.microsoft.com (miễn phí)
2. Mở file customer_behavior_dashboard.pbix
3. Refresh data source nếu được yêu cầu
4. Khám phá dashboard tương tác
```

---

## 📬 Liên Hệ

**Hồ Lê Trung Bảo**
- 🔗 GitHub: [@HoLeTrungBao](https://github.com/HoLeTrungBao)

---

## 📄 Giấy Phép

Dự án được cấp phép theo [MIT License](LICENSE).

---

*⭐ Nếu bạn thấy dự án này hữu ích, hãy để lại một star để ủng hộ!*
