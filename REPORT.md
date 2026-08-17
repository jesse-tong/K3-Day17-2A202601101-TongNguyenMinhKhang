# Báo cáo LAB 17 — Data Pipeline Engineering

**Họ tên:** Tống Nguyễn Minh Khang  **Lớp:** AICB-P2T2  **Ngày:** …

---

## 0 · Kết quả `make verify`

<details>
<summary>Dán nguyên output ba lần chạy vào đây</summary>
> Lần 1
```
 ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  LAB 17 · make verify
  ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  run 1/3 … 28.2s
  run 2/3 … 32.6s
  run 3/3 … 47.2s

  BẢNG                  ỔN ĐỊNH          SỐ HÀNG     KỲ VỌNG   GHI CHÚ
  ──────────────────────────────────────────────────────────────────────────
  gold_training_set     ✓ ok              12,480      12,480   ✓
  gold_feature_daily    ✓ ok               8,645       9,100   ✗ thiếu 455 hàng
  gold_doc_chunks       ✓ ok              31,200      31,200   ✓
  quarantine_tickets    ✓ ok                   0         312   ✗ thiếu 312 hàng

  CHECKSUM từng lượt
  ──────────────────────────────────────────────────────────────────────────
  gold_training_set     8622572a97    8622572a97    8622572a97   ✓
  gold_feature_daily    4eee63cd82    4eee63cd82    4eee63cd82   ✓
  gold_doc_chunks       92d8e50131    92d8e50131    92d8e50131   ✓
  quarantine_tickets    empty         empty         empty        ✓

  KIỂM TRA KHÁC
  ──────────────────────────────────────────────────────────────────────────
  dbt test                                    ✓ 9/9 pass
  silver_tickets.priority ∈ 1..4, không NULL  ✗ 6,606 hàng sai
  quarantine_tickets đúng số bản ghi lỗi      ✗ 0 / 312
  gold_training_set: 1 hàng / 1 ticket        ✗ 12,480 ticket bị lặp
```
> Lần 2
```
  ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  LAB 17 · make verify
  ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  run 1/3 … 56.1s
  run 2/3 … 48.3s
  run 3/3 … 52.2s

  BẢNG                  ỔN ĐỊNH          SỐ HÀNG     KỲ VỌNG   GHI CHÚ
  ──────────────────────────────────────────────────────────────────────────
  gold_training_set     ✓ ok              12,480      12,480   ✓
  gold_feature_daily    ✓ ok               9,100       9,100   ✓
  gold_doc_chunks       ✓ ok              31,200      31,200   ✓
  quarantine_tickets    ✓ ok                   0         312   ✗ thiếu 312 hàng

  CHECKSUM từng lượt
  ──────────────────────────────────────────────────────────────────────────
  gold_training_set     8622572a97    8622572a97    8622572a97   ✓
  gold_feature_daily    3db448685c    3db448685c    3db448685c   ✓
  gold_doc_chunks       92d8e50131    92d8e50131    92d8e50131   ✓
  quarantine_tickets    empty         empty         empty        ✓

  KIỂM TRA KHÁC
  ──────────────────────────────────────────────────────────────────────────
  dbt test                                    ✓ 9/9 pass
  silver_tickets.priority ∈ 1..4, không NULL  ✗ 6,606 hàng sai
  quarantine_tickets đúng số bản ghi lỗi      ✗ 0 / 312
  gold_training_set: 1 hàng / 1 ticket        ✓ không lặp
  dashboard rows scanned                      ✗ 5,000,000 → 5,000,000 (1.0×, cần ≥ 10×)
    số file parquet                           ✗ 5,000 → 5,000
    kết quả truy vấn không đổi                ✓
  DAG: catchup / max_active_runs              ✓ False / 1

  TỔNG KẾT
  ──────────────────────────────────────────────────────────────────────────
  ✓  1 · gold_training_set idempotent & đúng số hàng
  ✓  2 · gold_feature_daily đủ hàng (dữ liệu về muộn)
  ✗  3 · contract + quarantine + dbt test
  ✓  4 · gold_doc_chunks vẫn ổn định (đối chứng)
```
> Lần 3
```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  LAB 17 · make verify
  ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  run 1/3 … 58.0s
  run 2/3 … 51.1s
  run 3/3 … 59.0s

  BẢNG                  ỔN ĐỊNH          SỐ HÀNG     KỲ VỌNG   GHI CHÚ
  ──────────────────────────────────────────────────────────────────────────
  gold_training_set     ✓ ok              12,480      12,480   ✓
  gold_feature_daily    ✓ ok               9,100       9,100   ✓
  gold_doc_chunks       ✓ ok              31,200      31,200   ✓
  quarantine_tickets    ✓ ok                 312         312   ✓

  CHECKSUM từng lượt
  ──────────────────────────────────────────────────────────────────────────
  gold_training_set     8dd7c98653    8dd7c98653    8dd7c98653   ✓
  gold_feature_daily    3db448685c    3db448685c    3db448685c   ✓
  gold_doc_chunks       92d8e50131    92d8e50131    92d8e50131   ✓
  quarantine_tickets    ebb89036fb    ebb89036fb    ebb89036fb   ✓

  KIỂM TRA KHÁC
  ──────────────────────────────────────────────────────────────────────────
  dbt test                                    ✓ 11/11 pass
  silver_tickets.priority ∈ 1..4, không NULL  ✓ sạch
  quarantine_tickets đúng số bản ghi lỗi      ✓ 312 / 312
  gold_training_set: 1 hàng / 1 ticket        ✓ không lặp
  dashboard rows scanned                      ✗ 5,000,000 → 5,000,000 (1.0×, cần ≥ 10×)
    số file parquet                           ✗ 5,000 → 5,000
    kết quả truy vấn không đổi                ✓
  DAG: catchup / max_active_runs              ✓ False / 1
```
</details>

