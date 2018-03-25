# 函数指针

指针!指针!指针!

```c++
#include <iostream>

using namespace std;

/*
 * 普通函数
 */
void hello(string s);

/*
 * 函数指针
 */
void (*hello_p)(string);

int main()
{
    hello("lee");
    hello_p = &hello;
    hello_p("xianzhan");

    return 0;
}

void hello(string s)
{
    cout << "hello " << s << endl;
}
```
