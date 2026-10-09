# PLAN.md — Day 23: AI Product Growth & Operating Dashboard

## 1. Mục tiêu bài lab

Xây dựng một Operating Dashboard cho sản phẩm AI, giúp đội ngũ theo dõi các chỉ số vận hành hằng tuần, nhận biết rủi ro sớm và đưa ra quyết định dựa trên ngưỡng đã xác định trước.

**Đầu ra bắt buộc:**

* `README.md`: Thông tin cá nhân, sản phẩm và loại mô hình kinh doanh.
* `worksheet.md`: Nội dung làm việc từ Trạm 1 đến Trạm 4.
* `dashboard.md`: Dashboard hoàn chỉnh, mục tiêu vừa một trang.
* `dashboard.doc`: Bản PDF dashboard và phụ lục phép tính, tối đa 2 trang.

**Tiêu chí quan trọng:**

* Chọn đúng một mô hình: B2C, B2B hoặc B2B2C.
* Có 1 North Star và 6–8 đèn chỉ số.
* Có đúng 3 tầng: Leading, Operating, Lagging.
* Có ít nhất 2 đèn Leading và 1 đèn chi phí AI.
* Mọi đèn có ngưỡng xanh, vàng, đỏ; nguồn và lý do.
* Có ít nhất 2 ngưỡng [MH] kèm phép tính.
* Có 5 luật quyết định, trong đó ít nhất 2 luật dừng.
* Có 3 cổng gác ngày 30/60/90 và Kill Criteria.
* Trung thực ghi rõ các chỉ số chưa đo được.

## 2. Chuẩn bị repo

### 2.1. Tạo repo cá nhân

Tên repo theo quy ước:

`K4-L3-DAY23-HoVaTen-MSSV-AIProductGrowth`

Thay `HoVaTen` và `MSSV` bằng thông tin thật, viết không dấu và không có khoảng trắng.

Repo đề bài:

https://github.com/VinUni-AI20k/K4-L3-Day23-AI-Product-Growth

Clone repo đề bài:

```bash
git clone https://github.com/VinUni-AI20k/K4-L3-Day23-AI-Product-Growth.git
```

### 2.2. Sao chép file mẫu

Lấy hai file sau từ thư mục `templates/` của repo đề bài:

* `templates/worksheet.md`
* `templates/dashboard.md`

Sao chép vào thư mục gốc repo cá nhân. Giữ nguyên cấu trúc mẫu trước khi bổ sung nội dung.

### 2.3. Cấu trúc repo cần nộp

```text
K4-L3-DAY23-HoVaTen-MSSV-AIProductGrowth/
├── README.md
├── worksheet.md
├── dashboard.md
└── dashboard.pdf
```

### 2.4. Chuẩn bị số liệu đầu vào

Trước khi làm Trạm 1, tìm lại kết quả của bài Mô hình tài chính và bài Value Metric & Cost/Job.

Điền các thông tin vào đầu `worksheet.md`:

| Dữ liệu              | Nội dung cần có                                               |
| -------------------- | ------------------------------------------------------------- |
| ARPU                 | Doanh thu trung bình trên mỗi khách hàng trong một kỳ         |
| Gross Margin (GM)    | Tỷ suất lợi nhuận gộp                                         |
| CAC                  | Chi phí để có được một khách hàng                             |
| CAC Payback mục tiêu | Số tháng tối đa mong muốn để thu hồi CAC                      |
| Runway               | Thời gian còn có thể duy trì hoạt động với nguồn lực hiện tại |
| Value Metric         | Đơn vị giá trị mà khách hàng nhận được hoặc được tính phí     |
| Cost/Job             | Chi phí hoàn thành một công việc có giá trị cho khách hàng    |

Ghi rõ đơn vị, kỳ đo và nguồn của từng số. Không tự đặt số nếu chưa có dữ liệu.

Nếu dữ liệu tài chính mật, có thể nhân các số tiền với cùng một hệ số và ghi rõ trong `README.md` rằng số liệu đã được scale. Các tỷ lệ cần được tính nhất quán.

## 3. Quy trình thực hiện 5 trạm

## Trạm 1 — Chốt mô hình kinh doanh (15 phút)

