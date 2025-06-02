# 1. Stack
## 1.1 Khái niệm
Stack là một dạng cấu trúc dữ liệu có cơ chế hoạt động: LIFO – Last In First Out, tức là phần tử đi vào cuối cùng sẽ là phần tử đi ra đầu tiên. 
<p align = "center">
<img src="https://github.com/user-attachments/assets/7b1e1176-7a25-4c8a-9334-a6454c71a036" alt="image" width="450" height="350">  
  
## 1.2 Các thao tác cơ bản
Với top là vị trí hiện tại của stack ta có các thao tác:
+ push: Thêm một phần tử vào đỉnh của stack -> top++
+ pop: Lấy ra một phần tử ở đỉnh stack. -> top--
+ peek/top: để lấy giá trị của phần tử ở đỉnh stack.  
-> Kiểm tra Stack Full: top = size - 1  
-> Kiểm tra Stack Empty: top = -1
## 1.3 Tạo kiểu dữ liệu mô phỏng stack bằng ngôn ngữ C
+ Tạo kiểu dữ liệu lưu trữ các đặc tính của Stack.
```bash
#include <stdio.h>
#include <stdbool.h>
#include <stdlib.h>
#include <stdint.h>

#define STACK_EMPTY -1

typedef struct
{
    int size;  // Kích thước của stack
    int top;   // Vị trí đỉnh của stack
    int *item; // mảng lưu trữ giá trị stack
} Stack;
```
+ Hàm khởi tạo Stack.
```bash
// init
void stack_init(Stack *stack, int newSize)
{
    stack->item = (int *)malloc(newSize * sizeof(int));
    stack->top = STACK_EMPTY;
    stack->size = newSize;
    printf("Initialize successfull!\n");
}
```  
+ Các hàm kiểm tra trạng thái của Stack (Full, Empty).
```bash
// is empty
bool stack_empty(Stack stack)
{
    return (stack.top == STACK_EMPTY);
}

// is full
bool stack_full(Stack stack)
{
    return (stack.top == stack.size - 1);
}
```  
+ Các hàm thao tác với Stack.
```bash
// push
void stack_push(Stack *stack, int add_data)
{
    if (stack_full(*stack))
    {
        printf("Stack is full\n");
    }
    else
    {
        stack->top++;
        stack->item[stack->top] = add_data;
    }
}

// pop
int stack_pop(Stack *stack)
{
    if (stack_empty(*stack))
    {
        printf("Stack is empty\n");
        return STACK_EMPTY;
    }
    else
    {
        int value = stack->item[stack->top];
        stack->item[stack->top] = 0;
        stack->top--;
        return value;
    }
}

// top
int stack_top(Stack stack)
{
    if (stack_empty(stack))
    {
        printf("Stack is empty\n");
        return STACK_EMPTY;
    }
    return stack.item[stack.top];
}

// display stack
int stack_display(Stack stack)
{
    if (stack_empty(stack))
    {
        printf("Stack is empty\n");
        return STACK_EMPTY;
    }
    printf("\nStack display:\n");
    for (int i = 0; i < stack.size; i++)
    {
        printf("Value: %d - Address: %p \n", stack.item[i], &(stack.item[i]));
    }
    return 0;
}
```
+ Hàm chương trình main.c
```bash
int main()
{
    Stack stack1;
    stack_init(&stack1, 5);

    stack_push(&stack1, 1);
    stack_push(&stack1, 2);
    stack_push(&stack1, 3);
    stack_push(&stack1, 4);
    stack_push(&stack1, 5);

    stack_push(&stack1, 6);

    stack_display(stack1);
    printf("Top element: %d\n", stack_top(stack1));
    printf("Pop element: %d\n", stack_pop(&stack1));
    printf("Pop element: %d\n", stack_pop(&stack1));
    printf("Top element: %d\n", stack_top(stack1));

    return 0;
}
```
Kết Quả:
```bash
Initialize successfull!
Stack is full

Stack display:
Value: 1 - Address: 000002b2747213c0
Value: 2 - Address: 000002b2747213c4
Value: 3 - Address: 000002b2747213c8
Value: 4 - Address: 000002b2747213cc
Value: 5 - Address: 000002b2747213d0
Top element: 5
Pop element: 5
Pop element: 4
Top element: 3
```
# 2. Queue
## 2.1 Khái niệm
Queue là một dạng cấu trúc dữ liệu có cơ chế hoạt động: FIFO – First In First Out, tức là phần tử đi vào đầu tiên sẽ là phần tử đi ra đầu tiên. 
<p align = "center">
<img src="https://github.com/user-attachments/assets/e58ba96f-146f-422c-b608-70dda230ffa6" alt="image" width="450" height="350">  

