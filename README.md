# Sổ doanh thu S2a-HKD

Trang web một file (`index.html`) để ghi sổ doanh thu bán hàng hóa, dịch vụ theo **mẫu số S2a-HKD**
(Thông tư 152/2025/TT-BTC), xem trước sổ theo tháng, quý hoặc năm, rồi xuất file Excel để in, ký và nộp.

Dữ liệu được lưu vào **một repo GitHub private của bạn**, trong file `data.json`. Mỗi lần lưu là một commit,
nên bạn xem lại được lịch sử sửa.

## Hai repo, hai vai trò

| Repo | Chế độ | Chứa gì |
| --- | --- | --- |
| `so-s2a` (repo này) | public hoặc private | Code của trang (`index.html`). Không chứa dữ liệu. |
| `so-s2a-data` | **bắt buộc private** | `data.json`: thông tin hộ, ngành nghề, các dòng doanh thu. |

Nếu repo dữ liệu đang public, trang sẽ từ chối kết nối, vì khi đó ai cũng đọc được doanh thu và mã số thuế của bạn.

## Cài đặt lần đầu

### 1. Tạo repo dữ liệu
1. Vào https://github.com/new
2. Repository name: `so-s2a-data`, chọn **Private**, tick **Add a README file** để repo có sẵn nhánh `main`.
3. Bấm **Create repository**.

### 2. Tạo token chỉ dùng cho repo đó
1. Vào https://github.com/settings/personal-access-tokens/new (Fine-grained token).
2. **Token name:** `so-s2a`. **Expiration:** tùy bạn, ví dụ 1 năm. Hết hạn thì tạo token mới.
3. **Repository access:** chọn *Only select repositories*, rồi chọn `so-s2a-data`.
4. **Permissions → Repository permissions → Contents:** chọn *Read and write*. Không cần quyền nào khác.
5. Bấm **Generate token** và copy token (bắt đầu bằng `github_pat_`).

### 3. Mở trang
Có hai cách:

- **Mở trực tiếp trên máy:** bấm đúp vào `index.html`. Không cần cài gì.
- **Đưa lên GitHub Pages** để mở được từ mọi máy: push repo này lên GitHub, vào *Settings → Pages*,
  ở *Source* chọn *Deploy from a branch*, nhánh `main`, thư mục `/ (root)`. Trang sẽ có địa chỉ
  `https://<tài-khoản>.github.io/so-s2a/`. GitHub Pages miễn phí với repo public; repo này chỉ có code
  nên để public được.

### 4. Kết nối
Trong trang, mở mục **Lưu trữ trên GitHub** và điền:
- Tài khoản GitHub: tên tài khoản của bạn
- Repo lưu dữ liệu: `so-s2a-data`
- Nhánh: `main`, File dữ liệu: `data.json`
- Token: token vừa tạo

Bấm **Kết nối**. Từ đó mỗi thay đổi sẽ tự lưu lên GitHub sau khoảng 4 giây. Dòng trạng thái góc trên bên phải
cho biết đã lưu hay chưa.

## Chuyển dữ liệu từ bản trên Claude sang
1. Mở trang trên Claude, ở mục **Sao lưu** bấm **Tải bản sao lưu** để lấy file `.json`.
2. Mở trang này, ở mục **Sao lưu** chọn file đó ở ô **Nhập**.
3. Dữ liệu được gộp vào sổ rồi tự lưu lên GitHub.

## Dùng trên nhiều máy
Mỗi máy kết nối một lần bằng cùng repo. Trang luôn giữ một bản trong trình duyệt và đồng bộ với GitHub.
Nếu cả GitHub và máy đang dùng đều có thay đổi chưa đồng bộ, ví dụ sửa trên laptop khi điện thoại chưa kịp lưu,
trang sẽ hiện thông báo và cho bạn chọn:
- **Gộp hai bản:** giữ mọi dòng có ở một trong hai bản.
- **Dùng bản trên GitHub:** bỏ các thay đổi chưa lưu trên máy này.
- **Ghi đè GitHub bằng bản trên máy này.**

## An toàn
- Token chỉ lưu trong trình duyệt (localStorage) của máy bạn dùng, không nằm trong code hay trong repo.
- Token chỉ có quyền với đúng repo `so-s2a-data`. Nếu lộ token, vào trang token trên GitHub và bấm *Revoke*.
- Trên máy dùng chung, bấm **Ngắt kết nối** khi dùng xong để xóa token khỏi trình duyệt.
- Trang chỉ được phép kết nối tới `api.github.com`. Điều này được khóa bằng Content-Security-Policy trong `index.html`.

## Lưu ý về thuế
- Bố cục sổ theo đúng mẫu S2a-HKD trong Thông tư 152/2025/TT-BTC: cột A, B, C, 1; mỗi nhóm ngành có
  *Tổng cộng*, *Thuế GTGT*, *Thuế TNCN*; cuối sổ là tổng thuế phải nộp.
- File Excel xuất ra chỉ có dòng *Tổng cộng* của từng ngành. Không có các dòng thuế GTGT/TNCN và tổng thuế phải nộp.
- Tỷ lệ % gợi ý cho từng ngành lấy theo Thông tư 40/2021/TT-BTC. Bạn cần kiểm tra lại với quy định đang áp dụng.
- Số thuế mặc định = doanh thu × tỷ lệ %. Nếu cơ quan thuế thông báo số khác, nhập số thực tế ở mục
  **Điều chỉnh số thuế phải nộp trong kỳ**.