Tổng kết: **… / 4 tiêu chí đạt**

---

## 1 · Kích thước bảng training tăng sau mỗi lần chạy

| | |
|---|---|
| **Triệu chứng** | Sau khi retry / Clear Task trên Airflow hoặc chạy lại pipeline, kích thước bảng `gold_training_set` tăng lên liên tục sau mỗi lượt (38,750 hàng sau 3 lượt so với kỳ vọng 12,480 hàng); kiểm tra bảng đích thấy 12,480 ticket bị lặp (mỗi ticket xuất hiện 2–3 lần), checksum giữa các lượt chạy không ổn định (`7c461563f4` → `d11657ff21` → `2b76a4f850`). |
| **Nguyên nhân** | **1. Ở tầng dbt model:** `gold_training_set.sql` được cấu hình `materialized = 'incremental'` nhưng không khai báo `unique_key` và `incremental_strategy`. Khi thiếu `unique_key`, dbt mặc định dùng chiến lược `append` và sinh ra câu lệnh `INSERT INTO gold_training_set ...`. Dữ liệu nguồn CDC có các bản ghi cập nhật (`op = 'u'`) diễn ra ở các ngày khác nhau, khiến một ticket đi qua bộ lọc `_ingested_at` nhiều lần trong 14 ngày vận hành. Do câu lệnh là `INSERT` thuần, mỗi bản ghi cập nhật bị chèn thêm một hàng mới thay vì ghi đè lên hàng cũ. Tương tự, khi retry một ngày, partition của ngày đó lại được `INSERT` thêm lần nữa.<br>**2. Ở tầng DAG Airflow:** `ai_training_pipeline.py` cấu hình `catchup=True` và không giới hạn `max_active_runs` (mặc định cho phép nhiều run đồng thời), dẫn đến khi Clear Task hoặc có sự cố, Airflow kích hoạt nhiều DAG run chạy bù song song cùng ghi vào bảng đích mà không có cơ chế khoá/idempotency. |
| **Cách khắc phục** | **1. File `dbt/models/gold/gold_training_set.sql`:** Khai báo `unique_key = 'ticket_id'` và `incremental_strategy = 'delete+insert'` trong khối `config()` để dbt tự động xoá bản ghi cũ của ticket trước khi chèn bản ghi mới nhất.<br>**2. File `dags/ai_training_pipeline.py`:** Thiết lập `catchup=False` và `max_active_runs=1` trong cấu hình `DAG` để ngăn việc tự động schedule dồn dập các run quá khứ và đảm bảo chỉ có tối đa 1 run thực thi tại một thời điểm. |
| **Bằng chứng** | trước: 38,750 hàng (12,480 ticket bị lặp, FAIL) · sau: 12,480 hàng (1 hàng / 1 ticket, không lặp) · checksum 3 lượt: `8dd7c98653` (ổn định 100% qua cả 3 lượt chạy). |


