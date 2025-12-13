# 1. 常规语法

## 1.1 `const`用法

### 1.1.1 修饰指针

* `const int* p` 或 `int const* p` → 指针指向的内容不可修改（pointer to const int）
* `int* const p` → 指针本身不可修改（const pointer to int）

```C++
const int* a = &x;       // [1] 指针指向内容不可修改，指针可改
int const* a = &x;       // [2] 同 [1]，内容不可改
int* const a = &x;       // [3] 指针不可改，内容可改
const int* const a = &x; // [4] 指针不可改，内容不可改
```

### 1.1.2 修饰函数

**成员函数后加` const`** → 表示该成员函数不会修改对象的成员变量（`this` 指针是 `const` 类型）。

**静态成员函数或非成员函数** → 不能在函数后加 `const`，因为它们没有 `this` 指针。

**特殊情况** → 成员变量如果要在` const `函数里修改，可加 `mutable` 修饰。

### 1.1.3 修饰返回值

- **返回类型前 `const`** → 修饰返回值
- **C++11 以后**：
  - `constexpr` → 编译期常量
  - `consteval` → 强制编译期求值（C++20）

### 1.1.4 修饰普通变量

定义不可修改的常量，提高代码安全性。

### 1.1.5 修饰函数参数

防止函数修改传入对象（尤其大对象用引用或指针传递时）



## 1.2 拷贝构造和赋值运算符



## 1.3 深拷贝和浅拷贝



## 1.4 虚函数，多态，`vtable`(虚函数表)



## 1.5 内存管理 `new/delete` 与 `malloc` 与 `free`的区别



## 1.6 左值与右值， 左值引用与右值引用



## 1.7 Lambda 捕获方式 `[=]` `[&]`



## 1.8 移动语义 vs 拷贝语义



## 1.9 智能指针  `unique_ptr / shared_ptr/weak_ptr`



## 1.10 override / final



## 1.11 explicit 防隐式类型转换





## 1.12 `constexpr` 函数增强



## 1.13 std::optional / std::variant / std::any



## 1.14 `inline` 全局变量



## 1.15 结构化绑定





## 1.15 string_view vs string





## 1.16` [[nodiscard]]`





##  1.17 fold 表达式





## 1.18 `if constexpr`



# 2. 编程规范

## 2.1 类型安全

 **程序在编译期或运行期尽量保证数据类型不被误用或意外转换**

``` c++
int a = 10;
double b = 3.14;

a = b; // 隐式转换，可能丢失精度 → 不完全类型安全
```

**C++ 类型安全机制：**

- **强类型检查**：C++ 不允许把指针随意赋值给其他类型（需要显式转换）。
- **const 修饰**：防止修改不该修改的类型数据。
- **enum class**（C++11）：强类型枚举，不允许隐式转 int。

```c++
enum Color { RED, GREEN, BLUE }; 		// 老式 enum，可以隐式转 int
enum class Color2 { RED, GREEN, BLUE }; // 类型安全，不可隐式转 int
```



## 2.2 浮点数比较的特殊方法

浮点数（`float` / `double`）存在精度问题，不宜直接用 `==` 比较：

```
double a = 0.1 + 0.2;
double b = 0.3;

if (a == b) { /* 不可靠 */ }
```

### 正确做法：使用 **误差范围 epsilon**

```
#include <cmath>
const double EPSILON = 1e-9;

if (std::abs(a - b) < EPSILON) {
    // a 与 b 可认为相等
}
```

- EPSILON 表示容忍的最小误差
- 对浮点数比较要使用绝对误差或者相对误差
- C++17 可以用 `std::fabs` 或 `std::isclose`（部分库有实现）

