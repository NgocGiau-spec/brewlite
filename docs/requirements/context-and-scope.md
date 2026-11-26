# BrewLite — Bối cảnh, stakeholder và phạm vi hệ thống

- **Mã tài liệu:** BL-REQ-01 (Assignment 2 — Task 1.1)
- **Phiên bản:** 1.0
- **Đối tượng đọc:** Khách hàng/Chủ cửa hàng, Giảng viên nghiệm thu, Nhóm phát triển (BA, Frontend, Backend, QA, DevOps)

---

## 1. Bối cảnh và vấn đề nghiệp vụ

### 1.1. Hiện trạng và bài toán cần giải quyết
Trong quy trình phục vụ truyền thống tại một cửa hàng đồ uống, khách hàng phải xếp hàng trực tiếp tại quầy để xem thực đơn, gọi món và chờ thanh toán. Cách vận hành thủ công phát sinh bốn vấn đề chính:
1. **Thời gian chờ lâu vào giờ cao điểm** khi khách vừa chọn món vừa chờ thu ngân tính tiền, thối tiền mặt.
2. **Dễ sai sót thông tin đơn hàng** khi ghi nhận thủ công `Size`, `Topping`, số lượng hoặc áp dụng mã giảm giá.
3. **Thiếu minh bạch trạng thái xử lý**, khách hàng không biết đơn đã được tiếp nhận pha chế hay đã sẵn sàng để nhận tại quầy.
4. **Khó đồng bộ dữ liệu vận hành** giữa thực đơn, tồn kho theo từng kích thước sản phẩm, chương trình khuyến mãi, điểm thưởng và doanh thu ca làm việc.

### 1.2. Định hướng giải pháp
**BrewLite** là hệ thống đặt đồ uống trực tuyến theo hướng **mobile-first / PWA** dành cho mô hình **một cửa hàng, đặt trước và nhận nước tại quầy**, kết hợp hỗ trợ nghiệp vụ bán hàng và pha chế tại quầy.
- **Luồng nghiệp vụ cốt lõi:** `Xem menu → Chọn tùy chọn (Size, Topping) → Giỏ hàng → Đặt đơn → Thanh toán → Theo dõi đơn`.
- **Mục tiêu hệ thống:** Số hóa trọn vẹn luồng đặt món không tiền mặt (qua cổng thanh toán mô phỏng), kiểm soát chặt chẽ tồn kho theo `Size`, ưu đãi/điểm thưởng an toàn giao dịch, hỗ trợ nhân viên xử lý đơn/ca thu ngân và giúp chủ cửa hàng quản trị tập trung.

---

## 2. Ranh giới hệ thống và các bên liên quan 

### 2.1. Ngữ cảnh và ranh giới hệ thống 
Theo mô hình ngữ cảnh, hệ thống **BrewLite** (`Next.js Web/PWA` + `NestJS API` + `PostgreSQL`) giao tiếp với **03 tác nhân người dùng** (`Khách hàng`, `Nhân viên`, `Quản trị viên`) và **02 hệ thống bên ngoài**:
- **Cổng thanh toán (Mock Payment Service / VNPay Sandbox mở rộng):** Xử lý yêu cầu thanh toán không tiền mặt (`WALLET`, `CARD`) và đối soát trạng thái giao dịch.
- **Dịch vụ Email (Gmail SMTP):** Gửi mã OTP xác minh địa chỉ email khi người dùng đăng ký tài khoản.

### 2.2. Bảng phân tích Stakeholder