### Mục tiêu

Xác định đúng mô hình kinh doanh hiện tại và lập danh sách các chỉ số có thể đo.

### Việc cần làm

Trả lời ba câu hỏi:

1. Ai trả tiền cho sản phẩm?
2. Ai trực tiếp sử dụng sản phẩm?
3. Nếu có doanh nghiệp trung gian, sản phẩm có tiếp cận hoặc đo lường được người dùng cuối không?

### Cách chọn mô hình

* **B2C:** Cá nhân trực tiếp trả tiền và sử dụng sản phẩm.
* **B2B:** Doanh nghiệp trả tiền và là khách hàng trực tiếp; chưa trực tiếp kiểm soát hoặc đo lường được người dùng cuối theo cách cần thiết cho mô hình B2B2C.
* **B2B2C:** Doanh nghiệp đối tác trả tiền hoặc phân phối, người dùng cuối là khách hàng của đối tác và sản phẩm có khả năng tiếp cận hoặc đo lường nhóm người dùng đó.

Không chọn theo kế hoạch tương lai. Chọn theo cách sản phẩm đang hoạt động hôm nay.

### Nội dung cần điền vào `worksheet.md`

**A. Trả lời ba câu hỏi về mô hình.**

**B. Viết một câu chốt mô hình**, theo mẫu:

> Chúng tôi là [B2C/B2B/B2B2C] vì [đối tượng trả tiền], [đối tượng sử dụng] và [cách tiếp cận người dùng cuối nếu có].

**C. Chép bảng đèn phù hợp** từ HANDBOOK §3.1, §3.2 hoặc §3.3.

Đánh dấu từng đèn:

* ✅ Đã đo được: ghi số liệu và nơi lưu.
* 🔧 Có thể đo trong 2 tuần: ghi sự kiện, log hoặc công cụ cần bổ sung.
* ❌ Chưa đo được: nêu rõ dữ liệu hoặc phương pháp còn thiếu.

**Đầu ra:** Một câu chốt mô hình và bảng trạng thái dữ liệu của toàn bộ chỉ số trong bảng tương ứng.

## Trạm 2 — Xây dựng cây chỉ số 3 tầng (25 phút)

### Mục tiêu

Chọn 6–8 chỉ số thực sự cần theo dõi, thay vì đưa tất cả chỉ số kinh doanh vào dashboard.

### Ba tầng chỉ số

**1. Leading — Chỉ số báo trước**

Thay đổi theo ngày hoặc tuần, giúp phát hiện vấn đề sớm và có thể can thiệp.

Ví dụ:

* Activation Rate.
* Time-to-first-value (TTFV).
* Partner Activation Rate.
* Chi phí AI trên mỗi người dùng hoặc khách hàng.

**2. Operating — Chỉ số vận hành**

Thay đổi theo tuần hoặc tháng, phản ánh hiệu quả của các hoạt động vận hành.

Ví dụ:

* Trial-to-paid conversion.
* Sales cycle.
* Volume trên mỗi partner.
* Chi phí phục vụ khách hàng.

**3. Lagging — Chỉ số kết quả**

Thường phản ánh kết quả sau một khoảng thời gian dài hơn.

Ví dụ:

* LTV.
* CAC Payback thực tế.
* NRR.
* Gross Margin thực tế.

Ví dụ minh họa cho B2B:

`TTFV (Leading) → Sales Cycle (Operating) → NRR (Lagging)`

### Chọn North Star

Chọn một chỉ số quan trọng nhất thể hiện giá trị cốt lõi mà sản phẩm tạo ra. Với sản phẩm AI, ưu tiên chỉ số phản ánh công việc có giá trị thực sự được hoàn thành, không chỉ là số lượt gọi AI.

North Star phải phù hợp với Value Metric và mô hình kinh doanh.

### Nội dung cần điền vào `worksheet.md`

Chọn 6–8 đèn và điền các trường:

| Trường        | Cần viết gì                                                   |
| ------------- | ------------------------------------------------------------- |
| Tên           | Tên chỉ số                                                    |
| Tầng          | Leading, Operating hoặc Lagging                               |
| Định nghĩa    | Đối tượng, điều kiện tính và trường hợp không tính            |
| Công thức     | Tử số, mẫu số và đơn vị                                       |
| Nhịp đo       | Hằng ngày, hằng tuần hoặc hằng tháng                          |
| Báo trước cho | Chỉ số ở tầng dưới mà đèn này dự báo                          |
| Khi đỏ        | Hành động dự kiến khi vượt ngưỡng rủi ro; hoàn thiện ở Trạm 4 |

**Ràng buộc bắt buộc:**

* Tổng cộng 6–8 đèn.
* Ít nhất 2 Leading.
* Ít nhất 1 đèn đo chi phí AI.
* Không quá 3 Lagging.
* Mọi Leading đều phải chỉ ra đèn ở tầng dưới mà nó báo trước.

### Đèn chi phí AI

Chọn chỉ số phản ánh chi phí phát sinh trước khi biên lợi nhuận bị ảnh hưởng.

Ví dụ:

* B2C: p95 chi phí AI trên mỗi người dùng trả phí mỗi tháng / ARPU.
* B2B: chi phí triển khai / ACV hoặc chi phí AI trên mỗi khách hàng.
* B2B2C: chi phí inference / doanh thu của từng partner.

Chỉ sử dụng chỉ số phù hợp với dữ liệu và cách tính phí thực tế của sản phẩm.

**Đầu ra:** Một North Star và 6–8 thẻ đèn có định nghĩa, công thức, nhịp đo, tầng và mối quan hệ dự báo.

## Trạm 3 — Đặt ngưỡng có nguồn (30 phút)

### Mục tiêu

Mỗi đèn phải có ngưỡng xanh, vàng, đỏ, nguồn và lý do giải thích tại sao ngưỡng đó phù hợp.

### Ba loại nguồn

* **[BM] Benchmark:** Số liệu tham chiếu từ nguồn bên ngoài có thể kiểm tra.
* **[MH] Mô hình:** Ngưỡng được suy ra từ mô hình tài chính của chính sản phẩm.
* **[TB] Thực nghiệm:** Baseline từ dữ liệu lịch sử của sản phẩm hoặc ngưỡng tạm thời có lịch đo rõ ràng.

Không dùng một con số chỉ vì cảm thấy hợp lý.

### Ý nghĩa ba màu

* 🟢 **Xanh:** Nằm trong vùng chấp nhận được; duy trì hoạt động.
* 🟡 **Vàng:** Có rủi ro hoặc xu hướng xấu; điều tra nguyên nhân và đặt thời hạn xử lý.
* 🔴 **Đỏ:** Chạm điều kiện kích hoạt luật quyết định ở Trạm 4.

Mỗi ngưỡng phải có một câu lý do.

### Cách suy ngược từ mô hình tài chính

Ví dụ minh họa:

* ARPU = 200.000 đồng/tháng.
* GM mục tiêu = 60%.
* CAC Payback tối đa = 12 tháng.

Lãi gộp mỗi khách hàng mỗi tháng:

`200.000 × 60% = 120.000 đồng`

CAC tối đa theo mục tiêu payback:

`120.000 × 12 = 1.440.000 đồng`

Diễn giải:

* [MH] CAC đỏ nếu vượt 1.440.000 đồng, với điều kiện các giả định về ARPU, GM và kỳ đo vẫn phù hợp.
* Ngưỡng vàng phải có lý do riêng, chẳng hạn vùng cảnh báo trước khi chạm CAC tối đa.
* Ngưỡng xanh phải thấp hơn mức cảnh báo theo chính sách rủi ro đã chọn.

Đây chỉ là ví dụ phương pháp. Thay bằng số thực tế của sản phẩm.

Một phép tính [MH] khác có thể dựa vào giới hạn chi phí AI:

`Cost/Job tối đa = Doanh thu/Job × (1 − GM mục tiêu)`

Công thức chỉ áp dụng khi doanh thu và Cost/Job được tính cùng đơn vị, cùng phạm vi chi phí và GM mục tiêu bao gồm các chi phí liên quan tương ứng.

### Nội dung cần điền vào `worksheet.md`

Với từng đèn, ghi:

1. Ngưỡng xanh.
2. Ngưỡng vàng.
3. Ngưỡng đỏ.
4. Nguồn [BM], [MH] hoặc [TB].
5. Lý do chọn ngưỡng.
6. Ngày kiểm tra nếu sử dụng benchmark.
7. Phép tính chi tiết trong phụ lục nếu là [MH].

**Bắt buộc:** Ít nhất 2 ngưỡng [MH] có đầu vào, phép tính và kết quả.

Với [BM], ưu tiên mở nguồn gốc và kiểm tra lại số liệu. Ghi ngày kiểm tra bên cạnh ngưỡng.

Với [TB], nếu chưa có baseline, ghi rõ chưa có chuẩn, cách đo hai chu kỳ và ngày dự kiến có số.

**Đầu ra:** 100% đèn có đủ ba ngưỡng, nguồn và lý do; phụ lục có ít nhất hai phép tính [MH].

## Trạm 4 — Viết 5 luật quyết định (30 phút)

### Mục tiêu

Biến ngưỡng đỏ thành hành động cụ thể, có thời hạn và có điều kiện ngăn phản ứng sai.

### Cấu trúc một luật

Mỗi luật phải có đủ bốn phần:

* **NẾU:** Chỉ số và ngưỡng đỏ cụ thể.
* **TRONG/TRÊN:** Khoảng thời gian hoặc số quan sát.
* **THÌ:** Hành động bắt đầu bằng động từ cụ thể.
* **KHÔNG THÌ:** Hành động bị cấm để tránh phản xạ sai.

Thêm điều kiện **VÀ** khi chỉ số dễ nhiễu do mẫu nhỏ.

### Mẫu viết luật

**Luật 1 — ⏹ Luật dừng**

* NẾU: [Chỉ số] chạm ngưỡng đỏ [giá trị].
* TRONG/TRÊN: [Cửa sổ thời gian hoặc số quan sát].
* VÀ: [Điều kiện mẫu tối thiểu nếu cần].
* THÌ: Dừng [hoạt động cụ thể] trong [thời hạn]; người phụ trách [vai trò] thực hiện.
* KHÔNG THÌ: Không tăng [nguồn lực hoặc hoạt động] để bù cho vấn đề chưa giải quyết.

Đây là mẫu cấu trúc, không phải luật hoàn chỉnh để sao chép. Cần thay toàn bộ nội dung trong ngoặc bằng ngưỡng và hành động của sản phẩm.

### Yêu cầu của 5 luật

* Viết đúng 5 luật cho 5 đèn quan trọng.
* Vế NẾU sử dụng đúng ngưỡng đỏ ở Trạm 3.
* Có thời gian hoặc số quan sát rõ ràng.
* Hành động cụ thể, có thể bắt đầu thực hiện.
* Vế KHÔNG THÌ cấm một phản xạ sai cụ thể.
* Ít nhất 2 luật dừng, đánh dấu ⏹.
* Không kết thúc bằng các câu như “xem xét lại”, “cân nhắc” hoặc “theo dõi thêm”.

### Gợi ý phản xạ sai cần chặn

| Tình huống             | Phản xạ sai có thể xảy ra                    |
| ---------------------- | -------------------------------------------- |
| Retention giảm         | Tăng quảng cáo để bù người dùng rời bỏ       |
| TTFV quá dài           | Giảm giá thay vì xử lý thời gian triển khai  |
| Partner chưa kích hoạt | Ký thêm partner mới                          |
| Chi phí AI vượt ngưỡng | Tăng quota sử dụng mà chưa kiểm soát chi phí |
| CAC vượt giới hạn      | Tiếp tục mở rộng acquisition không điều kiện |

Chỉ sử dụng tình huống phù hợp với mô hình đã chọn.

**Đầu ra:** 5 luật có đủ vế, ít nhất 2 luật dừng và các hành động có thể kiểm tra việc thực hiện.

## Trạm 5 — Cổng gác 90 ngày và hoàn thiện dashboard (20 phút)

### Mục tiêu

Xác định ba mốc quyết định để tiếp tục, sửa, đổi hướng hoặc dừng sản phẩm.

### Bốn quyết định