## 2.2 Các thao tác cơ bản
Khác với stack, queue có hai thông số cần lưu ý là font và rear. Các thao tác cơ bản với queue bao gồm: 
+ enqueue: thêm phần tử vào cuối hàng đợi -> rear++
+ dequeue: xóa phần tử từ đầu hàng đợi. -> font++
+ front: đọc giá trị của phần tử đứng đầu hàng đợi. -> 
+ rear: đọc giá trị của phần tử đứng cuối hàng đợi. -> 
+ Kiểm tra hàng đợi rỗng. -> (font hoặc rear = -1) hoặc (font > rear) 
+ Kiểm tra hàng đợi full. -> (rear = size – 1)

## 2.3 Phân loại queue
### 2.3.1 Linear Queue
+ Trong Linear Queue, nếu rear đã đạt tới max, thì queue sẽ được coi là đầy và không thể thêm phần tử mới, ngay cả khi phía trước còn khoảng trống do các phần tử đã bị xóa.
+ Chỉ có thể thêm phần tử mới khi đã dequeue toàn bộ các phần tử hiện có (tức là queue rỗng hoàn toàn và front được reset về vị trí ban đầu).
Tạo kiểu dữ liệu Linear Queue bằng ngôn ngữ C:
+ Tạo kiểu dữ liệu lưu trữ các đặc tính của Queue.
```bash
#include <stdio.h>
#include <stdbool.h>
#include <stdlib.h>

#define IS_EMPTY -1

typedef struct
{
    int size;
    int font;
    int rear;
    int *item;
} Liqueue;
```  
+ Hàm khởi tạo Queue.
```bash
// init
void liqueue_init(Liqueue *liqueue, int newSize)
{
    liqueue->size = newSize;
    liqueue->font = -1;
    liqueue->rear = -1;
    liqueue->item = (int *)malloc(sizeof(int) * newSize);
    printf("initialyze successfull ! \n");
}
```  
+ Các hàm kiểm tra trạng thái của Queue (Full, Empty).
```bash
// is empty
bool liqueue_empty(Liqueue liqueue)
{
    return (liqueue.font == -1 || liqueue.font > liqueue.rear);
}

// is full
bool liqueue_full(Liqueue liqueue)
{
    return (liqueue.rear == liqueue.size - 1);
}
```  
+ Các hàm thao tác với Queue.
```bash
// dequeue
int liqueue_dequeue(Liqueue *liqueue)
{
    if (liqueue_empty(*liqueue))
    {
        printf("liqueue is empty \n");
        return (IS_EMPTY);
    }
    else
    {

        int value = liqueue->item[liqueue->font];
        liqueue->item[liqueue->font] = 0;

        if (liqueue->font == liqueue->rear && liqueue->rear == liqueue->size - 1)
        {
            liqueue->font = -1;
            liqueue->rear = -1;
        }
        else
        {
            liqueue->font++;
        }
        return value;
    }
}

// font
int liqueue_font(Liqueue liqueue)
{
    if (liqueue_empty(liqueue))
    {
        return (IS_EMPTY);
    }
    return liqueue.item[liqueue.font];
}

// rear
int liqueue_rear(Liqueue liqueue)
{
    if (liqueue_empty(liqueue))
    {
        return (IS_EMPTY);
    }
    return liqueue.item[liqueue.rear];
}
// display
void liqueue_display(Liqueue liqueue)
{
    if (liqueue_empty(liqueue))
    {
        printf("liqueue is empty \n");
        return (IS_EMPTY);
    }

    printf("display liqueue: \n");
    for (int i = 0; i <= liqueue.rear; i++)
    {
        printf("%d\n", liqueue.item[i]);
    }
    printf("\n");
}
```
+ Hàm trong main.c
```bash
int main()
{
    Liqueue liqueue;
    liqueue_init(&liqueue, 5);

    // check font rear
    printf("font: %d, rear: %d \n", liqueue_font(liqueue), liqueue_rear(liqueue));

    liqueue_enqueue(&liqueue, 1);
    liqueue_enqueue(&liqueue, 2);
    liqueue_enqueue(&liqueue, 3);
    liqueue_enqueue(&liqueue, 4);
    liqueue_enqueue(&liqueue, 5);

    liqueue_enqueue(&liqueue, 6); // full

    // check font rear
    printf("font: %d, rear: %d \n", liqueue_font(liqueue), liqueue_rear(liqueue));

    liqueue_display(liqueue);

    liqueue_dequeue(&liqueue);
    liqueue_dequeue(&liqueue);

    liqueue_display(liqueue);

    // check font rear
    printf("font: %d, rear: %d \n", liqueue_font(liqueue), liqueue_rear(liqueue));
}
```
Kết Quả:
```bash
initialyze successfull ! 
font: -1, rear: -1

liqueue is Full !!
font: 0, rear: 4
display liqueue:
1 2 3 4 5

display liqueue:
0 0 3 4 5
font: 2, rear: 4
```
### 2.3.2 Circular Queue
+ Tính chất của Circular Queue khá giống với Linear Queue nhưng khi khi rear đạt đến vị trí max, nếu font đã di chuyển (tức là có khoảng trống). -> Rear có thể quay vòng về vị trí 0 để tận dụng khoản trống.
<p align = "center">
<img src="https://github.com/user-attachments/assets/aa7b515a-24d8-46d7-a3ff-b298c59c91cf" alt="image" width="550" height="350">   
  
