# BrewLite - Bối cảnh, stakeholder và phạm vi hệ thống

- **Mã tài liệu:** BL-REQ-01 (Assignment 2 - Task 1.1)
- **Phiên bản:** 1.0
- **Đối tượng đọc:** khách hàng/chủ cửa hàng, giảng viên nghiệm thu, nhóm phát triển

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

| Mã | Stakeholder | Phân loại | Vai trò & Mục tiêu | Kỳ vọng chính đối với hệ thống |
|---|---|---|---|---|
| ST-01 | **Khách hàng**| End user *(Primary Actor)* | Người dùng cuối tìm kiếm đồ uống, tùy chọn món, đặt hàng, thanh toán và nhận nước tại quầy. | Thực đơn trực quan trên di động, tính giá/ưu đãi chính xác, kết quả thanh toán rõ ràng, theo dõi được tiến độ đơn và lịch sử điểm thưởng. |
| ST-02 | **Nhân viên** | End user *(Operational Actor)* | Vận hành hàng đợi pha chế, mở/đóng ca thu ngân và hỗ trợ tạo đơn trực tiếp tại quầy (POS). | Chỉ nhận đơn đã thanh toán vào hàng đợi pha chế, xem rõ cấu hình món, chuyển trạng thái đơn đúng thứ tự và quản lý thu/hoàn tiền tại quầy chính xác. |
| ST-03 | **Quản trị viên** | System manager *(Admin Actor)* | Quản trị danh mục kinh doanh, tồn kho, khuyến mãi, nhân sự và giám sát doanh thu cửa hàng. | Quản lý được sản phẩm/`Size`/`Topping`/tồn kho, mã giảm giá, phân quyền tài khoản, đối soát ca thu ngân và xem thống kê vận hành. |
| ST-04 | **Giảng viên / Khách hàng nghiệm thu** | System owner *(Stakeholder nghiệm thu)* | Đánh giá quy trình kỹ nghệ phần mềm từ yêu cầu đến triển khai và nghiệm thu sản phẩm. | Thực hiện tuần tự đủ 5 Task có lịch sử Git/PR rõ ràng; SRS, UML, kiến trúc, code, test và URL production HTTPS đồng bộ theo ma trận truy vết. |
| ST-05 | **Nhóm phát triển** | System developers & maintainers | Phân tích, thiết kế, hiện thực qua 2 Sprint, kiểm thử tự động và vận hành pipeline CI/CD. | Yêu cầu rõ ràng, có tiêu chí chấp nhận kiểm chứng được; kiến trúc tách lớp dễ bảo trì, tự động hóa kiểm thử và triển khai an toàn. |
| ST-06 | **Đơn vị thanh toán & Dịch vụ ngoài** (`Mock Payment` / `VNPay Sandbox`, `Gmail SMTP`) | External stakeholders *(Secondary Actors)* | Cung cấp dịch vụ xác nhận giao dịch thanh toán mô phỏng và gửi thư xác thực OTP. | Giao tiếp qua hợp đồng API xác định, hỗ trợ xử lý idempotent chống giao dịch trùng lặp và không thu thập dữ liệu thẻ/tiền thật. |


### 2.3. Phân quyền vai trò người dùng trong hệ thống
| Nhóm nghiệp vụ | Khách vãng lai (`Guest`) | Khách đăng nhập (`CUSTOMER`) | Nhân viên (`STAFF`) | Quản trị viên (`ADMIN`) |
|---|:---:|:---:|:---:|:---:|
| Xem thực đơn, tìm kiếm, lọc và xem chi tiết món | Có | Có | Có | Có |
| Thao tác giỏ hàng trên giao diện | Có *(giỏ tạm local)* | Có *(đồng bộ PostgreSQL)* | Có *(giỏ POS tạm tại quầy, không lưu DB)* | Không *(ẩn thao tác mua)* |
| Đặt đơn online, áp mã giảm giá/điểm, thanh toán & hủy đơn cá nhân trước pha chế | Không | Có *(đơn của chính mình)* | Không | Không |
| Xem lịch sử đơn cá nhân, sổ điểm thưởng (`Loyalty`) & thông báo (`Notification`) | Không | Có *(của chính mình)* | Không | Không |
| Xem hàng đợi đơn đã thanh toán & lịch sử đơn trong ngày | Không | Không | Có | Chỉ xem *(giám sát)* |
| Cập nhật tiến độ pha chế (`PAID → PREPARING → READY → COMPLETED`) | Không | Không | Có | Không |
| Mở/đóng ca thu ngân (`CashShift`), tạo đơn POS (`CASH`/QR) & hoàn tiền tại quầy | Không | Không | Có | Không *(chỉ xem & đối soát cuối ca)* |
| Đối soát ca thu ngân đã đóng (`CLOSED → RECONCILED`), giám sát toàn bộ đơn & thống kê | Không | Không | Không | Có |
| Quản lý danh mục, sản phẩm, tồn kho `Size`, `Topping`, mã khuyến mãi & khóa/gán quyền (`CUSTOMER ↔ STAFF`) | Không | Không | Không | Có *(không tự sửa/khóa `ADMIN`)* |