| Mã | Stakeholder | Phân loại | Vai trò & Mục tiêu (Why? - Role) | Kỳ vọng chính đối với hệ thống |
|---|---|---|---|---|
| ST-01 | **Khách hàng** (`Guest` / `CUSTOMER`) | End user *(Primary Actor)* | Người dùng cuối tìm kiếm đồ uống, tùy chọn món, đặt hàng, thanh toán và nhận nước tại quầy. | Thực đơn trực quan trên di động, tính giá/ưu đãi chính xác, kết quả thanh toán rõ ràng, theo dõi được tiến độ đơn và lịch sử điểm thưởng. |
| ST-02 | **Nhân viên quầy & pha chế** (`STAFF`) | End user *(Operational Actor)* | Vận hành hàng đợi pha chế, mở/đóng ca thu ngân và hỗ trợ tạo đơn trực tiếp tại quầy (POS). | Chỉ nhận đơn đã thanh toán vào hàng đợi pha chế, xem rõ cấu hình món, chuyển trạng thái đơn đúng thứ tự và quản lý thu/hoàn tiền tại quầy chính xác. |
| ST-03 | **Quản lý / Chủ cửa hàng** (`ADMIN`) | System manager / System owner *(Admin Actor)* | Quản trị danh mục kinh doanh, tồn kho, khuyến mãi, nhân sự và giám sát doanh thu cửa hàng. | Quản lý được sản phẩm/`Size`/`Topping`/tồn kho, mã giảm giá, phân quyền tài khoản, đối soát ca thu ngân và xem thống kê vận hành. |
| ST-04 | **Giảng viên / Khách hàng nghiệm thu** | System owner *(Stakeholder nghiệm thu)* | Đánh giá quy trình kỹ nghệ phần mềm từ yêu cầu đến triển khai và nghiệm thu sản phẩm. | Thực hiện tuần tự đủ 5 Task có lịch sử Git/PR rõ ràng; SRS, UML, kiến trúc, code, test và URL production HTTPS đồng bộ theo ma trận truy vết. |
| ST-05 | **Nhóm phát triển** (BA, FE, BE, QA, DevOps) | System developers & maintainers | Phân tích, thiết kế, hiện thực qua 2 Sprint, kiểm thử tự động và vận hành pipeline CI/CD. | Yêu cầu rõ ràng, có tiêu chí chấp nhận kiểm chứng được; kiến trúc tách lớp dễ bảo trì, tự động hóa kiểm thử và triển khai an toàn. |
| ST-06 | **Đơn vị thanh toán & Dịch vụ ngoài** (`Mock Payment` / `VNPay Sandbox`, `Gmail SMTP`) | External stakeholders *(Secondary Actors)* | Cung cấp dịch vụ xác nhận giao dịch thanh toán mô phỏng và gửi thư xác thực OTP. | Giao tiếp qua hợp đồng API xác định, hỗ trợ xử lý idempotent chống giao dịch trùng lặp và không thu thập dữ liệu thẻ/tiền thật. |


### 2.3. Phân quyền vai trò người dùng trong hệ thống 

| Nhóm nghiệp vụ | Khách vãng lai (`Guest`) | Khách đăng nhập (`CUSTOMER`) | Nhân viên (`STAFF`) | Quản trị viên (`ADMIN`) |
|---|:---:|:---:|:---:|:---:|
| Xem thực đơn, tìm kiếm, lọc và xem chi tiết món | Có | Có | Có | Có |
| Thêm/sửa/xóa giỏ hàng cá nhân | Có *(local)* | Có *(đồng bộ)* | Không | Không |
| Đặt đơn online, áp mã giảm giá/điểm, thanh toán & hủy trước pha chế | Không | Có | Không | Không |
| Tiếp nhận đơn `PAID` và cập nhật trạng thái pha chế (`PREPARING → READY → COMPLETED`) | Không | Không | Có | Không *(chỉ giám sát)* |
| Mở/đóng ca thu ngân (`CashShift`), tạo đơn POS (`CASH`/QR) & hoàn tiền quầy | Không | Không | Có | Không *(đối soát cuối ca)* |
| Quản lý danh mục, sản phẩm, tồn kho `Size`, `Topping`, khuyến mãi, tài khoản & thống kê | Không | Không | Không | Có |

---

