# CON TRỎ
## 1.1 Khái niệm
Pointer là 1 biến dùng để lưu địa chỉ của một đối tượng như biến, mảng, hàm, struct. Việc sử dụng pointer sẽ giúp thao tác trên bộ nhớ linh hoạt hơn.
## 1.2 Cú pháp
+ Khai báo một con trỏ và truy cập giá trị con trỏ.
```c
int year = 2025;
int *ptr = &year;	// khai báo con trỏ
printf(“giá trị con trỏ là: %d”, *ptr);	// lấy giá trị con trỏ
```
## 1.3 Kích thước con trỏ
Kích thước của con trỏ phụ thuộc vào kiến trúc máy tính và trình biên dịch hoặc kiến trúc vi xử lý (stm8, stm32, esp32).
```c
#include <stdio.h>
#include <stdint.h>
int main () 
{
// ví dụ về hệ điều hành kiến trúc x64bit = 8 bytes
	printf(“size of pointer: %d bytes\n”, sizeof(int *));	//8 bytes
	printf(“size of pointer: %d bytes\n”, sizeof(double *));	//8 bytes
	printf(“size of pointer: %d bytes\n”, sizeof(char *));	//8 bytes
	return 0;
}
```
## 1.4 Big endian và Little endian 
Ta có một kiến trúc máy tính x64bit, khi lưu giá trị một con số ví dụ 10. 
```c
int value = 0x01020304;	 
/*
 Kiểu int 4 bytes, số 10 tương đương:
	  MSB				LSB
địa chỉ  0x100    0x101    0x102    0x103
bytes     0x01      0x02      0x03      0x04  
Giá trị địa chỉ 0x01 lưu giữ bit hay byte quan trọng  MSB 
Giá trị địa chỉ 0x04 lưu trữ bit hay byte ít quan trọng  LSB
*/
```
+ Đối với một kiến trúc máy tính dạng Big endian thì giá trị địa chỉ thấp (0x100) sẽ lưu trữ byte MSB, và cách lưu trữ giống với ví dụ trên. Big endian được ứng dụng nhiều trong lập trình Network (IP, TCP, UDP).
+ Đối với một kiến trúc máy tính dạng Little endian thì giá trị địa chỉ thấp (0x100) sẽ lưu trữ byte LSB. và cách lưu trữ ngược lại với ví dụ trên. Đa số kiến trúc máy tính x86, x64, ARM đều sử dụng Little endian.
## 1.5 Kiểu dữ liệu con trỏ
Tuy kích thước con trỏ không phụ thuộc vào kiểu dữ liệu, nhưng việc khai báo kiểu dữ liệu cho con trỏ sẽ giúp quá trình truy xuất dữ liệu và kết quả kiểu dữ liệu trả về chính xác. ví dụ:
```c
double value = 155.56 ;	// kiểu double 8 bytes
int *ptr = &value ; 	// con trỏ chỉ đọc 4 bytes
int main() {
	printf(“value: %d”, *ptr);
	return 0;
}
```
## 1.6 Truyền tham trị và truyền tham chiếu
+ Truyền tham trị: Tức là truyền đi giá trị và giá trị của biến vẫn giữ nguyên sau khi gọi hàm.
```c
#include <stdio.h>
void ThamTri( int x) {
  	printf("Bên trong hàm, trước khi thay đổi: x = %d\n", x);
 	x = 10;
 	printf("Bên trong hàm, sau khi thay đổi: x = %d\n", x);
}

int main() {
  	int a = 0;
  	printf("Trước khi truyền tham trị: a = %d\n", a);
  	ThamTri(a);
 	printf("Sau khi truyền tham trị: a = %d\n", a);
 	return 0;
}
```
+ Truyền tham chiếu: Tức là truyền đi địa chỉ (con trỏ) và giá trị con trỏ thay đổi sau khi gọi hàm.
```c
#include <stdio.h>
void ThamChieu(int *ptr) {
  	printf("Bên trong hàm, trước khi thay đổi: *ptr = %d\n", *ptr);
	*ptr = 10;
	printf("Bên trong hàm, sau khi thay đổi: *ptr = %d\n", *ptr);
}

int main() {
 	int a = 0 ;
  	printf("Trước khi gọi hàm: a = %d\n", a);
  	ThamChieu( &a );
  	printf("Sau khi gọi hàm: a = %d\n", a);
  	return 0;
}
```
# CÁC LOẠI CON TRỎ
## 2.1 Con trỏ void - Void Pointer
+ Là con trỏ chưa khai báo kiểu (có thể trỏ tới bất kì kiểu nào). Con trỏ void ứng dụng trong việc sử dụng một con trỏ để quản lý nhiều con trỏ khác nhau nhằm tối dữ liệu sử dụng trong chương trình.
+ Để truy xuất dữ liệu con trỏ void thì phải ép kiểu để trình biên dịch biết cần truy xuất bao nhiêu bytes.
```c
#include <stdio.h>
int var_int = 10;	//4 bytes
double var_double = 5.6;	//8bytes
char arr[] = “hello”;  	//{h, e, l, l, o, NULL}
int main() 
{
// truy xuất 4 bytes do là kiểu int	
void *ptr = &var_int; 	
printf(“Địa chỉ: %p – Giá trị: %d\n”,ptr, *(int*)ptr);	

// truy xuất 8 bytes do là kiểu double
ptr = &var_double;
printf(“Địa chỉ: %p – Giá trị: %f\n”,ptr, *(double*)ptr); 

// truy xuất từ địa chỉ  từ ptr ( chữ ‘h’)  ptr + 4  (chữ ‘o’)
ptr = arr;
printf(“Địa chỉ: %p – Giá trị: %s\n”,ptr, *(char*)(ptr + 4)); 
return 0;
 }
```
Mảng con trỏ void  cũng có thể chứa các phần tử là địa chỉ các biến, ứng dụng trong việc tạo ra một hàm tổng quát xử lý nhiều kiểu đầu vào khác nhau. 
```c
void Print_Data(void *data, char type)
{
	switch (type)
	{
		case ‘int’:
			printf(“ data = %d”, *(int*)data);
			break;
		case ‘double’:
			printf(“ data = %d=f”, *(double*)data);
			break;
	}
}
```
## 2.2 Con trỏ hàm – Function Pointer
Con trỏ hàm là con trỏ trỏ đến địa chỉ của một hàm. Con trỏ hàm được ứng dụng trong việc quản lý các hàm có đặc điểm đầu vào giống nhau.
```c
<return_type> (*func_pointer)(<data_type_1>, <data_type_2>);
```
Chúng ta lấy ví dụ về các cách sử dụng con trỏ hàm thông qua một chương trình tính toán, ban đầu ta có sẵn các hàm như sau:
```c
void  Tong(int a, int b)
{
	printf(“a + b = %d\n”, a+b)	;
} 
void  Hieu(int a, int b)
{
	printf(“a - b = %d\n”, a-b);	
} 
void  Tich(int a, int b)
{
	printf(“a * b = %d\n”, a*b);	
} 
```
+ Trường hợp 1: Gán địa chỉ hàm cho con trỏ hàm.
```c
int main()
{
	void (*ptr)(int, int);	// gọi con trỏ hàm
	ptr = Tong;		
	ptr(5,2);
		
	ptr  = Hieu;
	ptr(5,2);
	return 0;
}
```
+ Trường hợp 2: Sử dụng con trỏ hàm như một thông số đầu vào.
```c
void TinhToan(void (*pheptoan)(int, int), int a, int b)
{
	pheptoan(a,b);
}
int main()
{
	TinhToan(Tong, 5, 2);
	TinhToan(Hieu, 5, 2);
	TinhToan(Tich, 5, 2);
	return 0;
}
```
+ Trường hợp 3: Sử dụng mảng con trỏ hàm.
```c
void (*TinhToan[])(int, int) = {Tong, Hieu, Tich};
int main()
{
	TinhToan[0](5, 2);		// gọi hàm tính tổng
	TinhToan[1](5, 2); 		// gọi hàm tính hiệu
	TinhToan[2](5, 2); 		// gọi hàm tính tích
	return 0;
}
```
## 2.3 Con trỏ hằng – Pointer to constant
Con trỏ hằng là con trỏ trỏ tới một địa chỉ và trong lúc truy xuất giá trị tại địa chỉ là hằng số. Con trỏ hằng sẽ bao gồm các đặc điểm:
+ Chỉ đọc tại địa chỉ trỏ tới.
+ Có thể thay đổi địa chỉ trỏ tới.
+ Không thể thay đổi giá trị đang lưu trữ của dải tham chiếu.  
Ứng dụng con trỏ hằng để bảo vệ những dữ liệu quan trọng, không muốn bị tác động trong quá trình thực thi chương trình.
```c
int value = 5;
const int* const_ptr = &value;
```
##  2.4 Hằng con trỏ - Constant Pointer 
Là con trỏ chỉ trỏ đến một địa chỉ duy nhất, hằng con trỏ có các đặc điểm:
+ Có thể đọc – ghi giá trị tại địa chỉ ban đầu.
+ Không được thay đổi địa chỉ.  
Ứng dụng hằng con trỏ để lưu trữ địa chỉ một thanh ghi, người dùng chỉ thao tác với các giá trị trên thanh ghi đó. Việc sử dụng thanh ghi sẽ giúp tốc độ chạy chương trình nhanh hơn.
```c
int value = 5;
int *const const_ptr = &value;
```
##  2.5 Con trỏ NULL
Khi khai báo con trỏ mà không sử dụng, trình biên dịch sẽ tự động gán cho nó một địa chỉ bất kì (có thể là địa chỉ của một trong các biến đã có trong chương trình). Để tránh việc gán vào địa chỉ các biến đã khai báo  ta gán cho nó một con trỏ NULL. Chúng ta cũng khai báo con trỏ NULL sau khi sử dụng xong một con trỏ mà không dùng nữa. 
```c
int *ptr = NULL;
```
##  2.6 Con trỏ tới con trỏ khác – Pointer to Pointer
Là con trỏ mà giá trị truy xuất từ địa chỉ con trỏ là địa chỉ của một con trỏ khác.
```c
int x =5;
int *ptr = &x;
int **ptp = &ptr; 
```

