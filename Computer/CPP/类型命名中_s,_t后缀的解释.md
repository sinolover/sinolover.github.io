# 类型命名中_s,_t后缀的解释
在阅读linux内核的过程中，经常会碰到自定义类型，如：typedef struct aa_s{...};起初看时有点不明白_s和_t的区别，直到前几天才恍然大悟。按照我的理解：_s后缀应该是表示struct（一个结构体）的意思。_t后缀应该是表示一个type(一个类型)。下面举个例子:
~~~ cpp
struct record_s{
    int a;
    int b;
};
typedef struct record_s record_t;
typedef int pid_t;
~~~