---

## 2 · Bảng đặc trưng theo ngày thiếu hàng ở các ngày quá khứ

| | |
|---|---|
| **Triệu chứng** | Bảng `gold_feature_daily` bị thiếu 455 hàng (chỉ đạt 8,645 / 9,100 hàng, thiếu khoảng 5.05%); các ngày mới chạy thì đủ dữ liệu nhưng những ngày quá khứ bị thiếu các cặp `(event_date, customer_id)`. |
| **P99 độ trễ đo được** |  `2.7258` ngày· Max = `2.9447` ngày - Tỉ lệ đến muộn > 1 ngày = `5.05%` |
| **Lookback đã chọn** | **3 ngày** (`interval 3 day`) — vì P99 = 2.73 ngày và Max = 2.94 ngày đều $\le 3$ ngày, vừa đủ để thu hồi 100% dữ liệu bị đến muộn trong tập dữ liệu mà không làm tăng chi phí tính toán không cần thiết. |
| **Nguyên nhân** | **1. Cơ chế lọc dữ liệu:** Khối `is_incremental()` ban đầu sử dụng điều kiện `where event_date > (select max(event_date) from {{ this }})`. Khi một sự kiện phát sinh ở quá khứ (ví dụ `event_date = 08-12`) nhưng bị trễ đường truyền đến ngày `08-15` mới nạp vào kho (`_ingested_at = 08-15`), tại lượt chạy ngày 08-15 thì `max(event_date)` trong target đã đạt `08-14`. Do `08-12 > 08-14` trả về `FALSE`, toàn bộ các bản ghi đến muộn bị loại bỏ và vĩnh viễn không bao giờ được đưa vào bảng Gold.<br>**2. Thiếu cơ chế idempotent cho lookback:** Grain của bảng là 1 dòng / 1 cặp `(event_date, customer_id)`. Khi nới rộng window để tính lại ngày cũ, nếu không khai báo `unique_key` và `incremental_strategy` đúng, dbt sẽ dùng `append` (`INSERT`) gây nhân bản dòng và làm sai lệch số liệu. |
| **Cách khắc phục** | **File `dbt/models/gold/gold_feature_daily.sql`:**<br>1. Khai báo composite key `unique_key = ['event_date', 'customer_id']` và `incremental_strategy = 'merge'` trong khối `config()`.<br>2. Sửa điều kiện lọc trong khối `is_incremental()` thành `where event_date >= (select max(event_date) - interval 3 day from {{ this }})` để quét và tính toán lại các sự kiện trong cửa sổ 3 ngày gần nhất, tự động xoá dòng cũ và chèn dòng mới đã gộp đủ dữ liệu đến muộn. |
| **Bằng chứng** | trước: 8,645 hàng (thiếu 455 hàng) · sau: 9,100 hàng / 9,100 hàng (đạt 100%) · checksum 3 lượt: `3db448685c` (ổn định). |

Vì sao chọn P99 làm căn cứ thay vì `max`? Chi phí của mỗi lựa chọn là gì?

