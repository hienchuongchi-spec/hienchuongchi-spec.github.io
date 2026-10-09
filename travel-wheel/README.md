# Vòng quay du lịch — Travel Wheel

Website: **https://hienchuongchi-spec.github.io/travel-wheel/**

Bốn ô, cùng xác suất 25%:
1. München
2. Düsseldorf & Köln
3. Berlin & Prague
4. Hamburg

## Đưa lên GitHub Pages

Mã nằm trong thư mục `travel-wheel/` của repo `hienchuongchi-spec.github.io`. Không sửa trang chủ cũ. Nếu Pages chưa bật: GitHub repo → **Settings → Pages → Build and deployment → Deploy from a branch → main / (root) → Save**.

## Bắt buộc: bật đăng nhập và lưu kết quả bằng Firebase (Google)

**Chưa cấu hình Firebase, website ở chế độ xem thử và không lưu lượt quay.**

1. Vào https://console.firebase.google.com/ → **Add project** → tạo một project.
2. **Project overview → Add app → Web (</>)**, đăng ký app, copy các giá trị `firebaseConfig`.
3. Trong GitHub, mở file **`travel-wheel/firebase-config.js`**, nhấn ✏️ Edit, điền các giá trị thật (`apiKey`, `authDomain`, `projectId`, `appId`, `messagingSenderId`), rồi **Commit changes**. Đây là cấu hình public dành cho web, không phải private key/service account key.
4. Firebase Console → **Build → Authentication → Get started → Sign-in method → Google → Enable** và chọn email hỗ trợ.
5. Firebase Console → **Authentication → Settings → Authorized domains → Add domain** → `hienchuongchi-spec.github.io` (không điền `https://` hoặc `/travel-wheel`).
6. Firebase Console → **Build → Firestore Database → Create database**. Chọn vị trí dữ liệu thích hợp và **production mode**.
7. Firebase Console → **Firestore Database → Rules**. Copy TOÀN BỘ nội dung file **`travel-wheel/firestore.rules`** vào và nhấn **Publish** (không được bỏ bước này).
8. Tải lại website, đăng nhập Google và quay thử với một tài khoản. Đăng nhập lại bằng tài khoản đó: không thể quay lần thứ hai.

## Xem kết quả đã lưu

Firebase Console → Firestore Database → Data → collection **`travelSpins`**.

Mỗi tài khoản Google có một document ID bằng Firebase UID; dữ liệu gồm `displayName`, `email`, `resultIndex`, `result`, `createdAt`. Chỉ chủ Firebase project xem được toàn bộ kết quả bằng Console; người tham gia chỉ xem được kết quả của chính họ. Không công khai email của người tham gia.

## Mức độ bảo vệ

- Firestore Security Rules chỉ cho phép **create**, không cho update/delete, và giới hạn document ID bằng UID tài khoản đăng nhập. Đóng trình duyệt, đổi thiết bị hoặc xóa cookie không giúp tài khoản đó quay thêm lần nữa.
- **Không thể đảm bảo 1 người = 1 tài khoản** nếu người đó có nhiều tài khoản Google.
- Lựa chọn ngẫu nhiên hiện được tạo bằng `crypto.getRandomValues()` phía trình duyệt và **được lưu trước khi chạy hiệu ứng**. Người cố tình can thiệp mã nguồn trình duyệt có thể tác động kết quả trước khi lưu. Nếu quay giải thưởng cần chống gian lận về *kết quả*, hãy chuyển lựa chọn ngẫu nhiên sang một backend tin cậy (ví dụ Firebase Cloud Functions) thay vì chỉ dùng Firestore rules.
- Không xóa dữ liệu trong collection nếu muốn giữ lịch sử một lượt/quy tắc khóa.
- Nên thông báo người tham gia rằng tên hiển thị, email và kết quả sẽ được lưu trong Firebase.

## Sửa tên ô quay

Mở `travel-wheel/index.html`, tìm mảng `destinations`. Nếu đổi tên, phải cập nhật nhãn trên vòng quay và các giá trị được cho phép trong `firestore.rules` để khớp.
