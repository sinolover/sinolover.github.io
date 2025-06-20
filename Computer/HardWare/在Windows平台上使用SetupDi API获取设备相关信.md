# 在Windows平台上使用SetupDi API获取设备相关信
Windows操作系统提供了一组功能强大的API，用于管理和获取与设备相关的信息。其中之一是SetupDi（Setup Device Installation） API，它允许开发者在Windows平台上轻松地获取与设备安装和配置相关的信息。本文将介绍如何使用SetupDi API来获取设备的相关信息。

## 1\. 引入SetupDi API

首先，要使用SetupDi API，需要包含头文件 ***setupapi.h*** 和链接库***setupapi.lib***，具体如下所示：
~~~ cpp
#include <Windows.h>`
#include <SetupAPI.h>`
#pragma comment(lib, "Setupapi.lib") //让编译器在链接阶段将指定的库文件与生成的可执行文件进行关联`
~~~
## 2\. 初始化设备信息结构体

在使用SetupDi API之前，需要初始化一些数据结构，其中最重要的是`SP_DEVINFO_DATA`结构体，它将用于存储设备信息。
~~~ cpp
SP_DEVINFO_DATA deviceInfoData;`
ZeroMemory(&deviceInfoData, sizeof(SP_DEVINFO_DATA));`
deviceInfoData.cbSize = sizeof(SP_DEVINFO_DATA);`
~~~
## 3\. 获取设备信息

接下来，使用SetupDi API来获取设备信息。

### 3.1 获取设备信息集

设备信息集是一个包含了一个或多个设备信息元素的集合，每个元素代表了一个特定的设备或设备类。要获取设备信息集，可以使用SetupDiGetClassDevs函数，它的原型如下：

```cpp
HDEVINFO SetupDiGetClassDevs(
  const GUID *ClassGuid,
  PCTSTR     Enumerator,
  HWND       hwndParent,
  DWORD      Flags
);
```

该函数的参数说明如下：

*   ClassGuid：指定要获取的设备类的GUID，如果为NULL，则获取所有设备类。
*   Enumerator：指定要获取的设备的枚举器名称，如果为NULL，则获取所有设备。
*   hwndParent：指定父窗口句柄，如果为NULL，则没有父窗口。
*   Flags：指定获取设备信息集的选项，可以是以下值的组合：
    *   DIGCF\_ALLCLASSES：获取所有设备类。
    *   DIGCF\_DEVICEINTERFACE：获取与指定类GUID关联的所有设备接口。
    *   DIGCF\_PRESENT：只获取当前存在的设备。
    *   DIGCF\_PROFILE：只获取当前配置文件中的设备。

该函数返回一个HDEVINFO类型的句柄，表示设备信息集。如果失败，则返回INVALID\_HANDLE\_VALUE，并通过GetLastError函数返回错误代码。

调用示例：

`HDEVINFO deviceInfoSet = SetupDiGetClassDevs(NULL, NULL, NULL, DIGCF_ALLCLASSES | DIGCF_PRESENT);`

### 3.2 遍历设备信息集

要遍历设备信息集中的每个设备信息元素，可以使用SetupDiEnumDeviceInfo函数，它的原型如下：

```c
BOOL SetupDiEnumDeviceInfo(
  HDEVINFO         DeviceInfoSet,
  DWORD            MemberIndex,
  PSP_DEVINFO_DATA DeviceInfoData
);
```

该函数的参数说明如下：

*   DeviceInfoSet：指定要遍历的设备信息集句柄。
*   MemberIndex：指定要获取的设备信息元素的索引，从0开始递增。
*   DeviceInfoData：指向一个SP\_DEVINFO\_DATA结构的指针，用于接收设备信息元素的数据。

该函数返回一个布尔值，表示是否成功。如果成功，则DeviceInfoData指向的结构被填充。如果失败，则通过GetLastError函数返回错误代码。当遍历到最后一个元素时，GetLastError返回ERROR\_NO\_MORE\_ITEMS。

调用示例：

`SetupDiEnumDeviceInfo(deviceInfoSet, deviceIndex, &deviceInfoData);`

### 3.3 获取设备属性

要获取设备属性，可以使用SetupDiGetDeviceRegistryProperty函数，它的原型如下：

```c
BOOL SetupDiGetDeviceRegistryProperty(
  HDEVINFO         DeviceInfoSet,
  PSP_DEVINFO_DATA DeviceInfoData,
  DWORD            Property,
  PDWORD           PropertyRegDataType,
  PBYTE            PropertyBuffer,
  DWORD            PropertyBufferSize,
  PDWORD           RequiredSize
);
```

该函数的参数说明如下：

*   DeviceInfoSet：指定要获取属性的设备所属的设备信息集句柄，由
    
    ```cpp
     SetupDiGetClassDevs函数返回的句柄。 
    ```
    
*   DeviceInfoData：指向一个SP\_DEVINFO\_DATA结构的指针，表示要获取属性的设备。
    
*   Property：指定要获取的属性代码，常用的值包括：
    
    *   SPDRP\_CLASS：表示设备类名称。
    *   SPDRP\_CLASSGUID：表示设备类GUID。
    *   SPDRP\_DEVICEDESC：表示设备描述。
    *   SPDRP\_DRIVER：表示设备驱动程序键名称。
    *   SPDRP\_HARDWAREID：表示设备硬件ID列表。
    *   SPDRP\_INSTALL\_STATE：表示设备安装状态。
    *   SPDRP\_MFG：表示设备制造商名称。
    *   SPDRP\_PHYSICAL\_DEVICE\_OBJECT\_NAME：表示物理设备对象名称。
    *   其他的属性可以参考微软官方文档
    
*   PropertyRegDataType：指向一个DWORD变量的指针，用于接收属性数据的注册表类型。
    
*   PropertyBuffer：指向一个缓冲区的指针，用于接收属性数据。如果为NULL，则只返回所需的缓冲区大小，不返回数据。
    
*   PropertyBufferSize：指定属性缓冲区的大小，以字节为单位。如果PropertyBuffer为NULL，则忽略此参数。
    
*   RequiredSize：指向一个DWORD变量的指针，用于接收属性数据所需的缓冲区大小，以字节为单位。如果为NULL，则忽略此参数。
    

该函数返回一个布尔值，表示是否成功。如果成功，则PropertyRegDataType、PropertyBuffer和RequiredSize指向的变量被填充。如果失败，则通过GetLastError函数返回错误代码。如果缓冲区太小，则GetLastError返回ERROR\_INSUFFICIENT\_BUFFER。

调用示例（获取设备硬件ID）：

`TCHAR hardwareID[MAX_DEVICE_ID];`

`if (SetupDiGetDeviceRegistryProperty(deviceInfoSet, &deviceInfoData, SPDRP_HARDWAREID, NULL, (BYTE*)hardwareID, sizeof(hardwareID), NULL)) {`

`// 硬件ID现在存储在hardwareID中`

`}`

## 4\. 清理资源

在使用完SetupDi API之后释放获取到的句柄资源。

`SetupDiDestroyDeviceInfoList(deviceInfoSet);`

完整示例代码：

以下是一个使用setupDi API获取系统中所有存在的USB设备友好名称的示例代码：

```cpp
#include <windows.h>
#include <setupapi.h>
#include <stdio.h>

#pragma comment(lib, "setupapi.lib")

int main()
{
    // 定义USB类GUID
    //GUID usbClassGuid = {0x36fc9e60,0xc465,0x11cf,{0x80,0x56,0x44,0x45,0x53,0x54,0x00,0x00}};
    GUID usbClassGuid = GUID_DEVCLASS_USB;   // #include <devguid.h>

    // 获取USB类的所有存在的设备信息集
    HDEVINFO hDevInfo = SetupDiGetClassDevs(&usbClassGuid, NULL, NULL, DIGCF_PRESENT);

    // 检查是否成功
    if (hDevInfo == INVALID_HANDLE_VALUE)
    {
        printf("SetupDiGetClassDevs failed with error %d\n", GetLastError());
        return 1;
    }

    // 定义索引变量和结构体变量
    DWORD index = 0;
    SP_DEVINFO_DATA devInfoData;
    devInfoData.cbSize=sizeof(SP_DEVINFO_DATA);

   // 遍历设备信息集
    while (SetupDiEnumDeviceInfo(hDevInfo, index, &devInfoData))
    {
        // 定义缓冲区变量和大小变量
        TCHAR friendlyName[256];
        DWORD friendlyNameSize = sizeof(friendlyName);
  
        // 获取设备友好名称
        if (SetupDiGetDeviceRegistryProperty(hDevInfo, &devInfoData, SPDRP_FRIENDLYNAME, NULL, (PBYTE)friendlyName, friendlyNameSize, NULL))
        {
            // 打印设备友好名称
            printf("Device %d Friendly Name: %s\n", index, friendlyName);
        }
        else
        {
            // 打印错误信息
            printf("SetupDiGetDeviceRegistryProperty failed with error %d\n", GetLastError());
        }

        // 增加索引
        index++;
    }

    // 检查是否遍历完毕
    if (GetLastError() != ERROR_NO_MORE_ITEMS)
    {
        printf("SetupDiEnumDeviceInfo failed with error %d\n", GetLastError());
    }

    // 释放设备信息集
    SetupDiDestroyDeviceInfoList(hDevInfo);

    return 0;
}
```