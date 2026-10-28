在Windows 7下安装U871或U872，在检测组件窗口时，会出现MDAC组没有安装的提示，是因为Windows 7所带的MDAC软件版本太高，U8无法检测到。 
### 针对32位的WIN7
运行regedit，打开注册表，进入
```
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\DataAccess
将
 "FullInstallVer"="6.1.7601.17514" 
"Version"="6.1.7601.17514"
 修改为 
"FullInstallVer"="2.82.3959.0"
 "Version"="2.82.3959.0" 
```
### 针对64位的WIN7
运行regedit，打开注册表，进入
```
HKEY_LOCAL_MACHINE\SOFTWARE\wow6432node\Microsoft\DataAccess
将
"FullInstallVer"="6.1.7601.17514"
 "Version"="6.1.7601.17514" 
修改为 
"FullInstallVer"="2.82.3959.0" 
"Version"="2.82.3959.0" 
```
安装成功后再将值改回来就OK了 
