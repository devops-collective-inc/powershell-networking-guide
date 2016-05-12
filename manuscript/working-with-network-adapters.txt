# Working with Network adapters
Windows offers many different ways to work with Network Adapters. The correct choice depends upon several things. First probably, I need to know what version of the operating system I am running. In most cases the version of the operating system will either limit or expand my options for working with Network Adapters. Next I need to know if I am working locally or remotely, because where I run my commands from often determine my choice of tool. Lastly I always choose the tool that is easiest for me to do the job I have to perform. This is not always the easiest tool for anyone to use, but I choose the tool that I know. For me, for example, typing even a dozen commands into the Windows PowerShell ISE is much easier than attempting to use NetSh in some context with which I am unfamiliar. In addition, by typing my commands into the Windows PowerShell ISE, I can easily save my commands off as a Windows PowerShell script, that I can reuse. Of course I can reuse NetSh commands - I do all the time, but it is an extra step. So, to summarize, what is my decision matrix (assuming identical capabilities)?

1. Version of Operating System
2. Remoting capability
3. Ease of use

All things being equal, what tools are available to me to use to accomplish my work with network adapters?

1. Windows PowerShell
2. NetSH
3. Windows Management Instrumentation (WMI)
4. VBScript
5. Console Utilities

In this booklet, I will talk about each of these approaches as I look at the different tasks. So what tasks am I talking about? Well, I am specifically talking about the network adapter. So here are the things I am going to cover:

1. Identifying network adapters
2. Enabling and disabling network adapters
3. Renaming network adapters
4. Finding connected network adapters
5. Identifying network adapter power setting
6. Configuring network adapter power settings
7. Gathering network adapter statistics

Along the way, I will be showing some pretty cool Windows PowerShell tricks.

**PowerTip** : Find protocol binding on net adapters using PowerShell

Question: How can you use Windows PowerShell to show which enabled protocols are bound to your network adapters using Windows 8.1 and PowerShell 4.0?

Answer: Use the Get-Netadapter cmdlet to retrieve all of the network adapters on your system. Then pipeline it to the Get-NetAdapterBinding cmdlet and filter on enabled is equal to true. This command appears here:
```
Get-NetAdapter|Get-NetAdapterBinding|-enabled-eq$true
```
