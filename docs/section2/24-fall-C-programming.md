# 24 fall C programming course notes

这是我24fall 程序设计课程所使用的笔记.  :smile:

由于这是从Obsidian库直接搬来的，部分链接404. :cry:

## Struct and linked list

### Ex.1 Basic struct

```c
#include <stdio.h>

struct dog {
    int age;
    void (*bark)(int);
} dog1;
// we can define struct here, but the bark function must be identified before that.

void bark(int age) {
    printf("woof at age%d\n", age);
}

int main() {
    struct dog dog = {3, bark};
    // struct name can be the same as struct variance name.
    
    struct dog* dogptr = &dog;
    
    printf("the size of a struct ptr is %d bytes.\n", (int)sizeof(dogptr));
    // 8 bytes.
    
    (*dogptr).bark(2);
    // . is prior to *, we need a ().
    
    dog.bark(dog.age);
    
    return 0;
}
```

### Ex.2 Forward declaration

```c
#include <stdio.h>

// 前向声明结构体
struct Date;  // 只声明，不定义

// 定义另一个结构体 "Person"，里面包含 "Date" 类型的指针
struct Person {
    char name[50];
    struct Date *birthdate;  // 声明 Date 类型的指针
};

/*
事实上， 结构体还可以嵌套定义
struct outer_struct {
    struct inner_struct{
    };
};

但是 inner 只能在outer作用域内访问。
struct Person {
    char name[50];
    struct Address {  // 错误：Address 结构体的定义仅限于 Person 内部
        char street[100];
        char city[50];
    } address;
};

// 此处不能再定义 Address 类型的变量，因为它的作用域仅限于 Person 内部
struct Address addr;  // 编译错误


*/
// 定义结构体 Date
struct Date {
    int day;
    int month;
    int year;
};

int main() {
    struct Date d1 = {15, 5, 1990};
    struct Person p1 = {"John Doe", &d1};  // 初始化 Person，传入 Date 指针
    
    // 访问嵌套结构体成员
    printf("Name: %s\n", p1.name);
    printf("Birthdate: %d/%d/%d\n", p1.birthdate->day, p1.birthdate->month, p1.birthdate->year);

    return 0;
}

```

### Ex.3 Struct array

```c
struct student {
    int num;
    char name[20];
    char sex;
} stu[100];
// here we define a struct array, or we define it later. We can also initialize it.
struct student st[2] = {{1, "Kobe", 'M'}};
// here all members of st[1] are 0 or "\0";

// use a ptr;
struct student* stuptr = st;

printf("%s\n", (*stuptr).name);
// remember ().
```

### Ex.4 Struct and function

```c
#include <stdio.h> 
int dayTable[ ][12]={ {31,28,31,30,31,30,31,31,30,31,30,31}, /* 平年 */ {31,29,31,30,31,30,31,31,30,31,30,31}};/* 闰年 */ 

struct Date { 
    int day; 
    int month; 
    int year; 
    int yearDay; 
} date; 
/* 定义一个Date结构类型的结构变量 */ 

/* 计算年中第几天函数 */ 
void dayofYear(struct Date* date) { 
    int i, leap; 
    date->yearDay = date->day; 
    leap = (date->year%4 == 0 && date->year%100) || date->year%400 == 0; 
    /* 是否闰年 */ 
    for (i = 0; i < date->month - 1; i++) 
        date->yearDay += dayTable[leap][i]; 
} 
    
    
int main() { 
    int leap, days; 
    printf("\tDate Conversion Program\n"); 
    printf("Year = "); 
    scanf("%d", &date.year); 
    /* 输入年份 */ 
    for (; ;) { 
        printf("Month = "); 
        scanf("%d",&date.month); 
        if (date.month >= 1 && date.month <= 12) 
        break; 
        printf("输入的月份必须在 1 到 12 之间\n" ); 
    } 
    /* leap=1是闰年，leap=0 不是闰年 */ 
    leap = (date.year%4 == 0&&date.year%100)||date.year%400==0; 
    
    days = dayTable[leap][date.month-1]; 
    /* 输入日期，并检查是否输入正确 */ 
    
    for (; ;) { 
    printf("Day = "); 
    scanf("%d", &date.day); 
    if (date.day >= 1 && date.day <= days) 
    break; 
    printf("输入的天必须在 1 到 %d 之间\n",days); 
    } 
    /* 调用dateofYear函数，实参为结构date的3个成员 */
    dayofYear(&date); 
    printf("The days of the year are: 
    %d\n",date.yearDay); 
    return 0;
} 

// 结构也可以作为函数参数
// 函数以结构作为形参，不能改变实参。
    d.yearDay = day.
// 此时一般采用结构指针作为参数（不安全， 效率高）
// 或者返回一个新的结构(效率低， 安全)
struct date dayofYear(struct date d){
//change member of d;
    return d;
} 
    date = dayofYear(date);
// update "date".

```

### Ex.5 Linked list basics

```c
#include <stdio.h>
#include <stdlib.h>

struct intNode {  
    int val;
    struct intNode* next;
};

struct stringNode {
    char str[40];
    struct stringNode* next;
};

//print linked list
void print_list(struct intNode* p) {
    if (p == NULL){
        printf("Empty list!\n");
        return;
    }
    int cnt = 1;
    while(p) {
        printf("The %d element of list is %d", cnt, p -> val);
        p = p -> next;
        cnt++;
    }
}

//define a function to create an intNode.

struct intNode* create_node(int val, struct intNode* next){
    struct intNode* p = (struct intNode*) malloc(sizeof(struct intNode));
//malloc(n * sizeof(type)) equals to calloc(n, sizeof(type));
    if (p == NULL){
        printf("Fail to malloc!\n");
    }
    p -> val = val;
    p -> next = next;
    return p;
}


struct stringNode* create_string_node(char* str, struct stringNode* next){
    struct stringNode* p = (struct stringNode*) malloc(sizeof(struct stringNode));

    // p -> str = str;
    strcpy(p -> str, str);
    // Notice, char* = char[] is wrong.
    
    p -> next = next;
    return p;
}

//input n numbers, create sorted list;

struct intNode* createSortList(int n) {
    printf("You are creating a %d sort list!\n", n);
    struct intNode* p, *tmp , *head = NULL;
//here, only head is initialized;
    for (int k = 1; k <= n; k++){
        int cur;
        printf("Enter the %d element!\n", k);
        scanf("%d", &cur);
        p = head;
        while (p && p -> val < cur) {
            tmp = p;
            p = p -> next;
        }
        if (p == head) {
            tmp = head;
            head = create_node(cur, tmp);
        }
        else {
            tmp -> next = create_node(cur, p);
        }
    }
    return head;
}

struct intNode* createSortList(int* list, int size) {
    struct intNode* p, *tmp , *head = NULL;
    for (int k = 0; k < size; k++){
        int cur = list[k];
        p = head;
        while (p && p -> val < cur) {
            tmp = p;
            p = p -> next;
        }
        if (p == head) {
            tmp = head;
            head = create_node(cur, tmp);
        }
        else {
            tmp -> next = create_node(cur, p);
        }
    }
    return head;
}
//copy h to p, where p is a new head node ptr.

struct intNode* copy_node(struct intNode* h){
    if (h == NULL) return NULL;
    return create_node(h -> val, copy_node(h -> next));
}

//有序链表查找.减少遍历时间.
struct intNode* searchSolink(struct intNode* h, int key){
    struct intNode* cur = h;
    while (cur && cur -> val < key) {
        cur = cur -> next;
    }
    return cur && cur -> next == key ? cur : NULL;
}

//指定节点插入，包含无序遍历，只插入一次.

void insert(struct intNode* h, int key, int num){
    struct intNode* cur = h;
    while (cur && cur -> val != key){
        cur = cur -> next;
    }
    if (cur == NULL){
        printf("Key not found.\n");
    }
//找到节点，在后面插入新节点
    cur -> next = create_node(num, cur -> next);
}
//单链表删除指定表元，返回头指针.（删除所有）

struct intNode* del(struct intNode* h, int key) {

    struct intNode* p = h;
//删除连续的头节点
    while (h && h -> val == key){
        struct intNode* temp = h;
        h = h -> next;
        free(temp);
    }

    p = h;
//p是第一个不等于key的节点
    while (p && p -> next){
        if (p -> next -> val == key){
            struct intNode* temp = p -> next;
            p -> next = temp -> next;
            free (temp);
        }
//只有在没有删除p的下一个节点时才更新p， 否则不能删除连续的节点。

        else p = p -> next;
    }
    return h;
}

//create a single list with continuous integers 1 to n, assert n >= 1;
struct intNode* create_integer_list(int n) {
    struct intNode* p = create_node(1, NULL);
    struct intNode* h = p;
    for (int k = 1; k <= n - 1; k++) {
        p -> next = create_node(k + 1, NULL);
        p = p -> next;
    }
    return h;
}


//A solution for copying using while iteration,

struct intNode* another_copy_node(struct intNode* h){
    if (h == NULL) return NULL;
// copy h, so that h would not be moved.
    struct intNode* tmp = h;
// create head node ptr p;
    struct intNode* p = create_node(h -> val, NULL);
// save a copy of p, which would be returned;
    struct intNode* res = p;
// move to second ptr
    tmp = tmp -> next;
    while (tmp){
        p -> next = create_node(tmp -> val, NULL);
        p = p -> next;
        tmp = tmp -> next;
    }
    return res;
}


int main() {

//Create a linked list.

// 1 -> 2 -> NULL;
    struct intNode* myNode = create_node(1, create_node(2, NULL));
    free (myNode);
    return 0;
}
```