---

## 3. Phạm vi sản phẩm

### 3.1. Phạm vi sản phẩm MVP
Phạm vi MVP của BrewLite bao gồm các chức năng cần thiết để thực hiện đầy đủ quy trình đặt đồ uống tại một cửa hàng, từ lựa chọn sản phẩm đến thanh toán và nhận đồ uống.

**a) Chức năng dành cho khách hàng**
- Xem menu, tìm kiếm, lọc sản phẩm và xem thông tin chi tiết đồ uống.
- Chọn kích thước (`Size`), `Topping` và số lượng sản phẩm.
- Thêm, cập nhật hoặc xóa sản phẩm trong giỏ hàng.
- Đăng ký tài khoản bằng email và mật khẩu, xác minh email thông qua mã OTP được gửi bằng Gmail.
- Đăng nhập, đăng xuất và duy trì phiên làm việc bằng cơ chế JWT access token và refresh token.
- Xác nhận mua hàng, áp dụng mã giảm giá và điểm thưởng hợp lệ.
- Tạo đơn hàng, thanh toán bằng ví hoặc thẻ mô phỏng; xử lý trường hợp thanh toán thành công, thất bại hoặc chưa xác định được kết quả; hỗ trợ hủy đơn trước khi pha chế.
- Theo dõi trạng thái đơn hàng và xem lịch sử đặt hàng.

**b) Chức năng dành cho nhân viên**
- Đăng nhập theo quyền `STAFF`.
- Xem danh sách các đơn hàng đã thanh toán và chi tiết đồ uống.
- Cập nhật trạng thái đơn hàng theo trình tự: `PAID → PREPARING → READY → COMPLETED`.
- Tạo và thu tiền đơn tại quầy (tiền mặt hoặc chuyển khoản QR, không áp dụng mã giảm giá và điểm thưởng), mở/đóng ca thu ngân, hoàn tiền đơn quầy trước khi pha chế và tra cứu lịch sử đơn trong ngày.
- Ghi nhận lịch sử thay đổi trạng thái để phục vụ theo dõi và kiểm tra.

**c) Chức năng dành cho chủ cửa hàng**
- Quản lý danh mục sản phẩm, kích thước, topping và giá bán.
- Quản lý tình trạng khả dụng và tồn kho theo từng kích thước sản phẩm (SKU).
- Tạo, cập nhật và vô hiệu hóa các chương trình khuyến mãi.
- Quản lý tài khoản người dùng và phân quyền truy cập (tự động thu hồi phiên đăng nhập khi đổi vai trò hoặc vô hiệu hóa tài khoản).
- Giám sát thông tin đơn hàng (chỉ đọc), đối soát quỹ tiền mặt cuối ca và xem báo cáo thống kê (doanh thu theo kênh, tỷ lệ trạng thái đơn và top món bán chạy).

**d) Phạm vi kỹ thuật**
Hệ thống được xây dựng theo kiến trúc ứng dụng web, sử dụng các công nghệ sau:
- **Next.js:** Phát triển giao diện frontend theo hướng mobile-first và Progressive Web App (PWA).
- **NestJS:** Xây dựng backend API, xử lý nghiệp vụ và xác thực, phân quyền người dùng.
- **PostgreSQL:** Quản lý dữ liệu sản phẩm, người dùng, giỏ hàng, đơn hàng, giao dịch thanh toán và điểm thưởng.
- **Docker Compose:** Đóng gói và vận hành các dịch vụ trong môi trường phát triển.
- **Git và GitHub:** Quản lý mã nguồn, tài liệu, lịch sử thay đổi và phối hợp phát triển.
- **CI/CD:** Tự động hóa kiểm tra chất lượng mã nguồn, xây dựng và triển khai ứng dụng.

