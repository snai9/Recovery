# 安装自己的一键恢复
## 标题显示版本号
```aardio
mainForm.text=mainForm.text+" V"+tostring(fsys.version.getInfo(io._exepath).productVersion);
```
## 判断当前系统是工作在UEFI还是BIOS模式
```aardio
//故意给出一个错误GUID值，OUT=0且ERR=998为UEFI，OUT=0且ERR=1，或者OUT=1时为BIOS
var out = ::Kernel32.GetFirmwareEnvironmentVariableW("","{00000000-0000-0000-0000-000000000000}",null,0);
var err = ::Kernel32.GetLastError()
if(0==out && 998==err ){
	mainForm.edit3.text="winload.efi"
} 
else {
	mainForm.edit3.text="winload.exe"
}
```
## 从后往前判断盘符的进阶
### 1.静态指定，适用盘符较少时
```aardio
if(io.exist("G:\snap")){
		mainForm.edit.text="G:\snap"};
	elseif(io.exist("F:\snap")){
		mainForm.edit.text="F:\snap"};
	elseif(io.exist("E:\snap")){
		mainForm.edit.text="E:\snap"};
	elseif(io.exist("D:\snap")){
		mainForm.edit.text="D:\snap"};
```
### 2.动态指定，拼接方式直观
```aardio
var drives = sys.volume.getLogicalDrives();
for(i=#drives;1;-1){
 	if(io.exist(drives[i]+"\snap")){
 	mainForm.edit.text=drives[i]+"\snap"
 	break
```
### 3.动态指定，改变拼接方式，更方便更巧妙
```aardio
var drives = sys.volume.getLogicalDrives();
for(i=#drives;1;-1){
    	var path = string.format("%s\snap", drives[i]); // 使用 string.format 拼接路径
    	if(io.exist(path)){
        	mainForm.edit.text = path;
        	break;
```
## 批处理管道进程的创建方法
### aardio
```aardio
var prcs = process.batch.wow64(battext, {
    par = io.splitpath(mainForm.edit.text).drive; // 命名参数
    name = mainForm.edit2.text; // 命名参数
    "位置参数1"; // 位置参数1
    "位置参数2"; // 位置参数2
})
```
###  批处理
```批处理代码
echo 驱动器是: %owner.par%
echo 名称是: %owner.name%
echo 第一个参数是: %1
echo 第二个参数是: %2
```

### 命名参数
- 传递方式：命名参数是通过表对象的键值对传递的。例如：  

  aardio var prcs = process.batch.wow64(battext, {par = io.splitpath(mainForm.edit.text).drive; // 命名参数 name = mainForm.edit2.text; // 命名参数 }) 
- 接收方式：在批处理中，命名参数通过 %owner.参数名% 接收。例如：  

  batch echo 驱动器是: %owner.par% echo 名称是: %owner.name% 

### 位置参数
- 传递方式：位置参数是直接作为数组传递的。例如：  

  aardio var prcs = process.batch.wow64(battext, {"位置参数1"; // 位置参数1"位置参数2"; // 位置参数2 }) 
- 接收方式：在批处理中，位置参数通过 %1, %2, %3 等方式接收。例如：  

  batch echo 第一个参数是: %1 echo 第二个参数是: %2 

### 总结两种参数
- 命名参数 和 位置参数 是分开处理的，它们的传递方式和接收方式不同。
- 命名参数通过 %owner.参数名% 接收，位置参数通过 %1, %2, %3 等方式接收。
- 第一个命名参数不是位置参数1，它们是两个独立的参数传递机制。




