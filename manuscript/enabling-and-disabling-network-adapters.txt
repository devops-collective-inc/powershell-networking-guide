# Enabling and disabling network adapters
One of the most fundamental things that I do with a network adapter is either enable or disable it. In fact, I perform these tasks several times a week. This is because my primary work device is a laptop and it has built-in wireless network adapters. Not surprising, all modern laptops have both wired and wireless connections available. But when I am at home, in my office I want to have my laptop use the gigabit Ethernet switch that I have, and not go through the significantly slower wireless adapter. If I am on the road, I want to know if my wireless network adapter is enabled or not, and I want to control whether it connects to say a network named Starbucks for example. If I do not control such a thing, my laptop will automatically connect to every wireless it has ever seen before. This is why, for example [, I wrote this blog article about cleaning out wireless network history](http://blogs.technet.com/b/heyscriptingguy/archive/2013/06/16/weekend-scripter-use-powershell-to-manage-auto-connect-wireless-networks.aspx). Chris Wu, a Microsoft PFE also [wrote an article that takes a different approach](http://blogs.technet.com/b/heyscriptingguy/archive/2012/06/10/weekend-scripter-use-powershell-to-manage-windows-network-locations.aspx). It is a good read as well.

**PowerTip** : Enable all network adapters

Question: You are troubleshooting your Windows 8.1 laptop and want to quickly enable all network adapters. How can you do this?

Answer: Use the Get-NetAdapter and the Enable-NetAdapter commands. The command line appears here:
````
Get-NetAdapter |Where status -ne up | Enable-NetAdapter
````
## Using Devcon

In the old days, back before Windows Vista and Windows Server 2008 when I needed to enable or disable a network adapter, I would actually use [Devcon](http://support.microsoft.com/kb/311272). Devcon is a command line utility that provides the ability to enable and to disable various hardware devices. It is billed as a command-line Device Manager. Here is a VBScript I wrote to enable and to disable the network interface adapter using Devcon. Keep in mind that Devcon is not installed by default, and therefore must be installed prior to use.
````
'==========================================================================
'
' VBScript: AUTHOR: Ed Wilson , MS, 5/5/2004
'
' NAME: <turnONoffNet.vbs>
'
' COMMENT: Key concepts are listed below:
'1.uses the c:\devcon utility to turn on or off net
'2.uses a vbyesNO msgBOX to solicit input
'3. KB 311272 talks about devcon and shows where to get
'==========================================================================

Option Explicit

Dim objShell
Dim objExec
Dim onWireLess
Dim onLoopBack
Dim turnON
Dim turnOFF
Dim yesNO
Dim message, msgTitle
Dim strText

message = "Turn On Wireless? Loop is disabled" & vbcrlf & "if not, then wireless is disabled and loop enabled"

msgTitle = "change Network settings"
onWireLess = " PCMCIA\Dell-0156-0002"
onLoopBack = " \*loop"
turnON = "enable"
turnOFF = "disable"
Const yes = 6
Set objShell = CreateObject("wscript.shell")
yesNO = MsgBox(message,vbyesNO,msgTitle)

If yesNO = yes Then
WScript.Echo "yes chosen"
Set objExec = objShell.exec("cmd /c c:\devcon " & turnON & onWireLess)
subOUT
Set objExec = objShell.exec("cmd /c c:\devcon " & turnOFF & onLoopBack)
subOUT
Else
WScript.Echo "no chosen"
Set objExec = objShell.exec("cmd /c c:\devcon " & turnOFF & onWireLess)
subOUT
Set objExec = objShell.exec("cmd /c c:\devcon " & turnON & onLoopBack)
subOUT
End If

Sub subOUT
Do until objExec.StdOut.AtEndOfStream
  strText = objExec.StdOut.ReadLine()
  Wscript.Echo strText
Loop
End sub
````
## Using WMI

Beginning with Windows Vista (and Windows Server 2008) the Win32\_NetworkAdapter class gains two methods: disable and enable. These [are documented on MSDN here](http://msdn.microsoft.com/en-us/library/aa394216%28v=vs.85%29.aspx). These methods are instance methods which means that to use them, I need to first obtain an instance of the WMI class. What does this mean? Well I am using Win32\_NetworkAdapter and therefore I am working with network adapters. So, I need to get a specific network adapter, and then I can disable it or enable it. Here is how it might work:
````
$wmi=Get-WmiObject-ClassWin32\_NetworkAdapter-filter"Name LIKE '%Wireless%'"

$wmi.disable()
````
OR
````
$wmi=Get-WmiObject-ClassWin32\_NetworkAdapter-filter"Name LIKE '%Wireless%'"

$wmi.enable()
````
The thing to keep in mind is that when calling a method in Windows PowerShell, the parenthesis are required.

If I need to specify alternate credentials, I can specify a remote computer name and an account that has local admin rights on the remote box. The code would appear like the following:
````
$wmi=Get-WmiObject-ClassWin32\_NetworkAdapter-filter"Name LIKE '%Wireless%'"-credential (Get-Credential) -computernameremotecomputer
$wmi.disable()
````
Keep in mind that WMI does not permit alternate credentials for a local connection. Attempts to use alternate credentials for a local connection results in the error appearing here:
````
PS C:\> gwmi win32\_networkadapter -Credential (Get-Credential)

cmdlet Get-Credential at command pipeline position 1

Supply values for the following parameters:

Credential

gwmi : User credentials cannot be used for local connections
At line:1 char:
+ gwmi win32\_networkadapter -Credential (Get-Credential)
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
  + CategoryInfo     : InvalidOperation: (:) [Get-WmiObject], ManagementException
  + FullyQualifiedErrorId : GetWMIManagementException,Microsoft.PowerShell.Comman

 ds.GetWmiObjectCommand
````
This error, for local connections, is not a Windows PowerShell thing, WMI has always behaved in this manner, even going back to the VBScript days.

## Using the NetAdapter module

In Windows 8 (and above), I can use Windows PowerShell to stop or to start a network adapter by using one of the CIM commands. Of course, the function wraps the WMI class, but it also makes things really easy. The _netadapter _functions appear here (gcm is an alias for the Get-Command cmdlet)
````
PS C:\> gcm -Noun netadapter | select name, modulename

Name                    ModuleName
----                    ----------
Disable-NetAdapter             NetAdapter
Enable-NetAdapter             NetAdapter
Get-NetAdapter               NetAdapter
Rename-NetAdapter             NetAdapter
Restart-NetAdapter             NetAdapter
Set-NetAdapter               NetAdapter
````
**NOTE:** To enable or to disable network adapters requires admin rights. Therefore you must start the Windows PowerShell console with an account that has rights to perform the task.

The various network adapters on my laptop appear in the figure that follows.
![image025.png](images/image025.png)

I do not like having enabled, disconnected network adapters. Instead, I prefer to only enable the network adapter I am using (there are a number of reasons for this such as simplified routing tables, performance issues, and security concerns). In the past, I wrote a script, now I only need to use a Windows PowerShell command. If I only want to disable the non-connected network adapters, the command is easy. It appears here.
````
Get-NetAdapter| ? status-ne up| Disable-NetAdapter
````
The problem with the previous command is that it prompts. This is not much fun when there are multiple network adapters to disable. The prompt appears here.

![image027.png](images/image027.png)

To suppress the prompt, I need to supply $false to the -confirm parameter. This appears here.
````
Get-NetAdapter| ? status-ne up | Disable-NetAdapter -Confirm:$false
````
A quick check in control panel shows the disconnected adapters are now disabled. This appears here.
![image029.png](images/image029.png)

If I want to enable a specific network adapter, I use the Enable-Network adapter. I can specify by name as appears here.
````
Enable-NetAdapter -Name ethernet -Confirm:$false
````
If I do not want to type the adapter name, I can use the Get-NetAdapter cmdlet to retrieve a specific network adapter and then enable it. This appears here.
````
Get-NetAdapter -Name vethernet\*| ? status -eq disabled| Enable-NetAdapter -Confirm:$false
````
It is also possible to use wild card characters with the Get-NetAdapter to retrieve multiple adapters and pipeline them directly to the Disable-NetAdapter cmdlet. The following permits the confirmation prompts so that I can selectively enable or disable the adapter as I wish.
````
PS C:\> Get-NetAdapter -Name vethernet\* | Disable-NetAdapter

Confirm

Are you sure you want to perform this action?

Disable-NetAdapter 'vEthernet (InternalSwitch)'

[Y] Yes [A] Yes to All [N] No [L] No to All [S] Suspend [?] Help

(default is "Y"):y

Confirm

Are you sure you want to perform this action?

Disable-NetAdapter 'vEthernet (ExternalSwitch)'

[Y] Yes [A] Yes to All [N] No [L] No to All [S] Suspend [?] Help

(default is "Y"):n
````