Hệ thống được triển khai trên môi trường production demo có HTTPS, hỗ trợ kiểm tra trạng thái dịch vụ và phục vụ nghiệm thu theo yêu cầu Assignment 2.

### 3.2. Phạm vi mở rộng và ngoài phạm vi

**a) Phạm vi mở rộng**
Các chức năng mở rộng hoặc có thể được phát triển trong những phiên bản tiếp theo bao gồm:
- Tích hợp cổng thanh toán trực tuyến VNPay Sandbox thử nghiệm.
- Phân tích và báo cáo doanh thu nâng cao.
- Thông báo đẩy về trạng thái đơn hàng.
- Quản lý nhiều cửa hàng hoặc nhiều chi nhánh.
- Gợi ý đồ uống dựa trên hành vi và sở thích khách hàng.
- Quản lý định mức nguyên vật liệu và tích hợp thiết bị bán hàng.
- Tích hợp hóa đơn điện tử và các chức năng quản trị mở rộng.

**b) Ngoài phạm vi**
Các chức năng sau không thuộc phạm vi triển khai của Assignment 2:
- Thanh toán bằng tiền thật hoặc tích hợp cổng ngân hàng, ví điện tử đang hoạt động thực tế.
- Thu thập hoặc lưu trữ thông tin thẻ thanh toán như số thẻ và CVV.
- Hoàn tiền tự động qua cổng thanh toán bên ngoài.
- Giao hàng tận nơi và theo dõi vị trí nhân viên giao hàng.
- Quản lý nhiều chi nhánh trong phiên bản MVP.
- Hệ thống kế toán, ERP và quản lý nguyên vật liệu chuyên sâu.
- Gửi SMS OTP thật và cung cấp dịch vụ thương mại với cam kết hoạt động liên tục 24/7.

---

## 4. Giả định và Ràng buộc

Các giả định được sử dụng làm cơ sở để xác định phạm vi nghiệp vụ. Ràng buộc là những điều kiện mà hệ thống phải tuân thủ trong quá trình thiết kế và triển khai.

### 4.1. Giả định

| ID | Giả định |
|---|---|
| AS-01 | BrewLite phục vụ một cửa hàng, khách hàng nhận đồ uống tại quầy. |
| AS-02 | Khách vãng lai được xem menu và tạo giỏ hàng nhưng phải đăng nhập trước khi đặt và thanh toán đơn. |
| AS-03 | Cổng thanh toán nghiệm thu chính hoạt động ở chế độ mô phỏng, hỗ trợ kết quả thành công, thất bại và truy vấn trạng thái giao dịch. |
| AS-04 | Mỗi kích thước sản phẩm được quản lý như một SKU thông qua `Size.id`; tồn kho chưa phân bổ không được phép bán. |
| AS-05 | Giá bán và tồn kho do backend quản lý; đơn hàng lưu giá và tùy chọn tại thời điểm đặt. |
| AS-06 | Mỗi đơn hàng online được áp dụng tối đa một mã giảm giá kết hợp với điểm thưởng hợp lệ (`1 điểm = 100 đồng`), tự động giới hạn điểm sử dụng để giữ mức thanh toán tối thiểu `10.000 đồng`. |
| AS-07 | Hỗ trợ hủy và hoàn tiền mô phỏng (với đơn ví/thẻ mô phỏng) hoặc hoàn tiền tại quầy khi đơn ở trạng thái `PAID` và chưa chuyển sang `PREPARING`; không hỗ trợ hoàn tiền tự động đối với giao dịch VNPay trực tuyến. |
| AS-08 | Môi trường production demo sử dụng dữ liệu thử nghiệm, không sử dụng dữ liệu thanh toán nhạy cảm thật. |

### 4.2. Ràng buộc

