# Demo Day 2026 Voting

Trang bình chọn song ngữ Việt–Anh cho **phần trình bày xuất sắc nhất tại UII Demo Day 2026**. Khách mở link, điền họ tên và đơn vị, chọn nhóm tham gia rồi bình chọn cho một dự án. Ban tổ chức dùng trang quản trị để mở hoặc đóng bình chọn và xem kết quả.

Giao diện, banner và mã SQL thiết lập nằm trong một file HTML. GitHub Pages phục vụ trang web. Supabase lưu cấu hình, nhận phiếu và xác thực tài khoản quản trị.

## Chạy thử trên máy

1. Mở `DemoDay_2026_Voting.html` bằng Chrome hoặc Edge. Nếu file đã được đổi tên để triển khai, mở `index.html`.
2. Khi hai giá trị trong `window.VOTE_CONFIG` còn trống, trang chạy ở chế độ thử. Chọn **Quản trị → Vào quản trị bản thử → Mở bình chọn**, sau đó quay lại **Trang bình chọn** để thử gửi phiếu.
3. Bản thử có 12 dự án và phiếu minh họa. Thay đổi trong bản thử chỉ lưu trên trình duyệt đang dùng. Bạn cần chỉnh lại tên, logo và câu hỏi trong chế độ thật sau khi kết nối Supabase.

## Thiết lập Supabase

1. Tạo project mới trong [Supabase Dashboard](https://supabase.com/dashboard).
2. Trong bản HTML chạy thử, chọn **Quản trị → Hướng dẫn kết nối → Tải SQL thiết lập**. Mở file SQL vừa tải, sao chép toàn bộ nội dung vào **SQL Editor → New query** của project Supabase và chạy một lần. Trong **Table Editor**, kiểm tra có ba bảng `dd_settings`, `dd_votes` và `dd_admins`. Bảng `dd_votes` bắt đầu với 0 phiếu.
3. Vào **Authentication → Users → Add user → Create new user** để tạo tài khoản email và mật khẩu cho BTC. Chọn **Auto Confirm User** nếu tùy chọn này xuất hiện, rồi sao chép **User UID** của tài khoản.
4. Vào **Table Editor → dd_admins → Insert row**, dán UID vào cột `user_id` rồi lưu. Tài khoản này có thể đăng nhập ở mục **Quản trị** trên trang bình chọn.
5. Lấy **Project URL** và **Publishable key** trong hộp thoại **Connect** của project. Mở HTML bằng VS Code hoặc trình soạn thảo văn bản, tìm `vote-config` và sửa đoạn có sẵn như dưới đây.

```html
<script id="vote-config">
window.VOTE_CONFIG = {
  supabaseUrl: "https://YOUR_PROJECT.supabase.co",
  supabaseKey: "sb_publishable_YOUR_KEY"
};
</script>
```

Chỉ dùng khóa **publishable** ở đoạn cấu hình này. Giữ riêng khóa `sb_secret_`, khóa `service_role` và mật khẩu quản trị. Xem [tài liệu API keys của Supabase](https://supabase.com/docs/guides/getting-started/api-keys).

## Đưa lên GitHub Pages

1. Lưu bản HTML đã cấu hình thành `index.html`. Đưa `index.html` và `README.md` lên thư mục gốc của nhánh `main`.
2. Trong repository, vào **Settings → Pages → Build and deployment**. Chọn **Deploy from a branch**, chọn `main` và `/(root)`, rồi bấm **Save**. Xem [hướng dẫn cấu hình nguồn xuất bản của GitHub Pages](https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site).
3. Mở link trong **Settings → Pages**. Trang khách sẽ hiện **Chưa mở bình chọn**. Vào **Quản trị**, đăng nhập bằng tài khoản Supabase đã cấp quyền và kiểm tra dashboard hiển thị 0 phiếu.

## Vận hành trong ngày sự kiện

| Thời điểm | Ban tổ chức | Người tham dự |
| --- | --- | --- |
| Trước phần pitching | Vào **Quản trị → Cài đặt** để cập nhật câu hỏi, tên và logo dự án. Chốt danh sách trước phiếu thật đầu tiên vì hệ thống sẽ khóa cấu trúc câu hỏi và danh sách dự án khi đã có phiếu. | Mở link và xem thông báo **Chưa mở bình chọn**. |
| Sau khi các dự án trình bày xong | Bấm **Mở bình chọn** khi MC công bố. | Điền thông tin, chọn nhóm, chọn một dự án và xác nhận gửi. |
| Hết thời gian | Bấm **Đóng bình chọn**. Xem **Tổng quan** để lấy bảng xếp hạng và **Chi tiết phiếu** để đối chiếu. BTC chuyển kết quả cho MC công bố. | Trang ẩn form và dừng nhận phiếu. |

Có thể kiểm tra toàn bộ luồng trên một project Supabase thử riêng trước sự kiện để dữ liệu thử và quyền chỉnh sửa cấu hình của project chính được giữ độc lập. Dùng hai trình duyệt để kiểm tra việc đồng bộ trạng thái, ghi nhận phiếu và hiển thị kết quả.

## Cách tính kết quả

| Nhóm người bầu | Tỷ trọng |
| --- | ---: |
| Nhà đầu tư | 50% |
| Startup / Khách tham dự | 30% |
| Nhân sự UII | 20% |

Điểm của dự án bằng tổng điểm đóng góp của ba nhóm. Trong mỗi nhóm, hệ thống chia số phiếu của dự án cho tổng số phiếu của nhóm rồi nhân với tỷ trọng. Nhóm chưa có phiếu đóng góp 0 điểm và dashboard đánh dấu kết quả tạm tính. Các dự án bằng điểm được xếp đồng hạng.

Người bầu tự chọn nhóm. Trang giới hạn lượt gửi theo dấu nhận diện lưu trong trình duyệt trên tên miền đang dùng. Đổi trình duyệt, thiết bị hoặc xóa dữ liệu trình duyệt có thể tạo lượt gửi mới. Chỉ tài khoản được cấp quyền quản trị mới xem được dữ liệu phiếu trong Supabase.
