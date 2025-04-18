# BITMASK
## 1. Khái niệm: 
Bitmask là một kỹ thuật sử dụng các bit để lưu trữ và thao tác với các cờ (flags) hoặc trạng thái. Có thể sử dụng bitmask để đặt, xóa và kiểm tra trạng thái của các bit cụ thể. Bitmask có các ưu điểm:
+ Tối ưu hóa bộ nhớ.
+ Thực hiện các phép toán logic trên một cụm bit (1 bytes = 8 bit).
+ Quản lí các trạng thái, quyền truy cập hoặc thuộc tính của đối tượng.

Ngoài ra thay vì int (4 bytes) chúng ta có nhiều kiểu dữ liệu khác để tối ưu bộ nhớ như: uint8_t (1 byte), uint_16t(2 bytes), int8_t, int16_t, …
## 2. Phép dịch bit:
+ Dịch trái: khi cần đẩy bit sang trái.
```bash
  uint8_t   x = 1;	//0b 0000 0001     
  x = x << 2;		//0b 0000 0100  x = 4;
```
+ Dịch trái: khi cần đẩy bit sang trái.
```bash
  uint8_t   x = 255;	//0b 1111 1111     
  x = x >> 4;		//0b 0000 1111  x = 15;

```
## 3. Bitwise ứng dụng:
<p align = "center">
<img src = "https://github.com/user-attachments/assets/c8f53bea-3146-41b2-bcec-e47d74565524" width = "700" height = "350">

```bash
#include <stdio.h>
#include <stdint.h>

// mỗi bit trong byte tượng trưng cho 1 Feature
#define FEATURE_GEN 1 << 0   // Bit 0: Giới tính (0 = Nữ, 1 = Nam)
#define FEATURE_SHIRT 1 << 1 // Bit 1: Áo sơ mi (0 = Không, 1 = Có)
#define FEATURE_HAT 1 << 2   // Bit 2: Nón (0 = Không, 1 = Có)
#define FEATURE_SHOES 1 << 3 // Bit 3: Giày (0 = Không, 1 = Có)
#define FEATURE_4 1 << 4     // Bit 4: Tính năng 1
#define FEATURE_5 1 << 5     // Bit 5: Tính năng 2
#define FEATURE_6 1 << 6     // Bit 6: Tính năng 3
#define FEATURE_7 1 << 7     // Bit 7: Tính năng 4

uint8_t user = 0b00000000; // xét xem các trạng thái user

// Thêm một feature với OR
void enableFeature(uint8_t *feature_x, uint8_t feature) // thứ được truyền tới là địa chỉ feature_x
{
    *feature_x |= feature; // gọi giá trị từ địa chỉ được truyền tới và thực hiện bitwise
}

// Xóa một feature với AND và NOR
void disableFeature(uint8_t *feature_x, uint8_t feature)
{
    *feature_x &= ~feature; // thực hiện bitwise & giữa giá trị feature_x và giá trị nghịch đảo bit của feature
}

// Xét trạng thái feature
int isFeatureEnabled(uint8_t feature_x, uint8_t feature) // truyền tham trị vì không cần thay đổi bit
{
    return (feature_x & feature) != 0;
}

// List các feature đang có
void listSelectedFeatures(uint8_t feature_x)
{
    const char *featureName[8] = {
        "FEATURE_GEN",
        "FEATURE_SHIRT",
        "FEATURE_HAT",
        "FEATURE_SHOES",
        "FEATURE_4",
        "FEATURE_5",
        "FEATURE_6",
        "FEATURE_7",
    };
    printf("feature selected:\n");
    for (int i = 0; i < 8; i++)
    {
        if ((feature_x >> i) & 1)
            printf("%s \n", featureName[i]);
    }
}

int main()
{
    enableFeature(&user, FEATURE_GEN | FEATURE_SHIRT | FEATURE_HAT | FEATURE_SHOES);
    disableFeature(&user, FEATURE_SHIRT);
    listSelectedFeatures(user);
    return 0;
}
```

