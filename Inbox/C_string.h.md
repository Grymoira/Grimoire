
## strcmp

string compare的缩写；

```c
int strcmp(const char *u, const char *v);
```

 * `strcmp(s1, s2)`：这不返回布尔值，而是一个整数
   * 返回 0：两者内容完全相同
   * 返回 > `0：s1` 在字典序中排在 `s2` 后面
   * 返回 < `0：s1` 在字典序中排在 `s2` 前面

## strcat

string concatenate的缩写；

```c
char *strcat(char *dest, const char *src);
```

- dest(Destination)目标字符串，连接后，结果将储存在这个位置；
- src(Source)源字符串，他会被复制到dest的末尾；
- 这行代码会返回目标字符串dest的指针；

注意：

1. dest所指向的数组空间，必须大于等于两个字符串长度之和在加上1，如果空间不足，会发生缓冲区溢出Buffer Overflow，这可能导致程序崩溃或者严重的安全漏洞
2. dest必须是以\0来结尾

## strncat

为了避免溢出的风险，更加建议使用strncat，他多出了一个参数，用来限制最度追加多少个字符；

```c
chat *strncat(chat *dest, const char *str, size_t n);
```

For example:

```c
strncat(dest, stc, sizeof(dest) - strlen(dest) - 1);
```

## strchr

string character的缩写；

他的作用就是在字符串中寻找特定的字符；

```c
char *strchr(const char *s, int c);
```

该函数会将字符串末尾的\0也作为字符串的一部分，因此也可以使用他来定位字符串的结束位置

在查找到目标的时候，会返回一个指针，指向该字符在原字符串中第一次出现的位置；在没有找到的时候，会返回NULL；

```c
char *result = strchr(str, target);

if (result != NULL) {
	printf("Founed the character: %c\n", target);
	printf("从这个位置开始的剩余字符串是：%s\n",result);
	printf("字符在原字符串中的索引位置是：%ld\n",result - str);
} else {
	return 0;
}
```

指针减法：

result - str利用了指针运算，因为两个指针都指向同一个连续内存块（字符串），相减就能够得到他们之间的元素个数，即这个字符的下标索引


## strstr

string find string的缩写；

在一个长字符串（母串）中，查找是否包含一个短字符串（子串）；

```c
char *strstr(const char *haystack, const char *needle);
```

- **haystack (干草堆)**：你要在其中搜索的完整字符串
- **needle (针)**：你想找的那段特定字符
- **返回值**：
    - 如果找到了：返回一个**指针**，指向 needle 在 haystack 中**第一次出现**的位置
    - 如果没有找到：返回 **NULL**
    - 如果 needle 是空字符串：返回 haystack 本身

### 需要注意的

#### ① 必须检查返回值是否为 NULL

这是最容易犯的错误！如果你直接对 strstr 的结果进行操作（比如 memcpy），而它恰好没找到子串返回了 NULL，你的程序会直接崩溃（段错误）。

```c
char *p = strstr(str, "key");
if (p) { // 永远先判断是否为非空
    // 做后续操作
}
```

#### ② 它是大小写敏感的

strstr 认为 "Apple" 和 "apple" 是不一样的。如果你需要不区分大小写的查找，在 Linux/Unix 下通常有 strcasestr。

#### ③ 配合 strlen 获取子串后的内容

如果你找到了 ID "102:"，想跳过这个 ID 去拿后面的内容，你可以这样做：

```c
char *p = strstr(buffer, "102:");
if (p) {
    char *content = p + strlen("102:"); // 指针向后移动，跳过 "102:" 这四个字符
    printf("内容开始于: %s\n", content);
}
```

---

### 5. 与 strchr 的区别

这是初学者最容易混淆的：
- **strchr**: 找**一个字符** (search **char**acter)。比如找第一个冒号 :
- **strstr**: 找**一个字符串** (search **str**ing)。比如找整个单词 "102:"
## strrchr

string reverse character的缩写；

从右边开始寻找；

## strtok

string tokenize的缩写;

他的作用是将一个完整的字符串根据指定的分割府（比如空格，逗号，分号）拆分成一个个小的片段（token);

```c
char *strtok(char *str, const char *delim);
```

- str要被拆分的字符串；
- delim包含所有分割符的字符串

1. 首轮调用：需要传入原始字符串str。他会找到第一个不是分割符的字符，标记为token的起始，然后继续往后找，直到遇到第一个分割符；
2. 替换为\0：一旦找到分隔符，strtok会修改原来的字符串，会将这个位置的字符替换为字符串结束符\0；
3. 后续调用：为了继续拆分剩下的部分，需要再次调用strtok，但是第一个参数要传NULL，他内部有一个静态指针，会记住上次拆分停止的位置;
4. 结束标志：当字符串里面没有更多的token的时候，函数返回NULL；

```c
int main() {
	char str[] = "Apple, Orange, Banana";
	const char *delim = ",";
	char *token = strtok(str, delim);
	while (token != NULL) {
		printf("Token: %s\n", token);
		token = strtok(NULL, delim);
	}
return 0;
}
```

```c
// Output
Token: Apple
Token: Orange
Token: Banana
```

由于strtok会将分隔符修改成\0,如果后面还有需要用到原始的，完整的字符串，一定要使用strcpy备份一次；

一定不要这样做！！！

```c
char *str = "Hello"; // 这是常量，保存在只读内存区
strtok(strm, " ");   // 程序会直接崩溃（段错误），因为strtok试图修改他
```

因为strtok内部使用了一个静态变量来记录位置，如果在多线程环境下同时调用，数据会乱套，在多线程编程中，建议使用线程安全版本的strtok_r(Linux)或者strtok_s(Window);

## memset

