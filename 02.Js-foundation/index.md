# Javascript Engine

Tập tin JS thông qua JS Engine tạo ra mã máy để CPU có thể hiểu và thực hiện

Các JS Engine được tạo theo chuẩn ECMAScript

1 số engine thông dụng: V8, Spider monkey, chakra,...

## Inside the Engine

Ví dụ về V8 engine

V8 là 1 chương trình chạy JS, được dùng trong trình duyệt chrome và nodejs, được viết bằng ngôn ngữ C++

V8 trộn quá trình interpreter và compiler thành 1, gọi là JIT compiler hay Just in time compiler, mang đủ ưu điểm của interpreter (thực hiện code ngay lặp tức) và compiler (tối ưu hóa code).

Viết code 1 cách hiệu quả, hỗ trợ tốt cho quá trình JIT để code chạy nhanh hơn

Vậy JS là ngôn ngữ thông dịch (interpreter)?

==> Phụ thuộc vào việc implement JS engine, có thể implement theo thông dịch => JS chạy theo kiểu thông dịch, có thể implement theo biên dịch => JS biên dịch ra 1 file rồi chạy. Ban đầu, Spider monkey được tạo ra và chạy JS qua quá trình thông dịch. Gần đây V8 kết hợp cả thông dịch và biên dịch tạo thành JIT compiler, như vậy code JS chạy tối ưu hơn.
