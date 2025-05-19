# Từ khóa goto
## 1.1 Định nghĩa và cú pháp
Định nghĩa: goto là từ khóa cho phép điều khiển luồng hoạt động của chương trình. Sau khi thực hiện lệnh goto, chương trình sẽ nhảy đến một label đã được người dùng định nghĩa.    
Cú pháp:  (điều kiện: label phải ở cùng phạm vi hàm với goto ).
``` bash
goto label;
label: 
	// câu lệnh 1	
 	// câu lệnh 2
```
## 1.2 Cách sử dụng
+ Dùng để thoát khỏi nhiều vòng lặp lồng nhau.
``` bash
include <stdio.h>
int main()
{
    while (1)
    {
        for (int i = 0; i < 5; i++)
        {
            for (int j = 0; j < 5; j++)
            {
                if ((i == 3) && (j == 3))
                    goto exit;
            }
        }
    }
exit:
    printf("stop\n");
}
```
+ Dùng để tạo vòng lặp.
``` bash
#include <stdio.h>

int main()
{
   int i = 0;

   start:
      if (i >= 5)
      {
         goto end;  // Chuyển control đến nhãn "end"
      }

      printf("%d ", i);
      i++;

      goto start;  // Chuyển control đến nhãn "start"

   end:
      printf("\n");
   return 0;
}
```
+ Xử lý khi chương trình gặp lỗi. Ví dụ về một lỗi khởi tạo một thiết bị X.
``` bash
#include <stdio.h>
void X_Start(int X_init)
{
    if (X_init == 0)
    {
        goto error;
    }
    else
        printf("initialize successful");
error:
    printf("fail");
}
int main()
{
    X_Start(0);
    return 0;
}
```
Việc sử dụng goto thường được xem là không tốt vì nó có thể làm cho mã nguồn trở nên khó đọc và khó bảo trì. Chỉ nên sử dụng trong các chương trình đơn giản. 
# Thư viện setjmp.h
## 2.1 Định nghĩa và cú pháp
Định nghĩa: Thư viện setjmp.h gồm hai hàm chính là setjmp và longjmp. Cách thức hoạt động giống với goto nhưng không bị giới hạn trong phạm vi của hàm trong file.  
+ setjmp(jmp_buf  env) -> Đánh dấu trạng thái vị trí hiện tại, ở lần gọi đầu tiên hàm trả về 0, các lần sau trả về giá trị của value.
+ longjmp(jmp_buf  env, int  value) -> nhảy về vị trí của setjmp và tiếp tục thực thi các câu lệnh phía sau. ( biến jmp_buf ).

Exception là các sự kiện người dùng không mong muốn có thể xảy ra trong quá trình chạy chương trình khiến cho chương trình hoạt động không ổn định hoặc dừng. Ví dụ:
+	Chia một số cho 0 (division by zero).
+	Truy cập mảng ngoài phạm vi (out of bounds array access).
+	Truy xuất con trỏ null (null pointer dereference).
+	Lỗi khi mở hoặc đọc tập tin (file not found).
+	Lỗi cấp phát bộ nhớ (bad allocation).

Exception Handling là một cơ chế xử lý các ngoại lệ giúp chương trình phản ứng với các Exception. Cơ chế Exception Handling thực hiện thông qua 3 từ khóa: TRY – CATCH – THROW.
+	TRY: Định nghĩa một khối lệnh có thể phát sinh lỗi.
+	CATCH: Xử lý ngoại lệ nếu có lỗi xảy ra.
+	THROW: Ném ra một ngoại lệ khi xảy ra lỗi.
``` bash
#include <stdio.h>
#include <setjmp.h>

jmp_buf buf;

int exception_code;

#define TRY if ((exception_code = setjmp(buf)) == 0)
#define CATCH(ErrorCodes) else if (exception_code == ErrorCodes)
#define THROW(ErrorCodes) longjmp(buf, ErrorCodes)

char *error_message[] = {
    "no error",
    "Lỗi đọc file: không có quyền đọc.",
    "Lỗi kết nối mạng: ping quá cao",
    "Lỗi tính toán: chia cho 0"};

typedef enum
{
    NO_ERROR,
    FILE_ERROR,
    NETWORK_ERROR,
    CALCULATION_ERROR
} ErrorCodes;

void readFile(char *name)
{
    char *file_name = "hala.c"; // "hala.c"
    printf("Đọc file...\n");
    if (name == file_name)
    {
        THROW(FILE_ERROR);
    }
    printf("Đọc file thành công");
}

void networkOperation(int ping)
{
    int network_ping = 500;
    printf("Kết nối Mạng...\n");
    if (ping == network_ping)
        THROW(NETWORK_ERROR);
    printf("Kết nối thành công");
}

void Tong(int a, int b)
{
    printf("a + b = %d\n", a + b);
}
void Hieu(int a, int b)
{
    printf("a - b = %d\n", a - b);
}
void Tich(int a, int b)
{
    printf("a * b = %d\n", a * b);
}
void Thuong(int a, int b)
{
    if (b == 0)
    {
        THROW(CALCULATION_ERROR);
    }
    printf("a / b = %f\n", a / b);
}

void calculateData(void (*pheptinh)(int, int), int a, int b)
{
    pheptinh(a, b);
}

int main(int argc, char const *argv[])
{
    exception_code = NO_ERROR;

    TRY
    {
        readFile("hala.c");
        networkOperation(100);
        calculateData(Thuong, 25, 0);
        calculateData(Tong, 25, 0);
    }
    CATCH(FILE_ERROR)
    {
        printf("%s\n", error_message[FILE_ERROR]);
    }
    CATCH(NETWORK_ERROR)
    {
        printf("%s\n", error_message[NETWORK_ERROR]);
    }
    CATCH(CALCULATION_ERROR)
    {
        printf("%s\n", error_message[CALCULATION_ERROR]);
    }
    printf("kết thúc chương trình");
    return 0;
}
```

Nên sử dụng thư viện setjump.h khi xử lý các lỗi phức tạp hơn goto, cần nhảy giữa các hàm để đảm bảo trạng thái chương trình.