## 3. Phạm vi sản phẩm 
| Phân vùng phạm vi | Nhóm hạng mục | Nội dung chi tiết |
|---|---|---|
| **In-scope (MVP cốt lõi — Sprint 1 & Sprint 2)** | **Khách hàng (`CUSTOMER_APP`)** | Xem thực đơn, tìm kiếm/lọc theo danh mục, xem chi tiết món; chọn `Size`, `Topping`, số lượng; quản lý giỏ hàng (tự động gộp giỏ local sau đăng nhập); đăng ký email + mật khẩu `bcrypt` xác minh qua **Gmail OTP**; đăng nhập/đăng xuất với **JWT HttpOnly Cookie & Refresh Session**; áp dụng tối đa 01 mã khuyến mãi (`Promotion`) và điểm thưởng (`Loyalty`); tạo đơn `PENDING` giữ chỗ tồn kho trong 10 phút; thanh toán **Ví/Thẻ mô phỏng** (`WALLET`/`CARD`), hỗ trợ thử lại khi `PAYMENT_FAILED` hoặc đối soát khi `UNKNOWN`; hủy đơn (kèm hoàn tiền mô phỏng nếu đơn `PAID` chưa sang `PREPARING`); xem xác nhận đơn, thông báo in-app, theo dõi trạng thái và lịch sử đơn/điểm thưởng. |
| **In-scope (Vận hành & Quản trị cửa hàng)** | **Nhân viên (`STAFF`) & Quản lý (`ADMIN`)** | **STAFF:** Duyệt hàng đợi đơn đã thanh toán theo thứ tự `PAID → PREPARING → READY → COMPLETED`; mở/đóng ca thu ngân (`CashShift`), tạo đơn bán tại quầy (`COUNTER_POS` thu tiền mặt `CASH`/QR) và hoàn tiền tại quầy trước pha chế.<br>**ADMIN:** CRUD danh mục, sản phẩm, `Topping`, phân bổ tồn kho theo `Size`; quản lý mã khuyến mãi; quản lý người dùng & phân quyền (thu hồi phiên khi khóa/đổi quyền); đối soát ca thu ngân (`RECONCILED`), giám sát đơn hàng và xem thống kê doanh thu/trạng thái đơn/top món bán chạy. |
| **In-scope (Kỹ thuật, Kiểm thử & Triển khai)** | **Hạ tầng & Quy trình CNPM** | Monorepo gồm `apps/web` (`Next.js` mobile-first/PWA) và `apps/api` (`NestJS` + `Prisma` + `PostgreSQL`); Unit, Integration và E2E test; `docker-compose.yml` chạy local; GitHub Actions **CI** (chặn merge PR lỗi) và **CD** (build image đẩy lên **GHCR**, deploy lên **AWS EC2** sau **Nginx HTTPS**, chạy migration an toàn, `/api/health`, smoke test và cơ chế rollback). |
| **Phạm vi mở rộng (Extended Scope)** | **Tính năng tích hợp mở rộng / Tương lai** | Cổng **VNPay Sandbox** (đã tích hợp sẵn dưới dạng tính năng mở rộng bật qua cờ cấu hình); định hướng tương lai sau đồ án: quản lý chuỗi đa chi nhánh, thông báo đẩy Web Push/SMS, gợi ý đồ uống cá nhân hóa, quản lý định mức công thức nguyên vật liệu và hóa đơn điện tử. |
| **Out-of-scope (Ngoài phạm vi — Không thực hiện)** | **Các nội dung loại trừ khỏi Assignment 2** | **1.** Giao hàng tận nơi (Delivery), tính phí vận chuyển và định vị tài xế.<br>**2.** Thu tiền thật qua ngân hàng/ví điện tử thương mại hoặc thu thập, lưu trữ thông tin thẻ thật (số thẻ PAN, CVV).<br>**3.** Hoàn tiền tự động qua cổng thanh toán bên ngoài (VNPay).<br>**4.** Hệ thống kế toán/ERP, quản lý kho nguyên vật liệu thô (gram cà phê, ml sữa) và dịch vụ SMS OTP thương mại.<br>**5.** Cam kết SLA thương mại 24/7 (môi trường triển khai là *production demo* phục vụ nghiệm thu môn học). |

