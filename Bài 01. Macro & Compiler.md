# MACROS

Chỉ thị tiền xử lý là những chỉ thị cung cấp cho trình biên dịch để xử lý những thông tin trước khi bắt đầu quá trình biên dịch. Chỉ thị tiền xử lý chia làm 3 nhóm chính:

## 1. Chỉ thị bao hàm tệp (#include)

Chức năng: Thêm vào file.c nội dung của file.h (file tiêu đề), khi một chương trình phức tạp sử dụng nhiều hàm.    
+ #include <…> // Compiler tìm kiếm file tiêu đề trong thư mục chứa các thư viện chuẩn, ví dụ: stdio.h, stdlib.h, math.h  
+ #include “…” // Compiler ưu tiên tìm kiếm trong thư mục hiện tại rồi mới tìm kiếm trong các thư mục chứa các thư viện chuẩn, ví dụ: “yasume.h”, “otoko.h” (các thư viện do người dùng tạo ta)
## 2.	Chỉ thị định nghĩa, hủy định nghĩa (#define, #undef)
Define: Thay thế một macro bằng một chuỗi khác.  
+ Định nghĩa một dòng:  
``` bash
#define YEAR  2025  
```
+ Định nghĩa nhiều dòng:
``` bash
#define Square(a)		\
printf(“Chu vi: %d”, a*4);	\
printf(“Dien tich: %d”, a*a);	
```
Ngoài ra cũng có một vài toán tử trong #define:  
“ ## ” dùng để nối chuỗi.
``` bash
#define VAR(name, type, value)	 \
type type_##name = value 

VAR(age, int, 20);
printf("Age: %d\n", int_age);

```
“#” dùng để chuẩn hóa đoạn văn bản thành chuỗi.  
``` bash
#define   PRINT_STRING(cmd)   printf(#cmd);
```
Variadic macro: Là một dạng macro dùng để xử lý khi biến đầu vào không rõ số lượng và có thể thay đổi.  
``` bash
#define SUM(...)				\
int array[] = {__VA_ARGS__,0};	\
int i, tong	;				\	
while(array[i] != 0 ) {           		\
   tong += array[i];             		\
   i++;                        			\  
}                              
```
Undef: xóa một macro đang có.  
``` bash
#define SALE  50
#undef  SALE
#define SALE  25                             
```
## 3.	Chỉ thị biên dịch có điều kiện (#if, #elif, #else, #ifdef, #ifndef, #endif)
+ ifdef: Xét điều kiện là một macro đã tồn tại hay chưa, trả về đúng khi đã tồn tại.
+ ifndef: Xét điều kiện là một macro đã tồn tại hay chưa, trả về đúng khi chưa tồn tại.
+ endif: Kết thúc một chỉ thị biên dịch có điều kiện.  
#ifdef, #ifndef, #endif thường được sử dụng để tránh việc lặp lại một file tiêu đề (.h) nhiều lần trong quá trình biên dịch.
``` bash
#ifndef MY_HEADER_H	
#define MY_HEADER_H
typedef struct {
    int year;
    int month;
} Date;
void print_Date(Date p);
#endif                           
```
Ví dụ các chỉ thị điều kiện #if, #elif, #else về hệ điều hành đang sử dụng.  
``` bash
#include <stdio.h>
#define SYSTEM_WINDOWS 1
// #define SYSTEM_LINUX 1
int main() {
    #if  SYSTEM_WINDOWS == 1
        printf("Đang chạy trên hệ điều hành Windows.\n");
    #elif  SYSTEM_LINUX == 1
        printf("Đang chạy trên hệ điều hành Linux.\n"); 
    #else  
        printf(“Đang chạy hệ điều hành khác.\n”);						
    #endif
    return 0;
}
                         
```
# QUÁ TRÌNH COMPILER
<p align = "center">
<img src = "https://github.com/user-attachments/assets/a1df7518-4ec2-4b45-8151-2d6f4acf8ec9" width = "500" height = "350">
  
Các chương trình C được lập trình viên viết ra sẽ có phần mở rộng .c và máy tính chỉ hiểu được ngôn ngữ dưới dạng nhị phân (gồm các bit 0 và bit 1). Vì vậy cần một quá trình trung gian giúp chuyển đổi ngôn ngữ C thành ngôn ngữ máy, đó là Compiler. Một quá trình compiler hoàn chỉnh sẽ bao gồm các bước:
<p align = "center">
<img src = "https://github.com/user-attachments/assets/1701d3c1-b91c-4cf3-b3ff-52df278bded9" width = "350" height = "500">

## 1. Preprocessor
### Chức năng:
Xử lý các chỉ thị tiền xử lý, và xóa bỏ comment để tạo ra file.i
### GCC Command: 
``` bash
gcc -E main.c -o main.i                       
```
## 2. Compiler
### Chức năng:
Chuyển đổi file.i thành file.s (assembly)
### GCC Command: 
``` bash
gcc -S main.i -o main.s                  
```
## 3. Assembler
### Chức năng:
Chuyển đổi file.s thành file.o (các bit 0 và bit 1)
### GCC Command: 
``` bash
gcc -c main.s -o main.o                  
```
## 4. Linker
### Chức năng:
Liên kết các file.o để tạo ra file.exe (file thực thi).  
+  dụ chương trình có file main.c và main1.c, sau khi thực hiện bước assembler ta sẽ có hai file main.o và main1.o, liên kết hai file này để tạo ra main2.exe
### GCC Command: 
``` bash
gcc main.o main1.0 -o main2                  
```
## 5. Chạy chương trình
### GCC Command: 
``` bash
./main2.exe                  
```


