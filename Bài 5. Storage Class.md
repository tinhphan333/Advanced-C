# STORAGE CLASS
## 1. Extern
+ Tái sử dụng một tài nguyên (có thể là biến, hàm,… ) ở file nguồn khác  chia sẻ tài nguyên ở các file khác nhau.
+ Chỉ được phép khai báo, không được định nghĩa hay gán giá trị.
+ Extern chia sẻ địa chỉ của hàm và biến. Đối với biến phải có extern với hàm không nhất thiết phải dùng extern nhưng nên khai báo để nhận biết xem hàm có lặp lại ở chương trình khác hay không.
+ Sau khi kháo báo extern phải liên kết bằng GCC.  

Ví dụ ta có một file function.c :
``` bash
#include<stdio.h>
extern int data1 = 2025;   
void printdata(int data)	//hàm không nhất thiết phải có extern
{     
	printf("%d",data);
}


Ta có một file main.c, file này vẫn được chạy mà không báo lỗi.
#include<stdio.h>
int main()
{
	printdata(data1);	// có ở file function.c
	return 0;
}
```
## 2. Static
### Static local
Đặc điểm:   
+ chỉ cấp phát giá trị địa chỉ 1 lần duy nhất
+ có thể thay đổi địa chỉ của biến bên ngoài hàm bằng con trỏ
``` bash
#include <stdio.h>
void count_static()
{
	static int count = 1;	 
	count++;
	printf(“a = %d”, a);
}

void count_normal ()
{
	int count = 1;	 
	count++;
	printf(“a = %d”, a);
}
int main()
{	printf(“count normal”);
	count_normal();
	count_normal();
	count_normal();
	printf(“count static”);
	count_static();
	count_static();
	count_static();	//giữ giá trị qua các lần gọi hàm do chỉ cấp phát 1 lần địa chỉ duy nhất
	return 0;
}
```
Static global:  
+ Giới hạn hoạt động của biến trong hàm nó được khai báo  Ví dụ khai báo biến static trong hàm thì nó chỉ tồn tại trong hàm, hay khai báo một hàm static trong một file_a.c thì nó chỉ được sử dụng trong file_a.c
+ Ứng dụng trong viết hàm  Ngăn sự can thiệp không mong muốn, khiến biến không thể bị thay đổi bằng cách gọi tên biến, phải thay đổi từ bên ngoài bằng cách gọi con trỏ hàm.
## 3. Volatile
Trình biên dịch có xu hướng tối ưu hóa các biến không thay đổi giá trị sau vài lần chạy chương trình  để tránh hiện tượng này cần báo hiệu cho chương trình rằng biến có thể thay đổi ngẫu nhiên, ngoài sự kiểm soát của chương trình  Sử dụng từ khóa volatile cho biến  
- Ví dụ về một chương trình trên vi điều khiển STM32F1 có sử dụng từ khóa volatile.
``` bash
#include "stm32f10x.h"

uint8_t *addr = (uint8_t*) 0x20000000;// con trỏ addr lưu địa chỉ trên RAM 

volatile uint8_t var = 0;

int main()
{
   while(1)
   {	
	var = *addr;	
      if (var !=  0) break;
	// hãy thay đổi giá trị tại 0x20000000 để thoát khỏi chương trình trong lúc debug
   }
}
```
Từ khóa volatile thường được ứng dụng với những biến phụ thuộc bên ngoài như: Nút nhấn, giá trị cảm biến, hoặc địa chỉ trên RAM (như ví dụ trên).
## 4. Register (thanh ghi)
### Quy trình tính toán chủ yếu:  
Khai báo biến chương trình -> Lưu giá trị vào RAM -> Lưu giá trị từ RAM vào thanh ghi -> Lấy giá trị từ thanh ghi để ALU (khối tính toán logic) tính toán -> cập nhập kết quả vào thanh ghi -> Từ thanh ghi cập nhập kết quả cho RAM.  
Từ khóa register giúp lưu trữ biến trên thanh ghi thay vì RAM -> Tăng tốc độ tính toán do bỏ qua bước chuyển đổi từ RAM sang thanh ghi.  
### Đặc điểm: 
+ Sử dụng với các biến thực hiện tính toán. 
+ Các biến lưu trữ trên thanh ghi -> không thể đọc được địa chỉ.
+ Chỉ sử dụng cho các biến cục bộ thay vì biến toàn cục do không thể thay đổi địa chỉ -> sự linh hoạt bị giảm.
``` bash
#include<stdio.h>
#include<time.h>
int main(){
//Thay đổi biến count các trường hợp dùng register và không dùng để xe sự khác biệt
    clock_t start_time = clock();
    for( int count = 0; count < 5000000; count++);	//register int count = 0;
    clock_t end_time = clock();
    
    //in ra thời gian xử lý 
   double time_taken = ((double)(end_time - start_time)) / CLOCKS_PER_SEC;
    printf("time : %f",time_differece);
    return 0;
}
```