### 使用情况

1. 这个方法最为常用的地方在于初始化机构体或者数组，在c语言中，局部变量如果不初始化，其值是随机的，使用memset将其清零是最为稳妥的做法；

```c
struct User {
    int id;
    char name[20];
    float balance;
};

struct User user;
memset(&user, 0, sizeof(user)); // 将整个结构体所有成员（包括对齐填充部分）清零
```

2. 重置缓冲区Buffer

当循环使用一个数组作为缓冲区的时候，在处理下一批数据之前，通常需要清理一下旧的数据

3. 为memcmp做准备

如果需要比较两个结构体是否相等，应该先memset清零之后再赋值，因为结构体成员之间可能存在字节对齐产生的缝隙，这些缝隙中的随机值会导致memcmp失败；

---

### 基本情况

```c
void *memset( void *s, int c, size_t n);
```

- **s (ptr)**: 指向要填充的内存块的指针
- **c (value)**: 你想设置的值。注意，虽然它是 int 类型，但函数内部会将其转换为 **unsigned char**（1 个字节）
- **n (num)**: 你要设置的**字节数**

For example:

```C
char str[10];
memset(str, 'A', 10); // 把 str 的 10 个字节全部变成 'A'
```


---

### 注意细节

1. 这个语法是按照字节来填充的

memset 的操作粒度是Byte

正确的书写方式应该是：`memset(arr, 0, sizeof(arr))`可以将整数数组清零，因为每个字节都是0,和在一起组成的int也是0；

错误的写法：

如果需要将一个int数组的所有元素设置为1：

```c
int arr[10];
memset (arr, 1, sizeof(arr));
```

这不会让 arr[0] 变成 1，而是会让 arr[0] 的四个字节都变成 0x01。结果 arr[0] 的值变成了 0x01010101（即 16843009） 

**结论：除了 0 和 -1（-1 的补码是全 1），不要用 memset 给非字符类型的数组赋其它值**

2. sizeof的指针问题

```c
void init_array(int *ptr) {
    // 错误！这里的 sizeof(ptr) 只是指针的大小（4 或 8 字节），而不是数组大小
    memset(ptr, 0, sizeof(ptr)); 
}
```

需要始终明确需要填充的字节数


3. 复杂对象的自杀

如果你在 C++ 环境下（虽然你问的是 C，但这点很重要），绝对**不要**对拥有虚函数、继承或含有 std::string 等复杂对象的类使用 memset。这会破坏虚函数表指针或导致内存泄漏。在纯 C 中，如果结构体内部有指针，memset 会把指针设为 NULL，这通常是安全的，但要确保你没有因此丢失已经分配的内存引用

### 进阶

memset vs 直接初始化

- **int arr[100] = {0};**：这是**编译时**初始化。它简洁、安全，且性能极高（通常由编译器优化）
- **memset**：这是**运行时**操作。它更灵活，可以在程序运行的任何时刻重新清空内存
**建议：**
1. 如果是刚声明变量，优先使用 = {0}
2. 如果是重复利用已有的内存块，使用 memset

## memcpy

memory copy的缩写；

```c
void *memcpy(void *dest, const void *src, size_t n);
```

- **dest (Destination)**: 目的地的起始地址
- **src (Source)**: 源数据的起始地址
- **n**: 要复制的**字节数**（注意：不是元素个数）
- **返回值**: 返回指向 dest 的指针

### 使用情况

#### A. 结构体（Struct）的快速拷贝
如果你想把一个结构体的内容完全复制给另一个结构体，除了直接赋值（a = b），memcpy 是最明确的方法，尤其是在处理复杂缓冲区时
#### B. 数组或缓冲区（Buffer）的复制
当你需要备份一个数组，或者将网络接收到的原始字节流（Byte Stream）拷贝到具体的变量中时
#### C. 实现泛型数据结构
在 C 语言中模拟泛型（比如实现一个通用的 Vector），你需要处理未知类型的数据，这时只能通过 void* 指针和 memcpy 来按字节搬运数据

### 注意细节

#### ① 内存重叠（Overlapping）—— 最大的陷阱！
memcpy 的标准规定：**源内存区域和目标内存区域不能有任何重叠**
如果区域重叠，memcpy 的行为是未定义的（可能会导致数据乱码）
- **对策**：如果你怀疑内存可能重叠（比如在同一个数组里把后半部分移到前半部分），请务必使用 **memmove**。memmove 会自动处理重叠逻辑，虽然它慢那么一点点，但安全
#### ② 字节数 n 的计算
记住：memcpy 只认字节。
- 错误写法：memcpy(dest, src, 10); // 假设你想拷贝10个int，这只拷了2.5个int
- 正确写法：memcpy(dest, src, 10 * sizeof(int));
#### ③ 类型安全缺失
memcpy 接受 void*，这意味着它不会进行任何类型检查。你把一个 double 拷贝到一个 int 数组里，编译器不会报错，但你的程序逻辑会彻底崩溃
#### ④ 浅拷贝（Shallow Copy）问题
如果结构体中包含**指针**（比如 char *name），memcpy 只会拷贝指针的地址，而不会拷贝指针指向的实际内容
- 结果：两个结构体指向同一块内存，修改一个会影响另一个，甚至导致重复释放（Double Free）崩溃
#### ⑤ 目标空间必须足够大
这是导致“缓冲区溢出”的常见原因。你必须确保 dest 指向的内存空间至少有 n 个字节，否则会污染相邻的内存区域，引发段错误（Segmentation Fault）
#### ⑥ 不要用来初始化零
虽然 memcpy 可以，但如果是为了把内存清零，请使用 memset；如果是为了把结构体初始化为另一个，才用 memcpy