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

## 1 số trường hợp ảnh hưởng đến hiệu suất chạy JS

- eval()

- arguments: dùng cú pháp destructors

- for in: Object.keys rồi loop từng phần tử 

- with

- delete

- hidden classes

- inline caching

==> Write code more predictable for humans, also for machines

## Web assembly

1 format chuẩn để biên dịch file js và chạy trên bất kì trình duyệt nào

biên dịch file js ra web assembly và chạy trực tiếp, không cần quá trình thông dịch, biên dịch hay jit trên trình duyệt nữa. Đẩy nhanh tốc độ thực thi file js

## Stack and Heap

Memory heap: chứa dữ liệu, đọc và ghi dữ liệu trong chương trình

The memory heap is where the memory allocation happens

Call stack: keep track of code in order

The call stacks is where the engine keeps track of where your code is and it's execution

## Stack overflow

## Memory Heap - Garbage collection

- Dùng thuật toán mark và sweap

## Memory leaks

```js
let array = [];
for (let i = 5; i > 1; i++) {
    array.push(i-1);
}
```

1 số trường hợp gây memory leaks

- global variable
  
  ```js
  var a = 1;
  var b = 1;
  var c = 1;
  ```
  
  

- event listeners
  
  ```js
  var element = document.getElementById('button');
  element.addEventListener('click', onClick);
  ```

- setInterval
  
  ```js
  setInterval(() => {
      // referencing objects...
  })
  ```
  
  **Memory is limited!**
