# Bài 2: Tái cấu trúc lịch sử commit bằng Interactive Rebase

## 1. Mục tiêu

Thực hành sử dụng Interactive Rebase để tái cấu trúc lịch sử commit cục bộ.

Các thao tác được thực hiện:

* Gộp nhiều commit nhỏ thành một commit có ý nghĩa bằng `squash`.
* Loại bỏ commit không cần thiết bằng `drop`.
* Thay đổi thông điệp của commit sau khi gộp.
* Kiểm tra lại lịch sử commit sau khi hoàn tất.

---

## 2. Bối cảnh

Tạo một nhánh làm việc với 4 commit thử nghiệm:

1. `feat: khoi tao module auth`
2. `fix typo`
3. `adds utility functions`
4. `add temp file for debug`

Trong đó commit thứ 4 chứa file thử nghiệm `temp.txt` và không cần thiết trong lịch sử cuối cùng.

---

## 3. Tạo các commit ban đầu

Tạo file `auth.js` và commit:

```bash
git add auth.js
git commit -m "feat: khoi tao module auth"
```

Tiếp tục sửa `auth.js` và tạo commit:

```bash
git add auth.js
git commit -m "fix typo"
```

Tiếp tục sửa `auth.js` và tạo commit:

```bash
git add auth.js
git commit -m "adds utility functions"
```

Tạo file `temp.txt` để mô phỏng file rác thử nghiệm:

```bash
git add temp.txt
git commit -m "add temp file for debug"
```

Kiểm tra lịch sử:

```bash
git log --oneline
```

Kết quả ban đầu gồm 4 commit.

**Ảnh 1: Lịch sử 4 commit trước khi Interactive Rebase.**

---

## 4. Thực hiện Interactive Rebase

Sử dụng lệnh:

```bash
git rebase -i HEAD~4
```

Cấu hình lại các commit như sau:

```text
pick <commit-1> feat: khoi tao module auth
squash <commit-2> fix typo
squash <commit-3> adds utility functions
drop <commit-4> add temp file for debug
```

Trong đó:

* `pick`: giữ lại commit.
* `squash`: gộp commit vào commit ngay phía trước.
* `drop`: loại bỏ hoàn toàn commit khỏi lịch sử.

**Ảnh 2: Giao diện Interactive Rebase với cấu hình `pick`, `squash`, `squash`, `drop`.**

---

## 5. Thay đổi thông điệp commit

Sau khi thực hiện `squash`, Git yêu cầu chỉnh sửa thông điệp của commit được gộp.

Thay đổi thông điệp thành:

```text
feat: hoan thien module authentication
```

Lưu và đóng trình soạn thảo để hoàn tất quá trình Rebase.

**Ảnh 3: Giao diện chỉnh sửa commit message.**

---

## 6. Kiểm tra lịch sử sau khi Rebase

Sử dụng:

```bash
git log --oneline
```

Kết quả mong đợi:

```text
<commit-id> feat: hoan thien module authentication
```

Ba commit liên quan đến module authentication đã được gộp thành một commit duy nhất.

Commit chứa `temp.txt` đã bị loại bỏ bằng `drop`.

**Ảnh 4: Lịch sử commit sau khi hoàn tất Interactive Rebase.**

---

## 7. Kiểm tra file temp.txt

Kiểm tra:

```powershell
Test-Path temp.txt
```

Kết quả:

```text
False
```

Kiểm tra các file đang được Git theo dõi:

```bash
git ls-files
```

Kết quả không còn `temp.txt`.

---

## 8. Kiểm tra trạng thái repository

Sử dụng:

```bash
git status
```

Kết quả:

```text
nothing to commit, working tree clean
```

Điều này chứng minh quá trình tái cấu trúc lịch sử đã hoàn tất và thư mục làm việc không còn thay đổi chưa commit.

---

## 9. Kết quả đạt được

Sau khi Interactive Rebase:

* 3 commit nhỏ liên quan đến module authentication được gộp thành 1 commit.
* Commit `add temp file for debug` được loại bỏ.
* File `temp.txt` không còn trong lịch sử cuối cùng.
* Thông điệp commit được chuẩn hóa thành:

```text
feat: hoan thien module authentication
```

Lịch sử cuối cùng trở nên ngắn gọn, rõ ràng và dễ theo dõi hơn.