| ID | Ràng buộc |
|---|---|
| CO-01 | Bắt buộc sử dụng Next.js theo hướng mobile-first/PWA, NestJS và PostgreSQL. |
| CO-02 | Đảm bảo luồng nghiệp vụ: Xem menu, chọn tùy chọn, giỏ hàng, đặt đơn, thanh toán và theo dõi đơn. |
| CO-03 | Năm Task được thực hiện tuần tự, có lịch sử thay đổi và khả năng truy vết trên kho mã nguồn. |
| CO-04 | Các sơ đồ UML phải thể hiện luồng nghiệp vụ chính, các trường hợp ngoại lệ, xác thực và xử lý giao dịch. |
| CO-05 | Quy trình CI thực hiện các bước `Install`, `Lint`, `Unit Test` và `Build`; hệ thống có tối thiểu một kiểm thử End-to-End. |
| CO-06 | Môi trường triển khai có HTTPS, health check, bảo vệ secret, cơ sở dữ liệu không public trực tiếp và hỗ trợ rollback. |
| CO-07 | Thanh toán chỉ sử dụng cổng mô phỏng (hoặc Sandbox thử nghiệm), không thực hiện giao dịch tiền thật. |
| CO-08 | Các chính sách nghiệp vụ được truy vết và kiểm soát thay đổi thông qua issue và pull request. |

---

## 5. Rủi ro ban đầu và biện pháp giảm thiểu

| Mã | Rủi ro nhận diện | Mức độ (khả năng / tác động) | Biện pháp kiểm soát trong thiết kế & triển khai |
|---|---|---|---|
| RK-01 | **Bán vượt tồn kho (Overselling)** khi nhiều khách hàng cùng đặt một `Size` sản phẩm sắp hết hàng tại cùng thời điểm. | Trung bình / Cao | Kiểm tra `available = onHand - reserved` và giữ chỗ (`OrderReservation`) trong giao dịch `Serializable` kết hợp kiểm soát phiên bản (`Size.version`). |
| RK-02 | **Tạo đơn, thanh toán hoặc hoàn tiền trùng lặp** do mạng chậm khiến client gửi lại request hoặc callback lặp. | Cao / Cao | Bắt buộc sử dụng `Idempotency-Key` (`IdempotencyRecord` kèm băm payload) cho tạo đơn/thanh toán/hủy đơn và khử trùng sự kiện thanh toán (`PaymentEvent`). |
| RK-03 | **Mất đồng bộ trạng thái khi cổng thanh toán phản hồi chậm / timeout (`UNKNOWN`).** | Trung bình / Cao | Tách trạng thái `Payment` với `Order`; chặn tạo lượt thanh toán mới hoặc hủy mù quáng khi đang `UNKNOWN` cho đến khi đối soát xong; đơn hết hạn 10 phút chỉ tự nhả giữ chỗ khi không có giao dịch chờ xác minh. |
| RK-04 | **Sai lệch quyền hạn hoặc truy cập chéo đơn hàng của người dùng khác (IDOR).** | Trung bình / Cao | Xác thực JWT qua `HttpOnly` Cookie, kiểm tra RBAC (`CUSTOMER`, `STAFF`, `ADMIN`) và quyền sở hữu (`userId`) tại Backend API; tự thu hồi `AuthSession` khi khóa tài khoản hoặc đổi quyền. |
| RK-05 | **Xung đột đồng thời khi khách hủy đơn `PAID` đúng lúc `STAFF` nhận pha chế (`PREPARING`) hoặc khách đã tiêu điểm tích lũy.** | Trung bình / Cao | Khóa dòng `Order` (`FOR UPDATE`) kèm kiểm tra `expectedVersion` để chỉ một thao tác thành công; kiểm tra số dư điểm khả dụng trước khi thu hồi `EARN_REVERSAL` nhằm tránh âm điểm. |
| RK-06 | **Tấn công dò mã xác thực hoặc lỗi gửi thư từ dịch vụ ngoài `Gmail SMTP`.** | Trung bình / Trung bình | Lưu mã OTP dạng băm (`codeHash`), áp dụng TTL ngắn, giới hạn số lần nhập sai và tần suất gửi lại (`cooldown`/rate-limit), không ghi OTP hoặc SMTP secret ra log. |
| RK-07 | **Mâu thuẫn giữa SRS, sơ đồ UML và mã nguồn** qua hai Sprint phát triển. | Trung bình / Trung bình | Định danh duy nhất mọi yêu cầu (`FR-xx`, `NFR-xx`), quản lý thay đổi tài liệu qua Pull Request và đối chiếu chéo trong `docs/traceability-matrix.md`. |
| RK-08 | **Sự cố khi triển khai hoặc chạy migration trên môi trường Production.** | Thấp / Cao | Dùng `prisma migrate deploy` an toàn, kiểm tra `/api/health` và smoke test tự động sau deploy; hỗ trợ rollback lại Docker image trước đó nếu bước triển khai thất bại trước migration. |