> Trong môi trường vận hành thực tế, phân bố độ trễ luôn có hiện tượng long-tail outliers do thiết bị mất mạng dài ngày hoặc lỗi hệ thống upstream kéo dài nhiều tuần/tháng.
> - **Nếu chọn theo `max`:** Lookback window sẽ bị kéo dài theo những bản ghi cá biệt, buộc pipeline ở **mọi lượt chạy hằng ngày** đều phải quét lại, aggregate lại và ghi đè một lượng dữ liệu lớn trong quá khứ. Chi phí phải trả là lãng phí tài nguyên tính toán (compute cost), kéo dài thời gian chạy, dễ vi phạm SLA.
> - **Nếu chọn theo `P99`:** Đây là điểm cân bằng tối ưu giữa việc thu hồi gần như toàn bộ dữ liệu đến muộn (99%+) với chi phí tài nguyên tối thiểu. 1% dữ liệu trễ bất thường ngoài P99 sẽ được xử lý riêng, không làm ảnh hưởng đến daily pipeline.
> *(Trong bài lab này, vì cả P99 = 2.73 ngày và Max = 2.94 ngày đều < 3 ngày, nên lookback 3 ngày thu hồi được toàn bộ 100% dữ liệu thiếu).*

---

## 3 · Kiểu dữ liệu cột priority thay đổi giữa chu kỳ

| | |
|---|---|
| **Triệu chứng** | Sau khi team backend thay đổi định dạng cột `priority` sang chuỗi từ ngày 2026-08-10, `silver_tickets.priority` có tới 6,606 hàng bị sai (chứa giá trị `NULL`, `0`, `5`, `-1` ngoài khoảng quy định `1..4`); `quarantine_tickets` có 0 hàng (thiếu 312 hàng); mô hình phân loại dự đoán kém hẳn nhưng pipeline vẫn chạy qua mà không hề báo lỗi. |
| **Nguyên nhân** | **1. Chưa xử lý Schema Evolution:** Team backend chuyển sang gửi nhãn chuỗi (`'urgent'`, `'high'`, `'medium'`, `'low'`) nhưng logic cũ dùng `try_cast(priority_raw as integer)` nên biến toàn bộ nhãn chuỗi hợp lệ thành `NULL`, trong khi lại chấp nhận các giá trị số không hợp lệ như `0`, `5`, `-1`.<br>**2. Chưa bật Data Contract & Data Quality Tests:** `schema.yml` để `enforced: false` và chưa kích hoạt bộ test `accepted_values: [1, 2, 3, 4]`, khiến dữ liệu sai kiểu và sai miền giá trị vẫn lọt qua tầng Silver mà không bị bắt lỗi.<br>**3. Xếp hạng sai thứ tự trong Silver:** Nếu thực hiện `row_number()` trước rồi mới lọc lỗi ở cuối, một ticket có bản ghi mới nhất bị lỗi sẽ bị loại bỏ hoàn toàn khỏi Silver (số ticket tụt từ 12,480 xuống 12,168). |
| **Ba nhóm giá trị `priority` và cách xử lý từng nhóm** | **1. Nhóm 1 (Số hợp lệ - `1, 2, 3, 4`):** Đúng contract ban đầu $\rightarrow$ Giữ nguyên, ép kiểu sang `INTEGER`.<br>**2. Nhóm 2 (Nhãn chuỗi hợp lệ - `urgent, high, medium, low`):** Bản chất là Schema Evolution (ý nghĩa không đổi, chỉ đổi format) $\rightarrow$ Ánh xạ (Map) về số nguyên tương ứng: `urgent → 1, high → 2, medium → 3, low → 4`.<br>**3. Nhóm 3 (Dữ liệu lỗi thật - `P1, unknown, 0, 5, -1, '', NULL`):** Không đúng định dạng và miền giá trị $\rightarrow$ Trả về `NULL` trong macro chuẩn hóa để định tuyến sang bảng `quarantine_tickets`. |
| **Cách khắc phục** | **1. File `dbt/macros/normalize_priority.sql`:** Dùng khối `CASE` xử lý đủ 3 nhóm trên; cập nhật macro `priority_reject_reason` để phân loại chi tiết nguyên nhân lỗi.<br>**2. File `dbt/models/silver/silver_tickets.sql`:** Lọc bỏ bản ghi lỗi trước (`where normalize_priority(priority_raw) is not null`), sau đó mới đánh số thứ tự `row_number()` để giữ lại trạng thái hợp lệ gần nhất cho mọi ticket.<br>**3. File `dbt/models/silver/quarantine_tickets.sql`:** Thu thập các bản ghi lỗi với điều kiện `where normalize_priority(priority_raw) is null`.<br>**4. File `dbt/models/silver/schema.yml`:** Bật `contract: enforced: true` trên `silver_tickets`, thêm tests `not_null` và `accepted_values: [1, 2, 3, 4]`. |
| **Bằng chứng** | `quarantine_tickets` = 312 hàng (khớp 312/312) · `silver_tickets` = 12,480 ticket (đủ 100%, không bị hụt) · `silver_tickets.priority ∈ 1..4, không NULL` đạt sạch · `dbt test` pass 11/11 test (tăng 2 test mới so với bản gốc). |

