# 1. Struct
Struct là kiểu dữ liệu do người dùng định nghĩa bằng cách nhóm các biến có các kiểu dữ liệu khác nhau lại với nhau. Kiểu struct có thể quản lý nhiều biến với nhiều kiểu dữ liệu khác nhau.
## 1.1 Cách khai báo 
+ Cách 1:
``` bash
struct name_struct 
{
    <data type 1> <member 1>;
    <data type 2> <member 2>;
    // ...
};
```

+ Cách 2:
``` bash
typedef struct
{
    <data type 1> <member 1>;
    <data type 2> <member 2>;
    // ...
} name_struct;
```

+ Ví dụ:
``` bash
typedef struct 
{
	char *name;	
	int id;
} user;
user u1, *u2, u3;
```
+ Để truy cập phần tử của user1 : u1.name 
+ Để truy cập phần tử của user2 : u2->name (u2 là con trỏ)
## 1.2 Kích thước
data alignment  Tùy vào kích thước của kiểu dữ liệu mà sắp xếp vị trí địa chỉ lưu trữ phù hợp. Việc sắp xếp này giúp CPU truy cập đến địa chỉ chính xác và nhanh chóng hơn.
Lấy ví dụ đối với các kiểu: 
+ 1 byte : Đặt đâu cũng được.
+ 2 bytes: 0xa0, 0xa2, 0xa4,…
+ 4 bytes: 0xa0, 0xa4, 0xa8,…
+ 8 bytes: 0xa0, 0xa8, 0xb0, 0xb8,…
Mỗi lần cấp phát sẽ giống nhau về số lượng bytes và lấy biến có kích thước lớn nhất làm chuẩn, cấp phát theo thứ tự khai báo trong struct. Các biến thành viên sẽ có địa chỉ liền kề với nhau. 
``` bash
struct Example 
{
    uint8_t  a;    
    uint32_t b;
    uint16_t c;  
};
int main(){
	Example ex1;
	printf("%d bytes\n",sizeof(ex1));
	return 0;
}
```
<p align = "center">
<img src = "https://github.com/user-attachments/assets/c65df52f-684a-4906-baca-79bf68e4251d" width = "1000" height = "350">  

Kết quả: 12 bytes -> 7 bytes thực tế, 5 bytes padding.  
Như vậy nếu chúng ta thay đổi thứ tự khai báo các phần tử có thể ảnh hưởng đến kích thước của struct.  
	
``` bash
struct Example 
{
    uint8_t  a;    
    uint16_t c;  
    uint32_t b;
};
int main(){
	Example ex1;
	printf("%d bytes\n",sizeof(ex1));
	return 0;
}
```
<p align = "center">
<img src = "https://github.com/user-attachments/assets/50bb7f09-0056-4825-97cf-5b37d7b41022" width = "1000" height = "350">  

Kết quả: 8 bytes -> 7 bytes thực tế, 1 byte padding  
Như vậy có thể rút ra được:  
+ Kích thước Struct = Tổng bytes sử dụng + Tổng bytes padding.
+ Kích thước Struct là bội số của kiểu dữ liệu có kích thước lớn nhất.
## 1.3 Bit field (chỉ có ở Struct)
+ Cho sử dụng số lượng bit cụ thể đễ lưu trữ một số nguyên. được sử dụng để giới hạn gián trị scope. Với giá trị scope = 2^n -1 (n là số bit sử dụng). Ví dụ: 3 bit  2^3 -1 =7  giá trị scope từ 0 – 7.
+ Không thể truy cập đến địa chỉ các bit của bit field vì sẽ không biết bit nào sẽ được đặt lên.
``` bash
struct name_struct 
{
    <data type 1> <member 1> : <number of bits>;
    <data type 2> <member 2> : <number of bits>;
    // ...
};
```
Ví dụ kết hợp sử dụng bit field và tydef trong struct để tiết kiệm bộ nhớ:  
``` bash
#include <stdio.h>
#include <stdint.h>

//Định nghĩa mã màu xe.
#define COLOR_RED     0  /**< Màu đỏ    */
#define COLOR BLUE    1  /**< Màu xanh  */
#define COLOR_BLACK   2  /**< Màu đen   */
#define COLOR_WHITE   3  /**< Màu trắng */

//Định nghĩa mã công suất động cơ xe.
#define POWER_100HP   0  /**< Công suất 100HP */
#define POWER_150HP   1  /**< Công suất 150HP */
#define POWER_200HP   2  /**< Công suất 200HP */

typedef uint8_t CarColor;   /**< Kiểu dữ liệu cho màu xe          */
typedef uint8_t CarPower;   /**< Kiểu dữ liệu cho công suất xe    */

typedef struct
{
    CarColor  color  : 2;           /**< 2-bit cho màu sắc              */
    CarPower  power  : 2;           /**< 2-bit cho công suất            */
} CarOptions;
```
# 2. Union
Union là kiểu dữ liệu do lập trình viên tự định nghĩa, khác với Struct các biến thành viên của Union chia sẻ cùng một vị trí vùng nhớ  Trong một thời điểm xác định chỉ có thể gọi duy nhất một biến thành viên trong Union.
## 2.1 Cách khai báo
+ Cách 1
``` bash
union name_union 
{
    kieuDuLieu1 thanhVien1;
    kieuDuLieu2 thanhVien2;
    // ...
};
```