Tạo kiểu dữ liệu Circular Queue bằng ngôn ngữ C:
+ Tạo kiểu dữ liệu lưu trữ các đặc tính của Circular Queue.
```bash
#include <stdio.h>
#include <stdbool.h>
#include <stdlib.h>

#define IS_EMPTY -1

typedef struct
{
    int size;
    int font;
    int rear;
    int *item;
} Cirqueue;
```  
+ Hàm khởi tạo Circular Queue.
```bash
// init
void cirqueue_init(Cirqueue *cirqueue, int newSize)
{
    cirqueue->font = cirqueue->rear = -1;
    cirqueue->size = newSize;
    cirqueue->item = (int *)malloc(sizeof(int) * newSize);
    printf("initialyze successfull ! \n");
}
```  
+ Các hàm kiểm tra trạng thái của Circular Queue (Full, Empty).
```bash
// is empty
bool cirqueue_empty(Cirqueue cirqueue)
{
    return (cirqueue.font == -1);
}

// is full
bool cirqueue_full(Cirqueue cirqueue)
{
    return (cirqueue.font == ((cirqueue.rear + 1) % cirqueue.size));
}
```  
+ Các hàm thao tác với Circular Queue.
```bash
// enqueue
int cirqueue_enqueue(Cirqueue *cirqueue, int add_data)
{
    if (cirqueue_full(*cirqueue))
    {
        printf("cirqueue is Full !!\n");
        return 0;
    }
    if (cirqueue->font == -1)
    {
        cirqueue->font = cirqueue->rear = 0;
    }
    else if (cirqueue->rear == cirqueue->size - 1)
    {
        cirqueue->rear = 0;
    }
    else
    {
        cirqueue->rear++;
    }
    cirqueue->item[cirqueue->rear] = add_data;
    return 0;
}

// dequeue
int cirqueue_dequeue(Cirqueue *cirqueue)
{
    if (cirqueue_empty(*cirqueue))
    {
        printf("cirqueue is empty \n");
        return (IS_EMPTY);
    }
    else
    {
        int value = cirqueue->item[cirqueue->font];
        cirqueue->item[cirqueue->font] = 0;

        if (cirqueue->font == cirqueue->size - 1 && cirqueue->font > cirqueue->rear)
        {
            cirqueue->font = 0;
        }
        else
        {
            cirqueue->font++;
        }
        return value;
    }
}

// font
int cirqueue_font(Cirqueue cirqueue)
{
    if (cirqueue_empty(cirqueue))
    {
        return (IS_EMPTY);
    }
    return cirqueue.item[cirqueue.font];
}
// rear
int cirqueue_rear(Cirqueue cirqueue)
{
    if (cirqueue_empty(cirqueue))
    {
        return (IS_EMPTY);
    }
    return cirqueue.item[cirqueue.rear];
}

// display
int cirqueue_display(Cirqueue cirqueue)
{
    if (cirqueue_empty(cirqueue))
    {
        printf("liqueue is empty \n");
        return IS_EMPTY;
    }

    printf("display cirqueue: ");
    for (int i = 0; i < cirqueue.size; i++)
    {
        printf("%d  ", cirqueue.item[i]);
    }
    printf("\n");
    return 0;
}
```
+ Hàm trong main.c
```bash
int main()
{
    Cirqueue cirqueue;
    cirqueue_init(&cirqueue, 5);
    printf("font: %d, rear: %d \n", cirqueue_font(cirqueue), cirqueue_rear(cirqueue));

    cirqueue_enqueue(&cirqueue, 1);
    cirqueue_enqueue(&cirqueue, 2);
    cirqueue_enqueue(&cirqueue, 3);
    cirqueue_enqueue(&cirqueue, 4);
    cirqueue_enqueue(&cirqueue, 5);

    // display cirqueue
    cirqueue_display(cirqueue);
    printf("font: %d, rear: %d \n", cirqueue_font(cirqueue), cirqueue_rear(cirqueue));

    printf("\ndequeue now!! \n");
    cirqueue_dequeue(&cirqueue);
    cirqueue_dequeue(&cirqueue);
    cirqueue_dequeue(&cirqueue);
    cirqueue_dequeue(&cirqueue);
    cirqueue_display(cirqueue);
    printf("font: %d, rear: %d \n", cirqueue_font(cirqueue), cirqueue_rear(cirqueue));

    printf("\nenqueue now!! \n");
    cirqueue_enqueue(&cirqueue, 1);
    cirqueue_enqueue(&cirqueue, 2);
    cirqueue_enqueue(&cirqueue, 3);
    cirqueue_display(cirqueue);
    printf("font: %d, rear: %d \n", cirqueue_font(cirqueue), cirqueue_rear(cirqueue));

    printf("\ndequeue now!! \n");
    cirqueue_dequeue(&cirqueue);
    cirqueue_dequeue(&cirqueue);
    cirqueue_display(cirqueue);
    printf("font: %d, rear: %d \n", cirqueue_font(cirqueue), cirqueue_rear(cirqueue));
}
```
Kết Quả:
```bash
initialyze successfull ! 
font: -1, rear: -1 
display cirqueue: 1  2  3  4  5  
font: 0, rear: 4 

dequeue now!! 
display cirqueue: 0  0  0  0  5
font: 4, rear: 4

enqueue now!!
display cirqueue: 1  2  3  0  5
font: 4, rear: 2

dequeue now!!
display cirqueue: 0  2  3  0  0
font: 1, rear: 2
```