---

## 4. Giả định và Ràng buộc

### 4.1. Bảng giả định 

| Mã | Nội dung giả định | Hệ quả nếu giả định thay đổi |
|---|---|---|
| AS-01 | Hệ thống phục vụ duy nhất 01 cửa hàng vật lý; toàn bộ đơn hàng đều nhận trực tiếp tại quầy. | Cần bổ sung nghiệp vụ chọn chi nhánh, địa chỉ giao hàng và tính phí vận chuyển. |
| AS-02 | Khách vãng lai được xem thực đơn và thao tác giỏ hàng tạm, nhưng bắt buộc đăng nhập tài khoản `CUSTOMER` đã xác minh email trước khi đặt đơn. | Cần thay đổi luồng xác thực tại bước Checkout và cơ chế định danh chủ sở hữu đơn. |
| AS-03 | Mỗi kích thước sản phẩm (`Size.id`) đóng vai trò là một đơn vị tồn kho (SKU) độc lập gồm `onHand` và `reserved`; `Size` có `onHand = NULL` được hiểu là chưa phân bổ tồn kho và chưa mở bán. | Cần thiết kế lại mô hình kiểm kê kho nếu tính tồn theo công thức nguyên liệu pha chế. |
| AS-04 | Dữ liệu giá bán và tồn kho trên Backend là nguồn chuẩn duy nhất; mỗi đơn hàng lưu lại bản chụp giá (`unitPrice`, `lineTotal`) và tên tùy chọn tại thời điểm tạo đơn. | Thay đổi giá trên menu có thể làm sai lệch lịch sử tổng tiền các đơn hàng cũ. |
| AS-05 | Luồng nghiệm thu chính sử dụng **Ví/Thẻ mô phỏng** (`ENABLE_MOCK_PAYMENTS=true`) cho phép cấu hình kết quả `SUCCESS`, `FAILED`, `UNKNOWN` từ phía server mà không thu tiền thật. | Phụ thuộc vào tài khoản sandbox bên thứ ba khi chạy kiểm thử tự động E2E/CI. |
| AS-06 | Mỗi đơn đặt qua ứng dụng khách hàng được áp dụng tối đa 01 mã khuyến mãi (giảm tối đa 50% tạm tính) kết hợp quy đổi điểm thưởng hợp lệ (`1 điểm = 100 VND`, tối đa 50% số tiền sau mã giảm); đơn POS tại quầy không áp dụng mã giảm giá/điểm thưởng. | Thay đổi công thức tính `finalTotal` và quy tắc giữ chỗ ưu đãi. |
| AS-07 | Đơn hàng ở trạng thái `PAID` chỉ được phép hủy và hoàn tiền mô phỏng (`Refund`) khi nhân viên **chưa** chuyển đơn sang trạng thái `PREPARING`. | Phát sinh quy trình xử lý hủy món đang pha chế và khấu hao nguyên liệu. |

### 4.2. Bảng ràng buộc

