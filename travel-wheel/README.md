# Vòng quay du lịch — lưu chung tên và kết quả

## Website
https://hienchuongchi-spec.github.io/quay-du-lich/

## Cách sử dụng
Người tham gia nhập tên, bấm **QUAY NGAY**, xem kết quả và danh sách **Kết quả mọi người đã quay** ngay trên web. Có 4 lựa chọn 25%: München · Düsseldorf & Köln · Berlin & Prague · Hamburg.

**Không cần tài khoản Google.** Tuy nhiên, kết quả **chỉ được tạo khi Firestore đang hoạt động** để tránh việc quay xong mà ban tổ chức không thấy.

## BẮT BUỘC bật Firestore để lưu cho mọi người
Dự án Firebase dùng sẵn là `reise-3ae64`, file `firebase-config.js` đã được cấu hình.

1. Mở https://console.firebase.google.com/project/reise-3ae64/firestore
2. Nếu chưa có database: **Create database** → chọn vị trí → **Production mode** → Create.
3. Trong Firestore → **Rules**, dùng TOÀN BỘ file `travel-wheel/firestore.rules` trên GitHub và nhấn **Publish**. Lưu ý nếu trong database có rules/collections khác đang dùng, hãy gộp thay vì ghi đè.
4. Mở lại website. Phải thấy thông báo **Sẵn sàng! Kết quả sẽ được lưu chung vào Firebase.** Nếu hiện **CHƯA BẬT LƯU CHUNG** thì Firestore/rules vẫn chưa hoạt động.

File rules **cho phép xem công khai danh sách 100 kết quả mới nhất** (tên và thành phố) vì chủ website muốn mọi người cùng thấy tên đã quay. Người quản trị xem mọi dữ liệu qua Firebase Console → Firestore Database → Data → `travelNameSpins`.

## Kết quả cũ trước khi bật Firestore
Website phiên bản trước chỉ lưu tên trên máy người chơi (`localStorage`). Sau khi Firestore sẵn sàng, **khi chính người đó mở lại website trên trình duyệt cũ**, trang sẽ cố đồng bộ dữ liệu cũ vào Firestore mà không quay lại. Người chơi khác không thể lấy các kết quả chưa đồng bộ từ một thiết bị xa.

## Giới hạn
- Đây là giới hạn **mỗi tên một lần**, không phải mỗi người một lần (một người có thể nhập tên khác; hai người trùng tên cùng nhận một kết quả).
- Không có đăng nhập nên không thể bảo đảm người chơi không gian lận. Kết quả ngẫu nhiên phía trình duyệt, không phù hợp cho xổ số hoặc giải thưởng có giá trị.
- Vì ai cũng có quyền đọc tên trong danh sách, chỉ dùng tên/nickname mà người tham gia đồng ý công khai, không thêm dữ liệu nhạy cảm.
- Rules chặn sửa/xóa và giới hạn số kết quả đọc trong một truy vấn, nhưng không bảo vệ khỏi spam do không xác thực người dùng. Theo dõi Firestore usage.

Các file web: `quay-du-lich/index.html` và `travel-wheel/index.html`. Cùng dùng cấu hình `firebase-config.js`.