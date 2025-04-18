# STDARG & ASSERT
## 1. Thư viện STDARG < stdarg.h >
STDARG là thư viện cung cấp các macro để thao tác và xử lý danh sách chứa các biến đối số.  
### 1.1 Các macro phổ biến sử dụng trong STDARG
+ va_list: Khai báo một biến có kiểu dữ liệu con trỏ char (char*) để lưu trữ danh sách.
+ va_start: Tách danh sách dựa trên label để lấy ra args (có thể là chuỗi hoặc mảng).
+ va_arg: Truyền vào args chứa các đối số và kiểu dữ liệu (dùng để ép kiểu). Từ đó truy suất từng đối số trong danh sách.
+ va_copy: Trỏ vào địa chỉ của các đối số trong biến danh sách.
+ va_end: Giải phóng bộ nhớ dùng cho danh sách.
### 1.2 Sử dụng hàm khi thao tác với STDARG
##### 1.2.1 Trường hợp đã biết số lượng
```bash
#include <stdio.h>
#include <stdarg.h>

int sum(int count, ...){
// Khởi tạo danh sách
	va_list args;
	va_start(args, count);
// Tính tổng
	int sum = 0;
	while(count > 0) {
		sum += va_arg(args, int); // cộng từng đối số của args
		count-- ; 
	} 
	printf(“ Sum = %d\n”, sum);
	var_end(args);
	return sum;
}
int main(){
	int count = 3;
	sum(count, 7, 20, 12);
	return 0;	
}

```
##### 1.2.2 Trường hợp chưa biết số lượng
```bash
#include <stdio.h>
#include <stdarg.h>
#define tong(...)  sum(__VA_ARGS__, ‘\n’)
					// kí tự ‘\n’ để thoát khỏi while	
int sum(int count, ...){
	va_list args;
	va_list check;		// tạo một danh sách copy của args

	va_copy(check, args);
	va_start(args, count);
	
	
	int result = count;
	
	while((value = va_arg(args, char*)) != ‘\n‘ )  // vế trái bằng 0 thì thoát khỏi vòng lặp
	{
		result += va_arg(check, int);	 // cộng từng đối số của args
		count-- ; 
	}
	var_end(args);
	return sum;
}
int main(){
	int total = tong(6, 3, 5, 1, 5);
	printf(“Sum = %d”, total);
	return 0;
}
```
##### 1.2.3 sử dụng với STDARG kết hợp với struct
```bash
#include <stdio.h>
#include <stdarg.h>

typedef enum
{
    TEMPERATURE_SENSOR, // 0
    PRESSURE_SENSOR     // 1
} SensorType;

void processSensorData(SensorType type, ...)
{
    va_list args;
    va_start(args, type);

    switch (type)
    {
    case TEMPERATURE_SENSOR:
        int count = va_arg(args, int);          // phần tử thứ nhất - Số phần tử
        int sensorId = va_arg(args, int);       // phần tử thứ hai - ID
        double temperature = va_arg(args, int); // phần tử thứ ba - Value
        printf("Temperature Sensor ID: %d, Reading: %f do C\n", sensorId, temperature);

        if (count > 2)
        {
            char *unit = va_arg(args, char *);
            printf("Unit: %s\n", unit);
        }
        break;

    case PRESSURE_SENSOR:
        int count = va_arg(args, int);
        int sensorId = va_arg(args, int);
        int pressure = va_arg(args, int);
        printf("Pressure Sensor ID: %d, Reading: %d Pa\n", sensorId, pressure);
        break;
    }

    va_end(args);
}

int main()
{
    processSensorData(TEMPERATURE_SENSOR, 2, 1, 75.5, "Room temperature");
    processSensorData(PRESSURE_SENSOR, 2, 2, 15000);
    return 0;
}
```
## 2. Thư viện ASSERT < assert.h >
Thư viện assert là một thư viện hỗ trợ việc debug. Các ưu điểm của thư viện ASSERT:  
+ Cho biết loại lỗi (thường là lỗi logic).
+ Cho biết vị trí lỗi.
+ Dừng các câu lệnh phía dưới nếu phát hiện lỗi
### 2.1 Sử dụng thư viện ASSERT
##### 2.1.1 Sử dụng trực tiếp assert()
```bash
#include <stdio.h>
#include <assert.h>

double divide(int tu, int mau)
{
	assert(mau != 0 && “mau phai khac 0”);
	double thuong = tu/mau;
	return thuong;
}
int main(){
	printf(“ket qua phep chia : %f”, divide(25,0));	//mẫu bằng 0 để xem kết quả	
}
```
##### 2.1.2 Sử dụng thông qua macro
+ Kiểm tra logic
```bash
#include <stdio.h>
#include <assert.h>

#define  LOG(condition,cmd)  assert(condition && #cmd) 

double divide(int tu, int mau){
	LOG(mau = 0, mau phai khac 0);
	return (double)tu/mau; 
}
int main(){
	printf(“ket qua phep chia : %f”, divide(25,0));	//mẫu bằng 0 để xem kết quả	
}
```
+ Kiểm tra giá trị trong khoảng
```bash
#include <stdio.h>
#include <assert.h>

#define ASSERT_IN_RANGE(val, min, max) assert((val) >= (min) && (val) <= (max))

void setLevel(int level){
	ASSERT_IN_RANGE(level, 1, 10);
}
int main(){
	int level = 12;
	setLevel(level);
	printf(“level in range 1 to 10 ”,);
	return 0;
}
```
