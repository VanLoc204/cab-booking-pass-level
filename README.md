# Test case level 1

## TC 1
![alt text](img/image.png)

## TC 2
![alt text](img/image-1.png)

## TC 3
![alt text](img/image-6.png)

## TC 4
![alt text](img/image-7.png)

## TC 5
![alt text](img/image-5.png)

## TC 6
![alt text](img/image-6.png)

## TC 7
![alt text](img/image-3.png)

## TC 8
![alt text](img/image-4.png)

## TC 9
![alt text](img/image-8.png)

## TC 10
![alt text](img/image-2.png)


# Test case level 2

## TC 11
![alt text](img/image-9.png)

## TC 12
![alt text](img/image-10.png)

## TC 13
![alt text](img/image-20.png)
![alt text](img/image-21.png)

## TC 14
![alt text](img/image-11.png)

## TC 15
![alt text](img/image-12.png)

## TC 16
![alt text](img/image-13.png)

## TC 17
![alt text](img/image-14.png)

## TC 18
![alt text](img/image-19.png)
![alt text](img/image-18.png)

## TC 19
![alt text](img/image-15.png)

## TC 20
![alt text](img/image-16.png)
![alt text](img/image-17.png)

# Test case level 3

## TC 21
![alt text](img/image-22.png)

## TC 22
![alt text](img/image-23.png)

## TC 23
![alt text](img/image-24.png)
![alt text](img/image-25.png)

## TC 24
![alt text](img/image-26.png)
![alt text](img/image-27.png)
![alt text](img/image-28.png)

## TC 25
![alt text](img/image-29.png)

docker exec cab_kafka kafka-console-consumer --bootstrap-server localhost:9092 --topic BookingCreated --from-beginning --max-messages 1

![alt text](img/image-30.png)

## TC 26
![alt text](img/image-31.png)

## TC 27
![alt text](img/image-32.png)
![alt text](img/image-33.png)


## TC 28
![alt text](img/image-34.png)


## TC 29
![alt text](img/image-35.png)
![alt text](img/image-36.png)

## TC 30
Lần 1 và lần 2
![alt text](img/image-37.png)

Lần 3
![alt text](img/image-38.png)

Tắt pricing-service

Giá vẫn được tính trong khoảng 8->25
![alt text](img/image-39.png)
![alt text](img/image-40.png)


# Test case level 4
## TC 31
![alt text](img/image-44.png)


## TC 32
![alt text](img/image-41.png)
![alt text](img/image-42.png)
![alt text](img/image-43.png)

## TC 33
Tạo 1 booking mới
![alt text](img/image-45.png)

Kích hoạt lỗi số tiền âm
![alt text](img/image-46.png)

Kiểm tra kết quả Rollback tại Booking Service
![alt text](img/image-47.png)

## TC 34
Gửi yêu cầu đặt xe Lần 1 (Kèm Key duy nhất)
![alt text](img/image-48.png)

Gửi lại y hệt yêu cầu đó Lần 2 (Giả lập bấm nhầm 2 lần)
![alt text](img/image-49.png)

## TC 35
Chuẩn bị lệnh gửi đồng thời (Concurrent Request)
Đổi token
![alt text](img/image-50.png)

Ko cho tạo đồng thời
![alt text](img/image-51.png)

## TC 36
Tạo 1 booking mới (ví dụ đang có id là 30)
![alt text](img/image-52.png)

Thanh toán thành công (lấy cái id của booking vừa tạo)
![alt text](img/image-53.png)

Chuyển từ request sang paid
![alt text](img/image-54.png)

## TC 37
Tạo booking mới
![alt text](img/image-55.png)

Giả lập lỗi Thanh toán (Simulate Failure)
![alt text](img/image-56.png)

Kiểm tra kết quả Rollback tại Booking Service (Trạng thái trả về là 'failed')
![alt text](img/image-57.png)

## TC 38
DB commit và Kafka event đồng bộ và giải thích
node test-outbox.js
![alt text](img/image-58.png)

