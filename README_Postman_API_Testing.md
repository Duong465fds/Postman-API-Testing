# Báo cáo thực hành kiểm thử API bằng Postman

- **Sinh viên:** Duong465fds
- **Chủ đề:** Làm quen và thực hành kiểm thử API bằng Postman
- **Tài liệu tham khảo:** [Video hướng dẫn Postman](https://www.youtube.com/watch?v=MFxk5BZulVU)
- **API thực hành:** [JSONPlaceholder](https://jsonplaceholder.typicode.com/)

> Lưu ý: Hãy chạy các request trên Postman và thay các vị trí `[Chèn ảnh ...]` bằng ảnh chụp màn hình kết quả thực tế trước khi nộp. Không để ảnh minh họa giả thay cho kết quả kiểm thử của bạn.

## 1. Mục tiêu

- Làm quen giao diện Postman và cách tạo request HTTP.
- Thực hành các phương thức GET và POST.
- Kiểm tra mã trạng thái HTTP, dữ liệu phản hồi và thời gian phản hồi.
- Viết test script đơn giản để tự động kiểm tra kết quả API.
- Lưu lại request và ảnh chụp màn hình làm minh chứng.

## 2. Công cụ và môi trường

- Postman Desktop hoặc Postman Web.
- API công khai JSONPlaceholder dùng cho mục đích thực hành.
- Phương thức HTTP: GET, POST.
- JavaScript trong tab **Scripts / Post-response** (hoặc tab **Tests** ở giao diện Postman cũ).

## 3. Các bước thực hiện

### 3.1. Kiểm thử GET – lấy một bài viết

- **Method:** `GET`
- **URL:** `https://jsonplaceholder.typicode.com/posts/1`
- Nhấn **Send**.
- Kết quả mong đợi: HTTP `200 OK`; phần phản hồi JSON có các trường `userId`, `id`, `title`, `body`.

**Test script** (đặt trong tab Scripts → Post-response; giao diện cũ có thể dùng tab Tests):

```javascript
pm.test("Status code is 200", function () {
    pm.response.to.have.status(200);
});

pm.test("Response is JSON", function () {
    pm.response.to.be.json;
});

pm.test("Response contains required fields", function () {
    const data = pm.response.json();
    pm.expect(data).to.have.property("userId");
    pm.expect(data).to.have.property("id");
    pm.expect(data).to.have.property("title");
    pm.expect(data).to.have.property("body");
});
```

**Ảnh minh chứng cần chụp:** request GET và URL, status `200 OK`, JSON response, kết quả các test ở mục Test Results.

[Chèn ảnh 1: GET /posts/1 và phản hồi 200 OK]

[Chèn ảnh 2: Các test của request GET đều thành công]

### 3.2. Kiểm thử POST – tạo bài viết mới

- **Method:** `POST`
- **URL:** `https://jsonplaceholder.typicode.com/posts`
- Vào **Body → raw → JSON**, nhập:

```json
{
  "title": "Postman API Test",
  "body": "This is a practice request.",
  "userId": 1
}
```

- Nhấn **Send**.
- Kết quả mong đợi: HTTP `201 Created`; phản hồi có các trường `title`, `body`, `userId` và `id`.

**Test script:**

```javascript
pm.test("Status code is 201", function () {
    pm.response.to.have.status(201);
});

pm.test("Response contains submitted title", function () {
    const data = pm.response.json();
    pm.expect(data.title).to.eql("Postman API Test");
    pm.expect(data.body).to.eql("This is a practice request.");
    pm.expect(data.userId).to.eql(1);
});

pm.test("Response contains an id", function () {
    const data = pm.response.json();
    pm.expect(data).to.have.property("id");
});
```

**Ảnh minh chứng cần chụp:** phương thức POST, URL, Body JSON, status `201 Created`, phản hồi và kết quả test.

[Chèn ảnh 3: POST /posts với Body JSON và phản hồi]

[Chèn ảnh 4: Các test của request POST đều thành công]

> JSONPlaceholder là API giả lập phục vụ thử nghiệm. POST trả về dữ liệu phản hồi mô phỏng; dữ liệu không được lưu bền vững như một hệ thống thực tế.

## 4. Bảng tổng hợp kiểm thử

| STT | Phương thức | Endpoint | Kết quả mong đợi | Kết quả thực tế |
|---|---|---|---|---|
| 1 | GET | `/posts/1` | 200 OK, có các trường JSON cần thiết | Điền sau khi chạy |
| 2 | POST | `/posts` | 201 Created, phản hồi có dữ liệu đã gửi và `id` | Điền sau khi chạy |

## 5. Cách lưu ảnh minh chứng

Tạo thư mục `screenshots/` trong repository và lưu ảnh chụp thật từ Postman theo tên:

- `screenshots/get-post-200.png`
- `screenshots/get-post-tests.png`
- `screenshots/post-create-201.png`
- `screenshots/post-create-tests.png`

Sau khi thêm ảnh, có thể thay các dòng `[Chèn ảnh ...]` bằng cú pháp Markdown, ví dụ:

```markdown
![GET /posts/1 trả về 200 OK](screenshots/get-post-200.png)
```

## 6. Nhận xét và kết luận

Qua bài thực hành, sinh viên làm quen với việc gửi HTTP request bằng Postman, đọc phản hồi JSON và sử dụng test script để kiểm tra mã trạng thái cùng dữ liệu trả về. Việc lưu request và ảnh chụp kết quả giúp báo cáo dễ kiểm tra và tái hiện.

**Trước khi nộp:** chạy lại cả hai request, xác nhận test thành công, cập nhật cột “Kết quả thực tế”, chèn ảnh chụp thật, rồi commit/push toàn bộ thay đổi lên GitHub.
