# 创建windows驱动开发环境

[toc]

## 安装VS2019

​	1.下载VS2019社区版，[下载链接](https://visualstudio.microsoft.com/zh-hans/vs/older-downloads/)；

​	2.运行安装工具，修改安装目录为d盘，选择开发组件和windows sdk，windows sdk选择10.0.18版本；

![](https://gitee.com/ningbocai/pictures/raw/master/image-20221014/131938-88.PNG)

## 安装wdk

​	1.下载wdk，wdk版本要与上文中windows sdk版本保持一致，[下载链接](https://learn.microsoft.com/zh-cn/windows-hardware/drivers/other-wdk-downloads)；

​	2.下载完成后运行安装程序，安装路径选择之前的windows sdk路径；

![](https://gitee.com/ningbocai/pictures/raw/master/image-20221014/131939-4d.PNG)

​	3.运行“D:\Windows Kits\10\Vsix\VS2019\WDK.vsix”，给vs2019安装驱动开发插件，路径为wdk的安装路径；

## 创建第一个驱动程序

​	1.运行vs2019，新建一个空的驱动项目；

![](https://gitee.com/ningbocai/pictures/raw/master/image-20221014/131940-dd.PNG)

​	2.创建第一个驱动程序；

![](https://gitee.com/ningbocai/pictures/raw/master/image-20221014/131941-e6.PNG)

```c
#include<wdm.h>

//卸载函数
VOID unload(PDRIVER_OBJECT pdriver) 
{
	DbgPrint("unload is ok\n");
}

//驱动程序入口
NTSTATUS DriverEntry(PDRIVER_OBJECT pdriver,PUNICODE_STRING path)
{
	DbgPrint("driver inject is ok\n");
	pdriver->DriverUnload = unload;
	return STATUS_SUCCESS;
}
```

​	3.编译链接生成“.sys”驱动文件；

## 测试驱动程序

​	1.安装“[Oracle VM *VirtualBox*](https://www.baidu.com/link?url=CvKvj5qAatdqb0EOcEkQJoAtT0gG-xm6pV5qyyYvPF3g-ApXvw_ry0RCWU0ceH6F&wd=&eqid=df6b99e60008ee1e00000006633cab8c)”虚拟机，在虚拟机上安装windows10系统；

​	2.将tools文件下“signtools-v3.2.zip”、“driver monitor”、“DebugView.zip”、“签名证书.zip”拷贝到虚拟机中；

​		signtools-v3.2.zip：给驱动程序添加数字签名；

​		driver monitor工具：实现驱动程序的运行；

​		DebugView.zip：microsoft官方查看程序的debug信息；

​	3.调节系统时间为2011-10-01，为签名工具添加签名证书和签名规则；

​		也可以直接关闭系统的驱动程序签名功能；

![](https://gitee.com/ningbocai/pictures/raw/master/image-20221014/131941-0a.PNG)

​	4.将“.sys"驱动程序拷贝到虚拟机中，给驱动程序添加内核数字签名；

​	5.打开debugView窗口，设置监视内核调试信息；

​	6.使用“driver monitor”工具运行驱动程序，如看到以下输出则视为成功：

![](https://gitee.com/ningbocai/pictures/raw/master/image-20221014/131943-0a.PNG)

## 安装windbg实现双机调试

​	1.若按上文安装vs2019选择了debug组件，则不需要安装windbg，没有则单独进行安装；

​	2.目标系统运行“msconfig”，设置以调试模式引导，重启系统；

![](https://gitee.com/ningbocai/pictures/raw/master/image-20221014/131944-41.PNG)

​	3.按下图对“virtualbox”虚拟机串口进行设置；

![](https://gitee.com/ningbocai/pictures/raw/master/image-20221014/131945-5a.PNG)

​	4.设置windbg连接目标系统；

![](https://gitee.com/ningbocai/pictures/raw/master/image-20221014/131945-b4.PNG)

​	5.出现如下画面则表示双机调试连接成功；

![](https://gitee.com/ningbocai/pictures/raw/master/image-20221014/131946-1c.PNG)