* **GO:** Đạt ngưỡng; chuyển sang giai đoạn tiếp theo.
* **FIX:** Chưa đạt nhưng đã xác định được nguyên nhân; sửa một vấn đề trong 30 ngày.
* **PIVOT:** Giả định cốt lõi không còn phù hợp; thay đổi phân khúc, mô hình hoặc Value Metric.
* **KILL:** Đã FIX một lần cho cùng vấn đề nhưng không cải thiện theo tiêu chí đã định; dừng hướng hiện tại.

Không sử dụng FIX lần thứ hai cho cùng một vấn đề.

### Thiết kế ba cổng gác

| Cổng    | Trọng tâm                           | Bằng chứng cần có                                               |
| ------- | ----------------------------------- | --------------------------------------------------------------- |
| Ngày 30 | Học và kiểm chứng giả định ban đầu  | Baseline, log, kết quả thử nghiệm hoặc tài liệu xác định vấn đề |
| Ngày 60 | Kiểm tra khả năng cải thiện sau FIX | Báo cáo so sánh trước và sau, số liệu pilot hoặc cohort         |
| Ngày 90 | Quyết định tiếp tục đầu tư hay dừng | Báo cáo kết quả có số liệu và bằng chứng kiểm chứng             |

Mỗi cổng phải có đúng một metric chính, một ngưỡng cụ thể, một bằng chứng vật lý và quyết định nếu trượt. Không đặt mục tiêu doanh thu làm cổng chính ngày 30.

Thay các ví dụ trong HANDBOOK bằng mục tiêu phù hợp với loại mô hình, dữ liệu hiện tại và runway thực tế.

### Viết Kill Criteria

Kill Criteria phải là một câu có đủ:

* Chỉ số hoặc điều kiện thất bại.
* Ngưỡng số cụ thể.
* Thời hạn hoặc ngày quyết định.
* Hành động dừng rõ ràng.

Ví dụ về cấu trúc:

> Nếu [metric] vẫn thấp hơn [ngưỡng] tại ngày [mốc], sau một lần FIX kéo dài [thời gian] và có đủ [bằng chứng/mẫu], dừng [hướng sản phẩm hoặc hoạt động cụ thể].

Không dùng câu chung chung như “nếu không hiệu quả thì xem xét dừng”.

### Mục CHƯA ĐO ĐƯỢC

Lấy các chỉ số 🔧 và ❌ từ Trạm 1, sau đó ghi:

| Chỉ số       | Trạng thái | Cần bổ sung                        | Người phụ trách | Hạn có số |
| ------------ | ---------- | ---------------------------------- | --------------- | --------- |
| [Tên chỉ số] | 🔧/❌       | [Log, event, dữ liệu hoặc công cụ] | [Vai trò]       | [Ngày]    |

Không tự chuyển chỉ số chưa đo thành số liệu đã xác minh.

### Ráp `dashboard.md`

Dashboard cuối cùng phải chứa:

1. Tên sản phẩm và câu chốt mô hình.
2. North Star.
3. Cây chỉ số 3 tầng.
4. Các ngưỡng xanh, vàng, đỏ và nguồn.
5. Năm luật quyết định, đánh dấu ít nhất hai luật dừng.
6. Ba cổng gác ngày 30/60/90.
7. Kill Criteria.
8. Mục CHƯA ĐO ĐƯỢC.

Ưu tiên trình bày ngắn gọn bằng bảng để dashboard vừa một trang. Giữ phép tính dài trong `worksheet.md` và phụ lục PDF.

**Đầu ra:** Dashboard có đủ nội dung và ba cổng gác có thể kiểm chứng.

## 4. Hoàn thiện README.md

`README.md` cần có:

* Họ tên.
* MSSV.
* Tên sản phẩm AI.
* Một câu chốt B2C/B2B/B2B2C.
* Mô tả ngắn giá trị sản phẩm.
* Ghi chú số liệu đã scale nếu có.

Không đưa API key, mật khẩu, token truy cập, dữ liệu khách hàng hoặc thông tin tài chính mật vào repo công khai.

## 5. Xuất dashboard.pdf

1. Mở `dashboard.md` trong VS Code hoặc GitHub.
2. Mở Markdown Preview.
3. Chọn Print → Save as PDF.
4. Đảm bảo trang 1 là dashboard.
5. Thêm phụ lục phép tính [MH] ở trang 2.
6. Kiểm tra PDF không quá 2 trang và chữ có thể đọc được.