## TC 39
Partial failure (network issue)
Client bị mất mạng (Timeout 5ms) và không nhận được phản hồi từ Payment Service.
Nhưng phía Server, Payment Service vẫn tiếp tục xử lý bình thường và hoàn tất việc thanh toán.
Sau 1 giây kiểm tra lại, Booking đã tự động chuyển từ REQUESTED → PAID nhờ Saga Event từ Kafka.
Không có "inconsistent state", không có "kẹt" transaction.

node test-network-failure.js
![alt text](img/image-59.png)

## TC 40
Test Case 40 là bài test tổng hợp về ACID - Atomicity, Consistency, Isolation và Durability, kiểm tra cả 4 tính chất cùng lúc.

node test-acid.js

![alt text](img/image-60.png)

# Test case level 5
Kiến trúc em dùng là Generative AI (LLM-based) kết hợp Traditional ML làm fallback thay vì rule-based hardcode (nếu hết request)

ai-service → Gọi Gemini 2.5 Flash Lite cho mọi task (ETA, Fraud, Pricing, Dispatch)

Cách hoạt động: Mỗi request → gửi prompt → Gemini lý luận → trả kết quả.

=> Dùng AI để suy luận thay vì phải training toàn bộ cần tệp dữ liệu

Phương pháp Traditional ML Pipeline:
ml-training-service   → Train mô hình (scikit-learn / TensorFlow / PyTorch)
model-serving-service → Serve mô hình đã train (ONNX / TF Serving)
ai-eta-service        → Chạy model ETA đã train
ai-matching-service   → Chạy model ghép driver-rider
ai-surge-service      → Chạy model dự báo surge


## TC 41
node test-ai-eta.js
![alt text](img/image-61.png)

## TC 42 
Kiểm tra khả năng điều chỉnh giá linh hoạt của AI dựa trên cung cầu và môi trường.

node test-ai-surge.js

**Kịch bản kiểm thử:**
1. **Ngày bình thường:** Nhu cầu thấp, nhiều tài xế -> Surge = 1.0x.
2. **Giờ cao điểm:** Nhu cầu cao, ít tài xế -> Surge > 1.5x.
3. **Trời mưa:** Nhu cầu cao + thời tiết xấu -> Surge > 2.0x (nhưng < 3.0x).

**Kết quả kỳ vọng:**
- AI trả về đúng hệ số nhân giá (Surge Multiplier) theo thời gian thực.
- Có giải thích logic (`reasoning`) cho từng quyết định tăng giá.

**Minh chứng:**
![alt text](img/image-62.png)

## TC 43

node test-ai-fraud.js

![alt text](img/image-63.png)

## TC 44 - AI Driver Recommendation Validation
Kiểm tra khả năng thông minh của AI trong việc chọn lọc tài xế tốt nhất cho khách hàng.

node test-ai-recommend.js

**Kết quả kỳ vọng:**
- AI trả về đúng **Top 3** tài xế có điểm số cao nhất.
- Mỗi gợi ý có phần `reason` giải thích tại sao chọn tài xế đó (ví dụ: "Gần khách hàng và có đánh giá 5 sao").

![alt text](img/image-64.png)


## TC 45 - AI Demand Forecasting Validation
Kiểm tra khả năng dự báo nhu cầu khách hàng của AI dựa trên dữ liệu lịch sử.

**Cách thực hiện:**
Gửi chuỗi dữ liệu nhu cầu các giờ trước đó:
```powershell
node test-ai-forecast.js
```

**Kết quả kỳ vọng:**
- Trả về danh sách dự báo cho 2 giờ tiếp theo.
- Đúng schema: Có `timestamp`, `value` (số lượng cuốc xe dự kiến) và `confidence`.

**Minh chứng:**
![alt text](img/image-65.png)

## TC 46 - Model Version Validation
Kiểm tra tính chính xác và nhất quán của thông tin phiên bản AI Model trong các phản hồi từ hệ thống.

**Cách thực hiện:**
Chạy script quét toàn bộ các endpoint AI để kiểm tra metadata:
```powershell
node test-ai-version.js
```

