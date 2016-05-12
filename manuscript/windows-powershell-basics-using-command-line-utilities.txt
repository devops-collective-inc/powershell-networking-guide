# Windows PowerShell Basics - Using command line utilities
As easy as Windows PowerShell is to use, there are times when it is easier to find information by using a command line utility. For example, to find IP configuration information you only need to use the _Ipconfig.exe _utility. You can type this directly into the Windows PowerShell console and read the output in the Windows PowerShell console. This command and associated output appears here in truncated form.
```
PS C:\> ipconfig

Windows IP Configuration

Wireless LAN adapter Local Area Connection\* 14:

 Media State . . . . . . . . . . . : Media disconnected
 Connection-specific DNS Suffix . :

Ethernet adapter vEthernet (WirelessSwitch):

 Connection-specific DNS Suffix . : quadriga.com
 Link-local IPv6 Address . . . . . : fe80::915e:d324:aa0f:a54b%31
 IPv4 Address. . . . . . . . . . . : 192.168.13.220
 Subnet Mask . . . . . . . . . . . : 255.255.248.0
 Default Gateway . . . . . . . . . : 192.168.15.254

Wireless LAN adapter Local Area Connection\* 12:

 Media State . . . . . . . . . . . : Media disconnected
 Connection-specific DNS Suffix . :

Ethernet adapter vEthernet (InternalSwitch):

 Connection-specific DNS Suffix . :
 Link-local IPv6 Address . . . . . : fe80::bd2d:5283:5572:5e77%19
 IPv4 Address. . . . . . . . . . . : 192.168.3.228
 Subnet Mask . . . . . . . . . . . : 255.255.255.0
 Default Gateway . . . . . . . . . : 192.168.3.100

<OUTPUT TRUNCATED>
```
To obtain the same information using Windows PowerShell would require a more complex command. The command to obtain IP information is Get-NetIPAddress, But there are several advantages. For one thing, the output from the _IpConfig.exe _command is text, whereas the output from Windows PowerShell is an object. This means you can group, sort, filter, and format the output in an easy fashion.

The cool thing is that with Windows PowerShell console, you have not only the simplicity of the command prompt, but you also have the powerful Windows PowerShell language built in. Therefore, if you need to refresh Group Policy three times and wait for five minutes between refreshes, you can use the command appearing here (looping is covered in chapter eleven).
```
1..3 | % {gpupdate ; sleep 300}
```