Câu hỏi thiết kế: nên chặn ở tầng Bronze hay Silver? Vì sao **không** để pipeline dừng khi gặp bản ghi lỗi?

> - Nên chặn ở tầng Silver, không chặn ở tầng Bronze: Do tầng Bronze có nhiệm vụ là *Landing Zone* lưu trữ toàn bộ dữ liệu thô (*raw payload*) đúng như nguồn phát ra mà không phán xét dữ liệu (giữ `VARCHAR`, `JSON`). Nếu từ chối bản ghi ngay tại Bronze, ta sẽ mất audit trail, khiến việc điều tra sự cố, đối soát dữ liệu và replay khi upstream cập nhật contract trở nên bất khả thi.
>   - Tầng Silver là nơi thực thi Data Quality Contract (làm sạch, ép kiểu, kiểm tra schema). Bản ghi đạt chuẩn được đưa vào Silver để phục vụ phân tích, còn bản ghi vi phạm được định tuyến sang bảng cách ly `quarantine_tickets`.
> - Vì sao KHÔNG để pipeline dừng khi gặp bản ghi lỗi (DLQ pattern thay vì Fail-fast):
>   - Xét về quy mô: chỉ có 312 bản ghi lỗi trong khi có hơn 14,000 bản ghi CDC và hàng trăm nghìn sự kiện/transcript hoàn toàn hợp lệ. Nếu để pipeline dừng (crash), toàn bộ các mô hình downstream sẽ bị đình trệ dữ liệu mới, gây gián đoạn dịch vụ nghiêm trọng (vi phạm SLA).
>   - Áp dụng mô hình Quarantine / Dead Letter Queue (DLQ) giúp hệ thống đạt tính linh hoạt và khả dụng cao (graceful degradation): pipeline vẫn tiếp tục vận hành dữ liệu sạch cho người dùng, trong khi bảng quarantine đóng vai trò hàng đợi cảnh báo để đội Data Engineer phân tích, can thiệp và re-process sau.

---

## 4 · *(mở rộng, không bắt buộc)* Bài trong EXTRA.md

| | |
|---|---|
| **Bài đã làm** | A / B / không làm |
| **Nguyên nhân** | |
| **Cách khắc phục** | |
| **Bằng chứng** | |

---

## 5 · Tổng kết

| Nhiệm vụ | Khi tiếp nhận một hệ thống chưa quen, tôi sẽ kiểm tra điều này trước tiên |
|---|---|
| 1 | Kiểm tra tính idempotency của các incremental model (khai báo `unique_key`, `incremental_strategy = 'delete+insert'/'merge'`) và cấu hình scheduling của orchestrator (`catchup=False`, `max_active_runs=1`) để ngăn ngừa nhân bản/lặp dữ liệu khi có retry. |
| 2 | Đo phân bố độ trễ dữ liệu (`_ingested_at - event_time`) để xác định P99 latency, từ đó thiết lập **Lookback Window** phù hợp cho late-arriving data kết hợp composite key để cập nhật chính xác dữ liệu quá khứ. |
| 3 | Kiểm tra việc thực thi Data Contract (`enforced: true`), bộ kiểm tra chất lượng dữ liệu (Data Quality Tests) và cơ chế định tuyến bản ghi lỗi sang Quarantine / Dead-Letter Queue thay vì để dữ liệu lỗi lọt vào downstream hoặc làm sập toàn bộ pipeline. |
