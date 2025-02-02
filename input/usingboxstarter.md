﻿---
Order: 40
Title: Using Boxstarter Commands
---

# Using Boxstarter Commands

You can use the Boxstarter Shell to run Boxstarter Commands or any PowerShell console of your choice. Here is some guidance to get you started.

## The Boxstarter Shell

Especially if you are not comfortable with Windows PowerShell, you may prefer to use the Boxstarter Shell to run Boxstarter commands. The Boxstarter Shell will make sure that the user is running with administrative privileges, the execution policy is compatible and all Boxstarter PowerShell modules are loaded and accessible. This shell also prints some basic "Getting Started" text at startup to assist you in running your first commands. To access the Boxstarter Shell:

If you installed Boxstarter using Chocolatey, you can simply type BoxstarterShell from any command line:

```powershell
Microsoft Windows [Version 6.1.7601]
Copyright (c) 2009 Microsoft Corporation.  All rights reserved.
C:\Windows\system32>BOXSTARTERSHELL
Welcome to the Boxstarter shell!
The Boxstarter commands have been imported from C:\Users\mwrock\AppData\Roaming
\Boxstarter and are available for you to run in this shell.
You may also import them into the shell of your choice.
Here are some commands to get you started:
Install a Package:   Install-BoxstarterPackage
Create a Package:    New-BoxstarterPackage
Build a Package:     Invoke-BoxstarterBuild
Enable a VM:         Enable-BoxstarterVM
For Command help:    Get-Help  -Full
For Boxstarter documentation, source code, to report bugs or participate in dis
cussions, please visit https://boxstarter.org
PS C:\>
```

Or you can use the shortcuts installed to the desktop and start menu:

![Boxstarter shortcut icon](/assets/images/shortcut.png)

![Boxstarter shell](/assets/images/shell.png)

The remainder of the instructions on this page cover requirements to consider if using your own shell.

## Administrative Privileges

Many Boxstarter commands require an "Elevated" PowerShell console. This is a PowerShell window opened explicitly with administrative privileges. Where it can, Boxstarter will attempt to open a new command with administrative privileges, but in some other cases, you may get an error stating that administrative rights are required.

Depending on the OS version you are using, there are a few ways to launch PowerShell as an Administrator. One way that works everywhere is:

1.  Press the Windows key.
2.  Start typing "PowerShell" until you see the Windows PowerShell shortcut appear.
3.  Select this shortcut and then type **Ctrl+Shift+Enter**

**Windows 8**

![Windows 8 administrative privileges in Windows Powershell](/assets/images/win8admin.png)

**Windows 7**

![Windows 7 administrative privileges in Windows Powershell](/assets/images/win7admin.png)

## Execution Policy

By default, PowerShell blocks the running of scripts and will therefore block the execution of all Boxstarter commands. If you have not done so already on the machine you are using Boxstarter, there is a one time command you must run in order to lift this restriction:

```powershell
Set-ExecutionPolicy Unrestricted -Force
```

This must be run with administrative privileges. Now you can call Boxstarter commands.

## Importing the Boxstarter Modules

Like all other PowerShell modules, the Boxstarter modules must be imported in order to be used. As long as Boxstarter was installed using Chocolatey or the Boxstarter download zip file, the directory where the Boxstarter PowerShell modules are installed, is added to the PSModulePath. This means that the module can be imported simply by referencing the module name and there is no need to specify the exact path of one's module manifest:

```powershell
Import-Module Boxstarter.Chocolatey
```

Although there are several Boxstarter modules, you should only ever need to import the Boxstarter.Chocolatey module. This is because during Module initialization, all other modules are loaded by the Boxstarter.Chocolatey module.

## Discovering Boxstarter Commands

Once the Boxstarter Modules are imported, you can see them in your session:

```powershell
C:\dev\boxstarter> Get-Module
ModuleType Version    Name                                ExportedCommands
---------- -------    ----                                ----------------
Script     2.0.11     Boxstarter.Bootstrapper             {Enter-BoxstarterL...
Script     2.0.11     Boxstarter.Chocolatey               {Get-BoxStarterCon...
Script     2.0.11     BoxStarter.WinConfig                {Disable-InternetE...
Manifest   3.1.0.0    Microsoft.PowerShell.Management     {Add-Computer, Add...
Manifest   3.0.0.0    Microsoft.PowerShell.Security       {ConvertFrom-Secur...
Manifest   3.1.0.0    Microsoft.PowerShell.Utility        {Add-Member, Add-T...
```