**Kết quả kỳ vọng:**
- Mọi endpoint (`/eta`, `/fraud`, `/forecast`, `/recommend`) đều trả về đúng trường `model` là `gemini-2.5-flash-lite`.
- Không có sự nhầm lẫn giữa các phiên bản model trong cùng một phiên làm việc.

**Minh chứng:**
![alt text](img/image-66.png)

## TC 47
AI latency < 200ms

node test-ai-latency.js

![alt text](img/image-67.png)


## TC 48
node test-ai-drift.js

![alt text](img/image-68.png)


## TC 49
Model Fallback khi lỗi — đảm bảo hệ thống không crash khi AI fail, tự động chuyển sang ML rule-based.

node test-ai-fallback.js

![alt text](img/image-69.png)

## TC 50
Gửi dữ liệu bất thường cực đoan (distance=1000km, v.v.) và kiểm tra model không crash, output phải hợp lý hoặc reject đúng cách
![alt text](img/image-70.png)

# Test case level 6
## TC 51
Agent chọn driver gần nhất nhiều driver available 
D1: 5km
D2: 2km
D3: 3km

node test-ai-agent.js

![alt text](img/image-71.png)

Dù Gemini API đang bị từ chối do quá hạn mức (quota exceeded), nhưng nhờ lớp ML Fallback mà tính năng Agent Dispatch vẫn phân tích đúng tọa độ. Nó tính toán khoảng cách và chính xác chọn D2 làm tài xế gần nhất thay vì chọn ngẫu nhiên.

## TC 52
Kịch bản kiểm thử (TC 52):

Tài xế D1 ở gần hơn (cách 2km) nhưng có rating thấp (4.0).
Tài xế D2 ở xa hơn (cách 3km) nhưng có rating vượt trội (4.9).
Preference của hành khách là "rating".
Kết quả trả về chính xác chọn D2. Mặc dù Gemini API vẫn đang bị giới hạn, ML Fallback Agent đã áp dụng đúng quy tắc cân nhắc (rule-based reasoning) ưu tiên rating khi được yêu cầu, thay vì chỉ chăm chăm chọn người gần nhất như ở TC 51.

node test-ai-agent-rating.js

![alt text](img/image-72.png)

## TC 53

node test-ai-agent-balanced.js

TC 53: AI Agent Logic (Balanced ETA vs Price) để kiểm tra khả năng ra quyết định cân bằng (trade-off) của hệ thống.

![alt text](img/image-73.png)


## TC 54

Kiểm tra xem hệ thống Agent có khả năng "hiểu" yêu cầu bằng ngôn ngữ tự nhiên từ user và map/chỉ định đúng các tool cần gọi (như Pricing Tool, ETA Tool, Fraud Tool) mà không bị dư thừa hay nhầm lẫn thứ tự hay không.

User hỏi: "Giá cước chuyến đi từ Quận 1 đến Quận 7 là bao nhiêu?" -> Agent điều hướng tới tool: pricing_service
User hỏi: "Cho tôi biết ETA là gì và tài xế bao lâu nữa thì tới?" -> Agent điều hướng tới tool: eta_service
User hỏi: "Có vẻ tài xế này đang gian lận, có fraud không?" -> Agent điều hướng tới tool: fraud_service

node test-ai-orchestrator.js

![alt text](img/image-74.png)

## TC 55
Kiểm tra khả năng phục hồi của hệ thống khi nhận một luồng dữ liệu bị khuyết hoặc hỏng (Missing Context Data).

node test-ai-agent-missing-context.js

![alt text](img/image-75.png)

## TC 56
Agent retry khi service lỗi (ETA service fail, Không fail ngay)

node test-ai-agent-retry.js

![alt text](img/image-76.png)

## TC 57
Agent không chọn driver offline 
driver list có offline

node test-ai-agent-offline.js

![alt text](img/image-77.png)

## TC 58
Agent log decision đầy đủ, có trace_id, tôi đã chỉnh sửa API /api/ai/agent/dispatch trong services/ai-service/server.js để trả về thêm một thuộc tính trace_id (được sinh ra tự động) bên cạnh mảng decision_log vốn đã có. 

