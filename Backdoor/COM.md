## COM劫持

### 基本概念

- 接口：一组函数的总称，这些函数也被称为”方法”
- `Component object class(coclass)`：也就是组件，组件包含在一个DLL或者exe文件中，它包含了一个或多个接口的实现代码，组件实现了它包含的所有接口
- `COM object`：是组件的一个实例
- `COM server`：一个dll或者exe文件，包含了一个或者多个组件
- `COM library`：是操作系统的一部分，负责响应用户程序
- `GUID`：唯一的128位的标识对象的标识，全局唯一标识符
- `CLSID`：class id，唯一的标识组件
- `IID`：interface id，用来标识接口

### Windows常用CLSID [^1]

| 名称              | CLSID                                    |
| ----------------- | ---------------------------------------- |
| 我的文档          | ::{450D8FBA-AD25-11D0-98A8-0800361B1103} |
| 我的电脑          | ::{20D04FE0-3AEA-1069-A2D8-08002B30309D} |
| 网上邻居          | ::{208D2C60-3AEA-1069-A2D7-08002B30309D} |
| 回收站            | ::{645FF040-5081-101B-9F08-00AA002F954E} |
| InternetExplorer  | ::{871C5380-42A0-1069-A2EA-08002B30309D} |
| 控制面板          | ::{21EC2020-3AEA-1069-A2DD-08002B30309D} |
| 拨号网络/网络连接 | ::{992CFFA0-F557-101A-88EC-00DD010CCC48} |
| 任务计划          | ::{D6277990-4C6A-11CF-8D87-00AA0060F5BF} |
| 打印机（和传真）  | ::{2227A280-3AEA-1069-A2DE-08002B30309D} |
| 历史文件夹        | ::{7BD29E00-76C1-11CF-9DD0-00A0C9034933} |
| ActiveX缓存文件夹 | ::{88C6C381-2E85-11D0-94DE-444553540000} |

### Reg注册表

```powershell
# windbg.reg
Windows RegistryEditor Version 5.00 

[HKEY_CLASSES_ROOT\CLSID\{b5f8350b-0548-48b1-a6ee-88bd00b4a5e7}]

[HKEY_CLASSES_ROOT\CLSID\{b5f8350b-0548-48b1-a6ee-88bd00b4a5e7}\InProcServer32]

@="freebuf.dll"

"ThreadingModel"="Apartment"
```

### 利用方法

- 选择系统应用范围广的 `CLSID`，这样的模块可以保证系统在进行很多功能时都会加载 dll
- 新建文件夹，以`CLSID`做为后缀名，同时将我们的利用dll拷贝到系统目录下
- 打开文件夹，成功实现打开文件夹执行指定的程序



[原文](https://www.freebuf.com/articles/system/115241.html)

---

[^1]: `HKEY_CLASSES_ROOT\CLSID\`