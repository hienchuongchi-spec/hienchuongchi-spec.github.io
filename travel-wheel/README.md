# Travel Wheel – Nhập tên và quay

Website: https://hienchuongchi-spec.github.io/travel-wheel/

## Cách dùng
- Người tham gia **nhập tên → Quay ngay → kết quả hiện trên vòng quay**.
- 4 ô, mỗi ô xác suất 25%: München · Düsseldorf & Köln · Berlin & Prague · Hamburg.
- Tên đã quay trên **cùng trình duyệt** sẽ luôn hiện kết quả cũ. Danh sách đã lưu trên thiết bị hiển thị bên dưới.
- **Không cần đăng nhập Google.**

## Trạng thái lưu kết quả
### Sử dụng ngay (không cài đặt)
Trình duyệt lưu kết quả trong `localStorage`, không bị mất chỉ vì tải lại trang. **Chỉ lưu trên máy đó**, không thể tự xem chung giữa nhiều người/máy, và có thể mất khi người dùng xóa dữ liệu trình duyệt. Đây không phải hạn chế chặt chẽ mỗi người một lần.

### Lưu chung cho tất cả người tham gia (Firebase Firestore)
Cấu hình web app Firebase trong `firebase-config.js` đã được điền cho project **reise-3ae64**.

**Chủ project vẫn phải làm 2 bước trong Firebase Console:**
1. Vào https://console.firebase.google.com/project/reise-3ae64/firestore → **Create database**, tạo Firestore nếu chưa có (Production mode).
2. Vào tab **Rules** → thay thế bằng toàn bộ nội dung `travel-wheel/firestore.rules` → **Publish**. Nếu bạn dùng cùng Firestore cho các collection khác, hãy hợp nhất rules thay vì ghi đè.

Sau đó người tham gia chỉ cần nhập tên. Website sẽ kiểm tra document ID SHA-256 của tên chuẩn hóa: lần đầu được tạo; tên đã có kết quả sẽ xem kết quả cũ. Kết quả lưu vào collection `travelNameSpins` (Firestore Console → Data). Firebase không yêu cầu bật Google Sign-in.

**Nếu chưa bật Firestore hoặc Rules bị chặn, website vẫn dùng localStorage** và có thông báo rõ `Lưu trên thiết bị`.

## Giới hạn và cảnh báo
- **Tên không xác thực danh tính**: một người có thể nhập tên khác để quay nhiều lần; hai người trùng tên sẽ dùng cùng kết quả. Không thể ngăn gian lận thực sự chỉ với nhập tên.
- Với Firebase công khai không có đăng nhập, người khác có thể tự gửi yêu cầu tạo bản ghi nếu biết API/cấu trúc dữ liệu và gây phát sinh số lượng bản ghi/billing; rules hạn chế định dạng và chặn sửa/xóa, **không chống spam hoặc đảm bảo vòng quay trung thực**. Hãy theo dõi Firestore usage/thiết lập hạn mức ngân sách; nếu đông người hoặc có giải thưởng giá trị, cần backend tin cậy và cơ chế giới hạn.
- Random hiện được tạo từ `crypto.getRandomValues()` trong trình duyệt trước khi lưu, có thể bị người dùng kỹ thuật tác động. Không dùng cho xổ số/giải thưởng cần chứng minh công bằng.
- Chỉ nhập tên mà bạn đồng ý được lưu, và không dùng thông tin nhạy cảm. Document có tên và kết quả.
- Tên bị chuẩn hóa để so trùng (chuyển chữ thường, gộp khoảng trắng, unicode NFKC). Người tổ chức có thể xem kết quả trong Firestore.

Tất cả dữ liệu cũ ở collection `travelSpins` (nếu có) không bị xóa; website mới dùng collection `travelNameSpins`.