You can list all commands available from a PowerShell module by using:

```powershell
C:\dev\boxstarter> Get-Command -Module Boxstarter.Chocolatey
CommandType     Name                                               ModuleName
-----------     ----                                               ----------
Alias           Install-ChocolateyInstallPackage                   Boxstarte...
Function        Get-BoxStarterConfig                               Boxstarte...
Function        Get-PackageRoot                                    Boxstarte...
Function        Install-BoxstarterPackage                          Boxstarte...
Function        Install-ChocolateyInstallPackageOverride           Boxstarte...
Function        Invoke-BoxStarterBuild                             Boxstarte...
Function        Invoke-ChocolateyBoxstarter                        Boxstarte...
Function        New-BoxstarterPackage                              Boxstarte...
Function        New-PackageFromScript                              Boxstarte...
Function        Set-BoxStarterConfig                               Boxstarte...
Function        Set-BoxstarterShare                                Boxstarte...
```

## Help!

PowerShell can provide detailed help for any command that provides this metadata. Calling `Get-Help CommandName -Full` will return this information:

```powershell
C:\dev\boxstarter> Get-Help Install-BoxstarterPackage -Full
NAME
    Install-BoxstarterPackage
SYNOPSIS
    Installs a Boxstarter package
SYNTAX
    Install-BoxstarterPackage [-PackageName] <string> [-Credential
<pscredential>] [-Force] [-DisableReboots] [-KeepWindowOpen] [-LocalRepo
<string>] [<commonparameters>]
    Install-BoxstarterPackage [-ComputerName] <string> [-PackageName] <string>
    [-Credential <pscredential>] [-Force] [-DisableReboots] [-LocalRepo
<string>] [<commonparameters>]
    Install-BoxstarterPackage [-ConnectionUri] <uri> [-PackageName] <string>
    [-Credential <pscredential>] [-Force] [-DisableReboots] [-LocalRepo
<string>] [<commonparameters>]
    Install-BoxstarterPackage [-Session] <pssession> [-PackageName] <string>
    [-Credential <pscredential>] [-Force] [-DisableReboots] [-LocalRepo
<string>] [<commonparameters>]
DESCRIPTION
    This function must be run as administrator.
    This function wraps a Chocolatey Install and provides these additional
    features
     - Installs Chocolatey if it is not already installed
     - Installs the .net 4.5 framework if it is not installed which is a
    Chocolatey requirement
     - Disables windows update service during installation to prevent
    installation conflicts and minimize the need for reboots
     - Imports the Boxstarter.WinConfig module that provides functions for
    customizing windows
     - Detects pending reboots and restarts the machine when necessary to
    avoid installation failures
     - Provides Reboot Resiliency by ensuring the package installation is
    immediately restarted up on reboot if there is a reboot during the
    installation.
     - Ensures everything runs under admin
     - Supports remote installations allowing packages to be installed on a
    remote machine
    ...
```

Boxstarter also provides certain topical help resources you can find with:

```powershell
C:\dev\boxstarter> Get-Help about_Boxstarter
Name                              Category  Module                    Synopsis
----                              --------  ------                    --------
about_boxstarter_logging          HelpFile                            Descri...
about_boxstarter_bootstrapper     HelpFile                            Descri...
About_Boxstarter_Variable_In_B... HelpFile                            A Hash...
about_boxstarter_chocolatey       HelpFile                            Descri...
About_Boxstarter_Variable_In_C... HelpFile                            A Hash...
about_boxstarter_bootstrapper     HelpFile                            Descri...
About_Boxstarter_Variable_In_B... HelpFile                            A Hash...
about_boxstarter_chocolatey       HelpFile                            Descri...
About_Boxstarter_Variable_In_C... HelpFile                            A Hash...
about_boxstarter_logging          HelpFile                            Descri...
```
