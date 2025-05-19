# 1.Struct
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