Nếu cần, điều chỉnh độ rộng bảng, cỡ chữ và lề trang; không loại bỏ các trường bắt buộc chỉ để ép vừa trang.

## 6. Kiểm tra trước khi nộp

### Repo và file

* [ ] Tên repo đúng quy ước.
* [ ] Có đủ `README.md`, `worksheet.md`, `dashboard.md`, `dashboard.pdf`.
* [ ] `worksheet.md` có đủ kết quả Trạm 1–4.
* [ ] PDF không quá 2 trang, dashboard ở trang 1.

### Chỉ số và ngưỡng

* [ ] Đã chốt đúng mô hình hiện tại.
* [ ] Có 1 North Star.
* [ ] Có 6–8 đèn, đúng 3 tầng.
* [ ] Có ít nhất 2 Leading và 1 đèn chi phí AI.
* [ ] Không quá 3 Lagging.
* [ ] Mọi đèn có định nghĩa, công thức, nhịp đo và quan hệ dự báo phù hợp.
* [ ] Mọi đèn có ngưỡng 🟢🟡🔴, nguồn và lý do.
* [ ] Có ít nhất 2 phép tính [MH].
* [ ] Mọi benchmark [BM] có ngày kiểm tra.

### Luật và cổng gác

* [ ] Có đủ 5 luật với NẾU / TRONG-TRÊN / THÌ / KHÔNG THÌ.
* [ ] Có ít nhất 2 luật dừng đánh dấu ⏹.
* [ ] Hành động đủ cụ thể để thực hiện.
* [ ] Có đủ cổng ngày 30, 60, 90.
* [ ] Mỗi cổng có metric, ngưỡng, bằng chứng và quyết định.
* [ ] Kill Criteria có số và ngày.
* [ ] Mục CHƯA ĐO ĐƯỢC phản ánh trung thực trạng thái dữ liệu.

### Nộp bài

* [ ] Commit và push đủ bốn file lên GitHub.
* [ ] Mở repo bằng cửa sổ ẩn danh để kiểm tra quyền truy cập.
* [ ] Dán link repo lên đúng bài Day 23 trên LMS trước deadline được thông báo.

## 7. Phân bổ thời gian 120 phút

| Công việc                 |                            Thời gian |
| ------------------------- | -----------------------------------: |
| Chuẩn bị repo và số liệu  | Trước khi bắt đầu hoặc làm song song |
| Trạm 1 — Mô hình          |                              15 phút |
| Trạm 2 — Cây chỉ số       |                              25 phút |
| Trạm 3 — Ngưỡng           |                              30 phút |
| Trạm 4 — Luật             |                              30 phút |
| Trạm 5 — Dashboard        |                              20 phút |
| **Tổng thời gian 5 trạm** |                         **120 phút** |

Nếu chưa hoàn thiện một trạm khi hết giờ, chuyển sang trạm tiếp theo và quay lại chỉnh sửa ở cuối. Ưu tiên Trạm 3 và Trạm 4 vì hai phần này chiếm 60/100 điểm.

## 8. Nguyên tắc thực hiện

1. Dùng số liệu thật của sản phẩm hoặc ghi rõ số liệu chưa có.
2. Không biến giả định thành kết quả đã đo.
3. Không chọn ngưỡng chỉ vì số tròn hoặc dễ đạt.
4. Không dùng benchmark thay thế hoàn toàn cho ngưỡng suy từ mô hình tài chính.
5. Không đặt Leading chỉ để đủ số lượng; phải giải thích nó báo trước cho chỉ số nào.
6. Không viết luật có hành động mơ hồ.
7. Không trì hoãn quyết định ở cổng gác bằng lựa chọn “theo dõi thêm”.
8. Dashboard là công cụ ra quyết định hằng tuần, không chỉ là bảng tổng hợp số liệu.

**Thứ tự làm việc:** Chuẩn bị số liệu → Trạm 1 → Trạm 2 → Trạm 3 → Trạm 4 → Trạm 5 → Kiểm tra PDF → Push repo → Nộp link LMS.