### Ex.6 Exercises of linked list

- [https://leetcode.cn/problems/design-linked-list/description/](https://leetcode.cn/problems/design-linked-list/description/)
- [https://leetcode.cn/problems/remove-linked-list-elements/](https://leetcode.cn/problems/remove-linked-list-elements/)
- [https://leetcode.cn/problems/reverse-linked-list/](https://leetcode.cn/problems/reverse-linked-list/)
- [https://leetcode.cn/problems/swap-nodes-in-pairs/](https://leetcode.cn/problems/swap-nodes-in-pairs/)
- [https://leetcode.cn/problems/remove-nth-node-from-end-of-list/](https://leetcode.cn/problems/remove-nth-node-from-end-of-list/)
- [https://leetcode.cn/problems/intersection-of-two-linked-lists-lcci/](https://leetcode.cn/problems/intersection-of-two-linked-lists-lcci/)
- [https://leetcode.cn/problems/linked-list-cycle-ii/](https://leetcode.cn/problems/linked-list-cycle-ii/)
- [https://leetcode.cn/problems/rotate-list/description/](https://leetcode.cn/problems/rotate-list/description/)

#### 1 [loop pop](loop.c)

```C
// make a loop list .I n this case , enter n, m. 
// where n is length of loop list. Every mth member of the list pop out.
// print the order.

#include <stdlib.h>
#include <stdio.h>
struct intNode {
    int val;
    struct intNode* next
};

struct intNode* create_node(int val, struct intNode* next) {
    struct intNode* p = (struct intNode*) malloc (sizeof(struct intNode));
    p -> next = next;
    p -> val = val;
    return p;
}

struct intNode* makeloop(int n) {
    struct intNode* head = create_node(1, NULL);
    struct intNode* p = head;
    for (int i = 2 ; i <= n; i++){
        p -> next = create_node(i , NULL);
        p = p -> next;
    }
    p -> next = head;
    return head;
}

// To change the head pointer , we pass the address of pointer.

void operate(struct intNode** h){
    struct intNode* p = *h;
    int cnt = 0;
    for (; cnt < m - 2; cnt++) {
        p = p -> next;
    }
    struct intNode* v = p -> next;
    printf("Number %d out!\n", v -> val);
    p -> next = v -> next;
    free(v);
    (*h) = p -> next;
}

int cond(struct intNode** h){
    if ((*h) -> next == (*h)){
        printf("Number %d out\n", (*h) -> val);
        return 1;
    }
    return 0;
}

int main() {
    int m , n;
    printf("Enter n : \n");
    scanf("%d", &n);
    printf("Enter m : \n");
    scanf("%d", &m);
    struct intNode* list = makeloop(n);
    while(1){
        operate(&list);
        if (cond(&list)) break;
    }
    return 0;
}
```

---
#### 2 [poly add](poly.c)

```C
// 多项式相加, 不要求输入降序排列，输出降序排列

```
### Ex.7 Union
 联合的特点：
  1. 一个联合可存放多种不同类型的数据，但在每一具体时 刻只能存放其中一种数据，不能同时存放多种数据。 
  2. 联合变量中起作用的成员是最后一次存放的成员。 例如： x.ival = 1; x.fval = 2.0; x.chval= '?'; 121 说明：只有 x.chval 是有效的，而 x.ival 及 x.fval 引用其值已经变成不确定的了。 
  3. 联合变量的开始地址和它的各成员变量的开始地址 都是相同的。例如：&x，&x.ival，&x.chval 都是同一 地址值。 
  4. 对联合的初始化只能对其成员表中列举的第一个成员置初值。 
  5. 函数的形参不能是联合类型，函数的结果也不能是 联合类型。但指向联合的指针可以作为函数形参，函 数也可以返回指向联合的指针。 
  6. 联合可以嵌套在结构中。
  7. 联合的大小一般等于最大数据的大小。
```C
#include <stdio.h>

union Data{
    int ival;
    char chval;
    float fval;
}x, y ,z;

int main() {
    x.ival = 1;
    x.fval = 2.0;
//只有fval有效，
    return 0;
}
```


### Ex.8 [Bit-fields](bitfield.c)

精确控制结构成员占用位数
```C
#include <stdio.h>

struct Example {
    unsigned int a : 5;  // 5 bits for 'a'
    unsigned int b : 3;  // 3 bits for 'b'
    unsigned int c : 4;  // 4 bits for 'c'
};

int main() {
    struct Example ex;
    
    ex.a = 15;   // 5 bits can hold values from 0 to 31 (binary: 1111)
    ex.b = 5;    // 3 bits can hold values from 0 to 7 (binary: 101)
    ex.c = 9;    // 4 bits can hold values from 0 to 15 (binary: 1001)
    
    printf("a = %u, b = %u, c = %u\n", ex.a, ex.b, ex.c);
    printf("sizeof(struct Example) = %zu\n", sizeof(ex));
//这里的size和对齐有关。
    return 0;
}
```


用二进制的一位或连续若干位代表不同属性的状态。
例如： 某台计算机配置的磁盘机中的控制状态寄存器的字长为16 位(自右至左，第0位至第15位)。设其中某些位的意义如下：
第15位: 置 1 表示数据传送发生错误； 
第7位 : 置 1 表示设备已准备好，可传送数据； 
第6位 : 置 1 允许响应中断； 
第2位 : 置 1 表示读； 
第1位 : 置 1 表示写。 
为实现上述要求可以给对应字中的某些二进位定义一 系列表示特征的代码。

```C
#include <stdio.h>
#include <string.h>


struct ControlStatus {
    unsigned int error : 1;    // 第15位: 数据传送发生错误
    unsigned int reserved1 : 8; // 保留位 8-14
    unsigned int ready : 1;    // 第7位: 设备已准备好
    unsigned int interrupt_enable : 1; // 第6位: 允许响应中断
    unsigned int reserved2 : 3; // 保留位 3-5
    unsigned int read : 1;     // 第2位: 读
    unsigned int write : 1;    // 第1位: 写
    unsigned int reserved3 : 1; // 第0位: 保留
};

int main() {
    struct ControlStatus reg = {0}; // 初始化寄存器为0

    // 设置状态
    reg.error = 1;        // 数据传送发生错误
    reg.ready = 1;        // 设备已准备好
    reg.interrupt_enable = 1;  // 允许响应中断
    reg.read = 1;         // 读操作
    reg.write = 1;        // 写操作

    // 打印寄存器的每一位的状态
    printf("Error: %u\n", reg.error);
    printf("Ready: %u\n", reg.ready);
    printf("Interrupt Enable: %u\n", reg.interrupt_enable);
    printf("Read: %u\n", reg.read);
    printf("Write: %u\n", reg.write);

    // 打印寄存器的二进制表示
    unsigned int reg_value;
    // 通过内存拷贝将结构体内容复制到一个标准类型中
    memcpy(&reg_value, &reg, sizeof(reg_value));
    
    printf("Control Register (in binary): ");
    for (int i = 15; i >= 0; i--) {
        printf("%d", (reg_value >> i) & 1);
    }
    printf("\n");

    return 0;
}

```
说明： 
1. 一个字段只能在同一个整数字中，即限制字段不能跨 越整数字的边界。如果剩余的位太少不够下一个字段时， 下一个字段将占用下一个整数字。 
2. 字段可以不命名，称作无名字段，但无名字段仅用于 填充。 
3. 使用字段时，要注意具体机器分配字段的方向，有的 从左向右，也有的从右向左。 
4. 不能对字段施取地址运算(&)。

### Ex.9 [Enum](enum.c)


```C
#include <stdio.h> 

enum gender{MALE, FEMALE};

int main () {
     enum gender g;
     g = MALE;
     //编译时按照0， 1 给枚举常量赋值，而程序不能给他赋值。
     //但是可以 enum gender{MALE = 1, FEMALE}这样FEMALE是2
     //枚举变量与常量或整数可以做关系比较。
     //整数可以赋值给枚举变量但是不能赋值给枚举常量
     g = 1;
     return 0;
}
```
其实可以认为枚举常量就是一系列整数常量。

### Ex.10 Typedef
```C
#include <stdlib.h>

typedef int INTEGER;

typedef struct stt{
    int num;
    char name[20];
    int score;
}STDTYPE;

STDTYPE std1;
struct stt std2;

typedef int INTARRAY[20];

typedef enum{RED,YELLOW}COLOR;

INTARRAY v1;
COLOR c1;
```

### Ex.11 Variance definition

```C
//存储类
auto register extern static 缺省

//类型限定
类型名 ， typedef ， typedef 类型名
int
typedef int INT
typedef struct {

}a_struct;

//静态变量或者全局变量，自动初值0.
//对结构或者数组，用初值表，比成份个数多错误，少用0 填补
``` 

## Pointer

### Ex.1 Basics
地址一般是十六进制无符号整数。
& is an address operator 
只有全局或者静态才会赋值初值NULL
一个指针变量只能指向同一个类型的变量。
```C
//定义和间接引用指针变量的时候* 的含义不同，比如
int i, *ip = &i;
    //ip被赋值，一个地址
* ip = i;
//这里是一个值



//运算顺序，单目运算符*， &， ++， -- 是从右向左结合的。

int i, j, *ip = & i;

j == ++*ip
//i++ 以后赋给j

j = *ip ++;
//ip++ 但是返回值是ip， 所以j变成i，但是ip不再是i；

j = (*ip)++
//j = i ;
//i++

```

### Ex. 2 array and pointer

```C

int a[100], *p;
p = &a[0];

//p是a[0]的地址

//当指针指向同一个数组的元素的时候， 这两个指针的关系比较就有意义.

int* p = &a[0];
int* q = &a[1];

p < q is true

//指针进行加减运算

p = &a[0];
*(p + 1) 就是 a[1] 

int a[100], b[100], *p, *q;
for (p = a; p < a + 100; ) {
    scanf("%d", p++);
}

for (p = a, q = b; p < a+100){
    *q++= *p++;
}

//这里的a, b是什么？
//是代表数组第一个元素的是指针，是常量。


//字符串指针
//注意， 字符串需要结束符。

char s[] = "I am a string.";
//其实s有15个元素，包括最后的'\0'


//注意，字符串数组的特殊写法，可以把字符串常量赋值给字符指针

char *cp1, *cp2 = "I am a string";

cp1 = "Another string";
//但是， 这样的字符串常量不能修改
*cp = 'a';
是未定义行为。

char s[] = "Anotehr";
char* p = s;
这样可以修改

printf("%s",s);
同时也适用于p, 从p指向的第一个字符开始输出到\0


//copy a string, where from is a first pointer of a string ,to is what we gonna copy to

while(*to++ = *from++);

//把s中某个字符去掉
//双指针

char* p , *q = s;

while (*p){
    if (*p != target) {
        *q = *p;
        q++;
    }
    p++;
}

*q = '\0';

```

### Ex. 3 pointer as parameter

普通变量(variable) 按值调用
指针作为函数参数按地址调用

### Ex. 4 array parameter
```C

int sum(int a[], int n){
    int i, s;
    for (s = i = 0; i < n; i++){
    s += a[i];
    }
    return s;
}

//一般来说，形参a被传入了一个数组的首地址.
int x[] = {1, 2, 3, 4, 5};
int i, j;
i = sum(x, 5);
j = sum(&x[2], 3);

//一般需要一个整数表示size

//数组形参也可以是多维数组，数组形参多维，除了第一维可以不指定大小，其他都要明确制定。

void sumAToB(int a[][10], int b[], int n){
    int i, j;
    for (i = 0; i < n; i++){
        for (b[i] = 0, j = 0; j < 10; j++){
        b[i] += a[i][j];
        }
    }
}

//注意以下这几种形参的写法
void test(int** a)

这里不能传入数组比如a[10][10]的a
必须malloc

void test(int a[][10])
则可以。

但是如果是简单的一维数组
void test(int* a)
int a[10] = {0};
可以传入a
```

```C
//the maximum element in an array

int maxInArray(int a[], int n){
    int i, m = 0;
    for (i = 1; i < n; i++){
        if (a[m] < a[i]){
             m = i;
        }
    }
    return a[m];
}

```

```C
//strcpy
void strcpy(char* to, char* from) {
    while(*to++ = *from++);
}
//这里可以把'\0'也一起cpy

```

```C
//strcmp
int my_strcmp(char* s, char* t){
    while(*s ++ == *t ++){
        if(*s == '\0'){
            return 0;
        }
    }
    return (int)(*s - *t);
}
```
### Ex.5 pointer array
```C
int *p[10];
//10个元素，每一个都是可以指向整数的指针

//注意，下面的p是一个指向数组的指针，也可以理解为二维数组
int (*p)[10];

int a[3][10]= {1};
p = a;


int a, b, c;
int* ap[] = {&a, &b, &c};
for (k = 0; k < 3;k ++) {
    printf("%d\t", *ap[k]);
    //或者**(ap + k)
}
```

```C
#include <stdio.h> 
#define N sizeof ap/sizeof ap[0] 
/* 整个数组占用的字节数÷一个元素占用的字节数 得到该数组有多少个元素 */ 
int a, b, c, d, e, f; 
int main() { 
    int *ap[ ] = { &a, &b, &c, &d, &e, &f }; 
    int k, j, t; 
    printf ( "Enter a, b, c, d, e, f.\n" ); 
    for ( k = 0; k < N; k++ ) 
        scanf( "%d",ap[k] ); 
    for ( k = 0; k < N; k++ ){
        for ( j = 0; j < N - k - 1; j++){
            if(*ap[j] > *ap[j+1]){
                t = *ap[j];
                *ap[j] = *ap[j+1];
                *ap[j+1} = t;
            /*
            int* t = ap[j];
            ap[j] = ap[j + 1];
            ap[j+1] = t;
            */
            }
        }
    for (k = 0; k < N; k++)
        printf("%d\t", *ap[k]);
    return 0;
}
```
### EX. 6 function pointer and function which return a pointer
```C
int *f(int x, int y)
// a function that return an int pointer
int (*f)(int x, int y)
// f is a pointer that points at a function
```

### Ex. 7 string and pointer
```C
//首先， 字符串有两种定义方式
char* s1 = "string";
char s2[] = "string";

//这里s1是指向首字母的指针，s2是一个字符数组
//但是
*s1 == 's';
*s2 == 's';
//当且仅当字符数组自身独立作为表达式的时候，才会把字符数组自动转换成指向数组首元素的指针
//s1 = s2也是可以的

//那字符指针可不可以看作字符数组呢？某种意义上可以
(s1 + i)[0] == s1[i] == s2[i]

s1++;
//现在s1指向't'
s2++;
//报错，不能修改字符数组

char** p1 = &s1;
//p1是s1的地址，就是首字母的指针的指针
char (*p2)[7] = &s2;
//p2是指向数组的指针
char** p3 = &s2;
//报错，对字符数组取地址得到的是一个指向数组的指针而非char**

p1++;
//现在p1指向s1的地址的下一个地址，具体不知道
(*p1)++;
//s1被++，p1还是指向首字母的地址
(*p2)[0]
//这个东西是's'

//现在写一个函数交换两个字符

void swap_string(char* s1, char s2[]){
    char* tmp = s1;
    s1 = s2;
    //报错，字符数组不能赋值给字符指针
    s2 = tmp;
    //报错，字符指针不能赋值给字符数组
}

//应该先把s2变得可以修改
char* s2p = s2;
void swap_string(char* s1, char* s2){
    char* tmp = s1;
    s1 = s2;
    s2 = tmp;
}
swap(s1, s2p);
//但是没什么卵用，因为函数实参给形参是值传递

//函数调用的过程：先给形参分配空间，在把实参的值传进来，操作一波，返回，最后释放掉空间
//也就是说，这里s1,s2p没有被修改

void swap_string(char** s1pp,char** s2pp){
    char* tmp = *s1pp;
    *s1pp = *s2pp;
    *s2pp = tmp;
}
//才是对的
swap_string(&s1, &s2pp);

```
## Function 

### Ex. 1 Basics
### Ex.2 Library function
```C
#include <stdio.h>
#include <time.h>
#include <stdlib.h> 

int main(){
    int k;
    long long now;]
    srand(time(&now));
    // srand(time(NULL));
    for (k = 0; k < 10; k++) {
        printf("%d\n", rand()% 10);
    }
    return 0;
}
```

```C
int majorityElement(int* nums, int numsSize) {
    srand(time(NULL));

    while (1){
        int cur = nums[rand() % (numsSize)];
        int cnt = 0;
    for (int n = 0 ; n < numsSize; n++){
        if (nums[n] == cur) cnt++;
        }
    if (cnt > numsSize / 2) return cur;
    }
    return -1;
}
```
### Ex. 3 function definition
```C
//gcd

int gcd(int a, int b){
    if (a == b) return b;
    return a > b ? gcd(b, a - b) : (a, b - a);
// gcd(a, b) = gcd(min , max - min);

}

int gcd(int a, int b) {
    int r;
    while(r = a % b){
        a = b;
        b = r;
    }
    return b;
}

int gcd(int a, int b) {
    if (a == 0) return b;
    return gcd(b, a % b);
}

//实参传到形参是单向值传递

//C语言函数（）里的实参求值顺序并不确定


```
### Ex. 4 Function forward declaration 

```C
double min(double, double);
//or double min (double a, double b)
int main(){
}


double min(double a, double b){

}


```
### Ex.5 Recursion
```C
注意递归调用的深度问题，
```
[fib](24fall_C_programming/lab10/fib.c)

### Ex.6 Sort
基于比较的排序 选择排序、插入排序、归并排序、堆排序、快速 排序、壳排序（shellsort）、冒泡排序、二叉搜索树排序… 
基于非选择的排序 计数排序、桶排序、基数排序（radix sort）…
```C
//bubble sort
void bubble(int* list, int n) {
    for(int i = 0 ; i < n; i++){
        bool swapped = false;
        //提前退出机制
        for (int j = 0 ; j < n - i - 1; j++){
            if(list[j] > list[j + 1]){
                 int temp = list[j];
                 list[j] = list[j + 1];
                list[j + 1] = temp;
                swapped = true;
            }
        }
        if(!swapped) break;
    }
}

//selection sort
void selection(int* list, int n) {
    for (int i = 0 ; i < n - 1; i++){
        for(int j = i + 1; j < n; j++){
            if (list[j] < list[i]){
                int temp = list[j];
                list[j] = list[i];
                list[i] = temp;
            }
        }
    }
}

// insertion sort
void insertionSort(int* list, int n) { 
    for (int i = 1; i < n; i++) { 
        int key = list[i]; 
        // 当前待插入的元素 
        int j = i - 1; 
        // 将已排序部分大于 key 的元素移动到后面 
        while (j >= 0 && list[j] > key) { 
            list[j + 1] = list[j]; 
            // 移动元素 
            j--; 
            } 
        // 找到合适的位置，将 key 插入 
            list[j + 1] = key; 
        } 
    }
}
// shell sort
void shellSort(int* arr, int n) {
    // 初始增量设置为 n/2，每次减半
    for (int gap = n / 2; gap > 0; gap /= 2) {
        // 插入排序，gap 表示间隔
        for (int i = gap; i < n; i++) {
            int temp = arr[i];
            int j = i;
            
            // 将 arr[i] 插入到已经排好序的子序列中
            while (j >= gap && arr[j - gap] > temp) {
                arr[j] = arr[j - gap];
                j -= gap;
            }
            
            arr[j] = temp;
        }
    }
}

//merge sort

// 合并两个已排序的子数组
void merge(int* arr, int left, int mid, int right) {
    int n1 = mid - left + 1;
    int n2 = right - mid;

    // 创建临时数组
    int* L = (int*)malloc(n1 * sizeof(int));
    int* R = (int*)malloc(n2 * sizeof(int));

    // 将数据复制到临时数组 L[] 和 R[]
    for (int i = 0; i < n1; i++) {
        L[i] = arr[left + i];
    }
    for (int j = 0; j < n2; j++) {
        R[j] = arr[mid + 1 + j];
    }

    // 合并临时数组 L 和 R 回原数组 arr
    int i = 0, j = 0, k = left;
    while (i < n1 && j < n2) {
        if (L[i] <= R[j]) {
            arr[k] = L[i];
            i++;
        } else {
            arr[k] = R[j];
            j++;
        }
        k++;
    }

    // 如果还有剩余元素在 L 中，拷贝到 arr
    while (i < n1) {
        arr[k] = L[i];
        i++;
        k++;
    }

    // 如果还有剩余元素在 R 中，拷贝到 arr
    while (j < n2) {
        arr[k] = R[j];
        j++;
        k++;
    }

    // 释放临时数组
    free(L);
    free(R);
}

// 递归地进行归并排序
void mergeSort(int* arr, int left, int right) {
    if (left < right) {
        int mid = left + (right - left) / 2;

        // 递归排序左半部分
        mergeSort(arr, left, mid);

        // 递归排序右半部分
        mergeSort(arr, mid + 1, right);

        // 合并排序好的左右部分
        merge(arr, left, mid, right);
    }
}


int partition(int* arr, int low, int high) {
    int pivot = arr[high];  // 选择最右边的元素作为基准
    int i = low - 1;  // i 是小于基准元素的子数组的最后一个元素的索引

    // 遍历数组，按基准分区
    for (int j = low; j < high; j++) {
        if (arr[j] <= pivot) {
            i++;  // 如果当前元素小于或等于基准，i 增加
            // 交换 arr[i] 和 arr[j]
            int temp = arr[i];
            arr[i] = arr[j];
            arr[j] = temp;
        }
    }

    // 将基准元素放到正确的位置
    int temp = arr[i + 1];
    arr[i + 1] = arr[high];
    arr[high] = temp;

    return i + 1;  // 返回基准元素的索引
}

// 快速排序函数
void quickSort(int* arr, int low, int high) {
    if (low < high) {
        // 对分区后的数组递归排序
        int pi = partition(arr, low, high);  // 基准元素的索引

        // 递归对基准元素左侧和右侧的子数组进行排序
        quickSort(arr, low, pi - 1);
        quickSort(arr, pi + 1, high);
    }
}





```

### Ex. 7 Exercises
[hanoi](hanoi.c)
```C
//Hanoi tower problem;
#include <stdio.h>
//solve the problem from a to b, via c;
void hanoi(int n, char a, char b, char c, int* cnt){
    if (n == 1) {
        move(n, a, b);
        return;
    }
    hanoi(n - 1, a, c, b);
    move(n, a, b, cnt);
    hanoi(n - 1, c, a, b);
}

void move(int n, char a, char b,int* cnt) {
    printf ("Oper %d : Move disk %d from %c to %c.\n", cnt, n, a, b);
    (*cnt)++;
}

int main(){
    hanoi(10, 'A', 'B', 'C', &0);
    return 0;
}
```

```C
//calculate C(n, m);
long long com(int n , int m){
    if(m == 0) return 1;
    if(n == m) return 1;
    return com(n - 1, m) + com(n - 1, m - 1);
}
```

```C
//palindrome

int palindrome(int n) {
    int m , s;
    m = n;
    s = 0;
    while (m){
        int temp = m % 10;
        m /= 10;
        s = s * 10 + temp;
    }
    return s == n;
}
```

```C
//quickpow
//normally we consider a big prime as module
int power(int x, int y){
    if (y == 1) return x;
    if (y % 2 == 0){
        int j = power(x, y / 2);
        return j * j;
    }
    else {
        return x * power(x , y - 1);
    }
}
```

### Ex. 8 Storage type and scope

标识符的属性，存储类别， 存储期， 作用域等；

#### storage class
auto  , register, extern , static

A storage class in C is used to describe the following things:
1. The variable scope.
2. The location where the variable will be stored.
3. The initialized value of a variable.
4. A lifetime of a variable.
5. Who can access a variable.


| Storage class | Purpose                                                                                                             |
| ------------- | ------------------------------------------------------------------------------------------------------------------- |
| auto          | It is a default storage class.                                                                                      |
| extern        | It is a global variable.                                                                                            |
| static        | It is a local variable which is capable of returning a value even when control is transferred to the function call. |
| register      | It is a variable which is stored inside a Register.                                                                 |

|存储类|作用域|生命周期|初始化|特点|
|---|---|---|---|---|
|`auto`|仅在局部函数中有效|仅在函数调用期间有效|未初始化时未定义|默认存储类，变量存储在栈中|
|`register`|仅在局部函数中有效|仅在函数调用期间有效|未初始化时未定义|建议将变量存储在寄存器中，不能取地址|
|`static`|局部：仅在当前函数内有效， 全局：仅在当前文件内有效|程序执行期间有效|默认初始化为零|保持变量值，局部变量保留值，静态全局变量只能在当前文件访问|
|`extern`|在整个程序中有效|在程序执行期间有效|默认初始化为零|用于跨文件共享变量或函数|
局部默认auto， 全局默认extern。
static 全局， 当前文件有效。
static局部，这个变量还是只能在当前函数访问，但是执行完不会被释放，也就是说，下一次执行还可以访问。

#### scope
程序中可以引用该标识符的区域
全局变量scope从声明开始到文件结束
局部代码块先找最近的变量，从内到外。但是不会去同层的另外的代码块。

#### linkage
能被多个还是只能被当前的文件识别？

#### 自动存储期和静态存储期
auto register /////   static extern

Notice： 函数访问全局变量，也要事先声明

extern 声明以后， 函数的定义可以在任何位置， 包括别的文件

static 全局变量和 static局部变量有着不同的含义

前者表示限定该文件
后者表示生命周期是整个程序，scope不变
但是都会默认初始化0。

#### 如何跨文件使用extern变量
通常， 一个文件里定义， 一个文件里声明
编译时一起编译成一个exe

### Ex. 9 preprocessing directives

宏定义， 文件包含， 条件编译， 行控制。

以#开头

```C
#define int long long 
#define TRUE 1
#define NL printf("\n")
#undef NL
//新的宏定义可以引用前面的！！！！！
//宏定义一行写不下怎么办
#define SWAPXY {int t = x; x = y;\
y=t;}
//回车紧跟在\后面


//带形参的宏定义

#define MAX(A,B) ((A) > (B) ? (A) : (B))
y = MAX(p + q, u + v);
//会被对应替换
//Notice : 标识符和左边圆括号之间紧跟，否则形成不了带参数宏定义
//宏调用时，保证参数个数相同
//只传字符不计算值
#define sq(x) x * x

q = sq(a + b)
q = a + b * a + b
//所以后面语句一般加上括号
#define sq(x) (x) * (x)

//宏定义只是替换字符
```

```C
#include <>
#include ""
//一次include只能包含一个文件
//<>被包含文件应该在编译系统目录下
//“”先找当前目录， 在找编译系统目录，或者直接给出路径
//习惯上，把公共的常量定义，函数声明和全局变量的外部说明构成一个头文件。
//比如， 我在一个头文件里声明了若干函数，在一个源文件里包含这个头文件并且定义了这些函数，则在另一个源文件里，如果我包含了这个头文件，就可以调用这些函数(前提是进行联合编译)
//但是，标准库的函数会自动连接， 不需要联合编译
```

```C
file.c 包含file1.h和file2.h， 1引用2，就应该反过来
#include "file2.h"
#include "file1.h"


```

#### 条件编译
```C
#ifdef 标识符
    //程序段1
#else
    //程序段2
#endif
#ifndef // not def
#if 表达式
#elif
//当标识符已经被定义就编译1，否则2
//只关心有没有被定义， 而与定义成什么无关

```

广泛运用于debug
```C
//调试状态下define一下, 调试完就不define， 这样调试完就不用改程序了， 代码得留着
#define DEBUG
#ifdef DEBUG
 printf("a = %d", a);
#endif
```
避免头文件重复定义
```C
//in finite_field.h
#ifndef FINITE_FIELD
#define FINITE_FIELD

#endif
```
反例：
我在一个头文件声明了函数f1, 在两个.c文件里include这个头文件，没有机制，虽然可以编译，但是连接的时候会有问题
事实上，既不可重复声明，也不可重复定义函数。
（P.S.：报不报错和编译器有关，比如我的gcc，如果头文件只有声明没有定义，重复包含不报错，有定义才报错）

## Array 
### Ex. 1 Basics
```C
//Commonly size of an array cannot be changed or determined by variables.
int m; 
int arr[m];
//error in lower versions than C99;

//Initializing one dimension array
auto int arr[10];
register int arr[10];


static int arr[10];//initialize 0;
extern int arr[10];//initailize 0;

//more common cases
int fib[5] = {0, 1, 1, 2, 3};
int fib[5] = [0, 1, 1];
//fib[3], fib[4] = 0;

//when all elements are explicitly initialized, the length of the array need not be determined.
int even[] = {0, 2, 4, 6, 8};

//Insert  a new element val at position idx.
void insert_array(int* list, int* len , int idx, int val){
    for (int j = *len ; j > idx; j--) {
        list[j] = list[j - 1];
    }
    list[idx] = val;
    (*len)++;
}

//Delete the element at index idx in the array list;
void array_delete(int* list, int* len, int idx) {
    for (int j = idx; j < *len - 1 ;j++) {
        list[j] = list[j + 1];
    }
    (*len)--;
    //list(*len) = 0;
}

//Find the key in the array list of length n.
int array_search(int* list, int n, int key) {
    for (int i = 0; i < n; i++) {
        if(list[i] == key) {
            return i;
        }
    }
    return -1;
}



//binary search


int binary_search(int* list, int n, int key) {
    int low = 0;
    int high = n - 1;
    int mid;
    while(low <= high) {
        mid = low + (high - low) / 2;
        if(list[mid] == key) {
            return mid;
        }
        else if (list[mid] < key){
            low = mid + 1;
        }
        else {
            high = mid - 1;
        }
    }
    return -1;
}
```

### Ex. 2 Exercises
```C
//贪心/双指针
//1 \in M;
//if x \in M , then 2x + 1, 3x + 1 \in M
//no other integers in M
//generate the first n elements in M

//all elements in M has 3 types, original ones, 2x + 1 ones and 3x + 1 ones;
//first type , only 1.
//use two pointers p1, p2;
//p1 generate 2 * m[p1] + 1, p2 generate 3 * m[p2] + 1;
int m[100];
m[0] = 1;
int n;
printf("Enter n : \n");
scanf("%d", &n);
int p1, p2, cur;
p1 = 0;
p2 = 0;
cur = 1;
while(cur < n){
    int tmp1 = 2 * m[p1] + 1;
    int tmp2 = 3 * m[p2] + 1;
    if(tmp1 < tmp2) {
        m[cur] = tmp1;
        p1++;
    }
    else{
        m[cur] = tmp2;
        p2++;
    }
    cur++;
}
for (int i = 0; i < n; i++){
    printf("%d ", m[i]);
}
```
[https://leetcode.cn/problems/majority-element/description/](https://leetcode.cn/problems/majority-element/description/)
majority elements that emerges at least  \[n / 2] times；
using Boyer-Moore vote algorithm
```C
//find the first and most emerged integer in an integer array;
//we have to traverse all the elements
//notice we only need to traverse after the current index;
```

```C
//partition
void swap(int* a, int* b) {
    int temp = *a;
    *a = *b;
    *b = temp;
}

int partition(int* arr ,int low, int high){
    int pivot = arr[high];
    int j = low - 1;
    int i = j;
    while (j < high){
        j++;
        if (arr[j] < pivot) {
            swap(&arr[j], &arr[++i]);
        }
    }
    swap(&arr[i + 1], &arr[high]);
    return i + 1;
}
//注意， 这个函数无法只能将小于pivot的放左边，并且保证下一个元素是pivot，无法处理等于pivot的元素。


重要
//assuming that low to i - 1 < pi, i to j - 1 == pi, j to h unknown, h + 1 to high > pi
void strict_partition(int* arr, int low, int high, int pivot){
    int j = low;
    int i = j;
    int h = high;
    for (;j <= h; j++){
        if (arr[j] == pivot) { 
            continue;
        }
        if (arr[j] < pivot) {
            swap(&arr[j], &arr[i++]);
        }
        else{
            swap(&arr[j], &arr[h--]);
            j--;
        }
    }
}


//去除重复元素
//solution1;
void bubble_sort(int*arr, int n) {
    for(int i = 0 ; i < n; i++){
        int swapped = 0;
        for (int j = 0; j < n - i - 1; j++){
            if(arr[j] > arr[j + 1]){
                swap(&arr[j], &arr[j + 1]);
                swapped = 1;
            }
        }
        if (!swapped) break;
    }
}

void rm(int* arr, int* len){
    bubble_sort(arr, *len);
    for (int i = 0, j = 1; j < *len; j++) {
        if (arr[j] > arr[j - 1]){
            arr[++i] = arr[j];
        }
    }
    *len = i + 1;
}

重要
//improve bubble sort
void bubble_sort(int*arr, int n) {
    for(int i = 0 ; i < n; i++){
        int swapped = 0;
        for (int j = 0; j < n - i - 1; j++){
            if(arr[j] > arr[j + 1]){
                swap(&arr[j], &arr[j + 1]);
                swapped = 1;
            }
        }
        if (!swapped) break;
    }
}

void bubble_sort(int*arr, int n) {
    for(int i = 0 ; i < n; i++){
        int k = i;
        for (int j = 0; j < n - i - 1; j++){
            if(arr[j] > arr[j + 1]){
                swap(&arr[j], &arr[j + 1]);
                k = n - j - 2;
            }
        }
        i = k;
    }
}

//joseph ring
void joseph(int n, int m, int* arr){
    int cur = 0;
    for (int j = 1; j <= n; j++) {
        cnt = 1;
        while (cnt < m) {
            if (arr[(++cur) % n] != 0) cnt++;
        }
        printf("Number %d out\n", arr[cur % n]);
        while (arr[cur % n] == 0){
            cur = (cur + 1) % n;
        }
    }
}
```
### Ex. 3 Multi-dimension array
```C
//C99中可以写
int m = 1, n = 1;
int matrix[m][n];
//因为m, n 被初始化了

int a3[2][3] ={{1, 2}}//其余为0
1 2 0
0 0 0 

//不制定第一维

int a[][3] = {1, 2, 3, 4, 5}
1 2 3
4 5 0

int a[][3] = {{1, 2}, {3}};
1 2 0
3 0 0
```
```C
//多种表示方法
int m[2][2];
int (*p)[2] = m;
p[0][0] == m [0][0]
(p + 1)[0] == m[1][0]
(*p)[0] == m[0][0]
int **q = m;
//是错的,也就说一个什么数组在单独作为表达式的时候只能变成指向这个什么的指针
//这里m最多变成指向int[2]数组的指针，左边是指向一个指针的指针， 所以不行
```
[https://leetcode.cn/problems/toeplitz-matrix/description/](https://leetcode.cn/problems/toeplitz-matrix/description/)
[https://leetcode.cn/problems/spiral-matrix/description/](https://leetcode.cn/problems/spiral-matrix/description/)
[https://leetcode.cn/problems/spiral-matrix-ii/description/](https://leetcode.cn/problems/spiral-matrix-ii/description/)
[https://leetcode.cn/problems/rotate-image/description/](https://leetcode.cn/problems/rotate-image/description/)
### Ex.4 string
```C
//initializing strings
char s1[10] = {'k','o','b','e'};
char s[] = "kobe";
char s[5] = "kobe";
//or {"kobe"}, considered as {{'k','o','b','e'}}
//会自动添加\0
char s[] = {'k'};
char s[4] = "kobe"
//这个可能打印出乱码

"I am a st\
ring."
"I am" " a string"
可以连起来

printf("%s", str);
//到第一个/0为止， 即使后面还有
//相当于
for (int i = 0 ; s[i]; i++){
    printf("%c", s[i]);
}
sizeof(s);
//返回s的长度，包括\0
strlen(s);
//不包括\0
注意这和 int a[10]不一样，这个要乘4

//输入
scanf("%s", str);
//跳过前导空白，遇到空白类停止，或者满了，没满自动添加\0
gets(str);
fgets(str, sizeof(str), stdin);
//gets不加\n，加\0,fgets都加, 即gets把第一个空白变成\0
//fgets 确保第sizeof(str)个是\0
//直到换行
//逐个char输入记得末尾加\0

strcpy(str1, str2);//\0一起
strncpy(str1,str2,n);//n不包括\0，够的话自动添加
strcat(str1,str2);//连接
strcmp(str1, str2);
while((*s1) && (*s1 == *s2)){
    s1++;
    s2++;
}
return *s1 - *s2;

strlwr()//大转小
strupr()//小转大

puts(str)//输出后自动换行（\0变 \n）


```

```C
//统计单词个数
//注意状态的逻辑！！
int count_words(char s[]){
    char*p = s;
    int inword = 0;
    int cnt = 0;
    while(*p) {
        int letter = *p >= 'A' && *p <= 'z';
        if (inword){
            if (!letter) inword = 0;
        }
        else{
            if (letter) {
                inword = 1;
                cnt++;
            }
        }
        p++;
    }
    return cnt;
}
```




## Structured Programming

### Ex. 1 Basics

```C
//关注lab和hw

//几个可能不容易关注到的语句和结构
//goto. (最好不要用)

//语句标号
#include <stdio.h>

int main() {
    int i = 0;

    start:
        if (i >= 10)  // 如果 i 达到 10，退出循环
            return 0;
        i++;
        printf("i = %d\n", i);
        goto start;

    return 0;
}

//else 总是跟最接近的if!!!!!
//减少nested if , always continue the loop

//switch语句

char choice;
scanf("%c", &choice);
//限制整型表达式，字符型表达式，枚举型表达式
//如果要用比如浮点型，先映射到整型
//switch ((int) (x + 0.5))
switch (choice) {
    case 'A' : printf("A chosen\n");
    case 'B' : printf("B chosen\n");
    default :printf("default");
}
//如果输入A，会输出三行
//为了跳出switch，可以用break；



//compare float
#define EPS 1e-3
//a == 0;
fabs(a) < EPS; 
//a < 0;
a < -EPS;    
//a <= 0;
a <= EPS;
//a > 0;
a > EPS;
//a >= 0;
a >= -EPS;
//a >= b;
a + EPS >= b; 
//a == b
fabs(a - b) < EPS
//规律总结：开区间比较严格保证，闭区间比较可以放宽相等

//用while跳过输入的空白字符
while((c = getchar()) == '' || c == '\t');
//这样c是第一个输入的非空格或者tab

//统计各种字符个数
int c, nwhite, nother, ndigit; 
nwhite=nother=ndigit=0; 
printf(”输入字符行\n”); 
while ((c = getchar()) != ’\n’) { 
    switch ( c ) { 
    case ’0’:case ’1’:case ’2’:case ’3’: case ’4’: case ’5’:case ’6’:case ’7’:case ’8’: case ’9’: ndigit++; break; 
    case ’ ’:case ’\t’: nwhite++; break; 
    default: nother++; break; 
    } 
} 
printf(”digit = %d\twhite space = %d\tother = %d\n”, ndigit, nwhite, nother); 



//do-while, 至少执行一次循环体
int s = 0, i = 1;
do {
    s+=i++;
}while(i <= 100);




```

### Ex. 2 Exercises
#### newton methods

$$
x_{k + 1} = x_k - \frac{f(x_k)}{f'(x_k)}, \quad k = 0, 1, \cdots
$$

until

$$
d_k = \frac{f(x_k)}{f'(x_k)} < \epsilon 
$$

```C
//find the solution of 

#define EPSILON 1e-6
int main() {
    double x, d;
    x = - 2;
    do { 
        d = (((3.0*x+4.0)*x-2.0)*x+5.0)/ ((9.0*x+8.0)*x-2.0); /* f(x)/f'(x) */ 
        x = x-d; 
    } while (fabs(d) > EPSILON);
    //不要忘分号 
    printf("The root is %.6f\n", x);
    return 0;
}
```

#### binary-search
```C
#include <math.h>
double low = -INF, high = INF, ans;
//f(low) < 0, f(high) > 0;
while (low < high - EPS) {
    mid = low + (high - low) / 2;
    if (fabs(f(mid) < EPS) ans = mid;
    if (f(mid) < -EPS) {
        low = mid;
        continue;
    }
    else {
        high = mid;
    }
}
```

#### CRT
```C
//find the mininum = 2 mod 3, = 3 mod 5, = 4 mod 7;
#include <stdio.h>

int main() {
    int ans = 0;
    while (!(ans % 3 == 2 && ans % 5 == 3 && ans % 7 ==4)){
        ans++;
    }
    return 0;
}

//optimize
int i = 2;
do i += 3;
while (i % 5 != 3)

while (i % 7 != 4)
    i += 15;
    
```

#### Leibniz formula
$$
\frac{\pi}{4} = \sum_{k = 0}^\infty (-1)^{k} \frac{1}{2k + 1}
$$
```C
int d = 1;
double term = 1.0;
double sum = 0;
while (fabs(term) >= EPS){
    sum += term;
    d += 2;
    term = (d - 1) % 4 ? -1.0 / d : 1.0 / d;
}
return s;
```

[sieve.c](24fall_C_programming/lab07/sieve.c)
[ramanujan.c](24fall_C_programming/lab07/ramanujan.c)

### Ex. 3 summation of series
注意，级数求和的时候涉及到通向带阶乘， 一般用递推。
```C
double s = 0 ;
double t = t_0;//先算首项
int k = 0;//下标
while (fabs(t) >= EPS) {
    s += t;
    t = f(t, k);//更新t
    k++;
}
```

### Ex. 4 random
```C
//复习lab和hw
#include <time.h>
#include <stdlib.h>

srand(time(NULL));
int r = rand();
```
## Basic calculation rules

### Ex. 1 Basic data types
```C
int has 4 bytes 
signed -2^31 to 2^31 - 1
unsigned 0 to 2^32 - 1

//负数用补码表示
//先取反在加1
//可以知道，首位是0 代表正数， -1代表负数

//10， 8， 16进制数的书写形式

十进制正常
八进制 0123
011
01

十六进制 0x123 

08x16
```

integer types
char  1
unsigned char. 1
signed char.  1
int.  4
unsigned int.  4 
short  2
unsigned short. 2
long 8
unsigned long 8
long long 8
float  4.   6 decimal places
double 8.  15.  decimal places
long double 8.  19 decimal places


```C

长整型和无符号的表示

long 0L

unsigned int 1U

unsigned long 22UL or 22LU

浮点型表述

7.

.457 

1e-1
//e前后都要有

1.5 double
1.5f float
1.5L double


//比较浮点数
//绝对误差
fabs(x - y) < 1e-6
//相对误差
fabs(x - y) <= fabs(x * 1e-6)

```

```C
char
//char 可以当作8位整型

//ASCII码里面先A 65， a 97并非紧跟A

//转义字符

‘\r’//回车到当前行开头
'\n'
'\t'
‘\v’//垂直跳到下一行

hello
     world
'\f' //换页
'\'' //'
'\"' //"
'\ddd'. //ddd 0ddd表示的字符
'\xhh' //hh 0xhh表示的字符
'\\'  //反斜杠

//注意 windows 系统的地址\\分割，其中\是转义字符 mac / linux 分割用 /
'\0' 



字符型常量和字符串常量的区别

字符串常量 
"string"
```

### Ex. 2 input and output

```C
#include <stdio.h>
putchar(ch);

getchar();
//read a char from stdin44
//用getchar的时候，回车代表终结， 一般来说不会读入回车， 但是， 如果循环读入， 则很可能读入回车
while(1){
    char c = getchar();
    //if (c == '\n) continue;
    putchar(c);
    if (c == 'e') break;
}
//如果输入z加回车
//输出z加回车


//输入zzz加回车
//输出zzz加回车


printf();

""里有三类字符
普通字符，  转移字符， 和格式说明

格式符：
%d
%o
%x
%U //无符号整型10进制

printf("%u", -1);
//对于int 00000000000000000000000000000001 补码11111111111111111111111111111111

%c
%s
%f
//默认六位小数

%e
//指数形式输出实数
printf("%e", 12.0);
1.200000e+01

输出格式修饰
%[] 格式符
-//左对齐，和w有关
+//正数也带符号输出
#//八进制加0， 十六进制加0x，浮点数总是显示小数点

//好像只对%#.0f有用， 毕竟浮点数一般都是自动显示小数点
w
//制定输出字符数， 如果大于w就无视w， 否则根据-是否存在补出左边或者右边的空格
.p
//对lf， f，  e格式， 制定小数点以后的位数， 默认是6
h/l/L
紧跟d,f,lf etc.
h和整型用， 表示short或者unsigned short
hd, hu
l和整型用， 表示加上long
lu unsiged long
ld, long 
lld long long 
Lf, long double

//%Lf是 long double 
//%lf是double

scanf();

//格式控制字符串和printf不同的是
格式转换符
*//跳过一个输入数据项
w//宽度说明， <w实际宽度， >w 截断
h/l/L
//short int , long int, long double;
d o x u c s e,f
//注意给出变量地址

//普通字符输入的时候要严格输入
//认为字符串数组的名字就是一个可以输入的地址
char s[100];
scanf("%s",s);
//且只能输出99位， 因为要添加\0
//且忽略前面的空格， 并且遇到空白就停止

//注意

"%c"输入的时候，空白和转义字符都有效输入，如果跳过空白字符
用" %c"

//输入的终结

遇到空白类，已经读入足够%4d， 当输入下一个字符以后不能构成正确的格式

//连续输入

"%d%d"中间用空白分割

"%2d%2d" 
//输入123 12
//输出12 3
//先读取两位， 然后在读取两位， 但是后面不要读去了， 所以以空格作为终止

"%2d%*d%2d"
//输入123 12
//输出12 12
//先读取两位，然后以空格分割读取的下个是3， 所以跳过3，再读取12

"%3d%*4d%d"
//输入123456 78
//先123， 然后456跳过，78
//如果输入12345678
//先123， 然后4567跳过， 8
```
### Ex.3 Calculation
```C
//单目， 双目， 和三目运算符

//双目从左到右， 单目三目从右到左


单目：+ - =
双目； + - * / %
//注意， 整除向0取整

//对mod的特别说明

a % b = a - (a / b) * b;
//注意/的向零取整特性

//复合赋值运算的优先级很弱，或者说复合运算的优先级很弱

x+= j + 1
x = x + (j + 1)

//引用和自增的先后关系
++i//后引用

i++//先引用

//只能用于变量，不能用于常量或者表达式

//关系运算符，优先级低于算数运算符，应该高于赋值

< > <= > =
高于
= !=

逻辑运算
!高于&&高于||



优先级高低总结
(),[],.,->
非
算数
位
关系
&& ||
赋值

&& //左边0则不计算右边
|| //左边1则不计算右边

int a = 0;
++a || ++b
//b保持不变

while (p && p -> val != target)

//这样如果p是空指针，就不会出现访问错误

yr % 4 == 0 && yr % 100 != 0 || yr % 400 == 0 

//条件运算从右到左
x > y ? x : u > v ? u : v

//类型转化

i > j ? 2 : 1.5
如果i > j, 表达式的值是2.0

//逗号运算， 顺序计算，并且以最后一个作为结果
x= (i = 3, i * 2);
//x = 6

//逗号运算的优先级最低

//位运算
& | ^ ~

//一些典型用法

x & 0177
对应二进制7个1， 也就是取末尾7位，前面都是0

x | 017
最后四位都变成1， 其他不变

x = 017 ^ x
最后四位取反， 其他不变

^的反身性

x = x ^ y;
y = x ^ y;
x = y ^ x;

~53的结果是-54

比如001
110
101
010
也就是说1取反的结果是-2


解释((k - 1)^k)&k

111
110
001
001
110
101
011
010


>> << 
<< 乘2
>> 整除 2

右移对符号有不同的处理方法
对于负数， 0填充成为逻辑右移， 1填充位算术右移

11111101 //-3

00000011

00000001 -1//算数右移
01111110 //大整数 ， 逻辑右移

~(~0 << n)
右边n位为1， 其余为0；

//强制转换，float转int一般直接抹去小数

```
## FILE 