+ Cách 2:
``` bash
typedef union
{
    <data type 1> <member 1>;
    <data type 2> <member 2>;
    // ...
} name_union;
``` 

+ Ví dụ:
``` bash

typedef union 
{
	uint8_t  a;
	uint16_t b;
    	uint32_t c;
} data;
data  data1, *data2, data3;
```
+ Để truy cập phần tử của data1 hoặc data3 : data1.a, data3.b
+ Để truy cập phần tử của data2 : data2->a, data2->b, data2->c
## 2.2 Kích thước
Union chỉ sử dụng 1 vùng nhớ duy nhất cho tất cả biến  kích thước Union bằng kích thước của biến thành viên lớn nhất. 
+ Các biến thành viên nhỏ hơn kích thước union  Padding thêm. 
+ Ghi đè dữ liệu đúng vị trí byte.
``` bash
union Data 
{
    uint8_t  a;	// 1 + 15 padding 
    uint16_t b[5];	// 10 + 6 padding
    uint32_t c;	// 4 + 12 padding
    double   d;	// 8 + 8 padding
};
``` 
+ Tại một thời điểm nhất định chỉ gọi được một biến thành viên của Union.
``` bash
#include <stdio.h>
#include <stdint.h>

typedef union
{

    uint8_t var1;
    uint32_t var2;
    uint16_t var3;

} frame;
int main()
{
    frame data;
    //  address:                0x03      0x02     0x01     0x00
    data.var2 = 2555555; // 0b 00000000 00100110 11111110 10100011
    data.var3 = 12345;   // 0b 00000000 00000000 00110000 00111001
    data.var1 = 75;      // 0b 00000000 00000000 00000000 01001011

    printf("var1: %d\n", data.var1); // 0b 00000000 00000000 00000000 01001011
    printf("var2: %d\n", data.var2); // 0b 00000000 00100110 00110000 01001011
    printf("var3: %d\n", data.var3); // 0b 00000000 00000000 00110000 01001011
}
``` 

Kết quả in ra: 
``` bash
var1: 75
var2: 2502731
var3: 12363
``` 

# 3. Ứng dụng Struct kết hợp Union
Ứng dụng Struct và Union để tạo một khung truyền dữ liệu, khi dữ liệu các phần tử trong Struct thay đổi -> dữ liệu bytes vị trí tương ứng trên Union cũng thay đổi. Union sẽ tạo ra một khung tổng hợp các dữ liệu của Struct một cách hoàn chỉnh.
``` bash
#include <stdio.h>
#include <stdint.h>
#include <string.h>

typedef union
{
    struct
    {
        uint8_t id[2];        // uint16_t id
        uint8_t data[4];      // uint32_t data
        uint8_t check_sum[2]; // uint16_t check_sum
    } data;

    uint8_t frame[8]; // Frame để truyền khung hoàn chỉnh
} Data_Frame;

int main()
{

    Data_Frame Transmit_Data;
    Data_Frame Receive_Data;

    strcpy((char *)Transmit_Data.data.id, "10");
    strcpy((char *)Transmit_Data.data.data, "1010");
    strcpy((char *)Transmit_Data.data.check_sum, "20");

    strcpy((char *)Receive_Data.frame, (char *)Transmit_Data.frame);
    return 0;
}
``` 