node test-ai-agent-log.js

![alt text](img/image-78.png)

## TC 59
Agent xử lý nhiều request song song (nhiều request cùng lúc) nhằm đảm bảo không bị conflict và race condition

node test-ai-agent-concurrency.js

![alt text](img/image-79.png)

## TC 60
Evaluate Agent fallback rule-based khi AI fail (model crash) -> System vẫn chạy

node test-ai-agent-fallback.js

![alt text](img/image-80.png)

# Test case level 7
## TC 61
1000 requests/second booking 

nhiều user đặt xe cùng lúc 

1000 request/sec vào /booking 

- Không crash
- Response success cao (>95%)
- Latency ổn định

node test-performance-booking.js

![alt text](img/image-81.png)
![alt text](img/image-82.png)

## TC 62

- ETA service under load 
- AI service bị gọi nhiều 
- 500 request/sec ETA ETA vẫn trả đúng
- Latency < SLA (ví dụ <200ms)
- Không timeout


node test-performance-eta.js

![alt text](img/image-83.png)


## TC 63
Đối với TC 63 (Pricing Service under spike request - Surge Giờ Cao Điểm), vì đây là một test case về hiệu năng (load test) giả lập việc có 1000 yêu cầu tính giá cước gửi đến hệ thống cùng lúc, nên không thể test bằng tay hay dùng Postman thông thường được.

Bơm 1000 request "tính toán giá cước giờ cao điểm" thẳng vào Pricing Service, nhằm mục đích kiểm tra khả năng chịu tải cũng như đảm bảo giá trả về luôn hợp lệ (không bị quá tải làm mất kết nối DB dẫn đến giá bằng null hay NaN).

node test-performance-pricing.js

![alt text](img/image-84.png)

## TC 64
- Kafka throughput test 
- nhiều event (ride_requested) 
- hàng nghìn event/sec 

- Không mất event
- Consumer không bị lag lớn
- Queue ổn định


node test-performance-kafka.js

![alt text](img/image-85.png)

## TC 65
- DB connection pool exhaustion
- quá nhiều connection DB
- request concurrent cao
- Không vượt max connection
- Request bị queue hoặc reject
- Không crash DB

node test-performance-db-pool.js

![alt text](img/image-86.png)

## TC 66
- Redis cache hit rate > 90% 
- cache được sử dụng 
- nhiều request lặp lại 
- Cache hit rate > 90%
- Giảm load DB
- Response nhanh hơn

node test-performance-redis-cache.js

![alt text](img/image-87.png)

## TC 67
- API Gateway rate limit 
- traffic cao 
- vượt threshold 
- HTTP 429
- Không overload backend
- Traffic được kiểm soát

node test-performance-rate-limit.js

![alt text](img/image-88.png)

## TC 68
P95 latency < 300ms
đo performance
Không spike lớn có nghĩa là latency hoặc 
traffic không tăng đột biến bất thường khi 
gặp sự tăng đột ngột của traffic (request) 
trong thời gian rất ngắn


load test
P95 = 95% request nhanh hơn giá trị 
này

P95 latency < 200ms

node test-performance-p95.js

![alt text](img/image-89.png)

## TC 69
Load test giờ cao điểm mô phỏng giờ cao điểm load tăng dần: Số lượng request 
(traffic) tăng dần theo thời gian, lưu ý 
không phải tăng đột ngột như spike
System scale được
Không degrade mạnh: Hiệu năng có 
thể giảm --> điều này là bình thường. 
Nhưng không được giảm quá nhiều 
hoặc đột ngột.
User vẫn đặt xe được

node test-performance-peak-hour.js

![alt text](img/image-90.png)

## TC 70
- Auto scaling hoạt động 
- hệ thống dùng Kubernetes / Swarm 
- CPU/memory tăng 
- Pod scale up
- Load được phân phối
- Không bottleneck

node test-performance-auto-scaling.js

![alt text](img/image-91.png)