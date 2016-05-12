# Windows PowerShell Basics - Working with help options
The first thing you need to do is to update the help files on your system. This is because Windows PowerShell 3.0 introduces a new model in which the help files update on a regular basis.

To update help on your system, you must ensure two things. The first is that you open the Windows PowerShell console with ADMIN rights. This is because the Windows PowerShell help files reside in the protected Windows\System32\WindowsPowerShell directory. Once you have launched the Windows PowerShell console with admin rights you need to ensure your computer has Internet access so it can download and install the updated files. If your computer does not have Internet connectivity, it will take several minutes before the command times out (Windows PowerShell tries really hard to obtain the updated files). If you run the Update-Help cmdlet with no parameters Windows PowerShell attempts to download updated help for all modules stored in the default Windows PowerShell modules locations that support updatable help. To run Update-Help more than once a day use the -Force parameter as appears here.
```
Update-Help -Force  
```
Even without downloading updated Windows PowerShell help, the help subsystem displays the syntax of the cmdlet and other rudimentary information about the cmdlet. In this way.

To display help information from the internet, use the -Online switch. When used in this way, Windows PowerShell causes the default browser to open to the appropriate page from the Microsoft TechNet web site.

In the enterprise, network administrators may want to use the Save-Help cmdlet to download help from the Internet. Once downloaded, the Update-Help cmdlet can point to the network share for the files. This is an easy task to automate, and can run as a scheduled task.
