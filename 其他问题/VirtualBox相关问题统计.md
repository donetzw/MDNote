### 一、整理虚拟机空间占用
##### 将sdelete放在c盘根目录下，cmd里执行

```javascript
C:\sdelete64.exe –z C:\
```

##### 执行完后把虚拟机关机，在主机中打开cmd执行

```JavaScript
cd C:\Program Files\Oracle\VirtualBox\
VBoxManage modifyhd D:\VirtualOS\Windows7\Windows7.vdi -compact    win7虚拟机
VBoxManage modifyhd D:\VirtualOS\Windows10\Windows10.vdi -compact  win10虚拟机
```
### 二、安装XP系统
存储-控制器型号改成ICH6
系统-芯片组改成ICH9

### 三、复制粘贴只生效一次
在虚拟机里先复制粘贴一次，再从主机复制到虚拟机