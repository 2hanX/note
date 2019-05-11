## lib库、dll库的使用方法与关系

### lib库分类 

- 静态lib（static lib）[^1]
- 动态lib（dll）[^2]
- 共同点：两者都是二进制文件，都是在链接时调用，使用 static lib 的 exe 可以直接运行，使用另一种 lib 的 exe 需要对应的 dll 才能运行

### 静态lib创建和引用示例

1. VS2005(包含)以上的版本

2. 建立win32控制台工程

3. 下一步选择 static library

4. 头文件

   ```cpp
   //mylib.h
   #ifndef MYLIB_H_H
   #define MYLIB_H_H
   #include "stdafx.h"
   
   void display();
   
   #define A(x)  (x)*(x)
   #endif
   ```

5. 源文件

   ```cpp
   //mylib.cpp
   #include "stdafx.h"
   #include "mylib.h"
   
   void display()
   {
       cout<<"This is display"<<endl;
   }
   ```

6. 编译后生成一个 `mylib.lib` 的库文件

7. 引用库文件

   1. 右击工程-->属性（`alt+F7`）-->C/C++-->附加包含目录--->包含所需要的头文件

   2. 右击工程-->属性（`alt+F7`）-->链接器-->常规--->附加目录--->包含lib库所在的目录

   3. 右击工程-->属性（`alt+F7`）-->输入--->附加依赖项--->lib库名

      或者

   4. 使用` #pragma comment(lib, "LibPath")`的方法来调用Lib文件，同时必须包含相应的头文件

8. 引用库内的函数

   ```cpp
   #include "stdafx.h"
   #include "mylib.h"
   
   int _tmain(int argc, _TCHAR* argv[])
   {
       display();
       cout<<A(6-2)<<endl;
       getchar();
       return 0;
   }
   ```

### 动态dll创建和引用示例 [^3]

1. VS2005(包含)以上的版本

2. 建立win32控制台工程

3. 下一步选择 动态库 Dynamic Library

4. 定义导出函数接口的方法

   - 使用 `__declspce` 宏

     1. 源文件

        ```cpp
        // dllmain.cpp : 定义 DLL 应用程序的入口点。
        #include "stdafx.h"
        
        BOOL APIENTRY DllMain( HMODULE hModule,
                               DWORD  ul_reason_for_call,
                               LPVOID lpReserved
                             )
        {
            switch (ul_reason_for_call)
            {
            case DLL_PROCESS_ATTACH:
            case DLL_THREAD_ATTACH:
            case DLL_THREAD_DETACH:
            case DLL_PROCESS_DETACH:
                break;
            }
            return TRUE;
        }
        ```

        ```cpp
        //mydll1.cpp
        #include "stdafx.h"
        #include "mydll1.h"
        
        void display(void)
        {
            cout<<"call display()"<<endl;
        }
        ```

        

     2. 头文件

        ```cpp
        //mydll1.h
        
        #ifndef MYDLL_H_H
        #define MYDLL_H_H
        
        extern "C" _declspec(dllexport) void display(void);
        //这里的extern "C" 表示我们要按照C语言的方式编译该函数，防止在C++工程中编译出现函数名错误，因为C中没有重载而C++中允许重载，所以C++中函数编译后会出现display@1的形式；让编译器以C语言的编译方式编译可以保证C可以调用C++的动态链接库。`_declspec(dllexport)`表示下来的函数是dll的导出函数接口。没有导出的接口是不可使用的，这里和静态lib库有所区别，静态lib库中的所有函数、宏定义等都是可以使用的
        
        #endif
        ```

   - 使用 def 文件，该文件的功能类似于 `__declspec(dllexport)` 的功能

     1. 在资源文件目下添加def文件

        ```
        LIBRARY mydll1
        EXPORTS
        display	@ 1; Export the display function
        ```

5. dll的调用

   - dll 的显示调用 [^4]

     ```cpp
     // test_my_dll.cpp : 定义控制台应用程序的入口点。
     // 运行时动态链接
     
     #include "stdafx.h"
     #include "windows.h"
     typedef void(*print)(void);
     
     int main(int argc, char *argv[])
     {
         HMODULE hDll = LoadLibrary("mydll1.dll");
         if(hDll != NULL)
         {
             print printFun = (print)GetProcAddress(hDll,"display1");
             //print printFun = (print)GetProcAddress(hDll,MAKEINTRESOURCE(1));
         }
         if (printFun != NULL){
             display1();
         }
         FreeLibrary(hDll);
         return 0;
     }
     ```

     

   - dll 的隐示调用 [^5]

### 总结

显示调用和隐式调用这两种方法各有千秋，显示调用优点是只要接口参数列表没有发生改变，修改了函数实现的细节也没关系，所调用的exe程序不用重新编译只需替换新版的dll就可以。在大的项目中只用比较方便，但调用过程相对复杂需要使用一系列的windows函数还有函数指针等。对于隐式调用和static library用法一致比较容易理解，缺点是只要修改了任意内容相应的exe都需要重新编译

### Ref 

- [在VS中使用C++创建和使用DLL](https://www.cnblogs.com/ring1992/p/6003248.html)



[原文](https://www.cnblogs.com/maxiaofang/p/4303306.html)

---

[^1]: 在编译时直接将代码加入程序当中。静态lib中，一个lib文件实际上是任意个obj文件的集合，obj文件是cpp文件编译生成的
[^2]: 包含函数所在的DLL文件和文件中函数位置的信息（入口），代码由运行时加载在进程空间中的DLL提供，也就是平时编写dll时附带产生的lib，其中Lib只是Dll的附带品，是DLL导出的函数列表文件而已
[^3]: dll其实和exe是几乎完全一样的，唯一的不一样就是Exe的入口函数式 WinMain 函数（控制台程序是main函数），而Dll是 DllMain 函数，其他完全是一样的。所以有人也戏称dll是不能自己运行的exe。dll的创建也比较简单，唯一麻烦的就是要定义导出函数接口
[^4]: 前提是我们必须知道函数名、返回值、参数列表，此时不需要相应的头文件，也不需要lib文件而只需要对应的dll
[^5]: 用法与 static library 用法一致