| Mã | Loại ràng buộc | Nội dung ràng buộc bắt buộc | Cơ sở tham chiếu |
|---|---|---|---|
| CO-01 | Công nghệ | Bắt buộc sử dụng **Next.js** (mobile-first/PWA) cho Frontend, **NestJS** cho Backend, **PostgreSQL** cho CSDL và đóng gói với **Docker**. | Assignment 2 §1.1 |
| CO-02 | Quy trình | Năm Task (Task 1 → Task 5) phải thực hiện tuần tự, lưu vết lịch sử commit/Pull Request rõ ràng và liên kết đầy đủ trong `docs/traceability-matrix.md`. | Assignment 2 §1, §6.2, §8 |
| CO-03 | Nghiệp vụ & Mô hình | Đảm bảo luồng tối thiểu `Xem menu → Chọn tùy chọn → Giỏ hàng → Đặt đơn → Thanh toán → Theo dõi đơn`; sơ đồ UML phải xử lý các nhánh hết hàng, chưa đăng nhập, thanh toán thất bại, JWT, transaction và idempotency. | Assignment 2 §1.1, §4 |
| CO-04 | Chất lượng & CI | Mọi Pull Request phải vượt qua pipeline tự động `Install → Lint → Unit Test → Build` trước khi merge; hệ thống phải có đủ Unit, Integration và E2E test. | Assignment 2 §6.4, §7.2 |
| CO-05 | Triển khai & Bảo mật | Môi trường production phải truy cập qua **HTTPS**, có health check endpoint, CSDL không mở public trực tiếp ra Internet, không lộ secret (`.env`, khóa bí mật) trong mã nguồn hoặc log CI/CD, có quy trình rollback khi lỗi. | Assignment 2 §7.3, §7.4 |

---

## 5. Rủi ro ban đầu và biện pháp giảm thiểu

| Mã | Rủi ro nhận diện | Mức độ (Khả năng / Tác động) | Biện pháp kiểm soát trong thiết kế & triển khai |
|---|---|---|---|
| RK-01 | **Bán vượt tồn kho (Overselling)** khi nhiều khách hàng cùng đặt một `Size` sản phẩm sắp hết hàng tại cùng thời điểm. | Trung bình / Cao | Kiểm tra `available = onHand - reserved` và tạo bản ghi giữ chỗ (`OrderReservation`) bên trong Database Transaction nguyên tử có kiểm soát phiên bản (`version`). |
| RK-02 | **Tạo đơn hoặc thanh toán trùng lặp** do mạng chậm khiến client gửi lại request hoặc callback lặp. | Cao / Cao | Bắt buộc sử dụng `Idempotency-Key` (lưu `IdempotencyRecord` kèm băm payload) cho thao tác tạo đơn/thanh toán/hủy đơn và khử trùng sự kiện thanh toán (`PaymentEvent`). |
| RK-03 | **Mất đồng bộ trạng thái khi cổng thanh toán phản hồi chậm / timeout (`UNKNOWN`).** | Trung bình / Cao | Tách biệt trạng thái `Payment` (`PENDING`, `SUCCESS`, `FAILED`, `UNKNOWN`) với `Order`; khóa tạo lượt thanh toán mới khi đang `UNKNOWN` cho đến khi truy vấn đối soát xong; đơn quá hạn 10 phút tự giải phóng giữ chỗ nếu chưa thanh toán. |
| RK-04 | **Sai lệch quyền hạn hoặc truy cập chéo đơn hàng của người dùng khác (IDOR).** | Trung bình / Cao | Xác thực JWT qua HttpOnly Cookie, kiểm tra RBAC (`CUSTOMER`, `STAFF`, `ADMIN`) và kiểm tra quyền sở hữu (`userId`) tại tầng Backend API; tự thu hồi `AuthSession` khi khóa tài khoản hoặc đổi quyền. |
| RK-05 | **Mâu thuẫn giữa SRS, sơ đồ UML và mã nguồn** qua hai Sprint phát triển. | Trung bình / Trung bình | Định danh duy nhất mọi yêu cầu (`FR-xx`, `NFR-xx`), quản lý thay đổi tài liệu qua Pull Request và kiểm tra chéo bằng `docs/traceability-matrix.md`. |
| RK-06 | **Sự cố khi triển khai hoặc chạy migration trên môi trường Production.** | Thấp / Cao | Sử dụng `prisma migrate deploy` an toàn, kiểm tra `/api/health` và smoke test tự động sau deploy; giữ cơ chế rollback lại Docker image phiên bản trước khi deploy thất bại trước bước đổi schema. |