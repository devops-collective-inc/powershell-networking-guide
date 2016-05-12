# Windows PowerShell Basics - Working with modules
What makes the big difference in capabilities between Windows PowerShell 4.0 installed on Windows 7 or Windows 8.1 is not the difference in the capability of Windows PowerShell 4.0. The package provides the same abilities. The difference is the modules introduced with Windows 8 and expanded in Windows 8.1. To find out the commands that a module provides, I use the Get-Command cmdlet and specify the name of a particular module. In this example, I look at the commands provided by the NetAdapter.
```
Get-Command-ModuleNetAdapter
```
If I use the Get-Command cmdlet and an error arises, it may be because the module has not yet loaded. To load the module use the Import-Module cmdlet. This command appears here.
```
Import-ModuleNetAdapter
```
If I am curious as to the number of commands exposed by the module, I can pipeline the results to the Measure-Object cmdlet. This command appears here.
```
Get-Command-Modulenetadapter|Measure-Object
```
