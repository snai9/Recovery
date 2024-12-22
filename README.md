# Recovery
## 标题显示版本号
```aardio
mainForm.text=mainForm.text+" V"+tostring(fsys.version.getInfo(io._exepath).productVersion);
```
## 判断当前系统是工作在UEFI还是BIOS模式
```aardio
## 标题显示版本号
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
